---
title: "TopSetup: Getting on Board with Rails"
date: 2017-02-17T12:00:00-00:00
draft: false
tags:
  - ruby-on-rails
  - personal-projects
---

I am finally done with the design phase of TopSetup™! Time to start coding! 🤗

I just started learning Ruby on Rails about a month ago to prepare for my new internship at [Brand New Box][1]. I’ve used AngularJS which is another MVC framework, but it was JavaScript based. Easy stuff. Rails, on the other hand, is completely new territory since it is Ruby based.

I can’t believe I waited this long to learn it. Rails is a lazy programmer’s dream!

There is quite a bit of a learning curve, but once you get over that hump, you’ll find that it does a lot of the heavy work for you. Here are a couple things so far that I found amazingly helpful:

Generators
Rails has a set of `rails generate` commands that work on the terminal. They generate a lot of code for you so that you don’t have to go through the tedious task of creating and organizing the files yourself. For example, if you need to create a view for a blog post:

```bash
$ rails generate scaffold BlogPost title:string content:text
```

This will not only create the views, but it will also create the model, controller, assets, tests, and routes you might need. Plus, if you think you’ve made a mistake and want to start over, you can just run `rails destroy`. It will remove all the related files for you.

Migrations
One of the things I always hate doing is changing a database schema after it’s already been created. Rails is the bomb dot com with migrations. They are automatically timestamped and versioned. Oh, you’ve created a BlogPosts model and realized your silly tush forgot to include a column for the blog content? No worries. Just run:

```bash
$ rails generate migration add_content_to_blogPosts content:text
$ rake db:migrate
```

Boom! Done.

Check out some screenshots below. Our website is really starting to come along! 😊

<img src="http://i.imgur.com/DIoTY6B.png">

<img src="http://i.imgur.com/uIUALe7.png">

[1]: http://brandnewbox.com/
