---
layout: post
title: "Vagrant Boxes and veewee"
date: 2011-12-08 22:02
comments: true
categories: vagrant
---

###11PM Thursday Night

I've been trying to get Windows 2008 [vagrant](vagrantup.com) box up and running and I've had little luck.  According to instructions on [ducea.com](http://www.ducea.com/2011/08/15/building-vagrant-boxes-with-veewee/), the creation of a base windows box sould be a breeze using the [veewee](https://github.com/jedi4ever/veewee) gem, but the postinstall.sh script placed in the base of the cygwin install had several errors.

###10AM Friday Morning

It looks like cygwin was sporting an older version. 

TODO:

1. Edit the [postinstall.sh](https://github.com/jedi4ever/veewee/blob/master/templates/windows-2008R2-amd64/postinstall.sh) file to pull the latest cygwin files.
1. Also, the ruby installer looks like it wants to pull the 32 bit install.  Is that right?

