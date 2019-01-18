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

### echo
* echo used to print its parameters to the console 
* `echo "Hello there"`
* `echo "Hello" "there"` - can take >1 parameter 
* `echo "Hello \"Tom\""` - escape quotes with \” 
* `echo "Hello"; echo "There"` - ; separates commands 
* `echo -n "Hello"; echo "There"` - `-n` means no newline 
* `echo {1..10}` - {1..10} replaced with 10 parameters

> Next is about ls  Globbing
* `ls /bin/l*`  - list all files in /bin that start with l 
* `ls /bin/*d`  - list all files that end with d 
* `ls /bin/l?`  - list all that start with l, 2 letters long 
* `ls /bin/??d` - list all 3 letters long, end with d 
* The shell replaces the pattern you specify with the names of matching files, so ls can get passed multiple parameters  
* `echo /bin/l?`- print file names that match pattern
* More information can be obtained from [Bash Extended Globbing](https://www.linuxjournal.com/content/bash-extended-globbing)

### touch
* `touch` lets you update the access and last modification  time of a file to current time (default)
* If file doesn’t exist will create an empty file (default)
* See man page for non-default actions 
* `touch playlist.txt`
* `touch list1 list2 list3`

### cp, mv, rm
* cp copies files from one location to another 
  - `cp playlist.txt playlist2.txt` - copy 
  - `cp -R onedir targetdir` - `-R` copies recursively 
* mv moves files from one location to another 
  - `mv srcdir targetdir` - no need for `-R `
* rm remove (unlink) files 
  - `rm playlist.txt` - remove playlist.txt 
  - `rm -r targetdir` - `-R` to remove recursively 

> `ls` - Globbing 2
* Can limit match to a set of characters 
* `touch doc0 doc1 doc11 docA Doc9`
* `ls doc[0-9]` - files with doc followed by a digit 
* `ls doc[0-9]` - files with doc followed by a digit 
* `ls [dD]oc[0-9]` - files with d or D, then oc, then a digit 
* `ls doc[0-9A-Z]` - files with doc then a digit or capital 
* `ls doc[0-9][0-9]` - files with doc followed by two digits

### cd
* `cd` changes working directory 
* `cd /etc` - change working dir to /etc 
* `cd ~` - change to home dir (default on login) 
* `cd ~/..` - change to parent of home dir 
* `cd -` - change back to last dir [1]

### cat
* cat displays the contents of files 
* `cat /etc/passwd` - display contents /etc/passwd 
* `cat /etc/passwd` - number each line 
* `cat ~/.bash_history` - display command history

### head
* head displays first lines of files 
* `head /etc/passwd` - display first 10 (default) lines 
* `head -20 /etc/passwd` - first 20 lines

### tail
* tail displays last lines of files 
* `tail /etc/passwd` - display last 10 (default) lines 
* `tail -20 /etc/passwd` - last 20 lines








