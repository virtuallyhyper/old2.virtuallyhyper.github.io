---
author: karim
comments: true
date: 2013-04-12 14:11:21+00:00
layout: post
slug: deploy-an-amazon-ec2-instance-in-the-free-usage-tier
title: Deploy an Amazon EC2 instance in the Free Usage Tier
wordpress_id: 7841
categories:
- AWS
ef_usergroup:
- JJK
following_users:
- jarret
- joechan
- kelatov
tags:
- Amazon Cloud Watch
- aws
- ec2
---

If you want to try out AWS (Amazon Web Services), there is a 12 month (1 year) free trial (Free Usage Tier). Most of the information regarding the Free Usage Tier is outlined in the "[Getting Started Guide AWS Free Usage Tier](http://s3.amazonaws.com/awsdocs/gettingstarted/latest/awsgsg-freetier.pdf)". From the Guide here is a table of all the available services:





[![FUT_AWS](http://virtuallyhyper.com/wp-content/uploads/2013/03/FUT_AWS.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/FUT_AWS.png)





From the same PDF here are some limitations:





[![FUT_Usage_Limits](http://virtuallyhyper.com/wp-content/uploads/2013/03/FUT_Usage_Limits.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/FUT_Usage_Limits.png)





Here is more information from the PDF:





> 
  
> 
> The free usage tier model provides a number of free usage hours per month for these services. For example, the free usage tier pricing model provides 750 usage hours of an Amazon EC2 micro instance per month. (An instance is considered to be running from the time you launch it until the time you terminate it.) You can run one instance continuously for a month, or ten micro instances for 75 hours a month. How you spend your free usage is up to you.
> 
> 
  
  
> 
> In order to stay within the free usage tier, you’ll need to stay under 15 GB of outbound data transfer.
> 
> 






Here is list of the limitation:







  1. 30GB of Space


  2. 2M IOPs 


  3. 15GB Network Throughput


  4. 750 hours of CPU





There are a max of 744 (31 days in month * 24 hours in a day) hours of execution, so only one continuous instance can be deployed. In my example I decided to just use the Amazon EC2 (Elastic Cloud Computing) instance and no other services. But if you don't want a whole VM to play with then other services are available as well.





## Sign up for AWS Free Usage Tier and Check Eligibility





You can go to "**http://aws.amazon.com/**" click on the "Sign Up" button:





[![sign_up_aws](http://virtuallyhyper.com/wp-content/uploads/2013/03/sign_up_aws.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/sign_up_aws.png)





It will ask you for your Credit Card, but don't worry as long as you stay within the limitations you won't be charged. After you signed up you can go to **http://aws.amazon.com/account/**:





[![aws_account](http://virtuallyhyper.com/wp-content/uploads/2013/03/aws_account.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/aws_account.png)





And then Click on "Account Activity" and you will see the following:





[![FUT_Eligible](http://virtuallyhyper.com/wp-content/uploads/2013/03/FUT_Eligible.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/FUT_Eligible.png)





At this point we can deploy an EC2 Instance.





## Deploy an Amazon EC2 Instance





From the Getting Starting Guide:





> 
  
> 
> The easiest way to get started with the Amazon free usage tier is to launch a virtual server, which is referred to as an Amazon EC2 instance. Amazon Elastic Compute Cloud (Amazon EC2) is a powerful component of AWS and central to many cloud-based applications. In the free usage tier, you can launch a micro Amazon EC2 instance. Micro instances provide a small amount of consistent CPU resources and allow you to burst CPU capacity when additional cycles are available. A micro instance is well suited for lower throughput applications and web sites that consume significant compute cycles only occasionally.
> 
> 






Most of the Instructions on how to Deploy an EC2 instance can be found in "[Amazon Elastic Compute Cloud User Guide](http://awsdocs.s3.amazonaws.com/EC2/latest/ec2-ug.pdf)"





To get this started we have go to the "Amazon EC2 console". To get to the console, we first have to go to the "AWS Management Console". Go to **http://aws.amazon.com** and then click on the "My Account/Console" and you will see the following:





[![aws_my_account_console](http://virtuallyhyper.com/wp-content/uploads/2013/03/aws_my_account_console.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/aws_my_account_console.png)





From the Drop Down Menu select "AWS Management Console", and that will bring you to the following page:





[![aws_mgmt_console](http://virtuallyhyper.com/wp-content/uploads/2013/03/aws_mgmt_console.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/aws_mgmt_console.png)





This is the central portal to manage all the Amazon Web Services. From here click on "EC2 Virtual Servers In the Cloud" and that will take you here:





[![EC2_Dashboard](http://virtuallyhyper.com/wp-content/uploads/2013/03/EC2_Dashboard.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/EC2_Dashboard.png)





To create a new EC2 instance click on the "Launch Instance" button:





[![create_instance](http://virtuallyhyper.com/wp-content/uploads/2013/03/create_instance.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/create_instance.png)





You will then be presented with a choice of wizards:





[![New_Instance_Wizard](http://virtuallyhyper.com/wp-content/uploads/2013/03/New_Instance_Wizard.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/New_Instance_Wizard.png)





I just left the "Classic Wizard" selected and clicked on "Continue". At this point you can select what AMI (Amazon Machine Image) to deploy. If there is a yellow star next to the AMI that means you can deploy it in the Free Usage Tier. Here is a list that I saw:





[![select_AMI](http://virtuallyhyper.com/wp-content/uploads/2013/03/select_AMI.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/select_AMI.png)





I selected Ubuntu 12.04 LTS and clicked "Continue" and then I was able to see the Instance Details. I selected the "Instance Type" and we can see that a micro Instance consists of 613MB of RAM and a single Core CPU. Notice the yellow start next to the Micro Instance, indicating that this is available for the Free Usage Tier:





[![Instance_details](http://virtuallyhyper.com/wp-content/uploads/2013/03/Instance_details.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/Instance_details.png)





I left the defaults and clicked "Continue" at which point I saw the "Advanced Instance Details". I left the defaults here as well:





[![adv_inst_opt](http://virtuallyhyper.com/wp-content/uploads/2013/03/adv_inst_opt.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/adv_inst_opt.png)





After clicking "Continue", I was presented with the "Storage Device Configuration" page:





[![storage_dev_conf](http://virtuallyhyper.com/wp-content/uploads/2013/03/storage_dev_conf.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/storage_dev_conf.png)





If you click "Edit" you can configure EBS (Elastic Block Store) Volumes, from the user Guide:





> 
  
> 
> Amazon Elastic Block Store (Amazon EBS) provides block level storage volumes for use with Amazon EC2 instances. Amazon EBS volumes are highly available and reliable storage volumes that can be attached to any running instance in the same Availability Zone. The Amazon EBS volumes attached to an Amazon EBS instance are exposed as storage volumes that persist independently from the life of the instance. With Amazon EBS, you only pay for what you use.
> 
> 
  
  
> 
> Amazon EBS is recommended when data changes frequently and requires long-term persistence. Amazon EBS is particularly well-suited for use as the primary storage for a file system, database, or for any applications that require fine granular updates and access to raw, unformatted block-level storage. Amazon EBS is particularly helpful for database-style applications that frequently encounter many random reads and writes across the data set.
> 
> 






Here are the available options from the Wizard:





[![EBS_Volumes_New_Instance](http://virtuallyhyper.com/wp-content/uploads/2013/03/EBS_Volumes_New_Instance.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/EBS_Volumes_New_Instance.png)





I was only going to run one instance so I didn't add another volume to my VM. You can also change the type of the volume from "Standard" to "Provisioned IOPS". From the User Guide:





> 
  
> 
> To maximize the performance of your I/O-intensive applications, you can use Provisioned IOPS volumes. Provisioned IOPS volumes are designed to meet the needs of I/O-intensive workloads, particularly database workloads, that are sensitive to storage performance and consistency in random access I/O throughput. You specify an IOPS rate when you create the volume, and Amazon EBS provisions that rate for the lifetime of the volume. Amazon EBS currently supports up to 2000 IOPS per volume. You can stripe multiple volumes together to deliver thousands of IOPS per instance to your application.
> 
> 
  
  
> 
> A Provisioned IOPS volume must be at least 10 GB in size. The ratio of IOPS provisioned to the volume size requested can be a maximum of 10. For example, a volume with 1000 IOPS must be at least 100 GB.
> 
> 






I only had 2M IOPS on the Free Usage Tier and I didn't want to dedicate/reserve any amount of IOPS to my volume since I wasn't going to run any IO Intensive Application on my VM. Here is how the options looked like from the Wizard:





[![Provisioned_IOPS_g](http://virtuallyhyper.com/wp-content/uploads/2013/03/Provisioned_IOPS_g.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/Provisioned_IOPS_g.png)





8GB of space for my Ubuntu Test VM was good enough for now, so I just clicked "Continue". At this point I was presented with the "Tags" page. I just defined one tag and that was the "Name" tag like so:





[![Tags_New_Instance](http://virtuallyhyper.com/wp-content/uploads/2013/03/Tags_New_Instance.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/Tags_New_Instance.png)





I then clicked "Continue" and was presented with the "Create Key Pair" page. When you deploy a Linux VM in EC2, an SSH key pair is created so you can use it to login to the VM. I named my key:





[![Create_new_Key_Pair](http://virtuallyhyper.com/wp-content/uploads/2013/03/Create_new_Key_Pair.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/Create_new_Key_Pair.png)





I then clicked "Create and Download Key" and the key was downloaded to my download directory:




    

```
    elatov@crbook:~$ ls downloads/*.pem
    downloads/Ubuntu_VM.pem
    
```






This was just the private key of the SSH Key pair:




    

```
    elatov@crbook:~$ head -1 downloads/Ubuntu_VM.pem && tail -1 downloads/Ubuntu_VM.pem 
    -----BEGIN RSA PRIVATE KEY-----
    -----END RSA PRIVATE KEY-----
    
```






In between are the contents of the private key :) It then took me to the "Security Groups" Page. From the User Guide:





> 
  
> 
> A security group defines firewall rules for your instances.These rules specify which incoming network traffic is delivered to your instance. All other traffic is ignored.
> 
> 
  
  
> 
> If you're new to Amazon EC2 and haven't set up any security groups yet, AWS defines a default security group for you. The name and description for the group is quicklaunch-x where x is a number associated with your quicklaunch group. The first security group you create using the Quick Launch Wizard is named quicklaunch-1. You can change the name and description using the Edit details button. The group already has basic firewall rules that enable you to connect to the type of instance you choose. For a Linux instance, you connect through SSH on port 22. The quicklaunch-x security group automatically allows SSH traffic on port 22.
> 
> 






I selected "Create New Security Group" and created a new rule to only allow port 22, since for now that is all that I need:





[![create_new_security_group](http://virtuallyhyper.com/wp-content/uploads/2013/03/create_new_security_group.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/create_new_security_group.png)





After I clicked "Continue" I was presented with the Summary of all the Settings:





[![Review_new_instance](http://virtuallyhyper.com/wp-content/uploads/2013/03/Review_new_instance.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/Review_new_instance.png)





After I clicked "Launch" I was presented with the following dialog:





[![Instance_is_launching](http://virtuallyhyper.com/wp-content/uploads/2013/03/Instance_is_launching.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/Instance_is_launching.png)





I then went to the EC2 Dashboard and selected "Instances" and I saw the following:





[![ec2_intances](http://virtuallyhyper.com/wp-content/uploads/2013/03/ec2_intances.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/ec2_intances.png)





Now let's connect to our EC2 Instance with SSH.





## Assign an Elastic IP to an EC2 Instance





From the User Guide:





> 
  
> 
> When you launch an instance in EC2-Classic or a default VPC, we allocate a public IP address for the instance. Public IP addresses are reachable from the Internet. A public IP address is associated with an instance until it is stopped or terminated. If you require a persistent public IP address that can be assigned to and removed from instances as necessary, use an Elastic IP address.
> 
> 






I didn't really want to get a new IP every time I stop my instance. So I decided to setup an EIP (Elastic IP). From the same guide:





> 
  
> 
> An Elastic IP address (EIP) is a static IP address designed for dynamic cloud computing. With an EIP, you can mask the failure of an instance by rapidly remapping the address to another instance. Your EIP is associated with your AWS account, not a particular instance, and it remains associated with your account until you choose to explicitly release it.
> 
> 






Also here more information:





> 
  
> 
> Q: **Is there a charge for Elastic IP addresses?**
> 
> 
  
  
> 
> You are not charged for a single Elastic IP (EIP) address on a running instance . If you associate additional EIPs with that instance, you will be charged for each additional EIP associated with that instance per hour (prorated). Additional EIPs are only available in Amazon VPC.
> 
> 
  
  
> 
> To ensure efficient use of Elastic IP addresses, we impose a small hourly charge when these IP addresses are not associated with a running instance, or when they are associated with a stopped instance or an unattached network interface.
> 
> 






So we can use one EIP and if it's not associated with a running instance for more than 1 hour we get charged. Something to keep in mind :) To associate an EIP with my EC2 instance, from the EC2 Dashboard I clicked on "Elastic IPs" and saw the following:





[![EIPs](http://virtuallyhyper.com/wp-content/uploads/2013/03/EIPs.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/EIPs.png)





I then clicked on "Allocate New Address" and the following dialog popped up:





[![Allow_new_EIP](http://virtuallyhyper.com/wp-content/uploads/2013/03/Allow_new_EIP.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/Allow_new_EIP.png)





I left the defaults and then clicked on "Yes, Allocate" and then under "Elastic IPs" the new IP showed up:





[![New_EIP](http://virtuallyhyper.com/wp-content/uploads/2013/03/New_EIP.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/New_EIP.png)





Now we have to associate our EIP with our EC2 instance, cause if we don't and leave it like that for over an hour we will get charged. So from the above screen click on "Associate Address" and following dialog showed up:





[![Associate_EIP_with_Ec2_instance](http://virtuallyhyper.com/wp-content/uploads/2013/03/Associate_EIP_with_Ec2_instance.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/Associate_EIP_with_Ec2_instance.png)





I selected my Instance from the drop down menu and then clicked "Yes, Associate". Then I saw the following in the "Elastic IPs" section:





[![EIP_Associated](http://virtuallyhyper.com/wp-content/uploads/2013/03/EIP_Associated.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/EIP_Associated.png)





We can see that is' associated to our EC2 instance. At this point you can assign the EIP to a more friendly hostname if you have your own DNS service:




    

```
    elatov@crbook:~$ host awsub.dnsd.me
    awsub.dnsd.me has address 54.244.249.93
    elatov@crbook:~$ host 54.244.249.93
    93.249.244.54.in-addr.arpa domain name pointer ec2-54-244-249-93.us-west-2.compute.amazonaws.com.
    
```






Now let's connect to our EC2 Instance.





### Connect and Configure EC2 Linux Instance





We have the private SSH downloaded and we have setup an EIP for our instance, so now we can connect to our VM:




    

```
    elatov@crbook:~$ chmod 600 downloads/Ubuntu_VM.pem 
    elatov@crbook:~$ ssh -i downloads/Ubuntu_VM.pem root@54.244.249.93
    Please login as the user "ubuntu" rather than the user "root".
    
    Connection to 54.244.249.93 closed.
    elatov@crbook:~$ ssh -i downloads/Ubuntu_VM.pem ubuntu@54.244.249.93
    Welcome to Ubuntu 12.04.1 LTS (GNU/Linux 3.2.0-36-virtual i686)
    
```






Now let's add a regular user:




    

```
    ubuntu@ip-10-248-36-122:~$ sudo adduser elatov
    Adding user `elatov' ...
    Adding new group `elatov' (1001) ...
    Adding new user `elatov' (1001) with group `elatov' ...
    Creating home directory `/home/elatov' ...
    Copying files from `/etc/skel' ...
    Enter new UNIX password: 
    Retype new UNIX password: 
    passwd: password updated successfully
    Changing the user information for elatov
    Enter the new value, or press ENTER for the default
        Full Name []: 
        Room Number []: 
        Work Phone []: 
        Home Phone []: 
        Other []: 
    Is the information correct? [Y/n] y
    ubuntu@ip-10-248-36-122:~$ sudo usermod -a -G admin elatov
    
```






Now let's allow regular SSH login, not just via SSH keys:




    

```
    ubuntu@ip-10-248-36-122:~$ sudo sed -i 's/PasswordAuthentication\ no/PasswordAuthentication\ yes/' /etc/ssh/sshd_config
    ubuntu@ip-10-248-36-122:~$ sudo service ssh restart
    ssh stop/waiting
    ssh start/running, process 1293
    
```






Now let's try to login as a regular user with password authentication:




    

```
    elatov@crbook:~$ ssh elatov@54.244.249.93
    elatov@54.244.249.93's password: 
    Welcome to Ubuntu 12.04.1 LTS (GNU/Linux 3.2.0-36-virtual i686)
    
```






That is good, now let's update the system:




    

```
    elatov@ip-10-248-36-122:~$ sudo apt-get update
    elatov@ip-10-248-36-122:~$ sudo apt-get upgrade
    elatov@ip-10-248-36-122:~$ sudo apt-get dist-upgrade
    
```






And now you can configure a web server or whatever you desire and use it for a year for free. Just keep an eye on your usage.





## Monitor EC2 Instance Usage





If you want a general over view of the usage you can go to **http://aws.amazon.com** then hover over "My Account/Console" and from the drop down menu select "Account Activity". From here scroll down to the "AWS Service Charges" and Expand "Amazon Elastic Compute Cloud" and "AWS Data Transfer". You will then see the following:





[![AWS_Service_charges](http://virtuallyhyper.com/wp-content/uploads/2013/03/AWS_Service_charges.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/AWS_Service_charges.png)





Here you can see if you are getting close to IOPS or something and shutdown the VM for the rest of the month. If you want a more granular view of the VM. In the EC2 Dashboard you can click "Instances" and then select your Instance and then Select the "Monitoring" Tab and you will see the following statistics:





[![Instance_metrics](http://virtuallyhyper.com/wp-content/uploads/2013/03/Instance_metrics.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/Instance_metrics.png)





## Create an Alarm to Monitor EC2 Instance





If you want you can create an Alarm to monitor a specific metric. From the Monitoring Tab, you can select "Create Alarm" and then choose a metric to monitor:





[![create_an_alarm_g](http://virtuallyhyper.com/wp-content/uploads/2013/03/create_an_alarm_g.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/create_an_alarm_g.png)





For example here is alarm that would be triggered if the sum of the outgoing network traffic is more than 1GB in a span of 6 hours. If that keeps going then you will end up going above the limit real fast.





[![Alarm-net-out-1gb-6-hours](http://virtuallyhyper.com/wp-content/uploads/2013/03/Alarm-net-out-1gb-6-hours.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/Alarm-net-out-1gb-6-hours.png)





You can setup 10 Alarms in the Free Usage Tier, from the User Guide:





> 
  
> 
> Two Amazon CloudWatch alarms and five metrics. You can add up to 10 alarms and 10 basic metrics (at five-minute intervals) within the free usage tier.
> 
> 






and you can have 100 email notifications sent out for free:





> 
  
> 
> SNS email notification. By default, no email address is configured to receive email notifications when events happen; however, you can configure an email address, and you can then receive up to 1000 free email notifications each month.
> 
> 






After the alarm was set I clicked "Create Alarm" and the following pop-up showed up:





[![Successful_alarm_creation](http://virtuallyhyper.com/wp-content/uploads/2013/03/Successful_alarm_creation.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/Successful_alarm_creation.png)





You will also receive a confirmation email to the address that you specified and you need to confirm that you want to receive those notifications. You can confirm your Alarm by going to the "AWS Management Console":





[![aws_mgmt_console](http://virtuallyhyper.com/wp-content/uploads/2013/03/aws_mgmt_console.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/aws_mgmt_console.png)





and then selecting "CloudWatch" and then you will see the following:





[![cloud_watch_dashboard](http://virtuallyhyper.com/wp-content/uploads/2013/03/cloud_watch_dashboard.png)](http://virtuallyhyper.com/wp-content/uploads/2013/03/cloud_watch_dashboard.png)





You can also click on the Alarm and see the specifics if you want.



