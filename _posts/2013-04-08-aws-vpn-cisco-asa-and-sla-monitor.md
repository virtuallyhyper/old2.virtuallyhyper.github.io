---
author: joe
comments: true
date: 2013-04-08 19:16:59+00:00
layout: post
slug: aws-vpn-cisco-asa-and-sla-monitor
title: AWS VPN Cisco ASA and SLA Monitor
wordpress_id: 7723
categories:
- AWS
following_users:
- jarret
- joechan
- kelatov
tags:
- aws
- Cisco ASA
- IPSec
---

When configuring the AWS VPC VPN with a Cisco ASA, Amazon recommends that you configure SLA monitoring.





In the pregenerated Cisco ASA configuration downloaded from the AWS VPN Management console (In your AWS VPC Management Console, click on **VPN Connections**, Right Click on your VPN connection, and click **Download Configuration**), you'll see this:




    

```
    access-list acl-amzn extended permit ip any host 169.254.249.25
    
```






followed by...




    

```
    ! SLA monitoring keeps the tunnels in an active state. The monitor is created
    ! as #2, which may conflict with an existing monitor using the same
    ! number. If so, we recommend changing the sequence number to avoid conflicts.
    sla monitor 2
       type echo protocol ipIcmpEcho 169.254.249.25 interface outside_interface
       frequency 5
    exit
    sla monitor schedule 2 life forever start-time now
    
    ! In order to monitor the tunnels using "sla monitor" the firewall must allow icmp packets
    icmp permit any outside_interface
    
```






Per the [Troubleshooting Cisco ASA Customer Gateway Connectivity doc](http://docs.aws.amazon.com/AmazonVPC/latest/NetworkAdminGuide/Cisco_ASA_Troubleshooting.html), this is to keep the IPsec tunnel active:





> 
  
> 
> In Cisco ASA, the IPsec will only come up after "interesting traffic" is sent. To always keep the IPsec active, we recommend configuring SLA monitor. SLA monitor will continue to send interesting traffic, keeping the IPsec active.
> 
> 






# Why do AWS docs suggest to use the SLA monitor?





From [this doc](http://www.cisco.com/en/US/docs/ios/12_4/ip_sla/configuration/guide/hsicmp.html), the SLA Monitor feature doesn't appear to be designed specifically to keep IPsec tunnels up, but the attributes in _bold_ below seem to make it a perfect tool of choice (rather than setting up another device specifically for this purpose).





> 
  
> 
> IP SLAs uses active traffic monitoring—**the generation of traffic in a continuous, reliable, and predictable manner**—for measuring network performance
> 
> 






# What is this `169.254.x.x` address?





From the configuration above, `169.254.249.25` is the VPN endpoint's inside address (and can vary from different AWS VPN ID's, check your pregenerated configuration for these IP addresses) and is something should always be up if your VPN endpoint is up and accessible.





Note: You could also probably get away with using the VPC default gateway at the base of the VPC network range "plus one" as well. For example, the default gateway on `10.0.0.0/16` network is located at `10.0.0.1`.





So this SLA monitor is set up to keep the IPSec tunnel up by sending _interesting traffic_ to `169.254.249.25`.





# What is considered _interesting traffic_?





From this [Cisco press article](http://www.ciscopress.com/articles/article.asp?p=24833&seqNum=6), it's traffic that matches the ACL applied to the crypto map:





> 
  
> 
> Step 1: Interesting traffic initiates the IPSec process—Traffic is deemed interesting when the IPSec security policy configured in the IPSec peers starts the IKE process.
> 
> 
  
  
> 
> #### Step 1: Defining Interesting Traffic
> 
> 
  
  
> 
> Determining what type of traffic is deemed interesting is part of formulating a security policy for use of a VPN. The policy is then implemented in the configuration interface for each particular IPSec peer. For example, in Cisco routers and PIX Firewalls, access lists are used to determine the traffic to encrypt. The access lists are assigned to a crypto policy such that permit statements indicate that the selected traffic must be encrypted, and deny statements can be used to indicate that the selected traffic must be sent unencrypted. With the Cisco Secure VPN Client, you use menu windows to select connections to be secured by IPSec. When interesting traffic is generated or transits the IPSec client, the client initiates the next step in the process, negotiating an IKE phase one exchange.
> 
> 






On the ASA, it's comes from lines like this in the config:




    

```
     crypto map amzn-vpn-map 1 match address acl-amzn
     access-list acl-amzn extended permit ip any host 169.254.249.25
     access-list acl-amzn extended permit ip any 10.0.0.0 255.255.0.0
    
```




