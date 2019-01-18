# Scripting for System Administration

## Part 1 - Shell Scripting, Lecture 1

I'll try to cover as much details of shell scripting as possible. Although there's plenty of free information if you want to learn more by yourself. 

You'll see references to relevant content in the lecture footnotes.

**Also, for some quick reads see:** 
* http://linuxcommand.org
* http://matt.might.net/articles/basic-unix/

**And for mode detail and as a reference, read:**
* Advance Bash Scripting guide at http://www.tldp.org/LDP/abs/html/

> Node: 2e’ll use Ubuntu on this guide, but most command are applicable on all linux distributions

### The Shell
- It's a program that lets you run other programs 
- Lets you get your work done 
- There are several shell platform: `sh, ksh, bsh, csh, zsh`. They mostly do the same things, but differently 
- We’ll use bash - /bin/bash

### The File System (fs)
* A file system is the way in which files are named, stored, retrieved as well as updated on a storage disk or partition; the way files are organized on the disk.
* Linux system is nothing but Folders and files 
* There's one root folder: / - not like Windows C:\, D:\, E:\ 
* Programs are just files 
* Files can be aliases for other files (links) 
* How do we administer this from the shell? via commands as:

### The File System (fs)
* A file system is the way in which files are named, stored, retrieved as well as updated on a storage disk or partition; the way files are organized on the disk.
* Linux system is nothing but Folders and files 
* There's one root folder: / - not like Windows C:\, D:\, E:\ 
* Programs are just files 
* Files can be aliases for other files (links) 
* How do we administer this from the shell? via commands

### ls
- list directory
- ` ls [files ...]
- directories and files (and others) all called files
- `ls -l` to see more info 
- `ls -a` to also see hidden files 
- `ls -t` to order by time (with `-r` to reverse order)

### Getting help
* Google, StackOverflow etc.  
* the man command lets you RTFM[read the manual](https://xkcd.com/293/)
* `man ls` to see ls’s `man` page
* All tools have a man page 
* `man man`
* `ls --help`
* Most tools give usage info when called with `--help`
* Remember these, they work without Internet

### apropos, date
* apropos is used to search man pages 
* `apropos date` - search for pages with date 
* These days Googling is probably faster than apropos, but not all machines are online 
* date is used to get /set current date and time 
* `date` - prints current date (see man page to format)

### mkdir, rmdir, pwd
* mkdir used to make directories 
* `mkdir notes`
* rmdir used to remove directories, most use (rm -r) 
* `rmdir notes` 
* pwd prints the current working directory (which is like the folder the shell is ‘in’) 
* `pwd`

### ls (continue)
* `"."` refers to the current directory 
* `ls .`
* `".."` refers to the parent directory, up a level 
* `ls ..`
* `"~"` refers to your home directory 
* `ls l ~`
* files that start with a `"."` are hidden from ls 
* `ls -al` shows you all files, including `"."` and `".."`
* `ls /usr ` - list contents of usr under / 
* `ls /usr/bin` - list contents bin under user 
* `/usr/bin` - a path 
* / the path separator (\ on Windows) 
* `ls /usr /etc` [note the space, this is two parameters so lists contents of usr and etc under /]







