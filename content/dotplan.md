---
title: ".plan"
date: 2020-05-17T11:30:06-07:00
draft: false
---

# 2020 .plan

- - -

## Table of contents

* [January 2020](#january-2020)
    * [2020-01-02](#2020-01-02)
* [March 2020](#march-2020)
    * [2020-03-27](#2020-03-27)
    * [2020-03-28](#2020-03-28)
    * [2020-03-29](#2020-03-29)
* [May 2020](#may-2020)
    * [2020-05-04](#2020-05-04)
* [June 2020](#june-2020)
    * [2020-06-08](#2020-06-08)
    * [2020-06-12](#2020-06-12)

## January 2020

### 2020-01-16

Wow.  I hardly used this in 2019.  Lets see if 2020's different.

Been learning a ton of Scala.  Working my way through [The Red Book](https://www.manning.com/books/functional-programming-in-scala).  Got [Metals](https://scalameta.org/metals/) working nicely in Vim.  Maybe I'll finally make my own damn website this year.  Who knows.

## March 2020

### 2020-03-27

First update in ages.

My learning with Scala is going really well.  One of my biggest frustrations with the language so far has been the tooling.  Working in javascript has spoiled me.  Because of the languages extreme popularity the tooling is very good.  Metals is pretty damn solid, although still very beta.  But it has exposed me to Bloop.  Which is an alternative build tool for Scala.  Seems like it's trying to do to `sbt` what `yarn` did for `npm`.  It's pretty good.

Upgraded our SBT version to 1.3.8 which vastly improved the speed of things because SBT 1.3.\* uses Coursier under-the-hood.  I still barely know what it is though.  Need to learn.

I'm on chapter 5 of the Red Book.  I've unfortunately halted on progress with it and haven't made much progress.  I think my focus should be that moving forward.

I also figured out how to put the weather in my tmux status bar which seemed silly at first and now is unironically my main source of weather info. 😂

```tmuxtheme
WEATHER='#(curl -s wttr.in\?format\="%%c+%%t")'
```

### 2020-03-28

Attempted to figure out how to do some Project Euler problems in Scala.  Given that Scala is built on Java doing basic scripting with it is not intuative.  It took an embarassing amount of time for me to figure out the best way to be able to just _run_ a `.scala` file and see the output printed.  Turns out that latest version of Scala supports executing a `.scala` file without any flags.  So added the Scala binary to my set of `asdf` languages I install so I can run `scala 001/001.scala` and see it print out the answer.  As of right now I'm unaware of a way to execute a Scala file other than this.  Bloop requires a Bloop server to be running and a Bloop server will only run if you're in an actual Scala project with a build file.  SBT is the same story.  And I didn't feel like adding the massive overhead of a default SBT project to my Project Eueler repo.

Also did a fair amount of cleanup of my dotfiles.  Manually porting over stuff from my workbranch that's useful and non-work specific.

Spent some time watching an old Steve Losh stream where he's building a game in Common Lisp.  Some day I'll have the free time to dive into Common Lisp and really learn it.  Some day...

### 2020-03-29

A lot more cleanup of my dotfiles.  Specifically my `.vimrc` and `.tmux.conf`.  Decided having a separate theme file was not something I wanted to keep doing.  It was only 50 lines or so so I moved that right into my `.tmux.conf`.

I need to read up on Agile/Scrum best practices now that I'm the new Scrum Czar for my team.  I feel pretty confident in my skills in this regard, but most of my knowledge comes from actually doing Scrum/Agile for the past 10 years.  Not actually reading up what it is _supposed_ to be.  I should read a book or two so I don't blindly cargo cult over bad practices.

I never thought I'd be reading books & articles on Agile but here we are.

### 2020-05-04

Finally spent some time working on my website.  Settled on the Coder theme just to force myself to keep moving and not freeze on a decision.

Stealing the colorscheme from [Martin Agency](https://martinagency.com/about).

## June 2020

### 2020-06-08

Starting reading [Hands-on Scala Programming](https://www.handsonscala.com/) by Li Haoyi.

Dinked a little more on website.  Almost done and ready to launch.

### 2020-06-12

Late Wednesday, after announcing my new site ([https://jsatk.us](https://jsatk.us)) I `git clone`-ed the repo for it to my work machine to fix a typo. The theme is kept in a git submodule. I don't use `master` branch for my theme. Turns out, after fixing my typo and deploying and only double checking that the typo was published, I had deployed the site with the `master` branch stylings, not the ones I spent 15+ hours on. So the styling on my brand new website got b0rked, by me, 7 or so hours after launching and has been b0rked for something like 48 hours.

git submodules are hell. Never use them.
