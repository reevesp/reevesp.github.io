---
layout: post
title:  "DataPower SSH Service not starting on Docker"
---
Another day, another DataPower brainfart. Only a small one though.

I was starting a DataPower docker container from scratch. I had just pulled down the image from the ether/dockerhub and had just `run` my container.

```
mkdir dp-docker
cd dp-docker
# Just give me a blank DP runtime to hack about that I can trash
docker run -it    -v $PWD/config:/drouter/config    -v $PWD/local:/drouter/local    -e DATAPOWER_ACCEPT_LICENSE=true    -e DATAPOWER_INTERACTIVE=true    -p 9090:9090    -p 9022:22    -p 5554:5554    -p 8000-8010:8000-8010    --name idg    ibmcom/datapower
#Note: like everyone else, I just reuse Harley's one-liner which I nicked ages ago from the DP playground article
# https://developer.ibm.com/datapower/docker/
#You can, of course, tailor this command to your needs to give you the DP docker
# container that you want, there's a very good explanation here
# https://www.ibm.com/support/knowledgecenter/en/SS9H2Y_7.5.0/com.ibm.dp.doc/virtual_runningdockerimage.html
# and here
# https://www.ibm.com/support/knowledgecenter/en/SS9H2Y_7.5.0/com.ibm.dp.doc/virtual_fordocker.html
```

I could not bring the SSH service up on the DataPower.

```
$ docker attach idg

idg#
idg# co
Global configuration mode
idg(config)# ssh 0.0.0.0 22

%	Pending

SSH service listener enabled

idg(config)# 20180912T135118.536Z [0x00350015][mgmt][notice] ssh(SSH Service): tid(111): Operational state down
20180912T135118.536Z [0x8100003f][mgmt][notice] domain(default): tid(111): Domain configuration has been modified.
#
# About 5 mins of pulling out my hair while trying to connect from other
# terminals has been snipped from here
#
idg(config)# test tcp-connection 127.0.0.1 22

% TCP connection to "127.0.0.1 port 22" failed (connection refused)


idg(config)# 20180912T135136.101Z [0x810002d4][cli][error] : tid(207): TCP connection to "127.0.0.1 port 22" failed (connection refused)

```

I checked that the docker container had got its port mappings done correctly. It had. I checked DP's logs, and didn't glean anything there either.

A single cursory Google brought me to Aidan Harbison's post on developer.ibm.com:
https://developer.ibm.com/answers/questions/394482/ssh-service-on-new-datapower-docker-container-will/

To quote Aidan's answer:

>One of the changes in 7.6 is that by default, the DataPower Gateway process runs as the non-root drouter user inside the Docker container. Because of this, DataPower does not have permissions to use privileged ports.  
> Port 22 is the default port for the SSH service, and is among the privileged port range. You will need to change the port in the SSH service configuration to something outside of the privileged port range (>1024) to bring the SSH service up.
