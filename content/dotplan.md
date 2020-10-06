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
* [September 2020](#september-2020)
    * [09-06-2020](#09-06-2020)
    * [09-07-2020](#09-07-2020)
    * [09-09-2020](#09-09-2020)
    * [09-15-2020](#09-15-2020)
* [October 2020](#october-2020)
    * [10-05-2020](#10-05-2020)

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

Late Wednesday, after announcing my new site ([https://jsatk.us](https://jsatk.us)), I `git clone`-ed the repo for it to my work machine to fix a typo. The theme is kept in a git submodule. I don't use `master` branch for my theme. Turns out, after fixing my typo and deploying and only double checking that the typo was published, I had deployed the site with the `master` branch stylings, not the ones I spent 15+ hours on. So the styling on my brand new website got b0rked, by me, 7 or so hours after launching and has been b0rked for something like 48 hours.

git submodules are hell. Never use them.

## September 2020

### 09-06-2020

Started going through the Vlime tutor again.  I want to get back to learning Common Lisp and finish reading _Common Lisp: A gGentle Introduction to Symbolic Computation_.

I need to move my sbcl installation from brew to asdf.  I need to install Quicklisp as part of my Makefile.

### 09-07-2020

Need to take care of this error in Vlime.

> Note: Depending on how your Common Lisp implementation was built, you
>       may get the "No xref found" error message instead. Don't worry.
>       You can try other xrefs listed in |vlime-mappings-invoke-xref|,
>       or skip this operation and proceed to the next section.

Finished up the `vlime-tutor` and learned a ton.  Incredible tool set and language.  Lots to memorize regarding commands but the flexibility is amazing.  Here's hoping I stick to it.  I think I can do 30-60m a day.  Maybe new lunch-break hobby?

Updated `ep.fish` function to be a lot smarter with date/time stamps.  Learned a fair amount about `date` and Fish shell syntax in the process.

### 09-09-2020

Made significant updates to my `ep.fish` script.  A lot cleaner now.

### 09-15-2020

Reformatted my work computer.  That was hell.  JAMF and other remotely-manage systems just do not play with with macOS.  It so clearly does not want to be managed by anyone other than you/Apple.

My dotfiles repo simultaneously showed how powerful and valuable it is and also where the holes are.  Some issues that came up during the install process that I'd like to automate if possible:

* Installation order was an issue.  Look into this to see if we can do a few more if/else checks for what's installed.
* Importing keyring would be nice to automate.
* Is there a way to automate things like setting keyboard repeat speed? Autohide dock? Wallpaper?
* Is there a way to check and set Fish shell to default shell?
* Maybe it's time I add a sensible .zsh file now that it's macOS's default shell.  I really don't want to go down a rabbit hole with it, but just set the sensible defaults I'll want if I ever have to work in it.
* Maybe add some sort of prompt to run `tmuxinator doctor` as well as `nvim +checkhealth`.
* Automate installing certs that you want on every machine.
* Perhaps automate installing Fastmail profile?  This might be too damn hard.
* Auto-import fonts.
* Set up iTerm and auto-import its settings.
* Coc extensions.  Really annoying that you can't really manage these well.
* Install tmux plugins.  Automate this.
* Make mail directories automatically.  `~/.mail/` `~/.mail/jsatk/` & `~/.mail/temporary/`.
* Set up irc cloud.  This is probably not automatable.
* Make sure to run `notmuch new` on a new machine.
* Install weechat plugins such as `go` and `autosort`.

## Octorber 2020

### 10-05-2020

Finally sat down and attempted to unfuck YNAB today.  The system works if you diligently stick to it, but if you fall off at all it's a shitshow.  I fixed our budgets for the past three years starting at January 2018, which is when we last declared budget bankruptcy and started over.

Where our money went for the past three years is mildly inaccurate on there now (i.e. transactions might be labeled incorrectly), but how much we actually have and our current month's budget is spot on.  Budgeting is an anxiety inducing hell for me but it needs to be done and glad I did it.

I also spent time figuring out why I keep getting this warning when I run `make` in my dotfiles repo.

```
Makefile:107: warning: overriding commands for target `/Users/jsatk/.asdf/shims/neovim'
Makefile:98: warning: ignoring old commands for target `/Users/jsatk/.asdf/shims/neovim'
```

Neovim wants me to have a plugin called "neovim" from both `npm` and `gem`.  The issue is that using `asdf` to manage both causes the `target` in the `Makefile` to end up being literally identical for both.  `asdf` is smart enough to handle this naming collision and appends extra info to the end (i.e. `neovim-node-host` & `neovim-ruby-host`), but `make` understandably panics when it sees two targets named the same thing.  This took a bit to figure out why I was getting these warnings until I realized all my variables and string concatenation I do at the top of the `Makefile` resulted in the same `target` for both.


