---
layout:     post
author:     Shelby Doolittle
title:      "Test Smells"
subtitle:   "How your tests are telling you to fix your code"
date:       2015-01-25 15:00
header-img: "img/new-car-smell.jpg"
---

Code smells are common patterns that signal your code might not be maintainable in the future.
They come with strategies for cleaning up the smells, resulting in better code.
They were popularized by Martin Fowler in his book [Refactoring](http://martinfowler.com/books/refactoring.html).
If you're not familiar with code smells I highly suggest reading that book.

Tests are technically code, but don't experience the same smells.
Test Driven Design says that one should listen to feedback from tests as signs that code should be refactored.
But what does this feedback look like?

We'll identify some common bad patterns that appear in test code and discuss what they say about the corresponding production code.
As with code smells, these are just common patterns that lead to unmaintainable systems.
It would be a fool's errand to try to eliminate these patterns entirely from your codebase.
So use them as design tools and keep a look out, knowing you can ignore them if you can convince someone else it's a good idea.

Over the next few weeks, I'll be going deep into each smell.
Complete with refactorings to fix the problems.
Let's get to it!

The Smells
----------

- Spidering
- Business Setup
- Shared Expectations
- Setup Expectations
- Big Actions
- Fat Contexts
- Context Hell
- Expect Bloat
- Popular Fake
- Mystery Contexts

Spidering
---------

Your tests involve many different levels of behavior.

"Levels" is a loosely defined term.
The easiest example of spidering is a test that involves stubbing HTTP requests after interacting with view elements.
We can clearly see that this is working with no levels of abstraction.

In your application a level could mean anything, for example business logic vs serializing data for a response.
The only time this is okay is integrated tests, and [integrated tests in general cause problems](http://vimeo.com/80533536).

Spidering in unit tests is a sign that you are violating the [Single Responsibility Principle](http://blog.8thlight.com/uncle-bob/2014/05/08/SingleReponsibilityPrinciple.html).

- Break up classes into things that interact with at most one external service.
- Extract objects that encapsulate the responsibilities of another level and stub or mock it in the original test.

Business Setup
--------------

Your tests run business logic in setup.

In order to set up for one test, you run the real implementation of the same or another class.
Hopefully, the class under test interacts with that other class directly.

Business setup is a sign that your code is tightly coupled.
In order for one test to pass, you need to run real implementations.
This means that if that implementation code changes and breaks, your test breaks and it's not clear why.

Note: this is not limited to code you control.
You could also run real code from a library to setup some state for a test.

- Replace real code setup with stubs of dependencies
- Move code that relied on that setup into the dependencies that are now stubbed out

Shared Expectations
-------------------

Your tests have different setups that result in the same expectation(s).

This is a sign that your code has many paths to the same result.
That is fine, but what you're testing can be broken up.
 
- Extract the result into a method on an object 
- Replace the checks of the result with an expectation on a mock
- Extract objects for each path to the result

Setup Expectations
------------------

Your tests can fail before getting to the action of your test.

Don't do this.
If you feel the need to have an expectation in your setup, you need simpler setup or to extract a dependency that can encapsulate that setup in a stub.
Also, if that expectation starts to fail, many tests will fail and you cannot identify the interesting failures.

- Don't include expectations in setup
- If tempted to include expectations in setup, create a new test that has that expectation

Big Actions
-----------

Your tests have more than 1 line of action code.

This is a sign that the interface you're testing is suboptimal.
An interface that requires a client call methods in a certain order will cause hard to diagnose bugs in production.
These multiple lines can be consolidated into one method in the public interface.
This has the nice effect of clients needing to know less.

- Change the public interface of the code under test to require only one call to accomplish some goal

Fat Contexts
------------

Your tests have more than 1 line of setup per context.

This causes problems when trying to read the tests.
If someone who is not familiar with the system cannot read the test and immediately identify which line corresponds to which context, your tests have this smell.

This is a sign that the contexts with more than one line could be extracted into a query method on a dependency.
Then you could stub out the result of the query method to be your one line of setup for the context.

- Extract contexts with multiple lines into dependencies
- Replace complicated context setups with a stub of the new dependencies

Context Hell
------------

Your tests have more than 3 contexts per test.

We often know of many things that matter when implementing new behavior.
If we have only five things that matter, and each is binary, then that is thirty two tests we need to write!
Add one more context and the number of tests needed doubles.

Often, many combinations of these contexts will have the same behavior.
We can then separate these contexts into dependencies that will figure out which case we are in.
 
- Identify contexts that result in the same behavior
- Extract these contexts into query methods on dependencies

Expect Bloat
------------

Your tests have more than 1 type of expectation in a test.

Tests with more than one expectation are generally a problem.
But, serializers often include tens of expectations that do belong together.
So be wary of tests that have more than one "type" of expectation.
Again type is loosely defined.

An example may be making expectations of both the return value of a method and verifying it calls a method on a dependency in the same test.
Having tests with this problem results in vague, confusing test descriptions that obscure the result that matters.

- Separate types of expectations into multiple tests with the same setup and action

Popular Fake
------------

Your tests have a stub/mock used in most every test file.

Your code has one dependency that everything uses.
This is a sign that your code is working at too low a level of abstraction.
If most of your code calls an HTTP wrapper, for example, you could likely abstract one or more objects that encapsulate the tasks for which you use the HTTP wrapper.

- Introduce abstractions around common tasks performed with the shared dependency
- Replace fakes of the dependency with the new abstraction

Mystery Contexts
----------------

It is hard to tell which setup is related to which context.

Someone unfamiliar with the codebase cannot verify the tests are correct.
If a new team member cannot read a test and see that the setup corresponds exactly to the test description, they cannot trust the test.

- Arrange setup exactly with test descriptions
- Have setup appear in the same order as the contexts appear in the test descriptions

Conclusion
----------

These are common patterns I've found to be a sign of problematic tests.
Note that these say nothing about the speed of tests or many other metrics characteristic of good test suites.
Though, I have found tests suites with fewer test smells generally perform well on the other metrics.

Keeping these patterns in mind has helped guide me in identifying problems in my test suites.
They've led me to writing easily understood tests and maintainable code.
