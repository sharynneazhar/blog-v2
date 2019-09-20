---
title: "One Liners - Using Array.prototype.some"
date: 2017-07-19T12:00:00-00:00
draft: false
tags:
  - javascript
---

We all can agree that JavaScript can be a huge pain sometimes (ok, maybe all the time). It's constantly changing and there are a lot of nuances with its methods. A couple years ago, I was introduced to [ES6][1] and all of its awesome new features, particularly its new iterative methods like `some()`, `filter()`, `indexOf()` and a few others. Learning these methods made my JavaScript life a little bit sweeter.

I had the honor of refactoring some legacy code recently for a popular [MOOC][2] company. The task was to determine if a student had graduated from a degree program and if the program was eligible for certain employment opportunities. To do that, I'd have to iterate over a couple different lists: the student's list degree enrollments and the list of eligible degrees.

There are many ways to approach this, but my first instinct was to do:

```js
function isEligible(studentList, eligibleList) {
  var isEligible = false;
  for (var i = 0; i < studentList.length; i++) {
    for (var j = 0; j < eligibleList.length; j++) {
      if (studentList[i].graduated && studentList[i] === eligibleList[j])
        isEligible = true;
    }
  }

  return isEligible;
}
```

Wouldn't it be great if we could smash all that into one single line?

The `some()` method does exactly that. Here, I paired the `some()` method and the `indexOf()` method to get:

```js
let isEligible = studentList.some(degree => eligibleList.indexOf(degree) !== -1);
```

The `indexOf()` method searches the eligibleList array for the specified degree and return its position, or `-1` if not found. If at least one of the degrees was found in eligibleList, then `some()` returns true.

In terms of performance, native (vanilla) JS would almost always win. The question boils down to "Do you want to spend time code more than you have to or leverage what's already out there?" For me, I'm willing to sacrifice a little bit of performance to save time and to have cleaner code.

*Edit: The new iterative methods mentioned above was introduced earlier in [ES5 specifications][3].*

[1]: https://github.com/lukehoban/es6features
[2]: https://en.wikipedia.org/wiki/Massive_open_online_course
[3]: http://ecma-international.org/ecma-262/5.1/#sec-15.4.4.17
