Cheatsheet
==========

This is my cheatsheet mostly coming from "The entire Pro Git" book, written by
Scott Chacon and Ben Straub and published by Apress, is available at
https://git-scm.com/book/en/v2.

Get and set repository or global options
----------------------------------------

`git config` lets you get and set configuration variables that control all
aspects of how Git looks and operates. These variable can be stored in three
different places:

  1. `[path]/etc/gitconfig` file: Contains values applied to every user on the
     system and all their repositories. If you pass the option `--system` to
     `git config`, it reads and writes from this file specifically. Because this
     is a system configuration file, you would need administrative or superuser
     privilege to make changes to it.
  2. `~/.gitconfig` or `~/.config/git/config` file: Values specific personally
     to you, the user. You can make Git read and write to this file specifically
     by passing the `--global` option, and this affects all of the repositories
     you work with on your system.
  3. `config` file in the Git directory (that is, `.git/config`) of whatever
     repository you’re currently using: Specific to that single repository. You
     can force Git to read from and write to this file with the `--local` option,
     but that is in fact the default. Unsurprisingly, you need to be located
     somewhere in a Git repository for this option to work properly.
