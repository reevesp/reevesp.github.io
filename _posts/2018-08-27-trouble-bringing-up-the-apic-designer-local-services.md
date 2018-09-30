---
layout: post
title:  "Trouble Bringing up the APIC Designer's Local Services"
---

# Trouble bringing up the apic designer local services

I was doing some more work in the APIC Toolkit today.

After building some assemblies as part of my api (to be deployed on the DataPower gateway), I clicked the little “play” icon to start the locally running service.

Instead of being greeted with the “Running” declaration at the bottom of the window, I was presented with an angry `“Error Starting Application”`.`

```
Service apic-adventures-gw started but did not initialize within the timeout period. Dumping log buffer.
2018-09-11T19:46:40.669Z pid:13369 worker:0 WARN received SIGTERM, shutting down
2018-09-11T19:46:40.669Z pid:13369 worker:0 INFO supervisor size set to undefined
2018-09-11T19:46:40.669Z pid:13369 worker:0 INFO supervisor stopped
```

Some googling later…
(_NB- I still do not understand what was happening, so my steps below are technically the wavings-of-a-dead-chicken…_)
I came across this developerworks thread.
https://www.ibm.com/developerworks/community/forums/html/topic?id=1c82d091-de7e-4c3b-874b-87ecc1e69212

Two hints I got from this thread were-
1. You might be low on memory.
2. The quick fix seems to be to bounce everything.

First, I checked that I had sufficient free resources on my host OS, and closed some extraneous programs.

Then, I did the “official” bouncing of the toolkit, using `apic services:stop` from the directory that I was running the toolkit. And then `apic services:start` after a little while.

Most of the time now when the Toolkit services fail to start for me, bouncing the services has cleared up the matter.
In fact, I believe that `apic services` is a much more reliable method for managing the toolkit’s local hosting of apis, compared to the API Designer’s web interface.

###### However...

Sometimes, this "more-graceful" bounce doesn’t resolve the issue, so I will have to try the less-graceful option.

I find my `apic edit` command in the terminal, and `Ctrl+C` it to kill it.

I find the docker containers that the Toolkit was using by running `docker ps -a` and looking for the containers that were running from the directory. Then I stop and delete those containers.
(`docker stop <container ID>`, `docker rm <container ID>`)

```
Peters-MacBook-Pro:~ peterreeves$ docker ps -a
CONTAINER ID        IMAGE                                                      COMMAND                CREATED             STATUS                      PORTS                                                                                             NAMES
85ab376c6289        ibm-apiconnect-toolkit/datapower-api-gateway:1.1.10        "/bin/drouter"         About an hour ago   Up About an hour            0.0.0.0:32890->80/tcp, 0.0.0.0:32889->5554/tcp, 0.0.0.0:32888->9090/tcp, 0.0.0.0:4001->9443/tcp   apic-adventures_datapower-api-gateway_1
0f5332270b8b        ibm-apiconnect-toolkit/datapower-mgmt-server-lite:1.1.10   "node lib/server.js"   About an hour ago   Up About an hour            0.0.0.0:32887->2443/tcp                                                                           apic-adventures_datapower-mgmt-server-lite_1
b2b52279562c        ibmcom/datapower                                           "/start.sh"            3 weeks ago         Exited (0) 41 minutes ago                                                                                                     idg
Peters-MacBook-Pro:~ peterreeves$ docker rm c0e4ba3b7adb 7015094aed06
c0e4ba3b7adb
7015094aed06
```

Then, I wait a while.
I check that apic services is not saying any services are running.

Then, start it all up with `apic edit` again.

I won’t deny that this seems like a bit of a bodge- so I probably won't recommend this to anyone. This is something for me to look at in the future.
