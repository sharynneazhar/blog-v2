---
title: "Pretty JSON"
date: 2017-04-26T12:00:00-00:00
draft: false
tags:
  - javascript
---

I first learned about the `JSON.stringify()` method from a senior developer during an internship. Since then, I've used it A LOT to debug JS code, but one thing that always annoyed me was how it prints the whole JSON object in one long line.

I spent a good 30 minutes or so looking at different Stack Overflow solutions. Through combinations of googling bad keywords and lazy skimming, I found a <s>good</s> great solution.

The answer is in the [documentation][1]. Duh, why didn't I think of that in the first place?

The native `JSON.stringify()` method has a built-in option to format readable output. Here's the syntax:

```js
JSON.stringify(value[, replacer[, space]])
```

The first parameter `value` is the value you want to convert to a JSON string. The second parameter `replacer` is basically a filtering function you can pass in to ignore certain values. The third parameter `space` is the golden ticket. It is a String or Number object that is used to insert whitespace into the output.

Example:

```js
var myObj = {"foo": "bar", "nums": [{"one": 1, "uno": 1}, {"two": 2}]};
JSON.stringify(myObj, null, 2);
```


Output:

```json
{
  "foo": "bar",
  "nums": [
    {
      "one": 1,
      "uno": 1
    },
    {
      "two": 2
    }
  ]
}
```

Aha!

[1]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify
