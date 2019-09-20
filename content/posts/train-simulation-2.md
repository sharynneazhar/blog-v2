---
title: "Train Simulation, Pt. 2"
date: 2017-02-26T12:00:00-00:00
draft: false
tags:
  - cpp
  - multithreading
---

The first project in my parallel programming class is to implement a train scheduling system using threads (we will be coding this in C++). You can find more information about the project in my previous post.

Up to this point, I've only dealt with sequential programming which means instructions are executed one at a time. It's easy to visualize programs like that. Without running the program, you can easily trace through your code and know if something will be off.

Parallel programs are at a whole 'nother level. Program instructions - we call these "threads" - run independently at different times. Their execution depends on the operating system scheduler. It's hard to visualize what's going on because you have no way of knowing which thread will run at which time step. Threads can pretty much do whatever they dang well please. Debugging is a nightmare.

For the first time in a long time, I had to sit down and really think about how I was going to accomplish this. Here's the high level implementation:

```
if all trains are ready
  for all trains
    while a train is not at its final destination
      if the train track is not being used
        advance to next station
      else
        stay and wait
    endwhile
  endfor
endif
```

If you think about this sequentially, it's pretty simple right? All you need is an array of trains, an array of their schedules, and an array of train tracks in use. Then, at each iteration (time step), you'd just check if the train track you need is in the trains tracks array. Easy.

> Tip: When designing a parallel program, first think about how you would implement it in a sequential program. That way, it's quick and easy to pick out the sections that can be implemented in parallel.

From the pseudocode above, pay attention to the while loop. Why is that important? This section of code represents a single train operation and if we look further, it doesn't care about what other trains are doing at any given time. It only cares about which tracks are free to use. Here's where parallel programming comes in. 🙌 Since there are no real dependencies between the trains, we can divide them into their own tasks. Specifically, we'll assign each train to its own thread.

```cpp
std::thread** trainThreads = new std::thread*[numTrains];
for (int i = 0; i < numTrains; i++) {
  trainThreads[i] = new std::thread(runTrain, trains[i], numTrains);
}
```

What is happening here?

For this, we declare an array of threads called `trainThreads` that will hold our arbitrary number of trains. Then, to create and initialize our threads, all we need to do is call the `std::thread` constructor:

```cpp
std::thread(runTrain, trains[i], numTrains);
```

The first parameter in the constructor is the work assignment that we want to assign to the thread. In our case, that is the method `runTrain`. Any parameters after the first are passed as parameters to the first parameter. So, `trains[i]` and `numTrains` are parameters that belong to the function `runTrain` which will be responsible for handling the following code section we determined from above:

```
while a train is not at its final destination
  if the train track is not being used
    advance to next station
  else
    stay and wait
endwhile
```

In the meantime, our main function will just be waiting until the threads are done working. The builtin function for that is `join()`. So, before the main function returns, we'd call:

```cpp
for (int i = 0; i < numTrains; i++) {
  // Join the thread with the main thread
  trainThreads[i]->join();
}
```

One last thing to mention before diving into the nitty gritty of the `runTrain` code. Remember how I said threads can do whatever they please?

Try running the code below.

```cpp
#include <thread>
#include <iostream>

void work(int assignment) {
  std::cout << "I am thread #" << assignment << "\n";
}

int main(int argc, char* argv[]) {
	std::thread** t = new std::thread*[nThreads];
	for (int i = 0; i < 10; i++)
		t[i] = new std::thread(work, i);

	std::cout << "Hello from the parent thread.\n";
	for (int i = 0; i < 10; i++)
		t[i]->join();

	return 0;
}
```

You can see that the results are spit out in no particular order. It's all garbled! It is the programmer's job to make sure these threads are behaving themselves and share their resources or wait until the other thread is done.

There are these semaphores called a mutex (or mutices) that we need to use as barriers for our threads. It's a way for us to synchronize how they share a resource. In the example above, we need to wrap a mutex around the `std::cout` resource to block other threads from snatching it while it is in use. We can simply modified our code to be:

```cpp
std::mutex coutMutex;
void work(int assignment) {
	coutMutex.lock();
	std::cout << "I am thread #" << assignment << "\n";
	coutMutex.unlock();
}
```

Mutices are a big part of parallel programming. Erm, at least, for this project it is important. 😅 If you haven't already guessed it, our train tracks in this project will each be its own mutex. So, I had another array declared to represent all possible track mutex instances:

```cpp
// A given track will be identifed as i–j:
// the track between station i and station j.
// Note that i–j and j–i denote the same track.
std::mutex** trackMtxs = new std::mutex*[numStations];
for (int i = 0; i < numStations; i++) {
  for (int j = i; j < numStations; j++) {
    trackMtxs[i] = new std::mutex();
  }
}
```

Now, we can expand on our `runTrain` function. The pseudocode will looks like this:

```
while a train is not at its final destination
  if the track mutex is not locked, lock it
    advance to next station
    unlock the track mutex
  else
    stay and wait
endwhile
```

Another thing we need to make sure is that all the trains are running at the same time. To do that, we need some kind of barrier to synchronize our trains at each time step. For this, we have our `Barrier` class. This class has a condition variable and a counter. At each iteration (i.e. time step), the condition variable to block the program from moving on to the next iteration until the counter reaches a certain expected value. Here, our expected value would be the number of trains. When each thread hits the barrier, it will increase the counter variable. Once it reaches the expected value, the barrier will give all the running threads the green light to continue to the next iteration.

Finally, here is my `runTrain` implementation!

```cpp
void runTrain(Train* train, int numTrains) {
  // wait until all trains threads are initialized
  while (!ready) {};

  // the timestep for each train should be synchronized
  int timeStep = 0;

  while (numTrainsDone != numTrains) {
    int currentStop = train->getCurrentStop();
    int nextStop = train->getNextStop();

    if (!train->isAtEnd()) {
      // try to lock the track and advance if unoccupied
      if (lockTrack(currentStop, nextStop)) {
        train->move(timeStep);
      } else {
        train->stay(timeStep);
      }
    }

    // hit barrier to wait for the rest of the threads
    theBarrier.barrier(numTrains);

    // unlock the track mutex once the barrier gives the go
    unlockTrack(currentStop, nextStop);

    // increment to next time step
    timeStep++;
  }
}
```

So, that is all there is to it for this train simulation. Pretty simple starter project for a beginner. Hopefully, I can come back in the future to improve my code and um, make it work properly. 😅

[Source Code][2]

[2]: https://github.com/sharynneazhar/coursework/tree/master/eecs690/project1
