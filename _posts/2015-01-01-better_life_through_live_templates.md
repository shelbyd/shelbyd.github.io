---
layout:     post
author:     Shelby Doolittle
title:      "A Better Life through Live Templates"
subtitle:   "How to type less and make fewer mistakes at the same time."
date:       2015-01-01 19:46
# header-img: "img/post-bg-06.jpg"
---

The JetBrains family of IDEs have a spectacular feature that I have seen few people use: Live Templates.
Live Templates make the process of writing code much better.

Live Templates are essentially macros that work for any language.
They let you define often written boilerplate as a shortcut that you can fill by tabbing through.
You can find them by going to settings then searching for live templates.

Once at the live templates view, highlight the language the template is for and click the green plus button on the right, selecting "Live Template".
Abbreviation is the text one writes in the actual editor to trigger the template.
I keep the abbreviations short and memorable since they are only used at write time.
The description will show up when the live template appears in the autocomplete menu.
I find with a good abbreviation I don't really use this field.
The template is where the meat is; we'll get to that in a moment.

<small>One gotcha: don't forget to assign an applicable context.
I often write a template, wonder why it doesn't work, then remember applicable contexts...</small>

On to the actual template!
Let look at one that I love and use every day.

I love AngularJS.
While the support for testing is better than any other JS framework I've tried, there is still a lot of boilerplate in the tests.
For example, here is the coffeescript I need to write to mock one dependency (if there is a better way to do this please let me know):

```coffee
dependency = null
beforeEach module ($provide) ->
  dependency = jasmine.createSpyObj('dependency', ['someFunction'])
  $provide.constant 'dependency', dependency
```

That's four lines of code, and ```dependency``` written five times.
It's annoying and error prone having to remember this structure every time.
This is not a deal breaker of course, but we don't want to type the same thing again and again.

Let's replace this with a live template:

```
$DEPENDENCY$ = null
beforeEach module ($provide) ->
  $DEPENDENCY$ = jasmine.createSpyObj('$DEPENDENCY$', [$FUNCTIONS$])
  $provide.constant '$DEPENDENCY$', $DEPENDENCY$
$END$
```

We give this the abbreviation ```mock``` and we're ready to roll.
Now when we type ```mock``` and hit tab, most of this code is written for us.
We start typing the name of the dependency and our typing is replicated across the five ```$DEPENDENCY$``` variables we have in our template.
Tab again and our cursor is moved to the ```$FUNCTIONS$``` variable location where we can write the functions our object has mocked.
One more tab and our cursor is out of the block.

One thing to note is that the ```$END$``` variable is special.
It lands our cursor there without the red box that makes our tab and enter keys behave strangely.
I always make sure to have an ```$END$``` in my templates so that I'm not tripped up by the tab and enter keys.

I find Live Templates to be a huge gain.
I make dozens of them when I use a new machine and always teach my pair about them if they don't know.

Hopefully your life will be better after making a few Live Templates.
