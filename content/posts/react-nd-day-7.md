---
title: "React ND - Redux, Pt. 3"
date: 2017-11-23T12:00:00-00:00
draft: false
tags:
  - react
---

## Day #7
* Worked from 11pm - 1am
* Made it through Lesson 4

### Lesson 3: React & Redux
The creators of Redux wrote a really helpful package `react-redux` that provides us with a better abstraction for Redux. With the `connect()` method from `react-redux`, we won't have to keep passing the store down a bunch of nested components (this is called *prop threading*); instead, `connect()` allows use to specify which components should receive which data from the store.

#### Currying
**Currying** occurs when you call a function without all of its arguments and it returns a function that's waiting for the next argument. For example, suppose we have a simple `plate()` function that takes in two arguments: a `vegetable` and a `fruit`.

```js
function plate(vegetables, fruit) {
  return `I ate a plate of ${vegetables} and ${fruit}!`;
}

plate('corn', 'apples');
```

Now say that for whatever reason we want to delay getting the fruit until a later point. One way we could achieve this would be to return a function which accepts the fruit that can be invoked at a later point.

```js
function plate(vegetables) {
  return function fruitFunc (fruit) {
    return `I ate a plate of ${vegetables} and ${fruit}!`;
  }
}

const fruitFunc = plate('corn');
```

Now we have a `fruitFunc()` that we can call, pass it a `fruit`, and the `vegetables` (i.e. corn) are still accessible via closures.

Another way to write this would be like this:

```js
function plate(vegetables) {
  return function fruitFunc (fruit) {
    return `I ate a plate of ${vegetables} and ${fruit}!`;
  }
}

const sentence = plate('corn')('apples');
```

This technique - currying - is used a lot in functional programming and in Redux.

#### The Connect Method
`connect()` is a function that makes it possible for a component to get both state and dispatch from the Redux store. Fully used, it looks like this:

```js
connect(mapStateToProps, mapDispatchToProps)(MyComponent)
```

This is a classic example of currying in Redux! `MyComponent` is the component you want to receive store state, dispatch, or both. `mapStateToProps()` is a function that receives the current store, current props, and what it returns will be available to `MyComponent` as props. `mapDispatchToProps()` allows you wrap action creators inside of dispatch.

##### mapStateToProps()
`mapStateToProps()` allows you to specify which data from the store you want passed to a React component. The properties of the object returned from `mapStateToProps()` will be passed to the component as props. For example,

```js
import { connect } from 'react-redux';

const User = ({ name, age }) => {
  // ...
};

const mapStateToProps = (state, props) => ({
  name: state.user.name,
  age: state.user.age
});

export default connect(mapStateToProps)(User);
```

In the above example, both `name` and `age` will be available as props for the `User` component to access.

##### mapDispatchToProps()
`mapDispatchToProps()` is similar to how `mapStateToProps()` works. Instead of binding state, it binds the `dispatch()` method from Redux to our action creators before they hit a component.

```js
import React, { Component } from 'react';
import { connect } from 'react-redux';
import { updateName } from './actions';

class User extends Component {
  state = { name: '' }
  handleUpdateUser = () => {
    this.props.boundUpdateName(this.state.name)
  }
  render () {
    // ...
  }
}

const mapDispatchToProps = dispatch => ({
  boundUpdateName: (name) => dispatch(updateName(name))
});

export default connect(null, mapDispatchToProps)(User);
```

#### Building an Effective Redux Store
When architecting a Redux store, there are two things we should keep in mind:

1. **Do not duplicate data**. If data lives in multiple places, you have no single source of truth, and you waste resources trying to keep the data in sync with each other.
2. **Keep your store as shallow as possible**. Nested data makes reducer logic more complicated (trying to update deeply nested data can get slow and complex quickly).

**Normalization** is the process of removing duplicate pieces of data and making sure that the data is as shallow as possible. Not only does this allow applications to maintain the “single source of truth” in the store’s state -- reducer logic that updates that state is also kept clean and reasonable. Ultimately, normalizing your Redux store will lead to more efficient and consistent queries.

How do we normalize our data? We can create *references*. Keep every entity in an object stored with an ID as a key, and use IDs to reference it from other entities, or lists.

The basic concepts of normalizing data are:

* Each type of data gets its own "table" in the state.
* Each "data table" should store the individual items in an object, with the IDs of the items as keys and the items themselves as the values.
* Any references to individual items should be done by storing the item's ID.
* Arrays of IDs should be used to indicate ordering.

Learn more about normalization [here][1] from the Redux docs.



[1]: http://redux.js.org/docs/recipes/reducers/NormalizingStateShape.html
