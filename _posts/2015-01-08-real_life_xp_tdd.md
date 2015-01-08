---
layout:     post
author:     Shelby Doolittle
title:      "Real Life XP: TDD"
subtitle:   "How TDD works in real life"
date:       2015-01-08 00:00
header-img: "img/not-afraid-paris.jpg"
---

At Pivotal Labs, we do Extreme Programming (XP).
XP got its name from taking all the good principles from other practices to the extreme.
Today, we look at one of these examples and how it works in real life.

General knowledge among software developers is that having tests for code is good.
If you are a software developer and you disagree, this is one of the instances where you are just wrong.
So, how do we take "Tests are good" to the extreme?

Test Driven Development
-----------------------

I love Test Driven Development (TDD).
It helps you write easy to reason about code by giving you feedback on your design based on how hard your tests are to set up.
(More on that in a future article.)
But what does TDD look like at Labs?
Do we really write tests before every single line of production code in accordance with the XP principles?

No...
We don't always write tests first.
Most of the time, it's because we can't write tests (CSS anyone?).

Untestable Code
---------------

When we can't write tests for something, we make the untestable part of the code as small, simple, and clear as possible.
This helps against people changing or adding code that might break our application.
We also add comments noting very clearly when something is not tested.

Sometimes, a pair will think they can't test something but aren't sure.
In this case, they will always go to another pair and ask for more opinions on if something is testable and how to test it if it is.
Often that other pair will have a way to test it, and many times it leads to better design.

For instance, recently a pair was working on trying to figure out how fast a phone could download a file of a known size so we could stream data at a better resolution.
This problem dealt with both time and the network, two things notoriously hard to test.
They were stuck trying to figure out how to test their code and asked me and my pair for help.
We recommended pulling out two supporting objects.
One would give the current time in milliseconds, the other would just download the file.
Now they could mock both of those objects in the tests for the code that did the calculations.
And those two objects were each one line of standard library code.

We ended up with a better design and the time object was used from then on to get the current time.

Should we test this?
--------------------

Sometimes, when developing, my pair will ask the question "Should we test this?"
Almost always, my answer is yes.
I disagree with some other pivots about whether or not code that makes no decisions should be tested.
I think yes, if only to define what it is supposed to do.
They say no, because the code is essentially declarative, and you generally need to test declarative code less.

If there is a question about the code, someone will come along later and have a similar question.
If there are not tests to make them confident about changing code, then they will tend to put more behaviour in.
At this point, the code should definitely be tested.
But it's hard to write good tests for code that's already there, so the tests will often not be written.

If I'm involved in the discussion of "Should we test this?" I push hard to write the tests.
Often they're easy to write too.
If they're not, then the code is doing too much and needs to be broken up.

Conclusion
----------

Testing is one of the most discussed topics among pivots.
When to test and when not to test is one of the questions that spurs these discussions.
We test almost all of our code.
And if we don't, we make sure we have a good reason not to.
