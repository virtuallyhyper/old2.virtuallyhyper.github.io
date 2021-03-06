---
author: jarret
comments: true
date: 2012-08-14 00:25:32+00:00
layout: post
slug: srm-5-vrms-change-username-to-connect-to-vc
title: Changing the User and Password that VRMS Uses to Connect to vCenter
wordpress_id: 1516
categories:
- SRM
- VMware
- vTip
following_users:
- jarret
- kelatov
tags:
- command line
- python
- SQL
- srm
- VRMS
- vSphere Replication
---

A little while ago I had a customer that had accidentally changed the password for the user VRMS used to connect to vCenter. Since the VRMS could not connect to vCenter, all of the replication had stopped. There is no easy way to fix this, but we hacked away at the database and got the VRMS to connect back the vCenter.

**NOTE:** I am sure this is not supported by VMware. Please use at your own risk.

The basic steps are below.



	
  1. Encode the new password

	
  2. Update the hms-localvc-password and hms-localvc-user in the /opt/vmware/hms/conf/hms-configuration.xml

	
  3. Change the user and password in the ConfigEntryEntity table in the database

	
  4. Restart the VRMS


The first thing we need to do is encode the password to put it in the database. I dug through the web interface (VAMI) to find the cgi script that encodes the password in the database. In /opt/vmware/share/htdocs/service/hms/cgi/commands.py we can see how the password is saved. We can encode the password using the same mechanism.


```

# python -c "import base64; print(base64.b64encode('virtuallyhyper'))"
dmlydHVhbGx5aHlwZXI=

```


Now we should update the values in the hms-configuration.xml file. Use the password encoded in the last step and the new username to update the following lines in the /opt/vmware/hms/conf/hms-configuration.xml on the VRMS.


```

   srmsvc
   dmlydHVhbGx5aHlwZXI=

```


Now we should open up the database and check out the columns that we need to update. Below is a select command that will pull the key and value pairs for the configuration.


```

SELECT configKey,configValue FROM [srm51hbr-vrms-prod].[dbo].[ConfigEntryEntity]

```


Now it is time to update the username and password for the localvc user.


```

UPDATE [srm51hbr-vrms-prod].[dbo].[ConfigEntryEntity] SET configValue = 'srmsvc' WHERE configKey = 'hms-localvc-user'
UPDATE [srm51hbr-vrms-prod].[dbo].[ConfigEntryEntity] SET configValue = 'dmlydHVhbGx5aHlwZXI=' WHERE configKey = 'hms-localvc-password'

```


Let's do a select to see what the new values are.


```

SELECT configKey,configValue FROM [srm51hbr-vrms-prod].[dbo].[ConfigEntryEntity] WHERE configKey IN ('hms-localvc-password', 'hms-localvc-user')
configKey	configValue
hms-localvc-password	dmlydHVhbGx5aHlwZXI=
hms-localvc-user	srmsvc

```


Now that we have updated the configuration files and the database, we can restart the service to take the new changes.


```

# /etc/init.d/hms restart
Using default configuration at /opt/vmware/hms/conf/hms-configuration.xml

```


If everything was done correctly, the VRMS should connect back to vCenter and continue working. If there are problems, you will want to reconfigure connection between VRMS and reregister the VRS.
