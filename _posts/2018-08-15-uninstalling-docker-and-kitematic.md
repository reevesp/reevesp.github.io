---
layout: post
title:  "Uninstalling Docker and Kitematic"
---
I wanted to uninstall Docker and Kitematic, in order to replace/update them to their latest versions (I couldn't autoupdate them).
I had not used either of them in years, so they were very out-of-date. Also, I was very rusty at using both. I could not remember how I installed them, but I decided that I would remove Kitematic first, and then Docker.

I tried dragging Kitematic from the Dock into the Trash, however I was still able to open Kitematic from Finder.
I then went into the `/Applications/` directory and deleted Kitematic from there. I was still able to open Kitematic from Finder.
I checked whether Kitematic was still listed in Launchpad- it was.

At this point, I checked Google. Some blogs and github posts suggested:

```
- Remove Kitematic.app
- Remove any unwanted Virtual Machines in VirtualBox bash
- Remove app data rm -rf ~/Library/Application\ Support/Kitematic
```

I had already removed `Kitematic.app` (I thought). I was able to remove my Docker VM using the commands:

```
Peters-MacBook-Pro:~ peterreeves$ docker-machine ls
NAME      ACTIVE   DRIVER       STATE     URL   SWARM   DOCKER   ERRORS
default            virtualbox   Timeout
Peters-MacBook-Pro:~ peterreeves$ docker-machine rm default
About to remove default
Are you sure? (y/n): y
Successfully removed default
```

When I went to remove `/Library/Application\ Support/Kitematic` I found that the `/Kitematic` dir did not exist.

At this point, I also checked whether I had installed Kitematic via brew, however `brew list` did not return anything Kitematic or Docker related.

Finally, after more googling, I found I had installed Docker Toolbox which contains Docker and KM in one block. I had to follow the uninstall instructions for Docker Toolbox that are listed here. https://docs.docker.com/toolbox/toolbox_install_mac/#how-to-uninstall-toolbox
