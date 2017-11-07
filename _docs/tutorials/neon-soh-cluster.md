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
        <div class="panel-heading">IMPORTANT</div>
        <div class="panel-body">
            Critical notes that must not be ignored.
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
    
D23-HLTH-LC4 (same except for IP address)
    * IP address = "10.123.22.2"
    
3. Click the on screen switch to turn on the network device.

**Configure INSTALLATION DESTINATION**



<a name="configure"/>
## 2. Configure Cluster



<a name="start"/>
## 3. Start Cluster