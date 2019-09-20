---
title: "React ND - Redux, Pt. 1"
date: 2017-11-20T12:00:00-00:00
draft: false
tags:
  - react
---

## Day #5?
* Worked from 8:30pm - 9:50pm
* Made it through Lesson 1

It's been awhile, fam. Since school started, I haven't had very much time to work on these lessons. Though, it's Thanksgiving break this week and I plan on grinding. Hopefully, I can finish the two projects left in this Nanodegree by this weekend!

Let's get cracking.

### Lesson 1: Why Redux?
#### [Motivation for Redux][1]
If you think about bugs you've had come across while developing an app, the cause of those bugs probably came from some state or data *mismanagement*. React is already pretty good at state management but it is limited. What if we have two components that need access to the same state? Here's where Redux is useful.

One of the main benefits of Redux is **shared state** management. Recall that one of React's core features is its *unidirectional data flow*. So, if we have React by itself, we'd need to hoist the state up to a nearby parent that these two components share so that they can access the state. Now, what if the parent is several components up the tree. Passing state as a prop one-by-one down the tree could be a pain in the butt and extremely inefficient.

Redux allows us to better manage, access, and store data. All we need to do is tell Redux exactly which components would need which data, and it will take care of the rest.

#### The Store
Redux is based on the [principle][2] of a *single source of truth*: the store. The store contains an app's global state. Most of the state is stored within a single tree (a single source of truth) and lives separately from the components. The app's data, or state, lives in the store and individually components can access the data they need from the store directly.

{{< image src="/redux-store.png" alt="Redux Store" >}}

However, to maintain its predictability, the store does come with rules:
1. State is **read-only**.
2. State is **immutable**.
3. Components can only *request access* to update the state.

> But, Sharynne, how can we develop an app without ever changing the state???

State can only be change using *pure functions* called **reducers**. More on pure functions [here][3].

Now, let's back up a minute. If you didn't catch it earlier, Redux does not actually store *all* of an app's state. There is no hard rule that says all state *must* be contained in a Redux store. It's really up to you, but here's a helpful tip from Dan Abramov himself:

{{< image src="/react-or-redux.png" alt="React or Redux State?" >}}

Generally speaking, if more "localized" data is involved or if you're dealing with state that doesn't affect other components, then component state is a solid choice. For example,

* Form input
* Current tab selected
* Dropdown open/closed status

### Takeaways
Redux is a JavaScript library used to manage an application’s front-end state. Redux isn’t a requirement for React apps, but as web apps become more complex, bugs may arise from mismanaging state. Global state in Redux apps is held within a single source of truth: the store. Since updates to state are tightly controlled, this makes Redux very predictable.


[1]: https://redux.js.org/docs/introduction/Motivation.html
[2]: https://redux.js.org/docs/introduction/ThreePrinciples.html#three-principles
[3]: https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-pure-function-d1c076bec976
