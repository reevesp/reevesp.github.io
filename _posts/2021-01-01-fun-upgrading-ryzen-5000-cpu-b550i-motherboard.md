---
layout: post
title:  "Fun upgrading to a Ryzen 5xxx CPU and B550i Motherboard"
---

As a Christmas present to myself, I bought myself a new Ryzen CPU and compatible motherboard. The exact items were a ryzen 5600x and a B550i Aorus Pro AX motherboard. Note that I am upgrading my CPU, mobo, and RAM, however I am keeping my drives, PSU, and video card the same.

Below are all of the fun and games that I have experienced as part of upgrading.

## 1. B550 mobos need a BIOS update before they can use gen5 ryzen CPUs.

...I was worried about this, however Ubuntu just booted and seemed to work out the of the box. No mobo bios update was necessary (or so I thought...).

## 2. Ubuntu did not have Ethernet connectivity.

I was able to roll-over my Ubuntu drive easily (just unplug the sata cable from my old mobo, plug it into my new mobo). However, I noticed that I was not getting any network connectivity. Staring at the ethernet port on the back of the mobo, I could see that the orange LED was blinking, however the green LED never came on.

It was here, that a pal with excellent google-fu pointed me to [this post on reddit](https://www.reddit.com/r/gigabytegaming/comments/iomt1c/gigabyte_b550i_aorus_pro_ax_realtek_rtl8125bg/) which was my exact motherboard, Linux distro, and issue. In summary, "go download the driver because your ethernet chipset's driver isn't in your kernel".

I noticed that the suggested resolution included updating the linux kernel, however the links on the Realtek website stated that the Linux drivers should be valid for my kernel.

```
peter@C-3PO:~$ uname -a
Linux C-3PO 5.4.0-58-generic #64-Ubuntu SMP Wed Dec 9 08:16:25 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
peter@C-3PO:~$ 
```

The next gotcha was how would I download the driver without connectivity? I had been playing around with the `lspci -knn` command and noticed that although I had no ethernet driver, I did have a wi-fi driver. So I attached the aerial that screwed into the back of the motherboard, connected to my wifi, and was able to download the realtek ethernet driver that way.

I downloaded the `2.5G Ethernet LINUX driver r8125 for kernel up to 5.6` from the https://www.realtek.com/en/component/zoo/category/network-interface-controllers-10-100-1000m-gigabit-ethernet-pci-express-software link, unzipped the archive, and followed the instructions in the README (which were just to unzip the archive and run the `./autorun.sh` as root.

That was all I needed to do, I instantly got wired connectivity.

## 2.5 Bios Gotchas - my own mistake
While fiddling with my new bios, I set it to `Fast Boot: Ultra Fast` which apparently boots _so fast_ that you can't press F2, F12, or Del in order to open bios again.
So how do I get back to bios when this setting is enabled? (without opening the case and disconnecting all my drives?

Turns out there is a command for this:

`systemctl reboot --firmware-setup`

This will reboot and put you straight into Bios. This is the windows equivalent of `Start > Updates and Security > Recovery > Restart Now > *PC reboots into that Safe Mode-ish prompt* > Troubleshoot > Advanced Options > UEFI Firmware Settings > Restart`.

Then, back in my bios, I decided to turn Fast Boot back off.

## 3. Windows doesn't boot at all
Although Ubuntu immediately sprang to life, Windows would not boot. Every boot win10 would alternate between showing a black screen with a blue win10 logo, and then showing a black screen with the blue win10 logo and `Preparing Automatic Repair`. I was filled with dread when I googled "Preparing Automatic Repair" and I was shown many 3rd party links saying "HOW TO FIX PREPATING AUTOMATIC REPAIR BOOT LOOP" because I knew then that I was in problem territory.

Most help blogs said to reboot 3 times in a row, which will cue win10 to boot into safe-mode next time. That didn't work for me - my win10 alternated between being stuck on the win10 logo and being stuck on the `Preparing Automatic Repair` screens.

Further reading of blogs told me that I would need to prepare a win10 installation media to repair my installation ... although I suspect that the repair would involve a re-imaging.

I then realised that creating win10 installation media takes AGES. It is still going as a write this. It's on 32%.

I have also realised that this might have something to do with the fact that I am now using a 64bit CPU, whereas before I had a 32bit CPU. So perhaps I have a 32bit version of windows...?

My recovery USB completed its writing process, and I put it into my desktop and booted from it.
...and the recovery USB hung on a black screen with a blue win10 logo!

I did some more googling, and there were suggestions to check bios settings incase any kind of setting was disabled. I went through all my settings, and in a quest for more bios settings, I looked at updating my motherboard bios.

I knew that there was intrigue around the combination of this motherboard with a 5000-series ryzen CPU, and some people had needed to flash a new motherboard bios... but I was able to run ubuntu, run programs, surely my mobo and cpu are fine?

I poked around in the bios and saw that the mobo came with version F3 from August 2020. I had a look on [gigabyte's website for this motherboard](https://www.gigabyte.com/uk/Motherboard/B550I-AORUS-PRO-AX-rev-10/support#support-dl-bios) and found that a) the latest available version was F11p (note that gigabyte seems to have skipped a lot of numbers between 3 and 11) and b) version F10 was the version which included `Update AMD AGESA ComboV2 1.0.8.1 for AMD Ryzen 5000 processors support`. Was this what windows needed?

I patched the firmware, and when my machine rebooted, the recovery USB was picked up and started! Although, at that point, I decided to try booting my old win10 drive. And that booted correctly too! It displayed the "spinny dots" and did some fixing of itself, rebooted a few times, but finally windows booted, started, I was able to log in, and run windows update (which pulled down a load of new stuff).

## 4. Synology Drive Client keeps crashing on Ubuntu

Yeah. Synology Drive Client would start, immediately stop and display a pop-up saying "synology drive client an unknown issue has occurred, please restart". I'm glad that I didn't try doing some of the fixes I'd read on google - reinstalling the program, wiping all my settings, etc etc ... because applying that mobo bios update fixed this too.

## 5. This goddamn RGB on my mobo looks like a tiny fire has started inside my case

Final point, this motherboard is my first ever RGB enabled component. However the default RGB setting seems to be a faint orange glow, which when you stare at it looks like a motherboard glowing AMD red/orange ... but when seen through your case fans and grills out of the corner of your eye it looks identical to a small fire inside your computer.

Now that I had windows working, I could download the Gigabyte RGB Fusion program, run it, and apply some different colours. I picked RGB rainbow cycle awfulness, because I just wanted to change away from the orange.
