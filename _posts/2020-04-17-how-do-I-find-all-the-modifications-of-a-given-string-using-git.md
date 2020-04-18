---
layout: post
title: "How do I find all the modifications of a given string using git"
date: 2020-04-16 18:40:54 +0000
---

A few days ago I was reviewing some code. While looking at one class, one of the global variables caught my attention. I wanted to understand why it was to that class and who wrote it.

The first thing I did was to run `git blame` command. It gave me the context about the last modification and the developer who made it. Yet, sometimes seeing just the most recent change is not enough.

Luckily, there is a way to see the entire lifecycle of a variable changing through git history. You just need to dig deeper.

## Behold the git pickaxe ⛏🪓 🦖🦴

> Like an archaeologist, looking for an ancient civilization, we can use the (git) pickaxe!

I didn’t know about this until today, but looking at git’s documentation, I found this gem of a command, that lets you find all the commits contaning a modification of a given string:

`git log -S isPizzaDelivered --oneline`

Let’s say you’re looking for a flag `isPizzaDelivered`, you need to pass it as a `-S`[^1] argument.

You might be familiar with `git log` which will return list of all the commits, but this uses the `-S` flag which does all the heavy lifting. That’s your pickaxe that lets you dig through the commits.

Since git version [1.7.4](https://github.com/git/git/blob/master/Documentation/RelNotes/1.7.4.txt#L76) you can also use the flag `-G`[^2] which lets you regular expressions.

As a result you will receive a list of commits which include changes to the string that you’ve specified and that’s really useful to learn more about the context and lifecycle in which the variable was being used.

> It’s always valuable to invest your time in the tools you use on a daily basis. 

As my friend [@zmarkan](https://twitter.com/zmarkan) says, your [tools do sometimes indeed give you superpowers](https://skillsmatter.com/skillscasts/10698-one-to-10x-tools-that-give-you-superpowers).

How do you search through your git history? Do you have any tricks to share? 
[Let me know](https://twitter.com/lukabratos)!

[^1]: [-S\<string\>](https://www.git-scm.com/docs/git-log#Documentation/git-log.txt--Sltstringgt)
[^2]: [-G\<regex\>](https://www.git-scm.com/docs/git-log#Documentation/git-log.txt--Gltregexgt)
