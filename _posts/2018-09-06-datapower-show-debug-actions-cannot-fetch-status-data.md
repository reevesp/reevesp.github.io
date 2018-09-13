---
layout: post
title:  "DataPower show-debug-actions, Cannot fetch status data"
categories: foo bar baz
---
While recording some CLI snippets for use in my previous blog, I noticed myself "deskilling".
Now I couldn't even get the `show debug-actions` command to work!

```
idg[gwyscript-dev]# show debug actions

 Session ID    Transaction ID       Service Name       File Location                  Remote Address     In Use                 Remote User         User Location                Elapsed Time
 ----------    --------------       ------------       -------------                  --------------     ------                 -----------         -------------                ------------
Cannot fetch status data

idg[gwyscript-dev]#
```
The more eagle-eyed readers might have spotted the issue- I have missed out the hyphen.

However, I was very disappointed that the top result on google for "DataPower Cannot fetch status data" did not spoon-feed me the steps to resolve the issue, hence I'd better blog it!

###### And another thing-

I also found it quite interesting that:

- `show debug-actions`: works.
- `show debug actions`: doesn't work. But DOES print out the table formatting. How odd!
- `show debug`: works!

If you enter `show` on the DP command-line, you'll get a printout of everything you can query.

```
idg[gwyscript-dev]# show
Show what? Options:

    aaapolicy
    accepted-connections
    access-profile
# snip approx. 30 - 40 lines
    crypto-engine
    crypto-hw-disable
    crypto-mode
    debug-actions
    deployment-policy
    deployment-policy-variables
    dns
# snip approx. 30 - 40 bazillion lines
    xslcoproc
    xslproxy
    zos-nss
idg[gwyscript-dev]#
```
You'll notice `debug` is not a listed argument for `show` in itself, so I'm guessing `debug` is aliased under the cover to `debug-actions`. So when you incorrectly run `show debug actions`, DP's "shell" expands that to `show debug-actions actions`.

```
idg[gwyscript-dev]# show debug-actions actions

 Session ID    Transaction ID       Service Name       File Location                  Remote Address     In Use                 Remote User         User Location                Elapsed Time
 ----------    --------------       ------------       -------------                  --------------     ------                 -----------         -------------                ------------
Cannot fetch status data

idg[gwyscript-dev]#
```
Isn't this fun?
