---
layout: post
title:  "Using DBeaver as a tool to debug Data Issues on DB2"
---

I was recently working on debugging a basic service that inserts and deletes data from a DB, and I wanted a nice GUI to see everything. A colleague in my team recommended that I try out *DBeaver*, and I found it to be pretty good for what I wanted to do.

## Installing DBeaver

Installing was dead-easy. DBeaver was available as a Brew cask, so I was able to `brew cask install dbeaver-community`.

## Connecting DBeaver to DB2

Connecting to DB2 was slighty more fiddly, as the db2 driver does not come bundled with DBeaver by default (It seems that the hardest part of using DB2 is making sure the driver is present on a client machine...). Along with the DB2 driver, I also needed the DB connection details (which I already had in a DSN string).

### Getting the DB2 Drivers

First, I found it pretty misleading how DBeaver and certain docs asked for `db2jcc4.jar` which I was never able to find.

I found this link on google [https://www-01.ibm.com/support/docview.wss?uid=swg21363866](https://www-01.ibm.com/support/docview.wss?uid=swg21363866) and from there I selected the latest and highest version of the DB2 driver which was marked GA (which for me was `v11.5 FP0 (GA)`).

And then I had to click through the IBM Downloads machine...

- I was brought to this link [https://www-01.ibm.com/support/docview.wss?uid=swg21385217](https://www-01.ibm.com/support/docview.wss?uid=swg21385217).
- I logged into my IBM account with SSO.
- I selected the `IBM Data Server Driver for JDBC and SQLJ (JCC Driver)` option to download.
- I Clicked through to the download page, where I pulled down the file `db2_db2driver_for_jdbc_sqlj_v11.5.tar.gz`.
- I unzipped this file to get the file `db2_db2driver_for_jdbc_sqlj.zip`.

... and this was the file with my DB2 drivers!

## Putting the Connection Object together in DBeaver

So, back in the DBeaver GUI, and starting from my dsn connection string:

`"DATABASE=BLUDB;HOSTNAME=a-big-funny-looking-bluemix-hostname-with-all-these-subdomains.services.eu-gb.bluemix.net;PORT=50000;PROTOCOL=TCPIP;UID=mydbuser;PWD=MyDbPassword;"`

- From the GUI, click "New Database Connection" > "DB2 LUW"

- In the dialog box, paste the sections from the DSN string into the relevant fields:
    - in the `Host:` field, paste the "HOSTNAME="
    - in the `Database:` field, paste the "DATABASE="
    - in the `Username:` field, paste the "UID="
    - in the `Password:` field, paste the "PWD="

- Click "Edit Driver Settings:"

- Click "Add File" > Navigate to `db2_db2driver_for_jdbc_sqlj.zip`

- Click "OK", Click "Test Connection".. and that was it!

From then, DBeaver was exactly what I wanted- it gave me an "Eclipse-like" navigation-pane with a directory/tree-structure to nose through for schemas/tables.

And after 2 minutes of clicking around, I was able to figure it all out.