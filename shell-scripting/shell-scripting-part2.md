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

### xargs 1
* Carry out the same command on input parameters 
* `ls / | xargs echo "Read: "` -note: numerous parameters pass to end of echo at once 
* Turns it into `echo "Read: " etc root ...`
* `ls / | xargs -n 1 echo "Read: "` - can limit to one input parameter per command invocation 
* This turns into `echo "Read: " etc; echo "Read: " root; ...`
* But what if need input parameter in middle of command? 
* E.g. cp param somedest 
* Can name the parameter, then use as we wish 
* `find /etc -name 'pa*' -type f | xargs -I inputfile cp inputfile .` - here we name the param "inputfile" then copy it to the current dir (.) 
> Warning: won't work if there are spaces in the name
* `touch book1 book2 book3 "book 4"` - create files 
* `ls book* | xargs -n 1 echo "Read: "`
* "book 4" is being split into two parameters 
* Tell xargs to break up params by newlines, not spaces 
* `ls book* | xargs -d '\n' -n 1 echo "Read: "` - does what we expect 
* Failing to consider spaces (and other characters) or forgetting to quote can lead to subtle bugs. I advise further reading
> [Ref1](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_03_03.html),[Ref2](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_03_04.html)

### Herestrings

* A herestring is a way to pass a string into another program as if it was redirected from a file 
* Let's you avoid creating a file just to redirect it in 
* Handy for testing things out quickly 
* `cut -d ' ' -f 2 <<< "A simple line of text"` - feeds the sentence into cut as if it was redirected from a file[1]
>  can also do `echo "A simple line of text" | cut -d ' ' -f 2. `The difference is subtle; this piped version uses two processes which can make a difference e.g. with the read built-in.


### Heredocs 1
* A heredoc is way to pass multiple lines of data from the command line into the STDIN of a program, just as if it was redirected from a file 
* ```wc << EOF 
It was the best of times,
it was the worst of times 
EOF
```
* EOF can be anything you want, marks start and end.
* Variable expansions happen as usual 
```
cat << EOF 
Your home directory is ${HOME}
EOF
```
* If you don't want expansion to happen, single quote opening marker 
```
cat << 'EOF'
Your home directory is ${HOME}
EOF
```

### Regular Expressions

* A Regular Expression (regex) is a definition of a pattern in a formal language
* There's a lot of theory behind regexes and their efficient implementation 
* We'll skip the theory and learn how to use them by working through examples using grep
* Also supported by other Unix tools e.g. find, sed

* Several dialects in use
* We'll use Extended Reg Exes (ERE) since syntax is simpler and is similar to Python's 
* We tell grep we're using ERE with the -E switch 
* `grep "a?" <<< "aa"` - this uses the default BRE 
* `grep -E "a?" <<< "aa"` - uses ERE, ? special

* Regexes are built from two types of characters, literals and metacharacters 
* Literals are interpreted literally,  
* Metacharacters are special and used to define certain types of pattern 
* For all examples that follow we need to come with a regex that matches the first three lines in the heredoc and doesn't match the last three

### Literals

* We've already seen how to search for ordinary text 
* You just specify the text itself, as literals 
* E.g. to find lines that match the text "any": 
```
grep -E "any" <<EOF 
anything anytime anywhere 
many 
any 
banana 
barney 
annoy 
EOF
```

### The * quantifier

* This behaves differently to the `*` used in globbing
* `*` means match any number of the character (or group, more on this later) that came before it
* E.g. to match an "a" followed by one or more "b"s then an "a":
``` 
grep -E "ab*a" <<EOF 
babar 
abba 
yaay 
babbb 
a 
fab 
EOF
```

### The ? quantifier

* This behaves differently to the ? used in globbing
* `?` means match zero or one of the character that came before it (* is zero or any number) 
* E.g. to match an "a" followed by zero or one "b"s then an "a": 
```
grep -E "ab?a" <<EOF 
babar 
yaay 
yabbadabadoo 
abba 
babbb 
ab 
EOF
```

### The + quantifier

* + matches one or more of the character that came before it 
* E.g. to match an "a" followed by one or more "b"s then an "a": 
```
grep -E "ab+a" <<EOF 
babar 
abba 
yabbadabadoo 
yaay 
babbb 
ab 
EOF
```

### Specifying num of repeats {}

* + matches one or more of the character that came before it 
* E.g. to match an "a" followed by one or more "b"s then an "a": 
```
grep -E "ab+a" <<EOF 
babar 
abba 
yabbadabadoo 
yaay 
babbb 
ab 
EOF
```

### Specifying num of repeats {}

* {m,n} matches n to m of the character that came before it 
* {m} matches exactly m of the character that came before it
* {m,} matches m or more of the character that came before it
* {,n} matches 0 to n of the character that came before it
* E.g. to match an "a" followed by exactly two "b"s then an "a": 
```
grep -E "ab{2}a" <<EOF 
rabbar 
abba 
yabbadabadoo 
babar 
yaay 
babbb 
EOF
```

### Anchors ^ and $

* ^ and $ are special characters that match the start and end of a line  
* There are also ways to match e.g. word boundaries ([ref](http://www.gnu.org/software/grep/manual/html_node/The-Backslash-Character-and-Special-
Expressions.html#The-Backslash-Character-and-Special-Expressions))
* E.g. to match "any" only at the start of a line: 
```
grep -E "^any" <<EOF 
anything anytime anywhere 
any 
any ol' ting 
many 
absolutely anything 
nanny 
EOF
```






