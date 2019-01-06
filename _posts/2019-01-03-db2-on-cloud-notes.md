---
layout: post
title:  "Some Notes on Setting-up DB2 on Cloud, on IBM Cloud"
---

# How to Provision DB2 in the Cloud

_A few people in my team have asked me for some notes on using "DB2 on Cloud" on "IBM Cloud"(Bluemix). I've replicated my workflow below. Do bare in mind that because of my job-role, my experience of IBM Cloud might be different to yours (I get more freebies)._

1. Log into Bluemix (IBM Cloud)
https://console.ng.bluemix.net/

2. Click Catalog.

3. Scroll down to DB2

4. Click the panel:

```
DB2
Lite . IBM
A next generation SQL database. Formerly DashDB For Transactions.
```

5. Give the provisioned DB2-on-Cloud a name, choose a region, organization, space, select the Lite (Free) offering.

That's it.

# How to manage (or at least get started) with DB2 in the Cloud

1. Navigate to the Bluemix Dashboard:
https://console.bluemix.net/dashboard/apps

2. Click on your DB2, in the table under Cloud Foundry Services

_Notice that in the sidebar you have 3 options: Manage, Service Credentials, and Connections._

3. Click Manage, and then click the `Open Console` button to reach the DB2 on Cloud GUI.

4. Click the Hamburger menu in the top left, and click `EXPLORE`.

Note that in the `EXPLORE` view you can drill down through schemas, to tables, to individual columns and stuff. Note that you _can_ create tables, and customize rows and schemas and stuff in this view, but it gets a bit cumbersome doing database heavy-lifting through a point-and-click GUI. You're better writing script and then loading it.

5. Click on the Hamburger Menu again, and click `RUN SQL`.

6. Type or paste in your SQL to create and define all your tables, rows, columns, etc.

Once the SQL script has been run, you can then review your tables and bits and bobs in the `EXPLORE` view.

# How to then connect to the DB2 Instance

1. Navigate to the Manage/Service Credentials/Connections pane.

2. Click `Service Credentials`.

3. Click on the View Credentials `v` on any of the credentials to be given a block of JSON that describes all the settings needed to connect to the DB.
```
{
  "hostname": "mydbhostname.services.eu-gb.bluemix.net",
  "password": "mygobbledegookpassword",
  "https_url": "https://mydbhostname.services.eu-gb.bluemix.net:8443",
  "port": 50000,
  "ssldsn": "DATABASE=BLUDB;HOSTNAME=mydbhostname.services.eu-gb.bluemix.net;PORT=50001;PROTOCOL=TCPIP;UID=mydbusername;PWD=mygobbledegookpassword;Security=SSL;",
  "host": "mydbhostname.services.eu-gb.bluemix.net",
  "jdbcurl": "jdbc:db2://mydbhostname.services.eu-gb.bluemix.net:50000/BLUDB",
  "uri": "db2://mydbusername:mygobbledegookpassword@dmydbhostname.services.eu-gb.bluemix.net:50000/BLUDB",
  "db": "BLUDB",
  "dsn": "DATABASE=BLUDB;HOSTNAME=mydbhostname.services.eu-gb.bluemix.net;PORT=50000;PROTOCOL=TCPIP;UID=mydbusername;PWD=mygobbledegookpassword;",
  "username": "mydbusername",
  "ssljdbcurl": "jdbc:db2://mydbhostname.services.eu-gb.bluemix.net:50001/BLUDB:sslConnection=true;"
}
```

4. Copy-and-Paste the `dsn` field into your application.