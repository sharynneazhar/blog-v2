---
title: "CSS Tricks: Responsive Tables"
date: 2017-04-19T12:00:00-00:00
draft: false
tags:
  - css
---

For a current project I'm working on, I had the task to create a generic table view. There were a few caveats. The website will be used in schools so that means a lot of Internet Explorer users... possibly old versions of IE as well.

Well, that stinks.

My first instinct is to use Bootstrap's table component. Bootstrap already has responsive class all set up and ready to go, so why not? It turns out that Bootstrap's table doesn't work so well with mobile-size screens. I had expected the table to collapse, but Bootstrap's solution was to simply enable horizontal scrolling. 😐

So, I had to go on a Google quest.

The best solution I found was to "force the table to not behave like a table". Say whaat?! The solution works by setting the table components to be block-level and making each of the table rows to be a table in itself. The key to the solution is the CSS `:before` pseudo-class.

Best part, it work with IE!

I'll admit that the solution isn't perfect, but it works for what I needed. Let me know if you've found a better solution. I'd love to learn about it. 😊


Check out the article [here](https://css-tricks.com/responsive-data-tables/)!
