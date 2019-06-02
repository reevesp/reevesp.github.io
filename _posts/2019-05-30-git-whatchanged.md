---
layout: post
title:  "Using `git whatchanged` to pick apart accidental merge commits"
---

Since I have found myself doing this quite often now, I thought it best I write some notes on it.

## Background.

I accidentally stage files in git, a lot.

I like to leave lots of untracked files as scratchpads in and around in my project directory, I have lots of "file" and "_file" and "_final_file2". I never check these files in. I know, I'm trying to quit this habit.


Whenever I have a merge conflict, after the remote commits are pulled down to my local repo then _all_ of my untracked files are staged. (I'm not sure if this is standard git practice, or just something that my git cli or vscode does).

This nearly always means that after I've manually merged a file I will commit it completely on auto-pilot, only to realise a split-second later that my last commit just added all of my junk into my local repo.

_So, my dilemma is:_
1. I need to undo my last commit.
2. I need to find the files that I did not stage automatically (bearing in mind I have forgotten all their names and the list of touched files is normally 50+ files)(i.e. I'm normally picking apart a commit like this-)

![](/images/2019-05-30-git-whatchanged-average-git-commit.png)

Here is what I have found to be my best solution.

## Actually find out which files got _added_ in my huge merge commit

The best way to do this (that I have found) seems to be `git whatchanged --diff-filter=A`. I'd never heard of `whatchanged`, but its man page makes it out to be ancient. Anyway, the `--diff-filter=A` argument gives a pretty easy-to-read list of added files per commit. 

Here is what it gives for this blog's repo, for example.

```
Peters-MacBook-Pro-5:reevesp.github.io peterreeves$ git whatchanged --diff-filter=A
commit e106c3a9d0a7b5c5936fc50726cabdfc69d9cb37
Author: reevesp <...>
Date:   Thu Mar 21 18:55:25 2019 +0000

    adding blog about installing node on win10

:000000 100644 0000000 cd8bed5 A        _posts/2019-03-21-using-node-js-on-win-10-with-bash.md

commit 6cf65ce59c9d62ab6b64dafb58c7f9523e010fbf
Author: Peter Reeves <...>
Date:   Sun Jan 6 12:35:01 2019 +0000

    Adding DB2 blog entry.

:000000 100644 0000000 ef80fe5 A        _posts/2019-01-03-db2-on-cloud-notes.md

commit 9df9a19ad2b41f1c7a0090c681107638f08af7e1
Author: Peter Reeves <...>
Date:   Sun Jan 6 12:10:13 2019 +0000

    Adding Christmas Puzzle Solutions

:000000 100644 0000000 d6c9cfc A        _posts/2018-12-27-christmas-clock-puzzle.md
```

This is useful info which we can use for the next step.

## Undoing my last commit (local).

To undo my last commit, I run `git reset --soft HEAD~1`. I'll use the argument (`--soft`) reset, because I don't actually want any of the files to be changed, I'm about to add the correct ones into a proper merge commit.
