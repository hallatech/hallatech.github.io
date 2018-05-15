---
layout: "post"
title: "Creating localhost Oracle XE Instances on EC2"
categories: oracle-xe EC2
---

(Note: I'm ignoring the use of RDS in this issue)

When an IP changes on a restart (not reboot) of a shutdown instance or a launch of a new instance from an AMI, there can be access problems to an original Oracle XE installation.

In the basic install process of XE the following 2 files are created with the internal IP address of the EC2 instance.

`/u01/app/oracle/product/11.2.0/xe/network/admin/listener.ora`

`/u01/app/oracle/product/11.2.0/xe/network/admin/tnsnames.ora`

On restart they refer to the old internal IP address. If creating an AMI I would normally convert these to use localhost instead. I'm not intending to access the database from a remote instance as its an all-in-one installation.

Switch to the oracle user:

`sudo su - oracle`

Ensure the oracle user has its environment set up correctly. Add the following to the oracle user's .bash_profile :

`source /u01/app/oracle/product/11.2.0/xe/bin/oracle_env.sh`

Shutdown the listener:

`lsnrctl stop`

Update the database configuration:

```
sqlplus sys/password as sydba 
alter system set LOCAL_LISTENER="(ADDRESS=(PROTOCOL=TCP)(HOST=localhost)(PORT=1521))"; 
alter system register; 
shutdown immediate 
exit
```

Edit each of `listener.ora` and `tnsnames.ora` and change the IP address to localhost.

Restart the listener

`lsnrctl start`

Restart the database

```sqlplus sys/password as sydba
startup
exit```

Check the listener state

`lsnrctl status`

```
Listening Endpoints Summary...
(DESCRIPTION=(ADDRESS=(PROTOCOL=ipc)(KEY=EXTPROC_FOR_XE)))
(DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=127.0.0.1)(PORT=1521)))
(DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=ip-xxx-xx-xx-xxx.eu-west-1.compute.internal)(PORT=8070))(Presentation=HTTP)(Session=RAW))
Services Summary...
Service "PLSExtProc" has 1 instance(s).
Instance "PLSExtProc", status UNKNOWN, has 1 handler(s) for this service...
Service "XE" has 1 instance(s).
Instance "XE", status READY, has 1 handler(s) for this service...
Service "XEXDB" has 1 instance(s).
Instance "XE", status READY, has 1 handler(s) for this service...
The command completed successfully
```

Test connecting through the listener. Both examples should now work which means that the JDBC access should work as well.

`sqlplus system/password@XE`

`sqlplus system/password@//localhost:1521/XE`

Now on restarts or AMI burning and launch Oracle XE access should work everytime. Oracle is already installed as a service by default on installation so it will just startup as required.

You can also confirm the listener and its port by running

`ps -fu oracle | grep lsnr | grep -v grep`

Find the process id (pid) and use that as input to

`lsof -ni -p <pid>`

