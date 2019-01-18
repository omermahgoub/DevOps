# Scripting for System Administration

## Shell Scripting, Part 1

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

### ls - 1
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

### ls - 2
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

### ls - 3 globbing
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

### ls - 4 Globbing
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

### I/O redirection 1

* Can take the output of a command and redirect to a file - output redirection  
* `ls /etc > contents` - write output to file `contents`
> Note: colouring lost - some commands detect they’re writing to console and add user-friendly formatting 
* `ls /etc >> contents` -append output to file `contents`

### /dev/null

* /dev/null is a special file with nothing in it
* `cat /dev/null` - nothing 
* Even if you write data the the file it has nothing in it 
* `cat /etc/password > /dev/null` - add some text to it 
* `cat /dev/null` - still nothing 
* Sometimes useful! - you can redirect the output of commands here if you don’t want to see it 
* It’s like a bottomless hole for output you don’t want

### I/O redirection 2

* `ls /mmm > contents` - list non-existent dir.  
* `contents` created but empty and error message printed to console 
* Why printed to console when output redirected? 
* Each process has two outputs: STDOUT and STDERR 
* Can redirect them with 1>  for STDOUT and 2> for STDERR 
* > shorthand for 1>, STDOUT 
* `ls /mmm 1> contents 2> errmsgs `
* Can redirect STDERR to same place as STDOUT 2>&1
* `ls /mmm 1> alloutput 2>&1`

### I/O redirection 3, wc

* Can also redirect input with < 
* `wc < /etc/passwd` - count lines, words, characters 
in /etc/passwd 
* Note: some tools e.g. `tr` don’t take a file as a  parameter, you can only redirect into it 
* Unix tools typically support input and output streams first (via file descriptors), and support reading from file  by name as an extra convenience

### sort, uniq, tar, gzip
* sort is used to sort the elements of a file 
* uniq is used to find the uniq elements from a sorted file 
* tar is used to pack files into a single file 
* gzip is used to compress files 
* tar can also compress multiple files into a single file

### grep 1

* grep is used to search for text that matches a pattern 
* `grep oo /etc/passwd` - prints lines that have "oo"
* `grep -v oo /etc/passwd` - print lines that don’t  have "oo"
* `grep -l root /etc/*` - print names of files with match 
* `grep -l root /etc/* 2> /dev/null` - redirect error messages to /dev/null 
* grep is a very powerful tool -  we’ll revisit later

### find 1

* find is used to find files that match certain criteria based on file name, modification time, permissions etc 
* `find /etc -name "pa*"` - find all files starting with 'pa' in /etc and subdirectories under /etc
* `find /etc -name "pa*" 2> /dev/null` - to suppress error messages [1] 
> possible to prevent find from generating these messages in the first place with other switches
* `find /etc -name "pa*" -type d 2> /dev/null` - to find only matching directories

### Users, groups

* Unix was designed from the start to be multi-user 
* So need to be able to administer users too 
* /etc/passwd lists all users, /etc/group all groups 
* `useradd, usermod` - add, modify users
> Non-standard tools, differs by OS and distribution. Won't be examined
* `whoami` - tells you your username 
* `id` - your username, groups, etc 
* `who` - list logged in users

### Processes, ps

* Users run commands which spawn processes 
* Need to be able to administer these 
* `ps` - list your processes. Note you can see bash running - this is your shell 
* `ps -uroot` - list processes for specific user (root here)  
* `ps -ef` - list all users' processes 
* Note each process has an id (PID) - used to control it

### Digression: POSIX etc

If you look at the man page for ps on a Linux system  you'll probably see that it supports 3 sets of options - Unix, BSD and Gnu Linux. There are many varieties of  Unix and Unix derivatives and each developed there own special switches for programs. The POSIX standard was developed to provide a common set of behaviour between Unix based systems. This makes code developed on one system more easily portable to others. You should aim to use POSIX features to increase the portability of your code and your skills.

### kill

* kill is used to give signals to processes 
* There are several signals, but we'll usually just want to terminate a process 
* `kill -9 PID` - replace PID with the pid of bash, kills bash immediately (sends KILL signal) 

### Permissions 1

* Need to protect the system from users, and users from each other 
* Done with permissions and Access Control Lists (ACLs) 
* Permissions most common 
* You've seen them displayed already 
* `ls -l /` - leftmost column 
* E.g. `drwxr-xr-x`
* Note files have an associated user (owner) and group

* {-}  {rwx}  {r-x}  {r--}
  type  user  group  other

* The first character represents a type e.g. "d" for directory, "-" for file etc.
>  The characters can also encode more information than we see here e.g. sticky bit, setuid, sockets.More info see [link1](https://docs.oracle.com/cd/E19683-01/816-4883/secfile-69/index.html), [link2](https://en.wikipedia.org/wiki/Unix_file_types)
* Then there are 3 blocks of permissions 
* `r` means can read, `w` can write, `x` can execute 
* The first block describes what the owner is allowed do to the file, the second block what users who are members of the same group as the file are allowed do, and the third block describes what all other users are allowed do

### chmod

* chmod is used to modify file permissions 
* u - user, g - group, o - other, a - all
> ermissions can also be represented numerically, e.g. 666 for rw-rw-rw-. This is worth learning if you intend to use the shell a lot. See: [link](http://linuxcommand.org/lts0070.php)
* `chmod o+w permstest` - let other users write to testperms 
* `chmod u+x permstest` - let owner execute file 
* `chmod a-w permtest`  - remove write permission from all users 
* Be careful, possible to remove your permissions

### chown, chgrp

* chown - change the user that owns a file - need admin rights 
* `chown someuser permtest` - change owner to someuser 
* chgrp - change the group that owns a file - non-admins can only change to a group you're a member 
* `chgrp dialout permtest` - change group to dialout

### Chaining commands 1

* Already seen you can separate commands with ; 
* `ls -l`; `mkdir tmpdir`; `ls -l`; `rm -r tmpdir`; `ls -l`
* In that case commands execute one after the other 
* Sometimes you'd like to stop executing if one command fails 
* Sometimes you'd like a second command to execute only if the first one failed 
* How can we tell if a command failed?

### exit codes

* Each command returns an exit (or error or return) code (or status) 
* `0 - 255` - 0 indicates success, anything else failure 
* `echo $?` - display exit code of last command executed (even in case of chained commands) 
* `ls -l /mmm; echo $?` - make ls fail, see exit code 
* man pages for commands list the exit codes they generate, usually near the end.  
* `man ls` - for example
* Chain commands with && if you want commands to execute only if all before have succeeded 
* `mkdir tmpdir && touch tmpdir/file && echo "Done"` - make a dir, a file in it, print "Done"
* Chain commands with `||` if you want commands to execute only if preceding commands failed 
* Useful for e.g. presenting an error message or exiting a script if things go wrong 
* `mkdir /tmp || mkdir /etc || echo "Failed to make"` - fails to make /tmp so executes next command. 
  Fails to make /etc so executes next command which prints failure message. 
* `mkdir /tmp || mkdir mytmp || echo "Failed to make"` - fails to make /tmp, succeeds making mytmp so doesn't execute next command.

### Background processes
* If you have a long running process you may want to keep working while it runs. To do this, you can tell the shell to run it in the background, by adding an & 
* `ping -i 1 localhost` - keep pinging localhost. (ctrl-c to kill it) 
* `ping -i 1 localhost & ` - do it in the background. (type fg, return, ctrl-c to kill it) 
* `ping -i 1 localhost > output &` - redirect output too (file keeps getting bigger).  
* Notice the numbers printed. The first is a job id and the second is the process id (pid).
* jobs lists jobs running in background 
* `jobs` - lists by job id 
* `fg` makes the most recent job from the background into the foreground. From here you can kill it with ctrl-c 
* `fg %jobid` - to move a specific job to foreground  
* Can also stop it with `kill %jobid `
* Can move a foreground job to the background with ctrl-z, then typing `bg`
