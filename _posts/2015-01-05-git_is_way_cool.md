---
layout:     post
author:     Shelby Doolittle
title:      "Git is WAY Cooler than You Thought"
subtitle:   "Some things git has that you probably don't know about"
date:       2015-01-05 00:00
header-img: "img/hill.jpg"
---

For now at least, [git](http://git-scm.com/) has won the battle of the source control management systems.
It is a spectacular version control system that contributes to the success of many teams.
Here are some cool things git does that have proved useful to me.

Git Rebase
----------

This is pretty common knowledge, but you can use git without any merge commits!
Simply append a `--rebase` (`-r`) to `git pull` and if there are changes from your remote, your commits will be applied directly after them.
No merge commit required.

You can also do something like a merge with `git rebase <target> <source>`.
This command will take all the commits from `source` that `target` doesn't have and magic them on top, as if they were always there.
If `source` is a branch, then rebase will also point source to the result of this magic.

More reading:

* [Official docs](http://git-scm.com/docs/git-rebase)
* [A more complete workflow](http://randyfay.com/content/rebase-workflow-git)
* [Merge vs Rebase](https://www.atlassian.com/git/tutorials/merging-vs-rebasing/conceptual-overview)
    
Git Bisect
----------

So you have a bug?
But this behaviour used to work...
If you have a close to linear git history, you can use bisect to find the commit that introduced the issue in logarithmic time.

Start with `HEAD` at the commit that has the bug.
Run `git bisect start` to start the process and `git bisect bad` to mark that that commit is bad.
Now checkout a commit that you know works and run `git bisect good`.

Git will checkout a commit for you and ask if the commit is bad or good.
Run your app and figure out if the bug is still there.
If it is `git bisect bad`; if it's not `git bisect good`.
Git will repeat this until it has found the first commit that is bad.

Bisect has many more uses as you can define good and bad as whatever you want.
Also if you have a script that exits with zero if the commit is good, and a non-zero if the commit is bad, then git will automate the search for you.
(PS: if any git people read this, can we implement a doubling strategy to find the first good commit when a script is given?)

More reading:

* [Official docs](http://git-scm.com/docs/git-bisect)

Git Patch
---------

Many people use `git add .` or an IDE to stage their changes for a commit.
You can selectively stage certain changes by using `git add -p`.
This command will open session that will ask if you want to stage the current highlighted change.
It can also split changes, and let you edit what you stage without affecting the source file.
This is a shortcut to skipping an initial dialog of interactive adding.

Staging with `-p` lets you write smaller commits that only change one thing, instead of just holding a bunch of unrelated changes.

BONUS: You can also commit with `-p` and it will bring up the same interface to select what to commit, but with one less command.

More reading:

* [Official docs](http://git-scm.com/docs/git-add)
  
Git Aliases
-----------

We all love unix aliases, but you can make alias commands inside git also.
Running `git config alias.YOUR_ALIAS 'GIT_COMMAND'` will make `git YOUR_ALIAS` essentially run `git GIT_COMMAND`.

Some aliases I use:

- `ci` => `commit`
- `lol` => `log --graph --oneline --decorate`
- `lola` => `log --graph --oneline --decorate --all`
- `co` => `checkout`

More reading:

* [Official docs](http://git-scm.com/book/en/v2/Git-Basics-Git-Aliases)
  
Git Reflog
----------

If you made some major change and don't want to figure out how to reverse it, you can just check out a previous state of your HEAD by referencing the reflog.

`git reflog` will give you some hard to understand output.
If you want to checkout the state before a certain action, checkout the HEAD@{X} from the number before the action you want.
For example:

```bash
# Oh noes, we deleted our branch
173b1732 HEAD@{0} delete: deleted branch foo
71738290 HEAD@{1} do some thing
```

We would want to run `git checkout HEAD@{1}`.

More reading:

* [Official docs](http://git-scm.com/docs/git-reflog)

Conclusion
----------

As we can see, git is super awesome.
I had to stop writing here because I had at least five more things to talk about.
Maybe git will get some more love from later.
Let me know if you want more on how I use git.
