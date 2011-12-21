---
layout: post
title: "Chef Recipe To Upgrade Virtualbox Additions"
date: 2011-12-20 23:01
comments: true
categories: vagrant chef virtualbox
---

Every time the guys at VirtualBox update their software, you have to scramble to find resources to upgrade your virtualbox guest additions.  You also get the following annoying message.

    [default] The guest additions on this VM do not match the install version of
    VirtualBox! This may cause things such as forwarded ports, shared
    folders, and more to not work properly. If any of those things fail on
    this machine, please update the guest additions and repackage the
    box.

To prevent this from being a hassle, I created this chef recipe to help ease our suffering.

<script src="https://gist.github.com/1505022.js?file=upgrade_guest_additions.rb"></script>

You will probably have to restart your vagrant box for this to work.  I'm not 100% sure.
