---
layout: post
title:  "Installing Node.js on Win10 Windows Subsystem for Linux"
---

# Installing Node.js on Win10 Windows Subsystem for Linux

I wanted to play around with nodejs on my Win10 Surface. Rather than using the direct download of an installer from the nodejs website, I wanted to make sure that it was installed via the Win10 bash, to have confidence that it would fit in with my workflow (`node`, `npm`, `code .`, _etc_).

I found the nodejs website really helpful. I initially found [this page (Nodejs Package Manager Downloads)](https://nodejs.org/en/download/package-manager/) via Google, and from the OS options there I decided that the Win10 Subsystem was closest to an "Ubuntu based Linux Distribution". This linked me to [this page(Nodesource README on Github)](https://github.com/nodesource/distributions/blob/master/README.md) .

From there, I decided to try the Ubuntu instructions:

```
# Using Ubuntu
curl -sL https://deb.nodesource.com/setup_11.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Yeah, I wasn't keen on piping curl into bash, however I optically grep'd the code coming back from curl before I piped it into a sudo terminal.

After it all installed, I quickly tested that everything worked as expected.

```
peter@R2-D2:~$ node --version
v11.12.0
peter@R2-D2:~$ npm --version
6.7.0
```