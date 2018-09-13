---
layout: post
title:  "DataPower Gatewayscript: Unknown command or macro (debug-action)"
---
I was having a brainfart today and I could not get the Gatewayscript debugger to work on the DataPower CLI. However, after asking around some colleagues, it seems I wasn’t the only person to drop this particular clanger- so I had better write it up!

I started from this KC article:
https://www.ibm.com/support/knowledgecenter/en/SS9H2Y_7.7.0/com.ibm.dp.doc/debugger_startingsession.html

In that link, the DP team detail the two basic commands.
`show debug-actions`, which lists all the “debugger-able” transactions in a table, ordered by session-id.
`debug-action <session-id>` which opens the debugger on a particular transaction.

I was able to `show debug-actions` easily.

```
idg# switch gwyscript-dev
#
# BONUS GOTCHA - YOU NEED TO SWITCH INTO THE DOMAIN WITH YOU GATEWAYSCRIPT TO
# SEE THE DEBUG SESSIONS
#
idg[gwyscript-dev]# show debug-actions

 Session ID    Transaction ID       Service Name       File Location                  Remote Address     In Use                 Remote User         User Location                Elapsed Time
 ----------    --------------       ------------       -------------                  --------------     ------                 -----------         -------------                ------------
 63            6992                 gwtscript-debug-test-mpg local:///test-gwy.js           172.17.0.1         No                                                                      00:04:31
 67            6465                 gwtscript-debug-test-mpg local:///test-gwy.js           172.17.0.1         No                                                                      00:03:07
 90            8721                 gwtscript-debug-test-mpg local:///test-gwy.js           172.17.0.1         No                                                                      00:02:25

idg[gwyscript-dev]#
```

However when I enter the `debug-action 90` command, I received the response:
```
idg[gwyscript-dev]#
idg[gwyscript-dev]# debug-action 90
Unknown command or macro (debug-action 90)
idg[gwyscript-dev]#
```

My normal instinct when DP returns `Unknown command or macro` is that I’ve spelled the command wrong, so I double-checked, tried some similar spellings, removed the hyphen, etc. Still nothing.

My next idea was to check the permissions of my logged-in user, in case I did not have permission to run the `debug-action` command.

Since I was messing around, I was logged-in as `admin`, who is a privileged user. _(NB: please never use the DataPower admin user in any other scenario, unless you know what you're doing, or a PMR tells you to)._

I created a test user and user group with `r+w+a+d+x` permissions and still could not run the command.

Finally, I was tipped off by the syntax of the commands:
https://www.ibm.com/support/knowledgecenter/en/SS9H2Y_7.2.0/com.ibm.dp.doc/debug-action_global.html
```
debug-action
no debug-action
```
The “no” prefix is usually used to delete config objects. Perhaps if a debugger session is considered a config object then I need to be in configure terminal mode?

```
idg[gwyscript-dev]# co
Global configuration mode
idg[gwyscript-dev](config)# debug-action 90
1:console.error('Foo.');
=>2:debugger;
3:console.error('Bar.');
(debug)
```

And "quelle surprise", it works.
