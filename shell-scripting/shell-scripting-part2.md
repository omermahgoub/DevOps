# Scripting for System Administration

## Shell Scripting, Part 2

### I/O redirection 3, wc
* `wc /etc/passwd` - count lines, words, characters in /etc/passwd 
* We've already seen how you can redirect output to a file and then use that in a program 
* Each process has an associated input stream called STDIN
* When you run a process from the shell, STDIN is hooked up to what you type 
* `wc` - type some input into wc and then ctrl-d 
* It's also possible to redirect input into STDIN 
* `wc < /etc/passwd` - redirects the contents of /etc/passwd into wc's STDIN
* Unix tools typically support input and output streams first (via file descriptors), and support reading from file by name as an extra convenience 
* Note: some tools e.g. `tr` don’t take a file as a parameter, you can only redirect into it 
* Many tools that take a filename will read from STDIN if you specify `"-"` as the name of the file 
* `wc -`
* We can directly connect the STDOUT of one process to the STDIN of another 
> It’s also possible to use a technique called process substitution to accomplish redirection but we won’t go into that in this course. 
* `ls / | wc` - send the output of ls / into wc's STDIN 
* This is called piping, and "|" is called the pipe 
* This simple trick lets us solve complex problems by combining the various simple Unix tools 
* And this forms the basis of The Unix Philosophy
> https://en.wikipedia.org/wiki/Unix_philosophy

### more, less

* `more` is used to view a programs output page by page 
* `ls /usr/bin | more` - view `ls` output by page 
* space to move to next page, q to quit 
* less is a popular alternative to more 
* `man less` - to see what it can do 
* Has more features, e.g. lets you scroll back up

### Problem: find all ping procs
Problem: Find all ping processes running in bg 
Start by breaking it down: 
1. Find all processes
 `ps -ef`
2. Filter the processes that match "ping"
 `grep "ping"`
And then combine: 
 `ps -ef | grep "ping"`
> Notice that this shows the grep process too 
> We know how to filter that out: `grep -v "grep" `
Combine to get:
 `ps -ef | grep "ping" | grep -v "grep"`

### tr

* `tr` is used to replace text in a stream 
* `ls / | tr e 3`    - replace "e" with 3 
* `ls / | tr eo 30`  - and replace "o" with zero 
* `ls / | tr eu ue`  - swap "e" and "u" 
* `ls / | tr a-z A-Z`- to upper case 
* `ls / | tr -d e`   - remove "e"s 
* ls / | tr -s o     - remove repeats (root -> rot)

### cut

* `cut` is used to split input lines into the pieces we want 
* It uses a delimiter to split (defaults to tab character) 
* We tell it which parts of the line to output 
* `echo "a,b,c" | cut -d ',' -f 2` - split line based on "," and output 2nd field 
* ps -ef | cut -c 1-8 - can ask for a range

### Problem: how many root folders

Problem: how many folders are there under / 
Solution: break it down 
1. List the folders under / - easy, use ls 
2. Calculate stats for the list - easy, use wc 
3. Pull out the stats we want - easy, use cut. Easy?

* `ls / | wc | cut -d ' ' -f 1` - empty space 
* Need to strip the spaces from wc before cutting 
* `ls / | wc | tr -d ' '` - no good 
* `ls / | wc | tr -s ' '` - better 
* `ls / | wc | tr -s ' ' | cut -d ' ' -f 1 ` - leading space is picked up as field 1, remove it 
* `ls / | wc | tr -s ' ' | cut -c 2- | cut -d ' ' -f 1` - finally

* Or read --help and do: `ls / | wc -l`
* Point is, even if a tool doesn't give us exactly what we  need we can often get it into shape using other tools
