---
layout:     post
author:     Shelby Doolittle
title:      "Find Order Dependencies in RSpec Test Suites"
subtitle:   "How to quickly find which tests are causing which other tests to fail"
date:       2015-01-13 00:00
---

Order dependencies are one of the banes of test suites.
If tests do not consistently pass or fail, one cannot draw decent conclusions from them.
Luckily, for RSpec, there is a gem to help with that.

RSpec-Bisect
------------

[RSpec-Bisect](https://rubygems.org/gems/rspec-bisect) is that gem (and yes, I wrote it).
It will detect which tests are failing due to an order dependency, and will figure out which test(s) are causing it.

Have your rspec config set so that ```rspec --seed 1234``` runs your test suite with the specified seed.

Run ```rspec-bisect --seed 1234``` and rspec-bisect will tell you which tests are from an order dependency and the minimum tests required to reproduce it.

I've used rspec-bisect in a few projects to find order dependencies in our test suite, and it has saved an incredible amount of time.
I hope it will for you too!
