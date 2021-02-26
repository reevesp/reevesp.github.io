Nov 29 2020

---
layout: post
title:  "Actually Playing with my Ergodox"
---
I bought an [Ergodox-EZ](https://ergodox-ez.com/) a few years ago, and i have never reconfigured it.

There are a lot of things that I really like about it - I consider it to be the comfiest keyboard that I have ever used, and I think that having the two parts in line with your shoulders is absolutely amazing, and that the ortholinear arrangement of keys is great too.

And, to be honest, the default layout of keys has worked OK for me.

My desk setup is as follows: I have a cheap old Logitech K120 keyboard in the centre, the Ergodox left and right hands on either side of it with their wrist rests, and then in front of the K120 is the printed-out layout of the [Ergodox EZ Getting Started page layout](https://cdn.shopify.com/s/files/1/1152/3264/files/default_firmware_v1.2.pdf?2947908262754067686).

However, since I bought it, I have really gotten into Vim, and the default position of the Escape key has driven me mad.

That, and the fact that all of the computers that I have are set to the UK-GB input, so many of the keys have moved around. I have yet to find out how to type a pipe `|` on my Ergodox...

The lack of English character support (well, GB character keycode support) put me off the last time I tried this (at least a year ago), however this now seems to be a selectable option in the [online configurator tool](https://configure.ergodox-ez.com/ergodox-ez/).

So let's try it out!

## Updating my (keyboard's) Firmware to the Latest Level

I noticed that many of the keyboard layouts that I would be looking at mentioned that they were for different firmware versions.
Ergodox-EZ has a tool [Wally](https://ergodox-ez.com/pages/wally) to do this.

I clicked the Linux link [here](https://ergodox-ez.com/pages/wally) , and followed the instructions on their [Github](https://github.com/zsa/wally/wiki/Linux-install).

```bash
peter@C-3PO:~$ sudo apt install gtk+3.0 libwebkit2gtk-4.0 libusb-dev
[sudo] password for peter: 
Reading package lists... Done
Building dependency tree       
Reading state information... Done
E: Unable to locate package gtk+3.0
E: Unable to locate package libwebkit2gtk-4.0
```

...turns out it is all just installed by default on Ubuntu.

I just needed to download the Wally tool and `chmod` it.

I found that Wally seems a bit flaky and doesn't always run. hmmmmmmm.

```bash
wget https://configure.ergodox-ez.com/wally/linux -O wally
```

I then noticed sometimes I couldn't open Wally from certain directories, hmmmmm. But it opened once I put it in its own dir.

```
peter@C-3PO:~$ ./Downloads/wally 
```

_Break of about 2 months here while do other things like the disorganised and unfocussed person that I am._

## Finding a Firmware to put onto my Keyboard.

_...2 months later_

Initially I just wanted to find a generic layout, identical to the ergodox one, but with UK locale key locations. So I had a click around on the site, looked down the [results for the UK tag on the search](https://configure.ergodox-ez.com/ergodox-ez/search?q=uk&anonymous=false&withTour=false&legacy=false), however I decided to build my own layout.

Really, this was a distraction and I wanted to get a single "end-to-end process" to do:
- download-config
- flash-config-to-keyboard
- start-typing

I went to [https://configure.ergodox-ez.com/ergodox-ez/layouts/default/latest/0](https://configure.ergodox-ez.com/ergodox-ez/layouts/default/latest/0) and clicked "Download source (Original & Shine)". This downloaded a zip containing the default layout (which had changed a little since I bought the keyboard). I unzipped the file and it contained a .hex file.

I fed this .hex to Wally.

Next, I went and found a paperclip and pressed the reset button on my keyboard.


...Nothing. And I couldn't type on my keyboard any more. (I had to switch back to my spare K120 keyboard). In the Wally log (which was accessed via clicking on the `>_` icon, I saw:

```
warning OpenDevices: libusb: bad access [code -3]
```

I pasted this into Google. There were _two_ results. Not a good sign...

I went through the first result, a Github issue [https://github.com/OpenKinect/libfreenect2/issues/743](https://github.com/OpenKinect/libfreenect2/issues/743). It looks like people chatting about versions of `libusb`, so I decided to go back to the Wally Linux install page and double-check I had installed everything correctly.

```bash
sudo apt install libusb-dev
peter@C-3PO:~$ sudo apt install libusb-dev
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following packages were automatically installed and are no longer required:
  libfprint-2-tod1 libllvm9 libllvm9:i386
Use 'sudo apt autoremove' to remove them.
The following additional packages will be installed:
  libusb-0.1-4
The following NEW packages will be installed
  libusb-0.1-4 libusb-dev
0 to upgrade, 2 to newly install, 0 to remove and 47 not to upgrade.
Need to get 47.7 kB of archives.
After this operation, 294 kB of additional disk space will be used.
Do you want to continue? [Y/n] Y
Get:1 http://gb.archive.ubuntu.com/ubuntu focal/main amd64 libusb-0.1-4 amd64 2:0.1.12-32 [17.4 kB]
Get:2 http://gb.archive.ubuntu.com/ubuntu focal/main amd64 libusb-dev amd64 2:0.1.12-32 [30.3 kB]
Fetched 47.7 kB in 0s (191 kB/s)     
Selecting previously unselected package libusb-0.1-4:amd64.
(Reading database ... 243111 files and directories currently installed.)
Preparing to unpack .../libusb-0.1-4_2%3a0.1.12-32_amd64.deb ...
Unpacking libusb-0.1-4:amd64 (2:0.1.12-32) ...
Selecting previously unselected package libusb-dev.
Preparing to unpack .../libusb-dev_2%3a0.1.12-32_amd64.deb ...
Unpacking libusb-dev (2:0.1.12-32) ...
Setting up libusb-0.1-4:amd64 (2:0.1.12-32) ...
Setting up libusb-dev (2:0.1.12-32) ...
Processing triggers for libc-bin (2.31-0ubuntu9.1) ...
Processing triggers for man-db (2.9.1-1) ...
peter@C-3PO:~$ 
```

Hmmmm, I was able to install a new package. Let's try Wally again.
Same error.

Lets test the other packages on the Linux-install page.

First, I looked at this `udev` stuff.
[https://github.com/zsa/wally/wiki/Linux-install#2-create-a-udev-rule-file](https://github.com/zsa/wally/wiki/Linux-install#2-create-a-udev-rule-file).

```bash
peter@C-3PO:~$ 
peter@C-3PO:~$ cd /etc/udev/rules.d/
peter@C-3PO:/etc/udev/rules.d$ ls
70-snap.core.rules  70-snap.drawio.rules  70-snap.zoom-client.rules
peter@C-3PO:/etc/udev/rules.d$ sudo touch /etc/udev/rules.d/50-wally.rules
peter@C-3PO:/etc/udev/rules.d$ sudo vi 50-wally.rules 
peter@C-3PO:/etc/udev/rules.d$ 
peter@C-3PO:/etc/udev/rules.d$ 
peter@C-3PO:/etc/udev/rules.d$ 
peter@C-3PO:/etc/udev/rules.d$ cat 50-wally.rules 
# Teensy rules for the Ergodox EZ
ATTRS{idVendor}=="16c0", ATTRS{idProduct}=="04[789B]?", ENV{ID_MM_DEVICE_IGNORE}="1"
ATTRS{idVendor}=="16c0", ATTRS{idProduct}=="04[789A]?", ENV{MTP_NO_PROBE}="1"
SUBSYSTEMS=="usb", ATTRS{idVendor}=="16c0", ATTRS{idProduct}=="04[789ABCD]?", MODE:="0666"
KERNEL=="ttyACM*", ATTRS{idVendor}=="16c0", ATTRS{idProduct}=="04[789B]?", MODE:="0666"

# STM32 rules for the Moonlander and Planck EZ
SUBSYSTEMS=="usb", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="df11", \
    MODE:="0666", \
    SYMLINK+="stm32_dfu"
peter@C-3PO:/etc/udev/rules.d$ 
```

And now let's look at this `plugdev` group stuff.

...I think I'm already in this group. I'll skip this bit.

```
peter@C-3PO:/etc/udev/rules.d$ cat /etc/group | grep plugdev
plugdev:x:46:peter
peter@C-3PO:/etc/udev/rules.d$ whoami
peter
peter@C-3PO:/etc/udev/rules.d$ 
```

And then "make sure to log out after that".

I'll do one better, I'll reboot .. so that I can hopefully refresh my Ergodox, I can't be asked to reach down and reseat the USB for it to power cycle it.

Ok, after the reboot.

I open up Wally, load the `firmware.hex` in and ... it does it!
Wally displays the "Flashing your Keyboard" message, it takes about a second, and then is displays the "All done!" screen.

I decide to look at Wally's log again.

```
16:08:24
info
Probing compatible usb devices
16:08:24
info
Found 1 compatible device(s)
16:08:32
info
Starting Teensy Flash
16:08:32
info
Waiting for a DFU capable device
16:08:33
info
Sent 0 bytes out of 32256
16:08:33
info
Sent 128 bytes out of 32256
16:08:33
info
Sent 256 bytes out of 32256
16:08:33
info
Sent 384 bytes out of 32256
16:08:33
info
Sent 512 bytes out of 32256
16:08:33
info
...
<snip/>
...
Sent 31872 bytes out of 32256
16:08:34
info
Sent 32000 bytes out of 32256
16:08:34
info
Sent 32128 bytes out of 32256
16:08:35
info
Sending the reboot command
16:08:35
info
Flash complete
```

...that looks good!
Now to play with it!

## Playing with Ergodox, Part Two

The first thing I noticed was that my `'"` key had been replaced with an "Oryx" training key.
I the checked with my paper backup and I saw that the Oryx key is the only difference between the new latest layout and the default layout that the keyboard shipped with.

Rather than immediately edit out the Oryx key, I decided to try out the Oryx live training thing.

I went to [https://configure.ergodox-ez.com/train](https://configure.ergodox-ez.com/train). I _even_ checked the [instructions for Linux users](https://github.com/zsa/wally/wiki/Live-training-on-Linux).

...and then I was told that Firefox was not supported, because [Oryx required `webusb`](https://caniuse.com/webusb).

I needed to use Edge, something Chromium-based, or Opera.

I looked at `ungoogled-chromium` for a while, however it looked like effort to install and I wasn't sure that i trusted it. I saw that Opera was available as a snap.

I opened Opera and tried it out. It seemed like an OK browser, however the semi-translucent window bars reminded me of when I was a teenager ricing my Windows Vista laptop.

I didn't get the pop up about webusb being incompatible, however my keyboard still didn't connect when I pressed the Oryx button in Opera.

I then looked at the Linux instructions again and saw that I needed to do them... I skimmed them before. :blush: I also saw that I needed to give my browser permissions for usb direct connectivity.

I didn't see that option in the "Software" application (`gnome-software`) on Ubuntu, so I tried relogging to see if Opera would just start working.

_...and it was then that I got the sinking feeling that this would only work on chromium..._

And guess what... Chromium had that option when installed.

I wondered whether perhaps this permission is specific to the opera snap - lets remove Opera via `snap`, then install it via `.rpm`.

I also went and did some googling about why Firefox does not support `webusb` as a standard. Apparently it is because webusb is "fundamentally insecure", which I kinda get.

Some more digging. It seems webusb is only supported on chromium-based browsers (hence Opera, Chromium, Edge from the Oryx instructions). Because of this, I was really getting the sinking feeling that I should switch to `ungoogled-chromium` sooner rather than later...

I downloaded the Opera .deb from [here](https://www.opera.com/download).

```bash
peter@C-3PO:~/Downloads$ sudo apt install ./opera-stable_72.0.3815.400_amd64.deb 
[sudo] password for peter: 
Reading package lists... Done
Building dependency tree       
Reading state information... Done
Note, selecting 'opera-stable' instead of './opera-stable_72.0.3815.400_amd64.deb'
The following packages were automatically installed and are no longer required:
  libfprint-2-tod1 libllvm9 libllvm9:i386
Use 'sudo apt autoremove' to remove them.
The following additional packages will be installed:
  pepperflashplugin-nonfree
Suggested packages:
  chromium-browser ttf-dejavu ttf-mscorefonts-installer ttf-xfree86-nonfree
The following NEW packages will be installed
  opera-stable pepperflashplugin-nonfree
0 to upgrade, 2 to newly install, 0 to remove and 47 not to upgrade.
Need to get 5,492 B/69.9 MB of archives.
After this operation, 215 MB of additional disk space will be used.
Do you want to continue? [Y/n] Y
Get:1 http://gb.archive.ubuntu.com/ubuntu focal/multiverse amd64 pepperflashplugin-nonfree amd64 1.8.6ubuntu1 [5,492 B]
Get:2 /home/peter/Downloads/opera-stable_72.0.3815.400_amd64.deb opera-stable amd64 72.0.3815.400 [69.9 MB]
Fetched 5,492 B in 1s (9,137 B/s)
Preconfiguring packages ...
Selecting previously unselected package opera-stable.
(Reading database ... 243174 files and directories currently installed.)
Preparing to unpack .../opera-stable_72.0.3815.400_amd64.deb ...
Unpacking opera-stable (72.0.3815.400) ...
Selecting previously unselected package pepperflashplugin-nonfree.
Preparing to unpack .../pepperflashplugin-nonfree_1.8.6ubuntu1_amd64.deb ...
Unpacking pepperflashplugin-nonfree (1.8.6ubuntu1) ...
Setting up opera-stable (72.0.3815.400) ...
update-alternatives: using /usr/bin/opera to provide /usr/bin/x-www-browser (x-www-browser) in auto mode
update-alternatives: warning: skip creation of /usr/share/man/man1/x-www-browser.1.gz because associated file /usr/share/man/man1/opera.1.gz (of link group x-www-browser) doesn't exist
update-alternatives: using /usr/bin/opera to provide /usr/bin/gnome-www-browser (gnome-www-browser) in auto mode
update-alternatives: warning: skip creation of /usr/share/man/man1/gnome-www-browser.1.gz because associated file /usr/share/man/man1/opera.1.gz (of link group gnome-www-browser) doesn't exist
Setting up pepperflashplugin-nonfree (1.8.6ubuntu1) ...
--2020-11-29 19:43:55--  https://get.adobe.com/flashplayer/webservices/json/?platform_type=Linux&platform_arch=x86_64&browser_dist=Chrome
Resolving get.adobe.com (get.adobe.com)... 193.104.215.66
Connecting to get.adobe.com (get.adobe.com)|193.104.215.66|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: unspecified [application/json]
Saving to: ‘STDOUT’

-                                                      [ <=>                                                                                                            ]   1.89K  --.-KB/s    in 0s      

2020-11-29 19:43:56 (178 MB/s) - written to stdout [1937]

--2020-11-29 19:43:56--  https://fpdownload.adobe.com/pub/flashplayer/pdc/32.0.0.453/flash_player_ppapi_linux.x86_64.tar.gz
Resolving fpdownload.adobe.com (fpdownload.adobe.com)... 2.20.93.213, 2a02:26f0:13b:38f::11e2, 2a02:26f0:13b:39d::11e2
Connecting to fpdownload.adobe.com (fpdownload.adobe.com)|2.20.93.213|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 9764153 (9.3M) [application/x-gzip]
Saving to: ‘STDOUT’

-                                                  100%[===============================================================================================================>]   9.31M  4.35MB/s    in 2.1s    

2020-11-29 19:43:58 (4.35 MB/s) - written to stdout [9764153/9764153]

Processing triggers for mime-support (3.64ubuntu1) ...
Processing triggers for hicolor-icon-theme (0.17-2) ...
Processing triggers for gnome-menus (3.36.0-1ubuntu1) ...
Processing triggers for shared-mime-info (1.15-1) ...
Processing triggers for desktop-file-utils (0.24-1ubuntu3) ...
peter@C-3PO:~/Downloads$ 
```

Now, trying the Opera installed via deb.


Hmmmmmm, OK. So Opera downloaded via deb seems to see the Ergodox-EZ, however it never gets any further and still says:

```
No device selected.

Restart
```

OK. Removed Opera, installed ungoogled chromium.

```bash
peter@C-3PO:~/Downloads$ sudo apt remove opera-stable pepperflashplugin-nonfree
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following packages were automatically installed and are no longer required:
  libfprint-2-tod1 libllvm9 libllvm9:i386
Use 'sudo apt autoremove' to remove them.
The following packages will be REMOVED
  opera-stable pepperflashplugin-nonfree
0 to upgrade, 0 to newly install, 2 to remove and 47 not to upgrade.
After this operation, 215 MB disk space will be freed.
Do you want to continue? [Y/n] Y
(Reading database ... 243307 files and directories currently installed.)
Removing opera-stable (72.0.3815.400) ...
update-alternatives: using /usr/bin/firefox to provide /usr/bin/x-www-browser (x-www-browser) in auto mode
update-alternatives: using /usr/bin/firefox to provide /usr/bin/gnome-www-browser (gnome-www-browser) in auto mode
Removing pepperflashplugin-nonfree (1.8.6ubuntu1) ...
Processing triggers for mime-support (3.64ubuntu1) ...
Processing triggers for hicolor-icon-theme (0.17-2) ...
Processing triggers for gnome-menus (3.36.0-1ubuntu1) ...
Processing triggers for shared-mime-info (1.15-1) ...
Processing triggers for desktop-file-utils (0.24-1ubuntu3) ...
peter@C-3PO:~/Downloads$ 
```

I installed ungoogled-chromium via instructions from [here](https://github.com/ungoogled-software/ungoogled-chromium-debian).

```bash
peter@C-3PO:~/Downloads$ 
peter@C-3PO:~/Downloads$ echo 'deb http://download.opensuse.org/repositories/home:/ungoogled_chromium/Ubuntu_Focal/ /' | sudo tee /etc/apt/sources.list.d/home-ungoogled_chromium.list > /dev/null
peter@C-3PO:~/Downloads$ curl -s 'https://download.opensuse.org/repositories/home:/ungoogled_chromium/Ubuntu_Focal/Release.key' | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/home-ungoogled_chromium.gpg > /dev/null
peter@C-3PO:~/Downloads$ sudo apt update
Get:1 http://download.opensuse.org/repositories/home:/ungoogled_chromium/Ubuntu_Focal  InRelease [1,565 B]
Hit:2 http://ppa.launchpad.net/lutris-team/lutris/ubuntu focal InRelease                                                                                                                                  
Hit:3 http://gb.archive.ubuntu.com/ubuntu focal InRelease                                                                                                                                                 
Get:4 http://gb.archive.ubuntu.com/ubuntu focal-updates InRelease [114 kB]                                                                                                                                
Hit:5 http://ppa.launchpad.net/obsproject/obs-studio/ubuntu focal InRelease                                                                                                                               
Hit:6 https://download.docker.com/linux/ubuntu focal InRelease                                                            
Get:7 http://security.ubuntu.com/ubuntu focal-security InRelease [109 kB]                            
Get:8 http://download.opensuse.org/repositories/home:/ungoogled_chromium/Ubuntu_Focal  Packages [3,755 B]      
Get:10 http://gb.archive.ubuntu.com/ubuntu focal-backports InRelease [101 kB]                                              
Hit:9 https://packages.cloud.google.com/apt kubernetes-xenial InRelease                                                         
Get:11 http://gb.archive.ubuntu.com/ubuntu focal-updates/main amd64 DEP-11 Metadata [237 kB]
Get:12 http://gb.archive.ubuntu.com/ubuntu focal-updates/universe amd64 DEP-11 Metadata [205 kB]
Get:13 http://gb.archive.ubuntu.com/ubuntu focal-updates/multiverse amd64 DEP-11 Metadata [2,468 B]
Get:14 http://gb.archive.ubuntu.com/ubuntu focal-backports/universe amd64 DEP-11 Metadata [1,768 B]
Get:15 http://security.ubuntu.com/ubuntu focal-security/main amd64 DEP-11 Metadata [24.3 kB]
Get:16 http://security.ubuntu.com/ubuntu focal-security/universe amd64 DEP-11 Metadata [56.6 kB]
Fetched 855 kB in 2s (549 kB/s)             
Reading package lists... Done
Building dependency tree       
Reading state information... Done
47 packages can be upgraded. Run 'apt list --upgradable' to see them.
peter@C-3PO:~/Downloads$ sudo apt install -y ungoogled-chromium
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following packages were automatically installed and are no longer required:
  libfprint-2-tod1 libllvm9 libllvm9:i386
Use 'sudo apt autoremove' to remove them.
The following additional packages will be installed:
  libjsoncpp1 libminizip1 libre2-5 ungoogled-chromium-common ungoogled-chromium-sandbox
Suggested packages:
  ungoogled-chromium-l10n ungoogled-chromium-shell ungoogled-chromium-driver
Recommended packages:
  libva1
The following NEW packages will be installed
  libjsoncpp1 libminizip1 libre2-5 ungoogled-chromium ungoogled-chromium-common ungoogled-chromium-sandbox
0 to upgrade, 6 to newly install, 0 to remove and 47 not to upgrade.
Need to get 49.8 MB of archives.
After this operation, 163 MB of additional disk space will be used.
Get:1 http://gb.archive.ubuntu.com/ubuntu focal/universe amd64 libminizip1 amd64 1.1-8build1 [20.2 kB]
Get:2 http://download.opensuse.org/repositories/home:/ungoogled_chromium/Ubuntu_Focal  ungoogled-chromium-common 84.0.4147.135-1.focal1 [270 kB]
Get:3 http://gb.archive.ubuntu.com/ubuntu focal/main amd64 libre2-5 amd64 20200101+dfsg-1build1 [162 kB]
Get:4 http://gb.archive.ubuntu.com/ubuntu focal/main amd64 libjsoncpp1 amd64 1.7.4-3.1ubuntu2 [75.6 kB]
Get:5 http://download.opensuse.org/repositories/home:/ungoogled_chromium/Ubuntu_Focal  ungoogled-chromium 84.0.4147.135-1.focal1 [49.2 MB]
Get:6 http://download.opensuse.org/repositories/home:/ungoogled_chromium/Ubuntu_Focal  ungoogled-chromium-sandbox 84.0.4147.135-1.focal1 [56.4 kB]                                                        
Fetched 49.8 MB in 13s (3,910 kB/s)                                                                                                                                                                       
Selecting previously unselected package libminizip1:amd64.
(Reading database ... 243178 files and directories currently installed.)
Preparing to unpack .../0-libminizip1_1.1-8build1_amd64.deb ...
Unpacking libminizip1:amd64 (1.1-8build1) ...
Selecting previously unselected package libre2-5:amd64.
Preparing to unpack .../1-libre2-5_20200101+dfsg-1build1_amd64.deb ...
Unpacking libre2-5:amd64 (20200101+dfsg-1build1) ...
Selecting previously unselected package libjsoncpp1:amd64.
Preparing to unpack .../2-libjsoncpp1_1.7.4-3.1ubuntu2_amd64.deb ...
Unpacking libjsoncpp1:amd64 (1.7.4-3.1ubuntu2) ...
Selecting previously unselected package ungoogled-chromium-common.
Preparing to unpack .../3-ungoogled-chromium-common_84.0.4147.135-1.focal1_amd64.deb ...
Unpacking ungoogled-chromium-common (84.0.4147.135-1.focal1) ...
Selecting previously unselected package ungoogled-chromium.
Preparing to unpack .../4-ungoogled-chromium_84.0.4147.135-1.focal1_amd64.deb ...
Unpacking ungoogled-chromium (84.0.4147.135-1.focal1) ...
Selecting previously unselected package ungoogled-chromium-sandbox.
Preparing to unpack .../5-ungoogled-chromium-sandbox_84.0.4147.135-1.focal1_amd64.deb ...
Unpacking ungoogled-chromium-sandbox (84.0.4147.135-1.focal1) ...
Setting up libminizip1:amd64 (1.1-8build1) ...
Setting up ungoogled-chromium-sandbox (84.0.4147.135-1.focal1) ...
Setting up ungoogled-chromium-common (84.0.4147.135-1.focal1) ...
Setting up libre2-5:amd64 (20200101+dfsg-1build1) ...
Setting up libjsoncpp1:amd64 (1.7.4-3.1ubuntu2) ...
Setting up ungoogled-chromium (84.0.4147.135-1.focal1) ...
Processing triggers for mime-support (3.64ubuntu1) ...
Processing triggers for hicolor-icon-theme (0.17-2) ...
Processing triggers for gnome-menus (3.36.0-1ubuntu1) ...
Processing triggers for libc-bin (2.31-0ubuntu9.1) ...
Processing triggers for man-db (2.9.1-1) ...
Processing triggers for desktop-file-utils (0.24-1ubuntu3) ...
peter@C-3PO:~/Downloads$ 
```

I opened Chromium...

I now saw that there were two chromium options, appearing in my launchy thing. I decided to start `chromium` from the cli.

I went to https://configure.ergodox-ez.com/train .

I clicked on "Connect my keyboard" and not only did "Ergodox EZ" appear as an option, but I was able to select it. I pressed the "Oryx" button and I finally arrived. :tada:



## Some interesting links if you made it this far...

Interesting links:

- https://ubuntu.com/blog/a-guide-to-snap-permissions-and-interfaces

- https://askubuntu.com/questions/1206951/how-can-snap-permissions-be-viewed-and-modified

- https://web.dev/usb/

- https://caniuse.com/#feat=webusb