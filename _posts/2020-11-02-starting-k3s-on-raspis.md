---
layout: post
title:  "Starting k3s on my Raspberry Pis"
---
I bought some new [Raspberry Pi 4s](https://www.raspberrypi.org/products/raspberry-pi-4-model-b/) because I have been wanting to play "k8s-at-home" for a while.

And I wanted to use [k3s](https://k3s.io/) because it was recommended to me and all the k8s twitterati seem to like it.

## Setting up the Pis and getting them on my network
First, I set up the Pis.
I imaged my micro SD card, using [Raspi imager](https://www.raspberrypi.org/blog/raspberry-pi-imager-imaging-utility/), and using the default image. (32bit raspbian).

I added the empty file 'ssh' into the formatted card after Raspi imager had written the raspbian image (just creating a file at the root of the SD card) so that the Pi has ssh enabled on startup.

I did this for both SD cards. I plugged both SD cards into pis, and started the pis up.

Then, after a few mins, I looked up the given Ips that the pis had grabbed via dhcp (I justed used [Fing](https://play.google.com/store/apps/details?id=com.overlook.android.fing&hl=en_GB&gl=US) for this, there is probably a clever way to do this on the cli).

I then ssh'd to the Pis.

I also grabbed the mac addresses from the pis, assigned them IPs in my routers DHCP config and assigned those IPs hostnames and FQDNs on my pi.hole.

Finally, I rebooted the pis via `sudo reboot now`

## Putting k3s on there

I ssh'd back to the Pis, and did [the extra commands for default raspbian that is required by k3s](https://rancher.com/docs/k3s/latest/en/advanced/#enabling-legacy-iptables-on-raspbian-buster).

```bash
sudo iptables -F
sudo update-alternatives --set iptables /usr/sbin/iptables-legacy
sudo update-alternatives --set ip6tables /usr/sbin/ip6tables-legacy
sudo reboot
```
Then, I installed k3s with this command:
```bash
curl -sfL https://get.k3s.io | sh -s - --write-kubeconfig-mode 644
```
I found that I needed to add the `--write-kubeconfig-mode` so that I can kubectl without sudo

Then, I saw this error in my logs.
```bash
$ journalctl -u k3s
msg="failed to find memory cgroup, you may need to add \"cgroup_memory=1 cgroup_enable=memory\" to your linux cmdline (/boot/cmdline.txt on a Raspberry 
Pi)"
```

So I did as the log message asked. I added `cgroup_memory=1 cgroup_enable=memory` to my `/boot/cmdline.txt` file.

For reference, that file now looks like:
```bash
pi@TC-14:~ $ cat /boot/cmdline.txt 
console=serial0,115200 console=tty1 root=PARTUUID=db4dbc98-02 rootfstype=ext4 elevator=deadline fsck.repair=yes rootwait quiet splash plymouth.ignore-serial-consoles cgroup_memory=1 cgroup_enable=memory
```

...but then I kept getting the same error.
After some googling, I looked here and here:
- https://blog.codybunch.com/2020/07/31/Fixing-cgroup-memory-on-Raspbian-Buster-for-Kernel-54x/
- https://raspberrypi.stackexchange.com/questions/101790/how-to-revert-from-rpi-update-to-stable-build

And I found this command seemed to give it a kick:
```bash
sudo apt-get update; sudo apt-get install --reinstall raspberrypi-bootloader raspberrypi-kernel
```
and then after another `sudo reboot now`, it all started working.


