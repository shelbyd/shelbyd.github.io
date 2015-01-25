---
layout:     post
author:     Shelby Doolittle
title:      "TDD: Test Driven Design"
subtitle:   "How to write enjoyable code guided by tests"
date:       2015-01-01 00:00
# header-img: "img/post-bg-06.jpg"
---

TDD is often the initialism for Test Driven Development.
It's unfortunate that it would also be the initialism for Test Driven Design, a much better practice.

What's the difference?
--------------------------------

Test Driven Development is a way to develop software by writing tests before writing code.
The common mantra of red-green-refactor is a good guiding force.
The benefits of writing tests before production code have been espoused elsewhere, so I won't bother defending them here.

Test Driven Design emphasizes what to think about while practicing Test Driven Development.
Tests are great for checking that your code does what it's supposed to, but they can be so much more than that.
Tests can lead you down the road to clean, loosely coupled, and simple code.
Like many things in life, all you need to do is listen.

On my current project, the team works with two codebases primarily.
Our Rails codebase is easy to understand, simple to work with, and rarely is the root cause of bugs.

On the other hand, our Android codebase is bug-ridden, tightly coupled, and incredibly difficult to understand.
To be fair, the features desired from the Android code may lead to intrinsically more complexity than the desires from the server.
But all that means is that it's even more important to write quality code.

The main difference I see is in the quality of the tests.
In Android most of our tests are in the view components.
The tests say things like "When someone touches this button, and the HTTP request returns that, then this button disappears."
There will be, in some cases, 10 tests that start with the same setup and actions but check interactions with UI components and external libraries.

This codebase exemplifies the point made in J.B. Rainsberger's excellent talk [Integrated Tests are a Scam](http://vimeo.com/80533536).
If you don't have time to watch that talk, make the time.
If you still don't have time to watch it, he argues that writing integrated tests leads to sloppier design which leads to more bugs which leads to more integrated tests.
The cycle continues and you have worse and worse code through every iteration.

What's the solution?
--------------------

Unit tests are the solution!
The best way to unit tests are listening to tests that are hard to write.
Some signs to look for:

1. More that one line of unique setup code per condition in your description
1. The same setup checking effects at different levels (UI, HTTP, DB, etc)
1. Different setups checking the same effects.

If you start seeing these signs, or your tests are hard to set up, or they just feel gross, you need to listen to them and work on your design.
