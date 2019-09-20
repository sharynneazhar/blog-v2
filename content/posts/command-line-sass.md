---
title: "Command Line Sass"
date: 2017-03-23T12:00:00-00:00
draft: false
tags:
  - sass
---

Turns out there is a way to use SASS without an application like Rails or Compass. It is fairly easy to get it up and running, especially if you are developing on a Mac because Ruby already comes pre-installed! ️️Otherwise, you'll need to install Ruby on your computer.

After Ruby is installed, you need to install SASS. Just run the following:

```bash
gem install sass
```

*Note: if you run into errors, you might need to use the sudo command.*

When the installation is done, you can double-check by typing:

```bash
sass -v
```

This should show the SASS version installed. For example, it might say `Sass 3.4.22 (Selective Steve)`.

After all that is done, SASS is ready to run! You can run either individual files or entire directories. You can even watch folders or directories with the `--watch` flag. For example, if I have all my stylesheets in a folder called "stylesheets", I can just run the following command:

```bash
sass --watch path/to/project/stylesheets/
```

*Reference: [Using SASS](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#using_sass)*
