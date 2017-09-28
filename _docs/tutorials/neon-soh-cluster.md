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
The typical audience for this tutorial will be a NEON employee looking to start a cluster and load the state of health application.

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
        <div class="panel-heading">WARNING</div>
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

###### Notes
For comments, questions, or mistakes found in this tutorial, please post an 
issue on the site's [Github Issue Tracker](https://github.com/danrlyon/danrlyon.github.io/issues).

<a name="install"/>
## 1. Install Software

<a name="configure"/>
## 2. Configure Cluster

<a name="start"/>
## 3. Start Cluster