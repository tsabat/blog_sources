---
layout: post
title: "Installing Chef Client and Chef Server"
date: 2011-12-11 23:31
comments: true
categories: 
---

###This guide will get you up and running with chef server and client on the same Windows 2008 Server. 

The instructions outlined herin are a distillaiton of the [Chef Fast-Start Guide For Windows](http://wiki.opscode.com/display/chef/Fast+Start+Guide+for+Windows#FastStartGuideforWindows-Step1%253ACreateaHostedChefaccount)

###Create a Hosted Chef Account

The instructions for [Creating a Hosted Chef Account](http://wiki.opscode.com/display/chef/Fast+Start+Guide+for+Windows#FastStartGuideforWindows-Step1%253ACreateaHostedChefaccount) are easy to follow.  Do that and return to this guide.

###Install Chef Client and Server

Run the [Chef Full Installer](http://opscode-full-stack.s3.amazonaws.com/windows/chef-client-0.10.4-6.msi) and then verify your install with these commands.

    chef-client --version
    tar --version

should produce

    Chef: 0.10.4
    bsdtar 2.8.3 0 libarchive 2.8.3

###Install Git

Follow the instructions listed in the [github.com Windows Setup Guide](http://help.github.com/win-set-up-git/).

Then verify

    git --version

will produce

    git version 1.7.6.mmsygit.0

or whatever the latest git version happens to be.

###Prepare Your File System

Follow these instructions to set up your base Chef directory.  You'll use this when creating cookbooks.

    cd %HOMEPATH%
    git clone git://github.com/opscode/chef-repo.git
    mkdir %HOMEPATH%\chef-repo\.chef

Move in your Chef keys you created in the first step of this guide titled Create A Hosted Account.  Edit the snippet below for your system settings:

    move %HOMEPATH%\Downloads\knife.rb %HOMEPATH%\chef-repo\.chef
    move %HOMEPATH%\Downloads\tsabat.pem %HOMEPATH%\chef-repo\.chef
    move %HOMEPATH%\Downloads\fizbuzz-validator.pem %HOMEPATH%\chef-repo\.chef

Open WordPad to edit your knife.rb file.

    Write %HOMEPATH%\chef-repo\.chef\knife.rb, 

MEPATH%\chef-repo
In that file, change `cookbook_path ["#{current_dir}/../cookbooks"]` to `cookbook_path ["#{ENV['HOME']}/chef-repo/cookbooks"]`

###Verify Connection To Hosted Chef

Run the commands

    cd %HOMEPATH%\chef-repo
    knife client list fizbuzz-validator

**TODO: Explain the validator's role in Chef**

and you'll see your machine name listed there.

###Configure The Workstation as Client

Run these commands

    cd %HOMEPATH%\chef-repo
    knife configure client %HOMEPATH%\chef-repo

Then edit your client.rb

    Write %HOMEPATH%\chef-repo

making it look like this, substituting fizbuzz for your own organization.

    log_level        :info
    log_location     STDOUT
    chef_server_url  'https://api.opscode.com/organizations/fizbuzz'
    validation_client_name 'fizbuzz-validator'
    validation_key cd "#{ENV['HOME']}/chef-repo/.chef/fizbuzz-validator.pem"
    client_key "#{ENV['home']}/chef-repo/client.pem 

Run chef-client to register your client with the server.

    chef-client -c %HOMEPATH%\chef-repo\client.rb

You'll see output which looks like this:

    [Mon, 12 Dec 2011 00:48:03 -0800] INFO: *** Chef 0.10.4 ***
    [Mon, 12 Dec 2011 00:48:09 -0800] INFO: Client key C:\Users\Administrator/chef-r
    epo/client.pem is not present - registering
    [Mon, 12 Dec 2011 00:48:14 -0800] INFO: Run List is []
    [Mon, 12 Dec 2011 00:48:14 -0800] INFO: Run List expands to []
    [Mon, 12 Dec 2011 00:48:14 -0800] INFO: Starting Chef Run for WIN-JLR7H2GM3Q5
    [Mon, 12 Dec 2011 00:48:14 -0800] INFO: Loading cookbooks []
    [Mon, 12 Dec 2011 00:48:14 -0800] WARN: Node WIN-JLR7H2GM3Q5 has an empty run li
    st.
    [Mon, 12 Dec 2011 00:48:15 -0800] INFO: Chef Run complete in 1.484375 seconds
    [Mon, 12 Dec 2011 00:48:15 -0800] INFO: Running report handlers
    [Mon, 12 Dec 2011 00:48:15 -0800] INFO: Report handlers complete

Verify that your node was added

    cd %HOMEPATH%\chef-repo
    knife client list


