---
title: NEON State of Health Cluster Setup
permalink: /docs/neon-soh-cluster/
---

##### Table of Contents  
[Intro](#intro)  
[1. Install](#install)  
[2. Configure Cluster](#configure)  
[3. Start Cluster](#start)  
 

<a name="intro"/>
## Intro
The purpose of this tutorial is to provide instructions for bringing up a server cluster for hosting the [NEON](http://www.neonscience.org/)
state of health application.

This tutorial is a variation of the [Clusters from Scratch](http://clusterlabs.org/doc/en-US/Pacemaker/1.1-pcs/html-single/Clusters_from_Scratch/)
document. Some of the commands are modified for this specific application and we take it one step further by loading the state of health 
application onto the server.

###### Audience
This tutorial was written as a guide for a NEON employee looking to start a cluster and load the state of health application.

This tutorial may also be used as a guideline for similar server configurations.

###### Conventions

**Shell Commands** and **Code** inline will be noted `like this`.

**Code Blocks** and **Shell I/O** will be noted as shown below:
```bash   
[root@d23-hlth-lc1 ~]# ifconfig
eno1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.123.21.2  netmask 255.255.240.0  broadcast 10.123.31.255
        inet6 fe80::a694:524b:c051:66e  prefixlen 64  scopeid 0x20<link>
        ether 00:25:90:d0:67:7f  txqueuelen 1000  (Ethernet)
        RX packets 103360791  bytes 7549808475 (7.0 GiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1366053817  bytes 2065480083064 (1.8 TiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        device interrupt 20  memory 0xdfa00000-dfa20000
```

**Panels** are used to display notes, important details, and warnings:
<div class="panel-group">
    <!--<div class="panel panel-default">-->
    <!--    <div class="panel-heading">Panel with panel-default class</div>-->
    <!--    <div class="panel-body">Panel Content</div>-->
    <!--</div>-->
    <div class="panel panel-primary">
        <div class="panel-heading">NOTE</div>
        <div class="panel-body">
            Tips and Tricks, but not necessarily required.
        </div>
    </div>
    <!--<div class="panel panel-success">-->
    <!--    <div class="panel-heading">Panel with panel-success class</div>-->
    <!--    <div class="panel-body">Panel Content</div>-->
    <!--</div>-->
    <!--<div class="panel panel-info">-->
    <!--    <div class="panel-heading">Panel with panel-info class</div>-->
    <!--    <div class="panel-body">Panel Content</div>-->
    <!--</div>-->
    <div class="panel panel-warning">
        <div class="panel-heading">IMPORTANT</div>
        <div class="panel-body">
            Used to highlight details that are important 
            or that could cause issues if ignored.
        </div>
    </div>
    <div class="panel panel-danger">
        <div class="panel-heading">DANGER</div>
        <div class="panel-body">
            Critical warnings that must not be ignored.
        </div>
    </div>
</div>

###### Components

**Hardware**

*2x Rack Mount Servers*

- Manufacturer: Supermicro
- Product Name: X9SCL/X9SCM

**Software**

*CentOS 7*

*Cluster Stack*

- Pacemaker
- Corosync
- DRBD
    
*Web Stack*

- Apache
- PHP
- Javascript (d3.js)

###### Terminology

*Node* -> Single server.

*Cluster* -> The combination of nodes working together.

###### Notes
For comments, questions, or mistakes found in this tutorial, please post an 
issue on the site's [Github Issue Tracker](https://github.com/danrlyon/danrlyon.github.io/issues).

<a name="install"/>
## 1. Install Software

###### CentOS Install

**Boot the Install Image**

This tutorial does not cover the creation of a bootable USB flash drive. There 
are plenty of [resources](https://www.google.com/search?q=how+to+create+a+bootable+usb&rlz=1C1GGRV_enUS748US748&oq=how+to+create&aqs=chrome.1.69i57j0j69i60j0l2j69i60.10392j0j8&sourceid=chrome&ie=UTF-8) 
online that do a great job of covering the topic.

<div class="panel panel-primary">
    <div class="panel-heading">NOTE</div>
    <div class="panel-body">
        If you have an extra flash drive, you can make 2 bootable USB drives and
        install CentOS on both machines simultaneously
    </div>
</div>

<div class="panel panel-warning">
    <div class="panel-heading">IMPORTANT</div>
    <div class="panel-body">
        At the time of writing, we are using <code>CentOS Linux release 7.4.1708 (Core)</code>, but
        the latest release of CentOS 7 should be used to guarantee compatibility with 
        the latest versions of the cluster software.
    </div>
</div>

These first few steps will require direct access to the server. In our (NEON's) 
case this means either using the display and keyboard on the tower, or 
connecting remotely via the "out of band" ethernet port. 

On the first node, connect the USB drive and power up. While the server is 
starting, press the \<DEL\> key to get into the BIOS boot options. Select the USB 
drive as the boot device then exit and save the new configuration.

<div class="panel panel-warning">
    <div class="panel-heading">IMPORTANT</div>
    <div class="panel-body">
        If using different hardware, the key to enter the BIOS options could be 
        something other than the Delete key.
    </div>
</div>

The node should restart into the CentOS installation GUI. Select the option to 
test the installation media before install.

After starting the installation, select your language and keyboard layout at the
welcome screen. At this point, you should be at a screen with all of the 
installation options.

**Configure NETWORK & HOSTNAME**

1. Edit the *Host Name* as desired. In this example I will use D23-HLTH-LC2 (and 
D23-HLTH-LC4 for the second node).
2. Select you're network device (eno1 or similar), press *Configure...* 
and manually assign a fixed IP address. Our Settings:

D23-HLTH-LC2
    * IP address = "10.123.22.2"
    * Netmask = "255.255.240.0"
    * Gateway = "10.123.16.1"
    * DNS = "10.203.22.41, 10.100.62.10, 8.8.8.8"
	* Search Domain = eco.neoninternal.org
    
D23-HLTH-LC4 (same except for IP address)
    * IP address = "10.123.22.2"
    
3. Click the on screen switch to turn on the network device.

**Configure INSTALLATION DESTINATION**

Select the "INSTALLATION DESTINATION" button.

<div class="panel panel-warning">
    <div class="panel-heading">IMPORTANT</div>
    <div class="panel-body">
        If you are using new storage devices without any partitions on them,
		then you can skip this comment. Otherwise you will need to follow these 
		few steps to reclaim the space on your drives.<br/>
		!!!PIC
		Select both drives (but not the USB) to reclaim the space. Under "Other
		Storage Options" choose to automatically consfigure partitioning and select
		the checkbox to make additional space available.</br>
		!!!PIC
		There will be an option to "Delete all". Click this button and follow it with
		the "Reclaim space" button. This will take you back to the home screen where you
		will need to select "INSTALLATION DESTINATION" again. Now you are ready to 
		proceed with the manual partitioning.
    </div>
</div>

Select both storage devices (but not the USB) and under "Other Storage Options" choose 
"I will configure partitioning". Then click Done to proceed.

This will take you to the MANUAL PARTIONING page. First click the link to automatically 
configure partitioning. This will lead you to the following default partition set up:

!!!PIC5

Highlight the "/home" partition first. make sure the device type is LVM and the
file system is xfs, then click the button to modify the Volume Group. You can use 
any name, but it should be consistent across the nodes. Select RAID Level "RAID1"
and leave the other options as is:

!!!PIC6

We won't want any of the partitions to have too much space yet so we will decrease 
the capacity of the "/home" and "/" partitions:

!!!PIC7

Finally, click Done and accept the changes.

**Begin Installation**

You are now ready to click the "Begin Installation" button.

During the installation, you will need to set the root password and add an admin username 
with Administrative priviledges.

!!!PIC8

Wait for the installation to complete. Once you have completed installing CentOS 7, 
you will need to reboot the machine. Take this opportunity to change the boot 
options in the BIOS back to their original state  so that they will boot off of 
the local drive.

Repeat these instructions for the second node, using the appropriate hostname and IP addresses.

<a name="configure"/>
## 2. Configure Cluster

Now that you have CentOS installed on both nodes, it is a good idea to verify the installation and network settings.

**Verify OS Install**

There are a few things we can check to make sure the network settings are correct. First, check the ip address:

```bash
[root@d23-hlth-lc2 ~]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eno1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
    link/ether 0c:c4:7a:05:f9:c7 brd ff:ff:ff:ff:ff:ff
    inet 10.123.22.2/20 brd 10.123.31.255 scope global eno1
       valid_lft forever preferred_lft forever
    inet6 fe80::755b:41c2:367d:51/64 scope link
       valid_lft forever preferred_lft forever
3: enp2s0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc pfifo_fast state DOWN qlen 1000
    link/ether 0c:c4:7a:05:f9:c6 brd ff:ff:ff:ff:ff:ff
```

Then check that the routes are correct:

```bash
[root@d23-hlth-lc2 ~]# ip route
default via 10.123.16.1 dev eno1 proto static metric 100
10.123.16.0/20 dev eno1 proto kernel scope link src 10.123.22.2 metric 100
```

If everything looks correct, you should be able to ping the outside world (first try the other node, then www.google.com):

```bash
[root@d23-hlth-lc2 ~]# ping -c 3 10.123.24.2
PING 10.123.24.2 (10.123.24.2) 56(84) bytes of data.
64 bytes from 10.123.24.2: icmp_seq=1 ttl=64 time=0.423 ms
64 bytes from 10.123.24.2: icmp_seq=2 ttl=64 time=0.219 ms
64 bytes from 10.123.24.2: icmp_seq=3 ttl=64 time=0.247 ms

--- 10.123.24.2 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 1999ms
rtt min/avg/max/mdev = 0.219/0.296/0.423/0.091 ms
[root@d23-hlth-lc2 ~]# ping -c 3 www.google.com
PING www.google.com (172.217.12.36) 56(84) bytes of data.
64 bytes from dfw28s04-in-f4.1e100.net (172.217.12.36): icmp_seq=1 ttl=52 time=15.4 ms
64 bytes from dfw28s04-in-f4.1e100.net (172.217.12.36): icmp_seq=2 ttl=52 time=15.4 ms
64 bytes from dfw28s04-in-f4.1e100.net (172.217.12.36): icmp_seq=3 ttl=52 time=15.4 ms

--- www.google.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 15.427/15.447/15.458/0.014 ms
```

**Login via SSH**

To make it a little easier to work on our cluster, it is recommended to log in via SSH. This will allow you to copy and paste and work on both nodes simultaneously.

From your own local machine, check to see if you can reach the new host:
```bash
[2017-11-08 08:38.46]  ~
[dallen.NEON-07178] ➤ ssh root@10.123.22.2
Warning: Permanently added '10.123.22.2' (RSA) to the list of known hosts.
root@10.123.22.2's password:
X11 forwarding request failed on channel 0
Last login: Wed Nov  8 10:38:19 2017 from 10.123.96.220
[root@d23-hlth-lc2 ~]#
```

Once logged in, update the system:

```bash
[root@d23-hlth-lc2 ~]# yum update
```

Repeat these steps for the second node.

**Configure Communication Between Nodes**

I like to use the text editor "nano" but feel free to use any text editor that you prefer:

```bash
yum install nano
```

Edit the `/etc/hosts` file. 

```bash
nano /etc/hosts
```

Mine looks something like this:

```bash
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
10.123.21.2 d23-hlth-lc1
10.123.22.2 d23-hlth-lc2
10.123.23.2 d23-hlth-lc3
10.123.24.2 d23-hlth-lc4

```

And last, verify that the setup with ping:

```bash
[root@d23-hlth-lc2 ~]# ping -c 3 d23-hlth-lc4
PING d23-hlth-lc4 (10.123.24.2) 56(84) bytes of data.
64 bytes from d23-hlth-lc4 (10.123.24.2): icmp_seq=1 ttl=64 time=0.233 ms
64 bytes from d23-hlth-lc4 (10.123.24.2): icmp_seq=2 ttl=64 time=0.241 ms
64 bytes from d23-hlth-lc4 (10.123.24.2): icmp_seq=3 ttl=64 time=0.236 ms

--- d23-hlth-lc4 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2000ms
rtt min/avg/max/mdev = 0.233/0.236/0.241/0.018 ms
```

**Configure SSH**

Create and activate a new SSH key:

```bash
[root@d23-hlth-lc2 ~]# ssh-keygen -t dsa -f ~/.ssh/id_dsa -N ""
Generating public/private dsa key pair.
Created directory '/root/.ssh'.
Your identification has been saved in /root/.ssh/id_dsa.
Your public key has been saved in /root/.ssh/id_dsa.pub.
The key fingerprint is:
SHA256:Id4GddQn1ZxzOkYl0kaCCt5h0h9u5YLgdxNu7hIYCkw root@d23-hlth-lc2
The key's randomart image is:
+---[DSA 1024]----+
|       ...ooo+++o|
|  E   +.+.+ +o=++|
| o   oo*.B = = .o|
|  o  .++=.X . +  |
|   . ..+S= o . . |
|    . ... .      |
|         o       |
|        . .      |
|         .       |
+----[SHA256]-----+
[root@d23-hlth-lc2 ~]# cp ~/.ssh/id_dsa.pub ~/.ssh/authorized_keys
```

You can now copy the key onto the second node:

```bash
[root@d23-hlth-lc2 ~]# scp -r ~/.ssh d23-hlth-lc4:
The authenticity of host 'd23-hlth-lc4 (10.123.24.2)' can't be established.
ECDSA key fingerprint is SHA256:9kUDRFjXX0qD783scWplJrDCEQlhFuQuPq0hEv5tjvw.
ECDSA key fingerprint is MD5:01:1d:3b:55:a1:26:c3:f9:25:8c:af:e5:2d:22:45:0f.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'd23-hlth-lc4,10.123.24.2' (ECDSA) to the list of known hosts.
root@d23-hlth-lc4's password:
id_dsa                                                             100%  668    98.1KB/s   00:00
id_dsa.pub                                                         100%  607   524.1KB/s   00:00
authorized_keys                                                    100%  607   494.7KB/s   00:00
known_hosts                                                        100%  372   393.4KB/s   00:00
```

Test that you can run commands remotely:

```bash
[root@d23-hlth-lc2 ~]# ssh d23-hlth-lc4 -- uname -n
d23-hlth-lc4
```

**Install the Cluster Software**

Install pacemaker with some command-line tools on both nodes:

```bash
# yum install -y pacemaker pcs psmisc policycoreutils-python
```

<div class="panel panel-warning">
    <div class="panel-heading">IMPORTANT</div>
    <div class="panel-body">
        Commands that need to be run on both nodes will be noted with a simple `#` prompt.
    </div>
</div>

<div class="panel panel-primary">
	<div class="panel-heading">NOTE</div>
	<div class="panel-body">
		We will use pcs for cluster management.
	</div>
</div>

**Configure the Cluster Software**

Allow cluster-related services through the local firewall:

```bash
# firewall-cmd --permanent --add-service=high-availability
success
# firewall-cmd --reload
success
```

Enable the pcs daemon to start at boot time:

```bash
# systemctl start pcsd
# systemctl enable pcsd
Created symlink from /etc/systemd/system/multi-user.target.wants/pcsd.service to /usr/lib/systemd/system/pcsd.service.
```

Pacemaker creates a user `hacluster` with no password, but we will need to create a password to perform tasks across the cluster. To set the password:

```bash
# passwd hacluster
Changing password for user hacluster.
New password:
Retype new password:
passwd: all authentication tokens updated successfully.
```

<div class="panel panel-primary">
	<div class="panel-heading">NOTE</div>
	<div class="panel-body">
		For scripting this process to run on a single node: 
		```bash
		[root@d23-hlth-lc2 ~]# ssh d23-hlth-lc4 -- 'echo redhat6366 | passwd --stdin hacluster'
		```
	</div>
</div>

**Configure Corosync**

On either node, authenticate as the hacluster user:

```bash
[root@d23-hlth-lc4 ~]# pcs cluster auth d23-hlth-lc2 d23-hlth-lc4
Username: hacluster
Password:
d23-hlth-lc4: Authorized
d23-hlth-lc2: Authorized
```

Next, use pcs cluster setup on the same node to generate and synchronize the corosync configuration:

```bash
[root@d23-hlth-lc4 ~]# pcs cluster setup --name d23-hlth-dev d23-hlth-lc2 d23-hlth-lc4
Destroying cluster on nodes: d23-hlth-lc2, d23-hlth-lc4...
d23-hlth-lc4: Stopping Cluster (pacemaker)...
d23-hlth-lc2: Stopping Cluster (pacemaker)...
d23-hlth-lc4: Successfully destroyed cluster
d23-hlth-lc2: Successfully destroyed cluster

Sending 'pacemaker_remote authkey' to 'd23-hlth-lc2', 'd23-hlth-lc4'
d23-hlth-lc4: successful distribution of the file 'pacemaker_remote authkey'
d23-hlth-lc4: successful distribution of the file 'pacemaker_remote authkey'
d23-hlth-lc2: successful distribution of the file 'pacemaker_remote authkey'
Sending cluster config files to the nodes...
d23-hlth-lc2: Succeeded
d23-hlth-lc4: Succeeded

Synchronizing pcsd certificates on nodes d23-hlth-lc2, d23-hlth-lc4...
d23-hlth-lc4: Success
d23-hlth-lc2: Success
Restarting pcsd on the nodes in order to reload the certificates...
d23-hlth-lc4: Success
d23-hlth-lc2: Success
```

This command generates the corosync configuration file stored at `/etc/corosync.conf`.

<a name="start"/>
## 3. Start Cluster





 




















