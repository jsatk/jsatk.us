+++
draft = false
date = 2014-01-01T00:00:00-00:00
title = "On Text Editors"
description = ""
slug = ""
tags = []
categories = []
externalLink = ""
series = []
+++

**Note: I've since lost when I originally wrote this so the publication date is an estimation by me from when I ported all my writings over to this website in 2020.  Also this was originally broken up into six parts and published in six separate entries.  I've since decided to combine them all into one here.  Lastly, as of this writing, May 16, 2020, I probably disagree with some of this now and would tell you to just use Vim or Code.**

## Introduction

I've been a web developer for roughly nine years now. Four years as a student and five years as my career. In that time I've tried nearly every popular text editor and IDE out there. This is a detailed history of my journey and how I ended up using TextMate 2 as my text editor of choice. This article will not tell you to use TextMate 2. It will tell you my personal experience and journey. I ended up deciding that TextMate 2 is currently (as of December 15, 2014) the best text editor for me. You may end up choosing differently. I hope that this article helps expose you to some IDEs or text editors you haven't used (or at least haven't used in a while) and makes you curious about what's out there.

## The Dreamweaver Phase

When I first started out in 2006, the choice was simple — if you wanted to make websites you used an [integrated development environment][ide] (IDE for short) and the IDE you chose was [Dreamweaver][dw]. Whatever your opinion of Dreamweaver may be today, in 2006, it was easily the best. Dreamweaver made starting projects incredibly easy and it provided you with a lot of nice boilerplate templates. To a new web developer, this was a dream come true. The other tools available to web developers (specifically front-end devs) were barbaric.

So Dreamweaver it was.

## The Coda Phase

That is until [Coda][coda] came along. In late 2009, I graduated from college with a computer science degree and in early 2010, I got my first professional job as a web developer. My peers and the internet seemed to agree that Dreamweaver was only for beginners (hint: it's not) and that I should move to a "better" code editor. The two options I heard over and over were Coda and [TextMate][textmate]. I downloaded the demos for both. Coda was gorgeous, elegant, incredibly well designed, and had everything I needed built right into it. TextMate was... a text editor. I remember staring blankly at TextMate wondering why anyone in their right mind would choose to use TextMate over Coda. I just didn't get it.

Coda suited me very well for that job. I was one of two web developers at the company. We had no version control. We updated the website via FTP. The server-side language was PHP, which Coda was built to work with very well. All was going swimmingly. Why would I ever leave? I still maintain that if you're doing websites as a freelancer or at a small agency for clients and your back-end is PHP, Coda is the best tool for the job.

## The Eclipse and Aptana Phase

After a year at a crappy job with terrible pay cutting my teeth in web development, I got wise and applied at [MRM][mrm]. MRM is the digital agency for [McCann-Erickson][mccan]. I had a friend-of-a-friend who worked there and applied. I couldn't even fathom working at a place that prestigious. I got the gig and quickly learned how big, powerful ad agencies worked (mostly terribly when it comes to web dev). It quickly became clear that Coda was completely ill-suited for the job. MRM was the digital agency of record for General Motors. Their site's server-side language was all Java and our templating was all in JavaServer Pages (JSP). So naturally, I had to move to [Eclipse][eclipse]. There was just simply no way around it. Everyone had to use it. I flipped between Eclipse and [Aptana][aptana] (a more front-end-focused fork of Eclipse) for those five months I worked there.

Five months may sound like a short time, but it was the richest learning experience I've ever had. Those five months working there are very vivid in my memory because I learned a ton. Namely, I learned that I hated having to work in Eclipse and couldn't wait to get back to Coda.

I got a call from a recruiter for a rival company four months in. I wasn't looking to leave, but I knew I was underpaid. I didn't mind too much because I enjoyed my job and I was learning. Also, it was the first job that made me feel like a professional. Still, my salary wouldn't exactly cut it for too long. So I took the interview, got the job, and asked for 80% more money than I was making at MRM — a salary I thought was surely absurd. No way would they say "yes" to that. They said "yes" without blinking. So off to [Team Detroit][tdi] I went.

Team Detroit (TDI) was Ford's digital agency of record (now you can see why MRM and TDI were rivals). Turns out, Ford's website had a very similar set up as GM's, and I was still stuck in Eclipse.

## The VIM Phase

I stayed at [Team Detroit][tdi] as long as I could. The problem was Ford thought we were all too expensive (remember that salary I got?). They decided to outsource all of their web development to Buenos Aires, Argentina. And this is when I learned a super-valuable lesson — make your boss like you.

My boss and I got along great. He pulled me aside one day and told me that I was going to be laid off soon. We all were. He didn't know when exactly, but I probably had three months at best. One of my favorite project managers from MRM was working at [ePrize][eprize] (now known as Hello World). I didn't really want to leave Team Detroit, but I had friends at ePrize and I knew I was getting laid off soon and didn't feel like job hunting, so I applied and got the gig.

One of the things that made me incredibly happy about the new job was this was the first job where I got a MacBook Pro. Previously, I was stuck on Windows machines: XP at MRM and 7 at TDI. Naturally, I was excited to get back to using my code editor of  choice, Coda, rather than being forced to use Eclipse. In my absence of being able to code on a Mac for my day job, TextMate 2 alpha had been released into the wild. However, the new kid on the block that everyone was telling me to use was [Sublime Text 2][sublimetext].

Excited, I began this new gig using Sublime Text 2. I read every article on all the tips and tricks I could find. The problem came when I realized ePrize had an incredibly asinine way of managing their codebase. Their codebase was remote and only remote. There was no pushing and pulling from your computer to a remote server. You had to connect to the remote server and work directly on it. Yes, I'm dead serious. Yes, I asked a lot of "Why is this this way?", but received no satisfactory answers. It was what it was. Most of my co-workers used a system of mounting the server as a drive and editing the files using Coda or Sublime Text 2. The problem was anytime two of us had to work on the same branch, we'd end up overwriting each other's work. You see, when you mount a drive and open a file on it your computer downloads and caches that file. When you save, it uploads the change. Next time you open the file it doesn't re-download it. It simply opens up the local copy it has. This is a problem if two people edit the same file. They will never realize that the other person has modified it.

The only sure way to avoid this is to actually work on the server. The cleanest, easiest, and most hassle-free way to accomplish this was with [VIM][vim]. I'd never used VIM except to write commit messages. I knew very little. Since I refused to follow this asinine and reckless workflow, my co-workers and boss insisted on using, I had to double-down on learning VIM. And boy did I ever. Again, I read everything I could about VIM. I learned that tool inside and out, backwards and forwards. I became an evangelist for VIM. Singing its praises and looking down my nose at other, lesser developers who had to have fancy things like a UI in their text editor. Man, did it ever feel good to be superior!

And I was superior in one way:I was the only developer who never accidentally overwrote his co-workers code. This sort of thing was rare, but it still happened every few weeks. Someone would inevitably overwrite another developer’s code. They never did learn (I think you can see why I didn't stay there long).

However, I was not superior in many other ways. In that VIM is not actually better than Coda or Sublime Text or TextMate or any other editor. It's just extremely different. After a year of working at ePrize, I quit. I hated the job and decided to leave even without another job lined up.

## The Sublime Text Phase

After a month of interviewing and job hunting, I ended up at TuneIn. This meant moving from Detroit to the Bay area. I started off in [VIM][vim], but was eventually convinced by a friend to try [Sublime Text][sublimetext] again. The beta for 3 had just come out and was pretty stable. So, I checked it out. I was surprised by how much I liked it. After a day of using it, I didn't see any reason not to keep using it. So I did. Once again, I poured myself into the app learning everything I could about it. In fact, I even wrote a starter guide for it that I would give to friends who code or new employees who were unfamiliar with Sublime Text that has been unfortunately lost to time.

Sublime Text (particularly Sublime Text 3 beta) felt like the culmination of years-long frustration at the lack of updates to TextMate and the lack of sensible choices for devs who didn't want to use a fully-fledged IDE. I'd used Sublime Text on and off, but I was never able to use it full-time, day-in-day-out due to how my previous job was.

Sublime Text is, as of this writing, still probably the most versatile and potentially best text editor out there. It's incredibly customizable. It's cross-platform. It's lightning fast. And, it's... ugly.

It's that final point — an intangible one — that most devs seem not to  care about. You can customize the hell out of Sublime Text's look and feel with themes. No matter how much I fiddled with it, it felt like putting lipstick on a pig. A very abandoned pig, I might add.

Still, Sublime Text was the best tool for the job. It worked really, really well. It was just not very polished. Then, along came the OS X Yosemite beta.

## The TextMate Phase

Once the Yosemite beta came out in summer 2014, and I'd heard from trusted friends and co-workers it was actually pretty stable, I did something stupid and installed it on my work machine. I figured I'd be able to identify what parts of our app wouldn't work on Yosemite and potentially contribute to some RubyGems to get them ready for Yosemite.

Sublime Text ran awful for me on Yosemite. Mostly because one of the billion packages I had installed on it kept crashing it. In frustration one day I just switched to [TextMate][textmate]. Fully assuming I'd move back to Sublime Text once the packages for it got updated to run on Yosemite.

And then a funny thing happened. I never switched back. In the two or so years since the 2 alpha was released for free and open-sourced, TextMate got really fast, really good, and, dammit, it was pretty. This is an OS X-only app that was clearly made by someone with taste. Someone who is an OS X user, who knows what those of us who live and work on OS X want in an app, both from utility as well as design standpoints.

I enjoyed having an actual preferences pane. I enjoyed installing bundles with a GUI. Wait ... why didn't I start using this earlier? Oh yeah, because there were little things about it that drove me nuts. But turns out after a quick Google search, I found out how to enable and disable various bundles. The main thing Sublime Text still had that TextMate didn't have (at least not out of the box) was autosuggest when typing in a method name. However, I quickly learned that ⎋(esc) would autocomplete, and it did a pretty damn good job of guessing what I wanted it to autocomplete to.

Another huge benefit of TextMate was how damn good the multi-cursor support is. Sublime Text has it, but not like this. TextMate (as of this writing) feels like one of those apps that was made specifically with nerds like me in mind. It has all the things I care about in spades and the things it doesn't have I don't mind so much. I've been using TextMate 2, which has now gone into "beta" (it's stable, don't fear) for the past six months and haven't once felt the desire to switch back to Sublime Text.

textmate: http://macromates.com

## What about Atom and BB Edit?

One you'll notice that I've completely skipped over [Atom][atom] and [BB Edit][bb]. Atom, if you don't know, is essentially Github taking Sublime Text, making it work entirely in a browser, and putting a little bit of their own stamp on it. It's a great text editor and a novel approach, but it is embarassingly slow and even more embarassingly a shameless ripoff of Sublime Text. Github, like many, are frustrated by the state of text editors and how each owner of them seems to abandon them. At least the owner of TextMate had the good sense to open source it and release it into the wild. If you like the look and feel of Sublime Text I'd say give Atom a shot. However, last I tried it out (roughly two months ago) it was a slow and sluggish beast.

BB Edit is absolutely fantastic. It's an incredible app with a long history of development and support. It's really well done. I recommend it whole heartily. However it veers sharply into the IDE territory which is not what I'm looking for. It's just not for me.

atom: https://atom.io
bb: http://www.barebones.com/products/bbedit/

## Conclusion

TextMate is, in my opinion, the most mature text editor out there. The fact that it is open source and free just speaks to how absolutely insane this industry, as well as the state of the tools we use to build it. is. I can wholeheartedly recommend Coda, VIM, Sublime Text, or TextMate to anyone. I think they're all excellent choices, but for me (at least for now) the choice is clearly TextMate.

vim: http://www.vim.org
aptana: http://www.aptana.com
coda: https://panic.com/coda/
dw: http://www.adobe.com/products/dreamweaver.html
eclipse: https://eclipse.org
eprize: http://www.helloworld.com
ide: http://en.wikipedia.org/wiki/Integrated_development_environment
mccan: http://mccann.com
mrm: http://mrm-mccann.com/en/index.html
sublimetext: http://www.sublimetext.com
tdi: http://teamdetroit.com
textmate: http://macromates.com

