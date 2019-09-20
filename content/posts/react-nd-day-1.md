---
title: "React ND - Day #1"
date: 2017-08-02T12:00:00-00:00
draft: false
tags:
  - react
---

[Last week][1], I mentioned enrolling in Udacity’s React Nanodegree program. I’m already pretty familiar with React, but I feel like I have a lack of foundation when it comes to the building principles of React. So, I was pretty excited when I found out about Udacity’s new React Nanodegree. Here’s to unlearning bad habits! 🍻

## Day #1
- Worked from 6:15 to 7:15pm
- Made it through Part 1: Lesson 1 & 2

### Lesson 1
First thing I learned was **function composition** - the idea of combining simple functions to build more complicated ones. I had flashbacks to Calculus when I heard it. Never forget: `(f ∘ g)(x) = f(g(x))`. React is heavy on function composition. All of its UI components are essentially built from the nesting of smaller functions.

A good function should follow the **DOT** rule: Do One Thing. It should contain logic for only one task. For example, a function `getProfileData()` should only return the profile data. If needed, any parsing logic should probably be factored out into another function, for example, `parseProfileData()`.

Another concept I learned was the difference between **imperative** and **declarative** code. I remember going over this in school but this was a great refresher. Imperative code is where you explicitly tell a program **how** to do something to get what you want done. Declarative code is where you tell a program **what** you want done and it will take care of the rest.

For example, changing the temperature in your room to 72 degrees with your brand new Google Home. In terms of imperative code, you would say “Ok Google, increase the temperature by one degree.” You would keep telling it that until you reach 72 degrees. With declarative code, you would instead say "Ok Google, set the temperature to 72 degrees” and your Google Home would take care of the rest. I drew a little visual to help me remember the difference.

{{< image src="/declarative-vs-imperative.jpg" alt="Declarative vs. Imperative Code" >}}

### Lesson 2
This lesson was a short one about how UI is rendered with React. It introduced `create-react-app` and explained `React.Component` briefly.

### Takeaway
React **favors composition over inheritance**. This is different than what we are used to. Many programming languages and frontend frameworks make use of inheritance. React, on the other hand, uses composition as mentioned in lesson 1. Instead of extending a base component with inheritance, we compose a new components with nesting. React components should be independent, focused, and reusable.

[1]: https://github.com/sharynneazhar/blog/blob/master/_posts/2017-07-23-udacity-discovery-week.markdown
