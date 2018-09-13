---
layout: post
title:  "One thing I learned today: ls syntax"
---
_Note: I’m using the BSD `ls` on a mac._

This is slightly embarrassing to admit, however I seem to have misunderstood how `ls` works for years.

There are three main use-cases that I find myself using ls:

1. `ls, pwd, ls, cd blah, ls, pwd, and so on` For when I'm fumbling around a filesystem on a CLI, ususally because I'm paranoid I am in the wrong directory.
2. `ls -al`: the classic, "give me everything in a directory", command.
3. `ls some*wildcard*mess`: I'm looking for a file or folder in the directory, but I’ve forgotten the name of it. Return some potential suggestions to me.

Turns out, use-case 3 is wrong. Onto my story from today.

In my directory, I knew there was a subdirectory called `api-test` or something similar. I couldn’t remember the exact name, so I figured that `ls *api*` would spit back everything with an “api” in the name.

It didn’t.

It listed the contents of the `api-test` directory, but nowhere did it ever print the string “api-test”. This was bending my mind, so I stopped to blog and Google.

After some figuring out, I found out that is the intended working of `ls` and I had misunderstood this for years. The shell expands `*api*` to a single directory name, and `ls <some directory>` lists the contents of a directory.

Quite nicely, if there are multiple subdirs and files that match the pattern, the files are listed, and then the subdirectories are listed (by name) and then their files. `ls` prints a nice tree of subdirectories. However, if the pattern matches a single subdirectory, then the `ls` output doesn’t print the subdirectory as a little heading!

**TL;DR** In my head, `ls *api*` worked like `ls -al | grep -e api`.

Every day is a school day.
