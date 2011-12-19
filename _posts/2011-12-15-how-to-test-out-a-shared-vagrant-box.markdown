---
layout: post
title: "How to test out a shared vagrant box"
date: 2011-12-15 12:17
comments: true
categories: 
---

###Intro

At some point, someone will offer to share a vagrant box with you.  These are the steps required to get that box working.

###Create a Working Folder

We'll need a place to house the `.box` file and a way to start it up, so create the directory and use the vagrant gem's `init` call, which will make a `VagrantFile` for you.

    mkdir WorkingFolder
    cd WorkingFolder
    vagrant init

###Download the `.box` File

Put the `.box` file into your Working Directory.  For this exercise, we'll call it sharedBox.box.

###Add The Box to Vagrant's Box Cache

The command below will import your .box file.

    cd WorkingFolder
    vagrant box add shared_box sharedBox.box

Importing a box file will copy it your `~/.vagrant.d/boxes` folder.  To prove this, run the `ls` command.

    ls ~/.vagrant.d/boxes
    yourshell$ shared_box

Notice that the `shared_box` argument to the `box add` command produces a `shared_box` file in your `~/.vagrant.d/boxes` directory.  Now, when dealing with this box in vagrant, you'll refer to it as `shared_box`.  So, you can safely delete the sharedBox.box file from your Working Directory.

    rm sharedBox.box

###Edit the VagrantFile

In order start the vagrant box, you'll need to reference it in your `VagrantFile`.  Using your editor, change

    config.vm.box = "base"

to

    config.vm.box = "shared_box"

Now when you tell vagrant to start, you'll be referring to the `shared_box`.

###All Done

With these steps in place, you're ready to start vagrant with the `vagrant_up` command.
