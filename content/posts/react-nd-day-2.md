---
title: "React ND - Day #2"
date: 2017-08-04T12:00:00-00:00
draft: false
tags:
  - react
---

## Day #2
* Worked from 10:45pm to 12:15am
* Made it through Part 1: Lesson 3

### Lesson 3: State Management
In this lesson, I learned more about **state management** in React. There are three main parts of state management:

1. With props
2. With function components
3. With controlled components

#### Section 3.1: Passing Data with Props
Remember that React is a Javascript library and it is nothing but a series of composed functions. **Props** are synonymous to arguments (or parameters). Think of passing props to components as passing arguments to functions. Note, however, props are immutable which means they are *read-only*. There are no “pass by reference” or “pass by value” shenanigans here.

#### Section 3.2-3.6: Functional Components
From Tyler McGinnis’s [article][1], I learned that there are three ways to describe React components:

1. Stateless Components
2. Stateless Functional Components
3. Functional Components

Tyler makes a really great point at the end of his article. I couldn’t say it better myself.

> …clearly understanding the vocabulary for technical topics will make learning and teaching said topics much easier.

It truly is important to know the proper terminology. It prevents confusion.

In contrast to props, states are mutable. In React, your UI is just a function of your state. Check out this nifty GIF from the video:

{{< image src="/react-state-ui.gif" alt="React UI" >}}

Stateless function components are really useful if all you need to do is render some UI. Less code and less complexity. For example, instead of writing

```js
class Profile extends React.Component {
    render() {
        return (
            <div>
                <span>Name: {this.props.name}</span>
                <span>Age: {this.props.age}</span>
            </div>
        );
    }
}
```

we can write,

```js
const Profile = (props) => (
    <div>
        <span>Name: {this.props.name}</span>
        <span>Age: {this.props.age}</span>
    </div>
);
```

***PRO-TIP!*** Avoid defining a component’s initial state with `props`. This is an error-prone *anti-pattern* since state will only be initialized with `props` when the component is first created. If props were to update, the current state will not change unless we “refresh" the component.

Don’t do this:

```js
this.state = {
    profile: props.profile
}
```

One possible solution is to write a separate handler to update your state using `this.setState()`. Here’s a short [article][2] that explains a bit more about why setting initial state with props is a bad idea.

#### Section 3.7: Controlled Components
Nothing new here except learning what the proper term is for these components and the benefits:

* Instant input validation
* Ability to conditionally disable/enable buttons
* Enforce input formats

More about it [here][3].

#### Miscellaneous
**Object destructuring** introduced with ES6 helps keep code clean. Instead of using `this.state.value` or `this.props.value` everywhere, we can create new variables with

```js
const { contacts, onDeleteContact } = this.props;
const { query } = this.state;
```

### Takeaway
React UI is just a function of mutable states!

[1]: https://tylermcginnis.com/functional-components-vs-stateless-functional-components-vs-stateless-components/
[2]: https://medium.com/@justintulk/react-anti-patterns-props-in-initial-state-28687846cc2e
[3]: https://facebook.github.io/react/docs/forms.html#controlled-components
