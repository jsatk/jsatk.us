---
draft: false
date: 2017-01-24T00:00:00-00:00
title: "How I Code: Part I - Alfred + Dash"
---

This is the first post in a series I plan on doing. Practical tips and lessons learned that I apply to my work or use daily.

As a developer, one of my favorite things is when I am pairing with someone and all of a sudden they do something magical. They convert JSON to CSV with a keystroke. They run only the staged files for git commit through the linter. They run the test suite from inside their editor. Whatever example you want to think up - insert here. And it blows my mind. I burst out,"How did you do that? That's so useful! I'd use that all the time!"

I live for these moments. It's partly why I love reading through people's dotfiles and sharing my [dotfiles](https://github.com/jsatk/dotfiles) with others. Today I wanted to cover one such go-to timesaver/hack/whatever-you-wanna-call it.

## Alfred + Dash

One of the most beneficial things I've taught myself is how to go to the documentation first instead of [Stackoverflow](https://stackoverflow.com). When I started out as a developer, anytime I ran into trouble or needed to look something up I'd Google it, which would almost always return a Stackoverflow post as the top result. This is incredibly convenient for finding a quick answer, but doesn't teach you how to read and navigate documentation. This might sound silly, but reading `man` files or API & library docs takes some getting used to. It's a skill that you have to develop. Especially since API docs are so incredibly varied and unfortunately do not follow any sort of standard.

There is a wonderful app called [Dash](https://kapeli.com/dash) for macOS and iOS that will archive any library, language, or API for offline reading. And best of all it's free (although you can purchase an upgrade license for $25). This is not only helpful for when you're actually offline (on an airplane per say), but also for raw speed. I am a very easily distractable creature. I find it hard to focus on something if I have to wait during the process. I find my brain thinks,"While this loads let me check this real quick…". Twenty-minutes later I find myself forgetting what I was working on and doing something else. Offline caching of API docs helps me to avoid this simply due to raw speed.

Now that we know what Dash is this brings us to [Alfred](https://www.alfredapp.com/). Alfred is an amazing Spotlight replacement for macOS and it's free. There are several out there (Quicksilver, etc…), but I've found Alfred to be the one that has seen the most active development and love from its contributors. I could spend an entire blogpost detailing why I like Alfred, but many others [have already done that](https://birchtree.me/blog/alfred-3-review/) so I won't do that here. But I do have to tell you about how it integrates with Dash. To get Alfred to integrate with other apps you must purchase the [Powerpack](https://www.alfredapp.com/powerpack/). It's only £19 and well worth it. (Support indie devs, people.)

![The Preferences screen for Dash integrations](/images/dash.png) There is a Dash "workflow" available for Alfred that pulls in the libraries you have in Dash. To add it to Alfred open up Dash's preferences pane (⌘+,). Then click on "Integration". And finally click on "Alfred".

Then you can quickly query Alfred for what you are looking for and it will instantly pull it up and auto-suggest answers in Dash. And since Alfred can be invoked with a simple keyboard shortcut I find myself using this about 10+ times a day. Especially when I'm working with langauges or libraries I'm not deeply familiar with.

This is best understood through an example. For example, let's say I can't quite remember the name of javascripts new-ish `includes` method. I know it's probably either called `includes` or `contains`. I can type `⌘,javas⇥array.cont` and see that no `contains` method exists, backspace a bit and type `inc` and see that `Array.prototype.includes` exists. I press enter and Dash instantly takes me to the documentation for `Array.prototype.includes`. No Googling necessary. No 4 year Stackoverflow posts with (potentially) out-of-date or incorrect information. Hell, no internet connection is even required. And best of all I'm reading the official documentation on the `includes method and not someone's random opinion which is _huge_.

I can't sing the praises of this seemingly tiny integration enough. It stops me from having to _unnecessarily_ dig through Google or Stackoverflow results and gets me in the habit of reading the offcial documentation for language features. The keyword there is _unnecessarily_. I adore Stackoverflow and the various other blogs and wonderful sources of information the developer community provides. But they should be resources for stuff that's not literally in the documentation. It should be used for solving complicated questions and fringe use cases.
