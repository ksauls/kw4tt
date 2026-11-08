+++
date = '2026-11-08T11:47:42-05:00'
draft = false
tags = ['radio','networking']
title = 'Setting up a 44Net Network at Home'
+++
## Goals

This project is part of a larger project related to learning how to integrate the internet with radio. As I learn more and build out the pieces of that project I will try to document them here.

This project has three goals:
* Learn more about the 44 net
* Build skills in creating and managing subnets, including firewall management
* Create a separate network for my AllStarLink, WPSD, and other radio related devices

## Prerequisites

* **An Amateur Radio License** (you will need to upload an official copy of your license to validate your eligibility to participate in 44net.)
* Some knowledge/skill in network administration
* Comfort on the commandline
* at least two devices that can be used to create this network. I used a Raspberry Pi 3B+ and it worked fine.
  * I installed a basic Debian OS on the device using the Raspberry Pi installer program on my Mac. My 'second' device was actually a switch that served four other computers. These were only for testing. 
* Patience

## First Steps
>NOTE:
>
> Following the instructions on the Getting Started page (linked below) ensures that you're following the most recent instructions. Those instructions are *very* clear and easy to follow.
>
>For my purposes here, I want to focus more on the "gotchas" that tripped me up, but I have to get you there first


The first step is to create an account at https://portal.ampr.org. As mentioned earlier, you will need an *official* copy of your FCC license in order to qualify.

You can follow the instructions on the wiki at https://wiki.ampr.org/wiki/Portal/Sign_Up to create your account.  Once you've created your account you'll be presented with a page that walks you through the process of setting up your account and getting your initial tunnel.

Because the instructions on that wiki page are quite thorough I will not repeat them here.

With your account created, let's create a tunnel and really have some fun.

## Creating Your First Tunnel

Follow the instructions found here: https://wiki.ampr.org/wiki/44Net_Connect/Quick_Start.
## A HUGE Caution -- FIREWALL FIRST!

***Before you do anything else***, make sure you have your firewall on you 44 device  set up to **block** everything coming into your network. Once you've done that then you can choose to allow certain services through. But first protect yourself.

When you receive a 44. network, you're getting an unfiltered pipe to (and from) the internet. *YOU* are responsible for your security. Bad actors can and will attempt to compromise your network. 

Addressing firewall configuration is beyond the scope of this document and is dependent on the firewall software your specific distribution uses. My distribution uses ufw; yours may be different.

One other note: The big advantage to having a 44 network is that it is separate from your regular home network. While you want to be able to access devices on that network from your home network, you *don't* want that network to reach devices on your network. Be sure to include a firewall rule to accomplish that.

On my device, using ufw, I accomplished this by adding two rules:
To block the 44 device from accessing my network:
```
sudo ufw deny out to 192.168.1.0/24
```
But I wanted to be able to ssh into that device so I added this second rule:
```
sudo ufw allow from 192.168.1.0/24 to any port 22 proto tcp
```
Now I can manage my 44 device from my home network but the 44 device cannot access my home network.






## Creating a Network

If you followed the instructions above, you should now have a working device that connects to the 44 network using your credentials and IP address. That's cool but what if you want to connect more devices?

The next step is to request additional addresses.  You do this by following the instructions here:  https://wiki.ampr.org/wiki/Requesting_a_block.  

Basically, go to https://connect.44net.cloud, log in and on that page click the Networks link on the left. 
Click the giant "Request Network Allocation" box.
>[NOTE]: The address block options will likely be different from the address of your tunnel. That's okay. We'll connect them shortly.
>
>Also note:If you intend to have more than two devices on your network, I recommend a /28 address block. Requests up to the /28 are automatically approved. Ask for a /27 or larger and you'll have to justify the need. Don't be greedy here; if you need more later, you can easily request another block.

Once you have received your block of addresses, you'll need to connec them to your tunnel. To do that, follow the instructions here: https://wiki.ampr.org/wiki/44Net_Connect/Routed_Subnet/Raspberry_Pi.  Note that there are also instructions for Debian and other Linux distributions linked at the bottom of this page: https://wiki.ampr.org/wiki/44Net_Connect/Routed_Subnet. 

Again, the process was pretty straight forward. Once I got the subnet set up, I was able to attach devices to the RPI (which has a second, USB network adapter) and those devices got a DHCP assignment. 

There are two things to note: 

1. At least on the RPI, three of your addresses are spoken for. Let's suppose your address block is 10.10.40.200/28 the first address in that block is 10.10.40.192 which would be "the wire" (equivalent to 192.168.0.0/24) and is, therefore not usable. Similarly, the last address in the block, 10.10.40.207 is the broadcast address for the subnet, similar to 192.168.0.255. So, your first *usable* address would be 10.10.40.193 and the last usable address is 10.10.40.207.


2. I had to assign an address from my pool to the "router". The instructions here are bit muddy. Since the RPI is serving to role of the router, I ended up assigning the first address in my pool (10.10.40.193 in the example above) to the second ethernet interface (USB adapter). This allowed the subnet to talk to the tunnel. Without this I was getting "no route to host" messages.

## Then There Was a Problem.

All seemed to be going swimmingly until I tried to get outside the network to the internet. No dice.  Ping gave me "no route to host" errors. 

I tried various fixes to no avail. The instructions for setting up the subnet on the RPI even provided guidance on this specific issue. But those instructions didn't fix the problem.

I finally turned off my firewall (UFW) and Voila! everything worked. 

But I can't leave my firewall off. Looking at my settings there didn't seem to be anything there to cause this problem.

The fix turned out to be simple, if a bit obscure:

I needed to add:
```
-A ufw-before-forward -j ACCEPT
```
To ```*/etc/ufw/before.rules```

From what I understand, ufw has a lot of default rules hidden in the /etc/ufw/ folder.  While we might add specific rules through *ufw add* commands, those are not the only ones in operation.  It was those hidden ones that bit me.

I now have a functional RPI based 44 network for segregating my radio devices (ASL, WPSD, etc) from the rest of my network.

That's pretty cool!


