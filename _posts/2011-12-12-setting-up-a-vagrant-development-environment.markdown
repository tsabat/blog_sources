---
layout: post
title: "Setting Up A Vagrant Development Environment"
date: 2011-12-12 13:59
comments: true
categories: 
---

###Install the Vagrant gem

You need vagrant installed for this process to work.  Vagrant depends on a version of ruby we'll set up using the [Ruby Version Manager](http://beginrescueend.com/), as shown below.  This can take a while, so be patient.  A quick note for mac developers: RVM installs Ruby from source.  In order to do so, you will need [Xcode](http://developer.apple.com/xcode/) installed.  You can try using another gcc, but for one-stop goodness, install Xcode and move along.

    bash < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer )
    echo '[[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" # Load RVM function' >> ~/.bash_profile
    source ~/.bash_profile
    rvm install 1.9.2
    rvm use 1.9.2 --default

Next install vagrant

    gem install vagrant

###Create A Home Base[HomeBase]

It is likely you'll be creating several vagrant boxes (individual machines) as you move along, so it makes sense to keep them organized in one place.  Let's create that now.

    mkdir ~/boxes

The `boxes` name given above is just my convention.  You may name that folder anything you like. We'll refer to this as your boxes directory.

###Clone The Opscode Cookbooks

Opscode has a set of [cookbooks](https://github.com/opscode/cookbooks) you'll use when writing chef scripts.  You'll want to pull these down and keep them within reach.

    cd ~/boxes
    git clone https://github.com/opscode/cookbooks.git

This will create a directory `cookbooks` in your base directory containing all of Opscode's great cookbooks.  I suggest deleting the `.git` folder at the base of this dir and checking your cookbooks into your own source control repo.  As you add write cookbooks of your own, you may save them there.

###Create A Project Directory[ProjectDirectory]

A project directory represents one (or more) VMs associated by a VagrantFile.  You'll learn more about the VagrantFile later in this article.  For now, just know that the VagrantFile acts as the configuration for your Vagrant project.  In the snippet below, we'll create a project directory called ProjectDirectory.  Choose a name that properly describes the box you're building.  For example, `WebServer` would be a good Project Directory name.

    cd ~/boxes/
    mkdir ProjectDirectory
    cd ProjectDirectory

###Clone the Chef Repo

The Chef Repo is the basic structure required by Chef.  Your cookbooks and other important files will be kept here. In the example below, we'll be turning your Project Directory into a Chef Repo.  That's why we add the `.` at the end of the git clone command.

    cd ~/boxes/AwesomeBox
    git clone https://github.com/opscode/chef-repo.git .

If you run a `ls -al` command, you'll notice the following directory structure in your Project Directory now:

    ls -al
    total 32
    drwxr-xr-x  13 timsabat  staff   442 Dec 13 12:36 .
    drwxr-xr-x   7 timsabat  staff   238 Dec 13 12:36 ..
    drwxr-xr-x  13 timsabat  staff   442 Dec 13 12:36 .git
    -rw-r--r--   1 timsabat  staff    18 Dec 13 12:36 .gitignore
    -rw-r--r--   1 timsabat  staff  3521 Dec 13 12:36 README.md
    -rw-r--r--   1 timsabat  staff  2171 Dec 13 12:36 Rakefile
    drwxr-xr-x   3 timsabat  staff   102 Dec 13 12:36 certificates
    -rw-r--r--   1 timsabat  staff   156 Dec 13 12:36 chefignore
    drwxr-xr-x   3 timsabat  staff   102 Dec 13 12:36 config
    drwxr-xr-x   3 timsabat  staff   102 Dec 13 12:36 cookbooks
    drwxr-xr-x   3 timsabat  staff   102 Dec 13 12:36 data_bags
    drwxr-xr-x   3 timsabat  staff   102 Dec 13 12:36 environments
    drwxr-xr-x   3 timsabat  staff   102 Dec 13 12:36 roles

Since we'll be creating our own git repository in this directory, let's delete the one provided by the previous clone command.

    cd ~/boxes/ProjectDirectory
    sudo rm -r .git .gitignore

###Initialize your Vagrant environment

Vagrant depends on a file called VagrantFile for configuration information.  The following command creates that file.

    vagrant init

The `ls` command will prove the `vagrant init` call did create your VagrantFile.

Now we'll create a `knife.rb` file to control how chef's `knife` command interacts with your project.

    cd ~/boxes/ProjectDirectory
    mkdir .chef
    touch .chef/knife.rb

What is `knife` you ask?  Opscode [describes](http://wiki.opscode.com/display/chef/Knife) this way:

> [knife] is used by administrators to interact with the Chef Server API and the local Chef repository. It provides the capability to manipulate nodes, cookbooks, roles, databags, environments, etc., and can also be used to provision cloud resources and to bootstrap systems.

The following values should be present in your `knife.rb` file.

    current_dir = File.dirname(__FILE__)
    cache_options( :path => "#{ENV['HOME']}/.chef/checksums" )
    cookbook_path            ["#{current_dir}/../cookbooks", "#{current_dir}/../site-cookbooks"]

The options you've set here tell chef where to create new cookbooks, and how/where to cache your erb templates.

###Create your `vagrant_main` cookbook

In order for vagrant to configure your virtual machine, you must tell it which Chef cookbook to run first.  For convention's sake, we'll call this cookbook `vagrant_main`. If you were using hosted Chef instead of chef-solo (Vagrant's default mode), the would represent the 'run list'.  If you don't know what that means, no big deal, you don't have to understand hosted chef to run Vagrant.

We'll run the following knife command create your `vagrant_main` cookbook.

    cd ~/boxes/ProjectDirectory
    knife cookbook create vagrant_main

Observe your handiwork :

    cd ~/boxes/ProjectDirectory

and you'll see output which looks like this:

    -rw-r--r--   1 timsabat  staff   88 Dec 14 08:56 README.md
    drwxr-xr-x   2 timsabat  staff   68 Dec 14 08:56 attributes
    drwxr-xr-x   2 timsabat  staff   68 Dec 14 08:56 definitions
    drwxr-xr-x   3 timsabat  staff  102 Dec 14 08:56 files
    drwxr-xr-x   2 timsabat  staff   68 Dec 14 08:56 libraries
    -rw-r--r--   1 timsabat  staff  249 Dec 14 08:56 metadata.rb
    drwxr-xr-x   2 timsabat  staff   68 Dec 14 08:56 providers
    drwxr-xr-x   3 timsabat  staff  102 Dec 14 08:56 recipes
    drwxr-xr-x   2 timsabat  staff   68 Dec 14 08:56 resources
    drwxr-xr-x   3 timsabat  staff  102 Dec 14 08:56 templates

Each directory here has special meaning to Chef. You and read about what each means by checking out the [Opscode cookbook documentation](http://wiki.opscode.com/display/chef/Cookbooks).

Finally, we'll tell the VagrantFile to run Chef against the `vagrant_main` cookbook we just created.  To do so, open the VagrantFile and change the values

    # config.vm.provision :chef_solo do |chef|
    #   chef.cookbooks_path = "cookbooks"
    #   chef.add_recipe "mysql"
    #   chef.add_role "web"
    #
    #   # You may also specify custom JSON attributes:
    #   chef.json = { :mysql_password => "foo" }
    # end

to

    config.vm.provision :chef_solo do |chef|
        chef.cookbooks_path = ["cookbooks", "site-cookbooks"]
        chef.add_recipe "vagrant_main"
    end

###
