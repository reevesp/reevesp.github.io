---
layout: post
title:  "The 'python' command vs the 'python3' command"
---

Just a little fact that I learnt today and that I found satisfying. Do bare-in-mind that I am a self-declared python novice (as you will see).

I wanted to get a bit more familiar with python. I can't remember the exact situation, I think it might have been one of the daily-programmer challenges or else something to do with python-flasks and k8s.

I was on win10 with with the Windows Subsystem for Linux/Ubuntu.
```
peter@R2-D2:~$ python
The program 'python' can be found in the following packages:
 * python-minimal
 * python3
Try: sudo apt install <selected package>
peter@R2-D2:~$ sudo apt install python3
[sudo] password for peter:
Reading package lists... Done
Building dependency tree
Reading state information... Done
python3 is already the newest version (3.5.1-3).
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
peter@R2-D2:~$ sudo apt-get update
#huge apt-get output snipped
peter@R2-D2:~$ sudo apt-get upgrade
#huge apt-get output snipped
peter@R2-D2:~$ python
The program 'python' can be found in the following packages:
 * python-minimal
 * python3
Try: sudo apt install <selected package>
peter@R2-D2:~$ python3
Python 3.5.2 (default, Nov 23 2017, 16:37:01)
[GCC 5.4.0 20160609] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> exit()
```

I was a little confused. Why was `python` not being installed? Why did I need to use the command `python3`?

I did a little googling, and came across some SO pages. In particular this one [How can I make the python command in terminal run python3 instead of python2](https://stackoverflow.com/questions/23048756/how-can-i-make-the-python-command-in-terminal-run-python3-instead-of-python2), which pointed to [PEP 394](https://www.python.org/dev/peps/pep-0394/).

It turns out it is a bona-fide python convention that `python` can never invoke python3. I just needed to understand even without python2 on a machine, that there are still semantic differences between `python` and `python3`.

So- I should just accept that running `python3` to start python is fine :D
