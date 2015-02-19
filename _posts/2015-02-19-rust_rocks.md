---
layout:     post
author:     Shelby Doolittle
title:      "Rust Rocks"
subtitle:   "My experience with the high level, systems level language"
date:       2015-02-19 00:00
# header-img: "img/post-bg-06.jpg"
---

In the past few weeks I've been learning more about [rust](http://www.rust-lang.org/).
Seeded by Yehuda Katz's excellent GoGaRuCo talk [Let's Talk About Rust](https://www.youtube.com/watch?v=ySW6Yk_DerY), I've been invited into the world of systems level programming.
To learn rust, I made a simple app that will compare two poker hands and output the winner (find it [here](https://github.com/shelbyd/rust-poker)).

There are so many talks and posts about why to use rust, I'll share my main likes/dislikes that I got from working on my first rust "project".

What I like
===========

Rust is fast
------------

I've come from a world of garbage collection and exclusively heap allocation.
In that world, computers do things fast.
But working in a systems level language, things are crazy fast.
I didn't know the magnitude of the difference, and I still don't have numbers, but it feels so much faster.

Rust is safe
------------

I wanted to learn rust because it promised that I could write code that was really fast without having to learn about the causes of segfaults and pointer problems.
In rust, if my code compiles then I know it won't crash while it's running.
There are a few cases where runtime errors occur, but if you work for it, you can write rust with nothing unsafe.

Rust is comfortable
-------------------

Writing in type safe languages gives a sense of comfort about using code you didn't write, or even code you wrote long ago, that you cannot get without types.
And without exceptions or nulls, you can be pretty sure that your code is using library code correctly.

TDD works
---------

Rust has the fastest support for testing I've seen.
It had a XUnit like test framework built into the language and build tools before 1.0.
Rust is the first language I've seen to build a testing framework into the language itself.

What I don't like
=================

Compiling is hard
-----------------

In all compiled languages there is a phase of learning the language where one needs to learn how to appease the compiler.
Rust is no exception to that.
Rust is special in that is does not just refuse to compile on type errors, it refuses to compile on pointer and memory errors.
I'm not saying this should be removed (this is the core benefit of the language), it's just annoying and takes time to learn.
I wonder if there are better errors the rust compiler can give.

Copy vs Move
------------

Related to how hard compiling is, memory in rust has move semantics by default.
This means (as far as I understand) that if someone needs to use some memory directly and not a reference to it, they become the owner of that memory.
This became a problem for me when I was writing a simple map and needed actual structs and not references to them.
I kept getting the error "cannot move from borrowed content".

It turns out that rust also supports doing copy semantics, where instead of moving the memory, rust will copy the memory, so the original and the new usage both have a valid reference, without moving ownership.
This fixed my problem.
My issue is that the docs [here](http://doc.rust-lang.org/std/marker/trait.Copy.html#when-should-my-type-be-copy?) say that you should use the copy semantics when you can.
But this is buried in documentation and is in no way connected to the error I got.
I don't remember how I got to Copy from my error, but it was harder than it should be.

Which brings me to...

The docs suck
-------------

I don't want to bash people for writing documentation.
All correct documentation is better that none.
But, as a newcomer to rust the docs were basically useless.
The docs have the rust language deeply embedded in them and it's really hard to get any useful information out of them when you don't deeply know the language.
I still only get little use from the docs and I feel like I understand what each section is trying to communicate.

I'm not sure what I'd like to see from the rust docs.
I think at least two way connections would be really useful.
When I'm looking at the [Map](http://doc.rust-lang.org/std/iter/struct.Map.html) type, I'd find it useful to see all the functions in the library that return a Map.
Also, more usage examples.
The ones that exist are really nice, but they're not around for all the stdlib functions.

Tooling sucks
-------------

The IDE support for rust is not available yet.
I'm used to coming from Java, with its amazing ability to jump all around the code and show me before compiling what my problems will be.
I'm confident that one day rust will be there too, but for now I'm stuck with trying to set variables to the unit type to see the type error at compile time.

Conclusion
==========

Don't take this post to mean that rust sucks.
Most of my complaints are pretty easily fixed, I may even fix some myself.
It's a really great language, and fixing the difficulties to new developers will allow it to overtake all other languages and dominate the world.
Okay, maybe that's a little too high of a target.

I enjoy programming in rust.
So expect more posts about it.
