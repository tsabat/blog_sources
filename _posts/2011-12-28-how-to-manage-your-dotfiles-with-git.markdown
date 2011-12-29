---
layout: post
title: "How to manage your dotfiles with git"
date: 2011-12-28 21:45
comments: true
categories: vim git
---

###Objective

Programs like vim, bash, and zsh all use `dotfiles` for configuration.  You want to back them up in case of disaster.  Here's how I handle that using a `.dotfiles` directory and symlinks.

###Where Do My `dotfiles` Live?

By default vim, bash, zsh and other programs store `dotfiles` in your home directory. You can view the dotfiles in your home directory like so:

    cd
    ls -al

###Vim As An Example

In the following steps, you'll learn how to back up your Vim configuration to a directory named `.dotfiles`.

To get started, create your `.dotfiles` directory.

    cd
    mkdir -p .dotfiles/vim

Note: The `-p` option tells bash to create the directory recursively, building the entire path if it does not exist.

Now, move your `.vim` and `.vimrc` files to your `.dotfiles` directory.

    mv .vimrc .vim .dotfiles/vim

Finally, [symlink](http://www.tech-recipes.com/rx/172/create_a_symbolic_link_in_unix_solaris_linux/) the files and folders you just moved back to their original location.

    cd
    ln -s .dotfiles/vim/.vimrc .vimrc
    ln -s .dotfiles/vim/.vim .vim

###Back It Up

Remember to use whatever source control system you like to back up your `.dotfiles` directory.  I prefer git.

    cd ~/.dotfiles
    git init
    git add .
    git commit -a -m 'My first dotfile commit'
