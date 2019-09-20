---
title: "React ND - Redux, Pt. 4"
date: 2017-11-27T12:00:00-00:00
draft: false
tags:
  - react
---

## Day #8
* Worked from 1:30pm - 2:30pm
* Made it through Lesson 5

### Lesson 5: Redux Middleware
When I was learning React-Redux, one concept that I could never wrap my head around was the concept of a middleware. What is a middleware?

**Middleware** is code that intercepts a request or a process, usually redirecting it or producing some sort of side effect. In [Redux][1], middleware lives in between the dispatching of action and the reducers.

{{< image src="/redux-middleware.png" alt="Redux Middleware" >}}

Here are some examples of what middlewares can do:

* Logging state
* Making asynchronous HTTP requests
* Running some code during the dispatch (e.g. parsing data)
* Dispatching other actions

So, where does the middleware go in a Redux app? Redux provides us with the `applyMiddleware()` method that we can pass as our `enhancer` argument to the `createStore()` method. For instance,

```js
import { applyMiddleware, createStore } from 'redux';
import logger from 'redux-logger';

const store = createStore(
  reducer,
  applyMiddleware(logger)
)
```

#### Thunks
Whenever people talk about middlewares, you'll also hear them talking about this thing called a **thunk**. What in the world are thunks?

Without middlewares, actions are dispatched synchronously by default. You can think of thunks as middleware wrappers for the store's `dispatch()` method. Instead of returning action objects, we can use **thunk action creators** to asynchronously dispatch functions or *Promises*.

Suppose we need to make the following HTTP request:

```js
// util/todos_api.js
export const fetchTodos = () => fetch('/api/todos');
```

Our action creator can look like:

```js
import * as TodoAPI from '../util/todo_api';

export const RECEIVE_TODOS = "RECEIVE_TODOS";

export const receiveTodos = todos => ({
  type: RECEIVE_TODOS,
  todos
});

export const fetchTodos = () => dispatch => (
  TodoAPI
      .fetchTodos()
      .then(todos => dispatch(receiveTodos(todos)))
);
```

By setting up a Promise, the action to receive all to-do items is dispatched *only* when the original request is complete.

To summarize, when the to-do list page renders, the component calls a **thunk action creator** to fetch the to-do items. The following events happen in the following order:

1. The TodoAPI request occurs
2. The API request is resolved
3. Thunk middleware invokes the function with dispatch()
4. Action is dispatched

#### More Resources
* [Redux Examples][2] from the Redux docs
* [Awesome-Redux][3] list of examples and middlewares on GitHub

### Takeaways
If a web application requires interaction with a server, applying middleware such as **thunk** helps solve the issue of asynchronous data flow. Thunk allows us to write action creators that return functions (rather than objects). The thunk can then be used to delay an action dispatch, or to dispatch only if a certain condition is met (e.g., a request is resolved).


[1]: https://redux.js.org/docs/advanced/Middleware.html
[2]: https://redux.js.org/docs/introduction/Examples.html
[3]: https://github.com/xgrommx/awesome-redux
