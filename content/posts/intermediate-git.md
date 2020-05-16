---
title: "Intermediate Git"
date: 2015-10-13T00:00:00-00:00
draft: false
---

## What this post is

* It is a tutorial of beginner to intermediate to maybe a light dusting of advanced git topics and practices.
* It is practical.

## What this post is not

* It is not philosophy or theory. (I will not be discussing the pros and cons of merge and rebase. Y'all can fight that battle elsewhere.)
* It is not _super_ complex or overly archaic.
* It is not preachy (See first point).
* It is not in any way exhaustive. This is a list of git commands, tips, tricks, and tools I personally find super useful and use nearly every day.

This is a series of things I often discover people struggle with, don’t know how to do, or have bizarre work arounds to get this information. You may know some or all of these already. But I figured I’d write this up and hopefully get a conversation going around git. I hope you find something useful in here and I hope it saves you time. NOTE: I personally use Fish Shell. I’ve conveted any Fish Shell specific syntax to Bash as 90% of you reading this probably use Bash.

## Shortcut for creating a new branch and switching to it

    $ git branch foo && git checkout foo
    $ git checkout -b foo # same as above command

## Change your editor for commit messages

Assuming you have [Sublime](https://www.sublimetext.com/docs/2/osx_command_line.html) or [TextMate](https://manual.macromates.com/en/using_textmate_from_terminal.html)'s command line tool installed.

    $ git config --global core.editor 'subl -w' # Sublime
    $ git config --global core.editor 'mate -w' # TextMate
    $ git config --global core.editor emacs     # Insanity

## Diff between remote and local

    $ git status
    On branch test_branch
    Your branch is ahead of 'origin/test_branch' by 1 commit.
    (use "git push" to publish your local commits)

    nothing to commit, working directory clean

    $ git diff # returns nothing because there is nothing different between our locals

    $ git diff git-talk # same exact thing as above. git-talk here refers to your local

    $ git diff jsatk/git-talk # diffs between remote and current local branch
    diff --git a/foo.txt b/foo.txt
    index 257cc56..5716ca5 100644
    --- a/foo.txt
    +++ b/foo.txt
    @@ -1 +1 @@
    -foo
    +bar

    $ git diff jsatk/git-talk git-talk # diffs between remote and specified local branch
    diff --git a/foo.txt b/foo.txt
    index 257cc56..5716ca5 100644
    --- a/foo.txt
    +++ b/foo.txt
    @@ -1 +1 @@
    -foo
    +bar

“But Jesse—this seems unnecessary. I can clearly see in git status that I have a commit that’s not pushed yet.” You’re correct. This isn’t the world’s greatest use-case example. However, I use this most often when I need to see what changes might be incoming or to diff between my branch and master. If I’ve been on a feature branch for a few days and want to see a diff between my feature branch and master doing `git diff master feature_branch` will only `diff` between my local copy of `master` and my `feature_branch`. What I do in these situations is this `git fetch origin && git diff origin/master feature_branch`. The `fetch` here is important. (Note: If you’re unclear on the difference between `fetch` and `pull` I highly suggest you take 15 minutes to [read up on it](http://stackoverflow.com/questions/292357/what-are-the-differences-between-git-pull-and-git-fetch).)

## How to check out a file from one branch into another

    $ git checkout -b baroness
    Switched to a new branch 'baroness'

    $ echo 'red album' > albums.txt

    $ cat albums.txt
    red album

    $ git add --all && git commit -m 'Added albums.txt'
    [baroness 728f16f] Added albums.txt
    1 file changed, 1 insertion(+)
    create mode 100644 albums.txt

    $ git checkout -b mastodon
    Switched to a new branch 'mastodon'

    $ echo 'once more round the sun' > albums.txt

    $ cat albums.txt
    once more round the sun

    $ git add --all&& git commit -m 'Added albums.txt'
    [mastodon 6fea3ec] Added albums.txt
    1 file changed, 1 insertion(+), 1 deletion(-)

    $ git show baroness:albums.txt # Prints the contents of albums.txt from branch baroness
    red album

    $ git checkout baroness -- albums.txt # Checks out baroness' version of albums.txt to branch mastodon
    # This is a perfect example of the insanity of git. You use : to show between branches but -- to checkout.
    $ cat albums.txt # now this branch's albums.txt is the same as baroness'
    red album

This comes up surprisingly a lot for me. I tend to use the show feature a lot more. A use case might be if I know a co-worker and I are working on the same file. He’s merged his branch into `master`, I’m on a `feature_branch`. I will then do a `git fetch && git show origin/master:file/coworker/and/i/are/working/on.js`. This then lets me easily see if his new work then affects my current work and if lets me know if I need to course correct.

## You can combine multiple shorthand flags into one

    $ git commit -n -m 'This is a commit message.'
    $ git commit --no-verify --mesage='This is a commit message.'
    $ git commit -nm 'This is a commit message.' # This is the same as the above two!

## You can combine paragraphs from the commandline with your git commit message!

    $ git commit -m Hello -m is -m it -m me -m you\'re -m looking -m for\?
    $ git log -1 # Using -Number limits the log to that number of commits.
    commit facbbb49d88182b50d8383323acae6696e33ff63
    Author: Jesse Atkinson <jesse.atkinson@me.com>
    Date:   Tue Apr 21 19:12:44 2015 -0700

        Hello

        is

        it

        me

        you're

        looking

        for?

Also, notice you don't have to use a string for your commit message if you escape? Not really useful, just fun to point out.

## Stashing with a message and stashing untracked files

At the beginning of this I said I wasn't going to preach. I lied. Always `stash` with a message. Just do it. No. No. No. Hush. Do it.

    $ git checkout -b pikachu
    Switched to a new branch 'pikachu'

    $ echo 'lightning bolt' > attacks.txt

    $ git status --short
    ?? attacks.txt

    $ git stash save --include-untracked 'Creating attacks file for Pikachu.' # Shorthand for --include-untracked is -u
    Saved working directory and index state On pikachu: Creating attacks file for Pikachu.
    HEAD is now at 33b6d46 Bug fix for ad scrapers. Have to specify placement when inserting iPad and iPhone into iOS product.

    $ git checkout master
    Switched to branch 'master'
    Your branch is up-to-date with 'origin/master'.

    $ git stash list
    stash@{0}: On pikachu: Creating attacks file for Pikachu.

    $ git stash show # In general this command isn't very useful. Just show's a diff of lines changed. But it's particularlly useless at this point. It doesn't show anything because attacks.txt isn't tracked by git. Git can only tell you information about things it has been told to track.

    $ git stash pop && git add --all && git commit -m 'Initial commit of attacks.txt for Pikachu.'

    $ echo 'shock' >> attacks.txt
    $ git diff
    diff --git a/attacks.txt b/attacks.txt
    index e8eb403..4642b97 100644
    --- a/attacks.txt
    +++ b/attacks.txt
    @@ -1 +1,2 @@
    lightning bolt
    +shock

    $ git stash save 'added shock to the list of attacks'
    Saved working directory and index state On pikachu: added shock to the list of attacks
    HEAD is now at 97921a2 Initial commit of attacks.txt for Pikachu.

    $ git stash show 'stash@{0}' -p # NOTE: the -p argument is short for --patch
    diff --git a/attacks.txt b/attacks.txt
    index e8eb403..4642b97 100644
    --- a/attacks.txt
    +++ b/attacks.txt
    @@ -1 +1,2 @@
    lightning bolt
    +shock

By default git stash does not stash untracked files. Throw a `-u` on the end. Also, `git stash list` shows you your stashes, however the default message it saves with it is the message of the latest commit and the branch you stashed it on. This is… super unhelpful. Make sure to add `save` to your `git stash` command and follow it with a message. You’re really smart. You’re also busy and you will totally forget what the hell is in your stashes if more than say 15 minutes pass. I promise you. Then when you come in the next day and run `git stash list` and see a list of stashes with commit messages that have nothing to do with the work in the stash you’ll want to scream.

## Getting your stashed stuff!

`git stash apply stash@{3}` will apply fourth (it’s zero-indexed) stash’s content to your current branch and the stash will remain in the stash list. However, `git stash pop stash@{3}` will remove the fourth stash from the list moving all stashes after it up one on the index. `pop` is probably what you most often want, but it’s also more dangerous for obvious reasons. If you would rather use `apply` you can saftely run `git stash drop stash@{3}` after you’re cool with getting rid of that stash.

## Don't shoot yourself in the foot with `git stash`

With git stash list the stashes reindex after you remove a stash. Let’s say you have three stashes. If you try to do something like this `git stash drop stash@{1} && git drop stash@{2}` this will fail because the stash at index 2 is now at index 1 after having dropped the stash at index 1. When removing multiple stashes remove from the highest numbered index on down or… just be really really careful.

## Clean up after yourself with `git rebase --interactive`

I like to commit a lot and often. At the end of a feature branch I have a ton of shitty commits. It's just the way I work. Those aren't going to be helpful to anyone once this is merged to master. And some day if they find a bug and have to do a `git blame` and see that Jesse commited this code six months ago with the commit message "WIP. Fixed shit." they are going to (rightfully) curse my name. Lets not be that person. Lets use interactive rebase.

    $ git rebase --interactive branch_you_want_to_rebase_against_typically_master # That's it! You can also use the shorthand -i flag.

This is a very, well, interactive mode. Git really holds your hand and gives you lots of nice messages. When you run that git will open your text editor and have a list of all of the commits on your branch that are not on the branch you're rebasing against.

    # Rebase c4758c3..c4758c3 onto c4758c3 (1 command(s))
    #
    # Commands:
    # p, pick = use commit
    # r, reword = use commit, but edit the commit message
    # e, edit = use commit, but stop for amending
    # s, squash = use commit, but meld into previous commit
    # f, fixup = like "squash", but discard this commit's log message
    # x, exec = run command (the rest of the line) using shell
    #
    # These lines can be re-ordered; they are executed from top to bottom.
    #
    # If you remove a line here THAT COMMIT WILL BE LOST.
    #
    # However, if you remove everything, the rebase will be aborted.
    #
    # Note that empty commits are commented out

See all that lovely help from git? There's not much more I can add here. You literally just follow along and git tells you what to do. I bring this up entirely because people seem to rarely use it.

## Interactively add your changes to the commit stage

    $ git add --patch

Note -- this only works with files that are already being tracked by git. It does not work with untracked files.
Split is super powerful and conveinent. If it gets *really* hairy you can use e to manually edit the hunk.

From the help:

>>>
Stage this hunk [y,n,q,a,d,/,e,?]?
y - stage this hunk
n - do not stage this hunk
q - quit; do not stage this hunk or any of the remaining ones
a - stage this hunk and all later hunks in the file
d - do not stage this hunk or any of the later hunks in the file
g - select a hunk to go to
/ - search for a hunk matching the given regex
j - leave this hunk undecided, see next undecided hunk
J - leave this hunk undecided, see next hunk
k - leave this hunk undecided, see previous undecided hunk
K - leave this hunk undecided, see previous hunk
s - split the current hunk into smaller hunks
e - manually edit the current hunk
? - print help
>>>

## When doing a comparison make sure you're comparing against the latest remote version with fetch

    $ git diff master # diffs current branch against your local copy of master
    $ git diff SocialCodeInc/master # will probably do the same thing as above in *most* scenarios.
    $ git fetch SocialCodeInc && git diff SocialCodeInc/master

## Squash your commit if the branch is truly a mess

If your branch's commit history is a complete mess but you know you want what is there currently another branch (most likely master) `--squash` is your friend.

    $ git commit -m 'ugh'
    [new_branch bacb008] ugh
    1 file changed, 10 insertions(+), 2 deletions(-)

    $ git checkout master
    Switched to branch 'master'

    $ git merge new_branch --squash
    Updating 051c56b..bacb008
    Fast-forward
    Squash commit -- not updating HEAD
    README.md           | 20 ++++++++++++++++++--
    awesometextfile.txt |  3 +++
    2 files changed, 21 insertions(+), 2 deletions(-)

    $ git branch --merged # To list local branches that have not been merged run --no-merged
    * master

    $ git branch -d new_branch
    error: The branch 'new_branch' is not fully merged.
    If you are sure you want to delete it, run 'git branch -D new_branch'.

    $ git branch -D new_branch
    Deleted branch new_branch (was bacb008).

## What if I want just a single commit from one branch into another branch?

    $ git checkout branch_you_want_to_move_commit_to && git cherry-pick 240982d # You don't need the full sha, just enough that git understands it.

Can be a tad dangerous. Specifically—git will assign a new sha to the cherry-picked commit. Which means git will now have record of two sha's with the same exact changes. Proceed with caution.

## Jesse, what if I want to figure what commit broke my favorite feature?!

[Bisect](http://git-scm.com/docs/git-bisect) is your friend! (Seriously the docs are super worth reading.)

    $ git bisect start
    $ git bisect bad # without passing a SHA it uses HEAD
    $ git bisect good SHAofLastGoodKnownCommit
    # At this point git takes over and tells you how many rivisions to test. Through a series of good and bad confirmations it will then tell you the bad commit.
    # Shoot off an email or slack message to the author with the commit sha or fix yourself if you're confident of the intended change in the commit or if you're the author
    $ git bisect reset # DO NOT FORGET TO DO THIS PLEASE. IF YOU FORGET YOU WILL BE VERY ANGRY LATER.

## More diffing!

    $ git diff ref1:path/to/file1 ref2:path/to/file2 #
    $ git diff origin/master -- [path/to/file] # Much simpler if you're diffing the same file at the same path.

## Reset can be gentle too, ya know

If there is a commit you just flat out don't want to have and want to fix it and it *has not* been pushed to the remote branch yet use `reset --soft`

    $ git reset --soft HEAD~1

## Finally

Git is a tool. It is a means to an end. Not the end. Mastering your tools is essential in becoming a better and more efficient programmer. However, use whatever you feel more comfortable with. If you are happy with your GUI git app of choice then use it. Occassionally though these apps cause problems and you gotta jump into the commandline and get your hands dirty.

## References

* [http://git-scm.com/book/en/v2](http://git-scm.com/book/en/v2)
* [https://training.github.com/](https://training.github.com/)
* [http://nicercode.github.io/git/setup.html](http://nicercode.github.io/git/setup.html)
* [http://gitref.org/](http://gitref.org/)
* [http://nuclearsquid.com/writings/git-add/](http://nuclearsquid.com/writings/git-add/)
