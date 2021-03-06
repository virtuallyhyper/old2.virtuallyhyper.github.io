---
author: karim
comments: true
date: 2012-08-30 01:05:09+00:00
layout: post
slug: psod-failed-at-vmkdriverssrc_9vmklinux_9vmwarelinux_scsi-c2221-not-reached
title: PSOD Failed at vmkdrivers/src_9/vmklinux_9/vmware/linux_scsi.c:2221 — NOT REACHED
wordpress_id: 3078
categories:
- VMware
following_users:
- jarret
- kelatov
tags:
- esxcfg-info
- IBM x3690 X5
- LSI2008
- mpt2sas
- PSOD
---

I recently ran into the following PSOD:

[![](http://virtuallyhyper.com/wp-content/uploads/2012/08/psod-lsi-.png)](http://virtuallyhyper.com/wp-content/uploads/2012/08/psod-lsi-.png)

Some other people in the VMware communities have seen the same PSOD. Here is a [link](http://communities.vmware.com/message/2102510) to the communities page. We are crashing at:


```

Failed at vmkdrivers/src_9/vmklinux_9/vmware/linux_scsi.c:2221 -- NOT REACHED

```


It looks like the local disk driver is having an issue. Checking out the host's local controllers, I saw the following:


```

~ # lspci | grep vmhba
000:000:31.2 Mass storage controller: Intel Corporation ICH10 4 port SATA IDE Controller [vmhba0]
000:004:00.0 Mass storage controller: LSI Logic / Symbios Logic LSI2008 [vmhba1]
000:014:00.0 Mass storage controller: LSI Logic / Symbios Logic MegaRAID SAS SKINNY Controller [vmhba2]
000:019:00.0 Mass storage controller: LSI Logic / Symbios Logic LSI2008 [vmhba3]

```


Checking for the device drivers and the Vendor IDs, I see the following:


```

~ # esxcfg-info -a | sed -n '/SCSI\ Interface/,/BAR\ Info/p' | grep 'Name.*vmhba1' -A 16 | egrep 'Name|Driver|Vendor\ Id|Device\ Id|Sub-Vendor\ Id|Sub-Device\ Id'
               |----Name............................................vmhba1
               |----Driver..........................................mpt2sas
                  |----Vendor Id....................................0x1000
                  |----Device Id....................................0x0072
                  |----Sub-Vendor Id................................0x1014
                  |----Sub-Device Id................................0x03ca

```


vmhba3 was the same as above, and here is what I saw for vmhba2:


```

~ # esxcfg-info -a | sed -n '/SCSI\ Interface/,/BAR\ Info/p' | grep 'Name.*vmhba2' -A 16 | egrep 'Name|Driver|Vendor\ Id|Device\ Id|Sub-Vendor\ Id|Sub-Device\ Id'
               |----Name............................................vmhba2
               |----Driver..........................................megaraid_sas
                  |----Vendor Id....................................0x1000
                  |----Device Id....................................0x0073
                  |----Sub-Vendor Id................................0x1014
                  |----Sub-Device Id................................0x03b1

```


So we are using the mpt2sas and megaraid_sas drivers for our local drives. Here is HCL [link](http://www.vmware.com/resources/compatibility/detail.php?deviceCategory=io&productid=13971&deviceCategory=io&VID=1000&DID=0072&SVID=0000&SSID=0000&page=1&display_interval=10&sortColumn=Partner&sortOrder=Asc) for vmhba1 and here is the [link](http://partnerweb.vmware.com/comp_guide2/detail.php?deviceCategory=io&productid=12373&deviceCategory=io&VID=1000&DID=0073&SVID=1014&SSID=03b1&page=1&display_interval=10&sortColumn=Partner&sortOrder=Asc) for vmhba2.

For vmhba1:

[![](http://virtuallyhyper.com/wp-content/uploads/2012/08/hcl-vmhba1.png)](http://virtuallyhyper.com/wp-content/uploads/2012/08/hcl-vmhba1.png)

and for vmhba2:

[![](http://virtuallyhyper.com/wp-content/uploads/2012/08/hcl-vmhba2.png)](http://virtuallyhyper.com/wp-content/uploads/2012/08/hcl-vmhba2.png)

Here is our hardware and VMware versions:


```

~ # vmware -l
VMware ESXi 5.0.0 GA

~ # $ vsish -e cat /hardware/bios/dmiInfo | head -3
System Information (type1) {
   Product Name:System x3690 X5 -[7147AC1]-
   Vendor Name:IBM

~ # $ vsish -e cat /hardware/bios/biosInfo
BIOS Information (type 0) {
   BIOS Version:-[MLE170CUS-1.70]-
   BIOS Release Date:09/23/2011
}

```


So we are running ESXi 5.0GA on a IBM x3690 X5 host. Very close to the same guy from [this](http://communities.vmware.com/message/2101833) VMware communities page. Now from the HCL we can see that for vmhba2 the recommended version of the driver is:


> megaraid_sas version 4.32-1vmw


Now checking the version on the host, I saw the following:


```

~ # esxcli software vib list | grep megaraid-sas
scsi-megaraid-sas     4.32-1vmw.500.1.11.469512           VMware  VMwareCertified   2012-02-28

```


The recommended version for vmhba1 and vmhba3 is :


> mpt2sas version 06.00.00.00-5vmw


Checking the current version, I saw the following:


```

~ # esxcli software vib list | grep mpt2sas
scsi-mpt2sas          06.00.00.00-6vmw.500.1.11.469512    VMware  VMwareCertified   2012-02-28

```


So we were at the latest version for both. Searching for the available mpt2sas drivers from the VMware downloads page, I saw the following:

[![](http://virtuallyhyper.com/wp-content/uploads/2012/08/mp2sas-drivers.png)](http://virtuallyhyper.com/wp-content/uploads/2012/08/mp2sas-drivers.png)

So these version of the drivers are available:


> 10.10.10.01.vmw
10.10.21.00.1vmw
11.00.00.00.2vmw
12.00.00.00.1vmw
13.00.00.00.1vmw
13.10.02.00.1vmw
14.00.00.00.1vmw


Even though version "13.10.02.00.1vmw" wasn't on HCL for this device, we went ahead and installed it on our hardware and the PSODs haven't happened for over two weeks now. Here is the [link](https://my.vmware.com/web/vmware/details?downloadGroup=DT-ESXI50-LSI-mpt2sas-131002001vmw&productId=24) to that driver. Hopefully this helps someone out.
