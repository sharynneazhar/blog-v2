---
title: "React ND - Redux, Pt. 2"
date: 2017-11-21T12:00:00-00:00
draft: false
tags:
  - react
---

## Day #6
* Worked from 10:15am - 1:10pm
* Made it through Lesson 2

### Lesson 2: Redux At Its Core
The concept of Redux, as we learned last time, was pretty simple to understand. However, learning how to implement Redux may be a little bit trickier. There are three major players in Redux:

1. **Actions**
2. **Reducers**
3. **The Store**

Learning Redux is difficult because in order to be able to implement anything, you really need to understand how these three components work together. Let's break it down.

First, we have the *ruler of the land*, the store. The store contains the app's global state which it receives from a reducer (there can be more than one reducer). Now for the state to change, the store needs to dispatch an action (there can be more than one action as well). This action is passed to the reducer and the reducer updates the store's state. They're all interconnected!

{{< image src="/redux-data-flow.png" alt="Redux Data Flow" >}}

#### Actions
Redux **actions** are similar to the DOM events like the `click`, `dblclick`, or `mouseenter` we are used to seeing in web development. Redux actions are used to represent the different type of events, or *actions*, that happen in an app.

Actions are really just JavaScript objects that describe any event that should update an app's state. What distinguishes an action from others is a specific `type` property we must include. For example,

```js
const EDIT_USER = 'EDIT_USER';
const editUser = {
  type: EDIT_USER,
  id: 123,
  user: {
    name: 'John Jacob Jingleheimer Schmidt',
    age: 23,
    isMyName: true,
    isOut: false,
    arePeopleShouting: false
  }
}
```

This indicates that an `EDIT_USER` event took place. As you can see, Redux actions can include data too. By adding in the `id` and `user` objects above, we're including which user should be edited and their new information.

There are a couple of conventions to keep in mind while building action objects:
* Prefer constants rather than strings as the values of `type` properties. Both work - but when using constants, the console will throw an error rather than fail silently should there be any misspellings (e.g. EIDT_USER vs. EDIT_USER).
* Keep the payload as small as possible. Have your resources only send the necessary data!

Now, these plain action objects are not very portable when you need to use it in multiple places. In order to make actions more portable and easier to test, we can wrap these actions in functions we call **action creators**. As they are called, these functions create and return an action. For example,

```js
const editUser = user => ({
  type: EDIT_USER,
  user
})
```

Note that these actions are not directly modifying the state themselves; instead, they are specifying an event that occurred which should update the app state. Recall that only reducers have access to update the store!

#### Reducers
A **reducer** sets up the initial state of an app which is then stored in the, well you guessed it, the store. A reducer is a pure function that gets passed two arguments: the current state and and the action that was just dispatched.

```js
function reducer(state = initialState, action) {
  // ...
}
```

The action is used to determine what *changes* should be made to the state. What we mean by change here is that a reducer copies the existing data, modify the copy, and return the updated copy. Remember that we are not allowed to change anything directly in the store.

Within the reducer, we create a switch statement (or if/else) to match the `type` property provided by the action. Then, we return the new state.

```js
function reducer(state = initialState, action) {
  switch(action.type) {
    case 'EDIT_USER':
      return Object.assign({}, state, {
        user: action.user
      });
    case 'DELETE_USER':
      return state.filter(user => user.id !== action.id)
    default:
      return state;
  }
}
```

Whenever an `editUser` action creator is called and passed to a reducer, the switch statement will match the `'EDIT_USER'` case. Then, a new state will be created and the `user` property will be updated accordingly.

Like the action creators, there are some rules for how reducers should behave as well. The more important rule is that a reducer *must* be a **pure function**. Pure functions

1. Return one and the same result if the same arguments are passed in
2. Depend solely on the arguments passed into them
3. Do not produce side effects

A reducer should only take in the current state and an action, and return the new state. That’s it! If you’re doing anything more than that in your reducer, your code is probably doing something wrong.

#### The Store
Time for the big honcho - the **store**. As mentioned before, the store holds the app's state, dispatches actions, and calls reducers. To create a store, we'll use Redux's `createStore()` method which takes a reducer and returns a new store object.

The store only has a few other methods:

* `getState()` which will return the current state of the store
* `dispatch(action)` which takes an action and passes the current state and the action to the reducer function
* `subscribe(listener)` which takes a listener callback function that will get triggered whenever the state of the store changes


### Takeaways
The main concepts of Redux are actions, reducers, and the store. The store is the source of truth for the state in your application, reducers specify the shape of and update the store, and actions are payloads of information which tell reducers which type of events have occurred in the application.
