---
layout: post
title: "Command-Line Resources"
date: 2011-12-19 08:07
comments: true
categories: tips
---

###SSH Tips

This excellent article entitled [Tips for Remote Unix Work](http://shebang.brandonmintern.com/tips-for-remote-unix-work-ssh-screen-and-vnc#) covers some vital SSH goodness.  For example, copying your public ssh key

    ssh user@example.com 'mkdir -p .ssh && cat >> .ssh/authorized_keys' < ~/.ssh/id_rsa.pub

and piping commands via SSH without logging into the remote machine

    cd && tar czv src | ssh example.com 'tar xz'


