---
layout: post
title:  "Installing a Newer Kernel on Ubuntu"
---

I wanted to install a new kernel.

I am currently running Ubuntu 20.04 LTS on my main computer at home. This is my desktop PC which I use for "fun" projects, browsing, and gaming.

I have recently purchased some new bits for it, in particular [a new motherboard and a ryzen 5600 cpu]({% link _posts/2021-01-01-fun-upgrading-ryzen-5000-cpu-b550i-motherboard.md %}). My new mobo uses the r8125 chipset for ethernet.

While I upgraded the parts a while back, there are two main things I have noticed that I do not have hardware support on my current kernel for.

The first is `k10temp`, which should allow me to monitor the temperature of my CPU.
This is available in kernel 5.10, according to [this link](https://www.phoronix.com/scan.php?page=news_item&px=Linux-5.10-AMD-Zen3-k10temp).

Similarly, the ethernet chipset support comes in 5.9 (I think, I haven't really seen much of an official source...)

My current kernel (on ubuntu 20.04) is 5.4.
```bash
peter@C-3PO:~$ uname -r
5.4.0-64-generic
```

Ok, so I need to update this to 5.10.

After some googling, it seems like the easiest way to manage kernels is to either use the `mainline` GUI program or this script https://github.com/pimlie/ubuntu-mainline-kernel.sh#install

I tried the script.

```bash
peter@C-3PO:~$ wget https://raw.githubusercontent.com/pimlie/ubuntu-mainline-kernel.sh/master/ubuntu-mainline-kernel.sh
--2021-01-26 20:14:36--  https://raw.githubusercontent.com/pimlie/ubuntu-mainline-kernel.sh/master/ubuntu-mainline-kernel.sh
Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 151.101.120.133
Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|151.101.120.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 26474 (26K) [text/plain]
Saving to: ‘ubuntu-mainline-kernel.sh’

ubuntu-mainline-kernel.sh 100%[===================================>]  25.85K  --.-KB/s    in 0.01s   

2021-01-26 20:14:37 (2.64 MB/s) - ‘ubuntu-mainline-kernel.sh’ saved [26474/26474]

peter@C-3PO:~$
peter@C-3PO:~$ chmod +x ubuntu-mainline-kernel.sh
peter@C-3PO:~$ sudo mv ubuntu-mainline-kernel.sh /usr/local/bin/
peter@C-3PO:~$
peter@C-3PO:~$ sudo ubuntu-mainline-kernel.sh -i
Finding latest version available on kernel.ubuntu.com
Latest version is: v5.10.10, continue? (y/N) 
Will download 6 files from kernel.ubuntu.com:
Downloading amd64/linux-headers-5.10.10-051010-generic_5.10.10-051010.202101231639_amd64.deb: 100%   
Downloading amd64/linux-headers-5.10.10-051010_5.10.10-051010.202101231639_all.deb: 100%   
Downloading amd64/linux-image-unsigned-5.10.10-051010-generic_5.10.10-051010.202101231639_amd64.deb:  100%   
Downloading amd64/linux-modules-5.10.10-051010-generic_5.10.10-051010.202101231639_amd64.deb: 100%   
Downloading amd64/CHECKSUMS: 100%   
Downloading amd64/CHECKSUMS.gpg: 100%   
Importing kernel-ppa gpg key ok
Signature of checksum file has been successfully verified
Checksums of deb files have been successfully verified with sha256sum
Installing 4 packages
Cleaning up work folder
peter@C-3PO:~$ ubuntu-mainline-kernel.sh -l
v5.10.10-051010
peter@C-3PO:~$
```

Now to reboot and see what happens!

...

I rebooted and it worked.
And now my cpu temperature sensor works too!

```bash
peter@C-3PO:~$ uname -r
5.10.10-051010-generic
peter@C-3PO:~$ 
peter@C-3PO:~$ 
peter@C-3PO:~$ 
peter@C-3PO:~$ 
peter@C-3PO:~$ 
peter@C-3PO:~$ sensors
amdgpu-pci-0900
Adapter: PCI adapter
vddgfx:      700.00 mV 
fan1:           0 RPM  (min =    0 RPM, max = 3200 RPM)
edge:         +34.0°C  (crit = +110.0°C, hyst = -273.1°C)
                       (emerg = +115.0°C)
junction:     +34.0°C  (crit = +105.0°C, hyst = -273.1°C)
                       (emerg = +110.0°C)
mem:          +34.0°C  (crit = +105.0°C, hyst = -273.1°C)
                       (emerg = +110.0°C)
power1:        6.00 W  (cap = 135.00 W)

acpitz-acpi-0
Adapter: ACPI interface
temp1:        +16.8°C  (crit = +20.8°C)

k10temp-pci-00c3
Adapter: PCI adapter
Tctl:         +45.0°C  
Tdie:         +45.0°C  

peter@C-3PO:~$ 
```
