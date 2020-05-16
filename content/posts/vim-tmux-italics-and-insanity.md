---
title: "A Descent Into Madness with Vim, Tmux, & Italics"
date: 2016-10-18T22:00:00-00:00
draft: true
---

It all started out innocently enough. [Eric Elliott](https://twitter.com/_ericelliott) retweeted this [tweet](https://twitter.com/mxstbr/status/778245230709571585) by [Max Stoiber](https://twitter.com/mxstbr).  The tweet was meant to impart some great React knowledge upon me, however I couldn't help, but be completely distracted by screen.  What was that typeface?  It's absolutely beautiful.  And his comments are rendered in italics, but this monospaced italics is different, it has... cursive characters?!  I must have this!

I quickly [tweeted](https://twitter.com/jsatk/status/782308142608637952) at him asking what font that was.  He informed me it was [Operator Mono](http://www.typography.com/blog/introducing-operator).  The following is an account of my mental process for the next four hours.

---

A quick Google search tells me that Operator Mono was a new typeface from Hoefler & Co.  (As an aside, it still feels weird to write "Co" and not "Frere-jones".)  It is both beautiful and expensive. Two-hundred United States of America dollars expensive.  95% of the software I use on a daily basis is free.  But hey, fonts are hard and I've never bought one before so... lets do this.

Damnit!  This font won't work with my current vim & tmux setup.  Both my vim & tmux themes make heavy use of [Powerline](https://github.com/powerline/powerline), which requires some unique glyph characters to be in the font.  All the top free monospaced fonts have [already been patched](https://github.com/powerline/fonts) to include these glyphs.  I can't go about my day with all these ugly � characters in my terminal instead of the normal arrow glyphs.  This will not do.

Not to worry though!  It's easy to patch fonts that aren't already patched!  Or so the internet says.  Powerline provides a [fontpatcher](https://github.com/powerline/fontpatcher) tool.  Awesome!  I'll just download this and run it against my Operator Mono and all will be well in the world.

Oh, look at that.  The fontpatcher tool doesn't work with Operator Mono.  Great.  Oh well, I'm sure *someone* has figured this out already.  Back to Google...

So apparently, according to [these instructions](https://apw-bash-settings.readthedocs.io/en/latest/fontpatching.html), I need to install this other thing called [Fontforge](https://fontforge.github.io/en-US/).  I've heard the name before, but I have idea what it does.  But I can install it with [homebrew](http://brew.sh/index.html) which means I can uninstall it after.  So this is [fine](http://i.imgur.com/c4jt321.png).  Lets do this.


Shit.  So after installing fontforge and following these new instructions It's still throwing and error when I try to patch Operator Mono.  I wonder if anyone's tried to patch this specific font?  Let me see... Oh [look](https://github.com/powerline/fonts/issues/154), apparently other people have tried.  Looks like someone has an [unmerged PR](https://github.com/powerline/fontpatcher/pull/13) out for this exact fix.  Sweet.  I'll just update my local fontpatcher script with this PR's diff and everything will work and I'll have Operator Mono with Powerline glyphs and the hole in my heart will be full.

And... it's still not working.  Shit... And here we go.  Another [lead](http://fareesh.com/operator-mono-powerline/) on patching this.  Let me try this...

Oh my god!  It worked!  🎉  Yes!  Okay let me just patch all of these different weights and we're good to go.

Awesome.  Now that that's done... wait why was I doing this again?  What was I doing?  Oh yeah, I wanted to use this new font and have my comments in italics like in that screenshot that dude on Twitter posted.  Sweet.  Now to enable italics...

Okay so I'll just tell my vimrc to highlight all `Comments` with italics by throwing ` highlight Comment cterm=italic` in there.  Wait... that didn't do what I wanted it to do.  It gave all comments light gray background and it's not italicized.  How do I tell if my terminal supports italics?  I really hope it does.  The internet says if I run `echo -e "\e[3mfoo\e[23m"]]"` in my terminal I should see "foo" printed in italics.  Whelp.  It's definitely not in italics.  Maybe if I exit tmux and try it in just iTerm.  Oh... Oh wow.  It works.  So the problem is tmux.  It's gotta be.

Lets regroup here.  So far I've got the new font I wanted and I've patched it to include Powerline glyphs so my themes can remain looking gorgeous.  And I've concluded that vanilla iTerm renders italics just fine but tmux doesn't.  Okay, get tmux to render italics.  This should be easy.

Wow... I sure have spent a lot of time on this.  What?  Two-hours have already passed?  This is ridiculous.  All I wanted was a new font and italicized comments.  Oh well, I'm so close I can't stop now.

Okay so apparently the `$TERM` I'm using in tmux, screen-256color, doesn't have support for italics, but the `$TERM` used by iTerm when not in tmux is `xterm-256color` which does support it.  Okay, so I'll just set tmux to use `xterm-256color` for the terminal.  Oh, apparently I'm not supposed to do that.  That's a tmux no-no or something.

Hmmm... Looks like the best option is to create my own terminal and just add the support for italics.  Okay I'll `infocmp | pbcopy` and then add support for italics by adding `ritm=\E[23m, rmso=\E[27m, sitm=\E[3m, smso=\E[7m, Ms@,]]]]` in there.

Still not working.  This isn't working.  It's been three hours now.  This is stupid.  Why do I always have to fiddle with my settings?  Why do I care about my development environment being pretty?  Well, I do spend 6-8 hours a day staring at this so that makes sense.  Yeah, this makes sense.  I *should* care what it looks like.  This is no time to give up, Jesse.

(30 minutes of more Googling and trying shit and failing pass...)

Oh [look](https://www.reddit.com/r/vim/comments/24g8r8/italics_in_terminal_vim_and_tmux/), someone on reddit tried to do this exact same thing and succeeded.  I'll follow these instructions.

Apparently you have to modify your .vimrc to support italics because... because computers are terrible.  That's why.

```vimscript
set t_ZH=^[[3m
set t_ZR=^[[23m
```

And I also need to invoke tmux with an environment variable setting `$TERM` to the terminal I want.  Why can't I just set it in my `.tmux.conf`?  Apparently that didn't work for this guy either.  Okay, I'll just create a Fish function called `tmux` that really passes the environment variable to tmux but allows tmux to still run with `scree-256color` as it's terminal.

```fish
function tmux --description Run\ tmux\ with\ support\ for\ italics
        env TERM=myterm-it tmux -2 $argv
end
```

HOLY SHIT!  IT'S WORKING! IT'S REALLY WORKING!  CELEBRATE!!!

---

That was an incredibly stupid use of my time, but holy hell was it satisfying to solve.  And throughout the process I learned a ton about unix, how patching fonts works, and what `infocmp` and `tic` are.   And hey, having this new pretty font and comments in italics makes me happy, so maybe it wasn't a stupid use of time after all?

Well, that all depends on who you are and what you care about.  I view my development environment like I view my apartment.  It's my home.  It's where I live and spend a lot of time.  It should be a place that makes me happy and feels tailored to me.  Not only that, but I should know and understand all of it (or as much of it as I feasibly can).  Not having a solid graps on all the development tools you use or the multiple layers to your development stack is a bit like not knowing what's in the closets and drawers in your house.  So for me it wasn't a stupid use of my time at all.  I ended up getting what I wanted and learned a ridiculous amount about Unix tools in the process.

With that said, I think it's very important to keep context in mind.  My pal and coworker [Pete Hodgson](https://twitter.com/ph1) likes to use the term "wearing different hats".  This has stuck with me and I love it.  Do not, under any circumstances, do the kind of stuff I just wrote about while you're on the clock trying to hit a deadline.  While doing this I was wearing the "homemaker hat".  I was beautifying my environment because it brings me joy, not because it is a core part of being productive.  Always be concious of what "hat" you're currently wearing.  Focus on the right thing.

[MPJ](https://twitter.com/mpjme) has a wonderful [video](https://youtu.be/dIjKJjzRX_E) about meta-work and false productivity and it is well worth your time.  Again, make sure that what you are focusing on is indeed the correct thing to focus on.  For me this was a perfectly appropriate thing to focus on on a Sunday afternoon.  And it brings me joy every time I open a file and see the comments in lovely cursive monospaced font.

After all that this is what it looks like: ![screenshot](https://d1zjcuqflbd5k.cloudfront.net/files/acc_222213/v7Zo?response-content-disposition=inline;%20filename=Screen%20Shot%202016-10-02%20at%208.51.12%20PM.png&Expires=1475467005&Signature=ZCVDFogbdsfhgETu8tu9MiYkNSWO1~gkXhOpRljWKb7fL49IrGsSJcYm9N~hYPZNbh3qPJkb263Y~dsbLOtsvHFt6yexuP9MMUXlRs3yjVfsQaJUuLSC7Y~rpv85ogRtlFb8H5LBsqK-8isjpoxBpey0QgC6pXJVzgcsV-pEVSQ_&Key-Pair-Id=APKAJTEIOJM3LSMN33SA)

I'm gonna go lie down now.
