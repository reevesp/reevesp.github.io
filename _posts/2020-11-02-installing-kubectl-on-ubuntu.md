---
layout: post
title:  "Installing kubectl on Ubuntu to connect to remote k3s"
---
This shouldn't be too hard.

After [installing k3s on some raspis]({% link _posts/2020-11-02-starting-k3s-on-raspis %}), i now want to manage it from my local Ubuntu.

Unfortunately, `kubectl` wasn't in the default Ubuntu repositories, so I had to do some thinking and read some documentation. It turns out that the doco on kubernetes.io looks pretty good!

I followed [the instructions here](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-using-native-package-management).

So, kubernetes.io says just run these commands:
```bash
sudo apt-get update && sudo apt-get install -y apt-transport-https gnupg2 curl
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl
```
...so I did.
```bash
peter@C-3PO:~$ sudo apt update
[sudo] password for peter: 
Hit:1 http://gb.archive.ubuntu.com/ubuntu focal InRelease
Get:2 http://security.ubuntu.com/ubuntu focal-security InRelease [107 kB]                       
Hit:3 http://ppa.launchpad.net/lutris-team/lutris/ubuntu focal InRelease                            
Get:4 http://gb.archive.ubuntu.com/ubuntu focal-updates InRelease [111 kB]                          
Hit:5 http://ppa.launchpad.net/obsproject/obs-studio/ubuntu focal InRelease                         
Get:6 https://download.docker.com/linux/ubuntu focal InRelease [36.2 kB]                            
Get:7 http://gb.archive.ubuntu.com/ubuntu focal-backports InRelease [98.3 kB]                       
Get:8 http://security.ubuntu.com/ubuntu focal-security/main amd64 DEP-11 Metadata [24.3 kB]
Get:9 http://security.ubuntu.com/ubuntu focal-security/universe amd64 DEP-11 Metadata [56.6 kB]
Get:10 http://gb.archive.ubuntu.com/ubuntu focal-updates/main amd64 DEP-11 Metadata [229 kB]
Get:11 http://gb.archive.ubuntu.com/ubuntu focal-updates/universe amd64 DEP-11 Metadata [205 kB]
Get:12 http://gb.archive.ubuntu.com/ubuntu focal-updates/multiverse amd64 Packages [15.6 kB]
Get:13 http://gb.archive.ubuntu.com/ubuntu focal-updates/multiverse Translation-en [4,352 B]
Get:14 http://gb.archive.ubuntu.com/ubuntu focal-updates/multiverse amd64 DEP-11 Metadata [2,468 B]
Get:15 http://gb.archive.ubuntu.com/ubuntu focal-backports/universe amd64 DEP-11 Metadata [1,768 B]
Fetched 892 kB in 1s (741 kB/s)                          
Reading package lists... Done
Building dependency tree       
Reading state information... Done
12 packages can be upgraded. Run 'apt list --upgradable' to see them.
peter@C-3PO:~$
peter@C-3PO:~$
peter@C-3PO:~$
peter@C-3PO:~$ sudo apt install -y apt-transport-https gnupg2 curl
Reading package lists... Done
Building dependency tree       
Reading state information... Done
curl is already the newest version (7.68.0-1ubuntu2.2).
apt-transport-https is already the newest version (2.0.2ubuntu0.1).
The following packages were automatically installed and are no longer required:
  libfprint-2-tod1 libllvm9 libllvm9:i386
Use 'sudo apt autoremove' to remove them.
The following NEW packages will be installed
  gnupg2
0 to upgrade, 1 to newly install, 0 to remove and 12 not to upgrade.
Need to get 5,316 B of archives.
After this operation, 51.2 kB of additional disk space will be used.
Get:1 http://gb.archive.ubuntu.com/ubuntu focal/universe amd64 gnupg2 all 2.2.19-3ubuntu2 [5,316 B]
Fetched 5,316 B in 0s (51.1 kB/s)  
Selecting previously unselected package gnupg2.
(Reading database ... 242216 files and directories currently installed.)
Preparing to unpack .../gnupg2_2.2.19-3ubuntu2_all.deb ...
Unpacking gnupg2 (2.2.19-3ubuntu2) ...
Setting up gnupg2 (2.2.19-3ubuntu2) ...
Processing triggers for man-db (2.9.1-1) ...
peter@C-3PO:~$ 
peter@C-3PO:~$ 
peter@C-3PO:~$ echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
peter@C-3PO:~$
peter@C-3PO:~$
peter@C-3PO:~$
peter@C-3PO:~$ sudo apt update
Hit:1 http://gb.archive.ubuntu.com/ubuntu focal InRelease
Hit:2 http://security.ubuntu.com/ubuntu focal-security InRelease                                    
Hit:3 http://gb.archive.ubuntu.com/ubuntu focal-updates InRelease                                   
Hit:4 http://gb.archive.ubuntu.com/ubuntu focal-backports InRelease                                 
Get:5 https://download.docker.com/linux/ubuntu focal InRelease [36.2 kB]                            
Hit:6 http://ppa.launchpad.net/lutris-team/lutris/ubuntu focal InRelease                       
Hit:8 http://ppa.launchpad.net/obsproject/obs-studio/ubuntu focal InRelease
Get:7 https://packages.cloud.google.com/apt kubernetes-xenial InRelease [8,993 B]
Get:9 https://packages.cloud.google.com/apt kubernetes-xenial/main amd64 Packages [41.3 kB]
Fetched 86.4 kB in 1s (63.4 kB/s)
Reading package lists... Done
Building dependency tree       
Reading state information... Done
12 packages can be upgraded. Run 'apt list --upgradable' to see them.
peter@C-3PO:~$
peter@C-3PO:~$
peter@C-3PO:~$
peter@C-3PO:~$ sudo apt install -y kubectl
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following packages were automatically installed and are no longer required:
  libfprint-2-tod1 libllvm9 libllvm9:i386
Use 'sudo apt autoremove' to remove them.
The following NEW packages will be installed
  kubectl
0 to upgrade, 1 to newly install, 0 to remove and 12 not to upgrade.
Need to get 8,350 kB of archives.
After this operation, 43.0 MB of additional disk space will be used.
Get:1 https://packages.cloud.google.com/apt kubernetes-xenial/main amd64 kubectl amd64 1.19.3-00 [8,350 kB]
Fetched 8,350 kB in 3s (2,975 kB/s)  
Selecting previously unselected package kubectl.
(Reading database ... 242222 files and directories currently installed.)
Preparing to unpack .../kubectl_1.19.3-00_amd64.deb ...
Unpacking kubectl (1.19.3-00) ...
Setting up kubectl (1.19.3-00) ...
peter@C-3PO:~$ 
peter@C-3PO:~$ 
peter@C-3PO:~$ kubectl version
Client Version: version.Info{Major:"1", Minor:"19", GitVersion:"v1.19.3", GitCommit:"1e11e4a2108024935ecfcb2912226cedeafd99df", GitTreeState:"clean", BuildDate:"2020-10-14T12:50:19Z", GoVersion:"go1.15.2", Compiler:"gc", Platform:"linux/amd64"}
The connection to the server localhost:8080 was refused - did you specify the right host or port?
peter@C-3PO:~$ 
```

...so easy!

OK, now let's see if i can connect to my raspi.

I read this page first on cluster-access. https://rancher.com/docs/k3s/latest/en/cluster-access/

... I need to copy the KUBECONFIG file across?
Can't I run a `kubectl login` command?

Lets dig...
well, it seems that `kubectl` doesn't have a `login` command? That is an `oc` only thing? Wow. I can see why k3s doco just says to copy the kubeconfig across.

```bash
peter@C-3PO:~$ scp pi@192.168.0.102:/etc/rancher/k3s/k3s.yaml ~/.kube/config
pi@192.168.0.102's password: 
/home/peter/.kube/config: No such file or directory
peter@C-3PO:~$ mkdir ~/.kube
peter@C-3PO:~$ scp pi@192.168.0.102:/etc/rancher/k3s/k3s.yaml ~/.kube/config
pi@192.168.0.102's password: 
k3s.yaml                                                           100% 1052     1.1MB/s   00:00    
peter@C-3PO:~$ 
```
I added the IP of my k3s into the kubeconfig file.
```bash
peter@C-3PO:~$ vi ~/.kube/config
```
...and then to retest -
```bash
peter@C-3PO:~$ kubectl version
Client Version: version.Info{Major:"1", Minor:"19", GitVersion:"v1.19.3", GitCommit:"1e11e4a2108024935ecfcb2912226cedeafd99df", GitTreeState:"clean", BuildDate:"2020-10-14T12:50:19Z", GoVersion:"go1.15.2", Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"18", GitVersion:"v1.18.9+k3s1", GitCommit:"630bebf94b9dce6b8cd3d402644ed023b3af8f90", GitTreeState:"clean", BuildDate:"2020-09-17T19:04:57Z", GoVersion:"go1.13.15", Compiler:"gc", Platform:"linux/arm"}
peter@C-3PO:~$ 
```

All done!
