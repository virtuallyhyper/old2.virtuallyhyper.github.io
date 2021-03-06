---
author: jarret
comments: true
date: 2012-04-17 01:22:11+00:00
layout: post
slug: cloning-a-vmdk-while-the-vm-is-powered-on
title: Cloning a VMDK While the VM is Powered On
wordpress_id: 850
categories:
- Storage
- VMware
- vTip
ef_usergroup:
- JJK
following_users:
- jarret
- joechan
- kelatov
tags:
- 00000000-00000000-0000-000000000000
- clone
- command line
- mode 2
- snapshots
- vmdk
- vmfs locks
- vtip
---

We can clone VMDKs that we can get a read only lock on. When a VM is actively writing to a VMDK, it needs the lock to be be read-write, which is an exclusive lock. If we wanted to make a clone of a VMDK from the command line while a VM was running we could take a snapshot and clone the base disk or one of the snapshots that it is not actively using. The downside is that new data written to the active snapshot will not be transferred to the new disk. This is the same process that cloning the VM through the vSphere Client will do, but on a single disk.

In the example below I have taken a snapshot of a running VM.

Ensure that the VM is not running on the base disk.


```
# grep vmdk vps.vmx
scsi0:0.fileName = "vps-000001.vmdk"
```


Check the base disk to see if it has a RW (exclusive) lock on the disk we want to clone. We can see that there is a single RO lock, but there is no owner to a RW lock (indicated by the 00000000-00000000-0000-000000000000).


```
# vmkfstools -D vps-flat.vmdk
Lock [type 10c00001 offset 133255168 v 1775, hb offset 3907584
gen 7633, mode 2, owner 00000000-00000000-0000-000000000000 mtime 94066 nHld 1 nOvf 0]
RO Owner[0] HB Offset 3907584 4f7b1c42-4b58f6dd-3e59-bcaec58ad83d
Addr <4, 309, 2>, gen 1746, links 1, type reg, flags 0, uid 0, gid 0, mode 600
len 10737418240, nb 1253 tbz 0, cow 0, newSinceEpoch 0, zla 3, bs 1048576
```


Since the base VMDK does not have an exclusive lock on it, we can clone it.


```
# vmkfstools -d thin -i vps.vmdk vps.running.vmdk
Destination disk format: VMFS thin-provisioned
Cloning disk 'vps.vmdk'...
Clone: 100% done.

```

