---
layout: post
title: "Set Up A Static Host-Only Network For Virtualbox"
date: 2012-02-28 09:09
comments: true
categories: 
---

##Intro

Setting up a Host-Only Network with Ubuntu Server requires some knowledge of networking.  But why accumulate knowledge when you can simply copy snippets from the internet?

##Set Up Host-Only Networking

Host-Only Networking is a setting in VirtualBox that allows your host machine to act like a DHCP server for a private network on your machine.  Using this setting, you may loom like a god above the private network you create on your garden of nodes.  Or, you can just test out some new service... Your choice.

###Enable Host-Only Networking

Right-click `settings` on your virtual machine of choice, then click the `Network` tab.  Choose `Adapter 2` and then click `Enable Network Adapter`.  Make sure the `Name` dropdown says `vboxnet1`.  If it does not, click `VirtualBox` from your menu bar, then `Preferences`, and then the `Network` tab because we're going to add a new network.  Click the `Add host-only network` button.  This will create a new Host-Only network with a gateway of `33.33.33.1`.  We'll set our Ubuntu Server up accordingly.



###Configure Your Ubuntu Box

Start the box, then issue the following commands:

    sudo vi /etc/network/interfaces

Then, make your interfaces file look like this:

    # The loopback network interface
    auto lo
    iface lo inet loopback

    # The primary network interface
    auto eth0
    iface eth0 inet dhcp

    auto eth1
    iface eth1 inet static
        address 33.33.33.11
        netmask 255.255.255.0
        gateway 33.33.33.1

Then, reboot your machine.

    sudo reboot

###Verify Your Settings

We want to make sure that the settings you put in place work.  To do so, issue this command

    ifconfig

And view the resulting settings:

    eth0      Link encap:Ethernet  HWaddr 08:00:27:c8:d3:98  
          inet addr:10.0.2.15  Bcast:10.0.2.255  Mask:255.255.255.0
            ...Truncated for brevity...

    eth1      Link encap:Ethernet  HWaddr 08:00:27:0e:e2:c0  
           inet addr:33.33.33.11  Bcast:0.0.0.0  Mask:255.255.255.0
            ...Truncated for brevity...

If you don't see that, be sure that the Host Only Network you created in the steps above is in the 33.33.33.1 gateway range.

###Reading More

[Ubuntu Network Configuration](https://help.ubuntu.com/8.04/serverguide/C/network-configuration.html)
[Accessing Ubuntu Server in a VirtualBox Virtual Machine](http://bowerstudios.com/node/722)
