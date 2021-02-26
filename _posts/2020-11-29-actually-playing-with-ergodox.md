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

[_Check out Part 2 here._]({% link _posts/2020-11-29-playing-with-ergodox-part-two.md %})