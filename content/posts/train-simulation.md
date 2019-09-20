---
title: "Multicore Programming - Train Simulation"
date: 2017-02-23T12:00:00-00:00
draft: false
tags:
  - cpp
  - multithreading
---

This semester, I’m taking a class on parallel programming - EECS 690: Multicore and GPGPU Programming.

It's a funny story actually. I meant to take the Computer Graphics (EECS 672) class, but it was only offered in the fall. Then, I saw this 690 class that is taught by the same professor. I had no idea what multicore meant but I know GPU. It had to have something to do with graphics, right?

Nope. I’m a nub. 😅

The motivation behind this class is [Moore’s Law][1]. It's more (pun intended) of an observation, really. It states that the operational processing speed of computers (though, we later discovered that it is actually the transistor count) doubles approximately every 18-24 months. This means we can get smaller and more powerful technology about every two years! For those of you who've seen [Hidden Figures][2], just look how far we've come from the IBM 7090. That single computer took up an entire room! Today, our teeny smartwatches have a heck more computing power that machine.

However, we are at a point where the trend is starting to break. It's becoming really difficult to find ways to dissipate all the energy that comes from packing so much power in a small space. Remember the whole Samsung Note 7 mess? 💥💥

As a result, the attention has shifted to different parallelization techniques. Two common approaches are: multicore parallelization (either in a single machine or in a networked collection of machines) or using GPUs for general-purpose parallel computing.

To start, we are learning multithreaded programming on a single machine. The first project is to simulate a train scheduling system. Here are the requirements:

Suppose you are responsible for the design of a train scheduling system with the following operational specifications:

1. There are *n* trains identified by letters: *A*, *B*, ... (Assume *n* ≤ 26)
2. There are *m* stations identified by sequential numbers: 0, 1, 2, ..., *m-1*
3. There is at most a single track between any pair of stations, hence only one train at a time may be traveling in either direction along the track between a given pair of stations
4. All stations have the capacity to hold *n* trains
5. A given track will be identifed as *i–j*: the track between station *i* and station *j*. Note that *i-j* and *j–i* denote the same track
6. At the start of a day, each train will be given an assigned route in the form of the list of stations to which it is to travel. For example: 3, 5, 1, 7, 6, 4. You are to assume:
  * Each train is initially at the first station in this list.</li>
	* Whenever the list has: ..., *i*, *j*, ..., assume that *i*≠*j*, there is a direct track between *i* and *j*, it takes one unit of time to go from *i* to *j* along this track, and the train must use that track to go from <i>i</i> to *j*
  * At the end of the day, the train will stay at the last station in the list.

Consider a simulation in which we determine at each time step which trains that are not yet at their final destination are cleared to go to the next station in their list (or whether they must stay where they are for that time step).

Want to see how I implemented it? Check out Part 2]!

[1]: http://www.investopedia.com/terms/m/mooreslaw.asp
[2]: http://www.imdb.com/title/tt4846340/
