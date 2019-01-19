# Scripting for System Administration

## Shell Scripting, Part 3

### Shell Scripts

* A script is just a file containing commands 
* First line is special, starts with #! (shebang) 
* Says what interpreter to use 
* We'll use #!/bin/bash
> There are other ways to specify this e.g #!/usr/bin/env bash is more appropriate in some cases 
* Possible to use scripts without this line, but standard 
* `man bash` - worth reading


### First script: hello.sh

* We'll create our first script with a heredoc 
```
cat > hello.sh <<EOF 
#!/bin/bash 
echo "Hello there" 
EOF
```

* Try run it with:  `./hello.sh`
* `ls -l` - we can't execute it 
* `chmod u+x hello.sh `
* Try again: `./hello.sh`
* Why not `hello.sh`?

### Comments

* Everything after a # is ignore - use for comments. E.g. 
```
#!/bin/bash 
#  
# Copyright AILabs 2016 
# This program is intended to simulate human consciousness 
#   
# v0.1 - Feb 1st 2016 - Basic framework 
# v0.2 - Feb 2nd 2016 - First argument 
... 
# extract lines with emotive words 
emotive = ${grep $emoregex inputfile) 
```
* Comments are to make it easier for someone to understand the intent of the code, or record important information 
* Only document what's not obvious to a new reader

### A note on editing files

* I'll present files as heredocs for ease of cut & paste 
* You'll want to modify existing documents as you work 
* You'll want a text editor 
* From shell: vi(m), emacs, nano 
* From GUI: Sublime, GEdit etc. 
* `vi` always available 
* Pick one and learn it

### Cmd-line arguments

* Let's modify the script to make it more personal: 
```
cat > hello_user.sh <<'EOF'
#!/bin/bash 
echo "Hello there, $1" 
EOF
```
> Note the quoting around 'EOF'. Try it without the quotes and see what happens. Why?

* don't forget to chmod u+x (assumed from now on) 
* `./hello_user.sh Keith`
* `./hello_user.sh Uncle Bob` - two parameters 
* `./hello_user.sh "Uncle Bob"` - one param 
* `$1` a variable that holds first parameter and so on
> http://www.tldp.org/LDP/abs/html/othertypesv.html
* `$0?`

* Let's look at more params: 
```
cat > params.sh <<'EOF' 
#!/bin/bash 
echo "$1 $5 $10" 
EOF
```
* `./params.sh {1..10}` - shell expansion occurs
> For more info on expansions see http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_03_04.html

* `echo {1..10}` - expanded to 10 parameters 
* `echo {1..30..3}` - steps of 3
* `./params.sh {1..30..3}` - `10?` 
* Expansion is greedy 
* Use `${10}`
* Safest to use `${}` everywhere

* `$#` - number of parameters 
* `$@` - all parameters (also $*)
> http://www.tldp.org/LDP/abs/html/internalvariables.html#APPREF

  - Useful when we want to loop over all of them 
* `shift n` shifts command line parameters "to the left" n times. Omit n to do once. Useful for iterating over variable number of parameters. 
* `./somescript.sh a b c d`, after shift 2 in script, `$1 = c`,  `$2 = d`

* Command line arguments visible to all users
* Let's see this in action: 
```
cat > insecure.sh <<'EOF' 
#!/bin/bash 
sleep 60 
EOF
```
* `./insecure.sh myusername mypassword &`
* now do `ps -f`
* so don't pass sensitive information that way if another user might see it - use e.g. environment variables

### Parameter substitution - substring

* We can transform values of variables 
* `export NICKNAME="Uncle Buck"`
* `echo "${NICKNAME}"`
* `echo "${NICKNAME:2:2}"` - known as substring expansion `${var:offset:len}`
* `echo "${NICKNAME:2}"` - no len, assumes to end 
* `echo '${NICKNAME: -3:2}"` - negative starts from end, note the space 
* `echo "${NICKNAME: -5:2}"`
* `echo ${NICKNAME: -5:2}` - notice no space before B, because only one parameter to echo. Need quotes to keep spaces.

### Parameter substitution - default

* `unset NICKNAME` - removes an env variable 
* `echo "${NICKNAME}"` sadf
* `echo "${NICKNAME:2:2}"` sdf
* `echo $?` - No error 
* `echo "${NICKNAME:-guy}"` - value if not set
> Note the '-'. This is why we specify a space before the '-' in substring expansion, to tell them apart 
>  For variations see http://tldp.org/LDP/abs/html/parameter-substitution.html

* `echo "${NICKNAME:=guy}"` - also changes var 
* Useful for using a default value if user doesn't provide one

### Parameter substitution - message

* Sometimes best to print a message and exit if not set 
```
cat > hello_user2.sh <<'EOF' 
#!/bin/bash 
nickname=${1?"You must pass a nickname as the parameter"}  
echo "Hello there, ${nickname}" 
EOF
```
* `./hello_user2.sh` - prints message and exits 
* `./hello_user2.sh pip` - as if we used `$1` 
* nickname variable local to script (scope) - shell var 
* some use lowercase for shell vars, capitals for env vars 
* prefix variables with $ when reading value 
* Other variable transformations include search-replace and removing start and end patterns e.g. .gif 
> See http://tldp.org/LDP/abs/html/parameter-substitution.html


### User input 

* We can ask the user for input and store in a variable: 
```
cat > hello_interactive.sh <<'EOF' 
#!/bin/bash 
read -r -p "Enter your name: " name 
echo "Hello there, $name" 
EOF
``` 
* `./hello_interactive.sh` - prints message and exits 
* `read` reads from STDIN and stores value in a variable with the specified name 
* `-p` tells read to print the prompt before reading 
> Could also just echo the prompt
* `-r` tells read not to interpret special characters. You'll almost always want this 
* Notice it works with parameters with spaces

* Let's read the first and last name separately: 
```
cat > hello_interactive2.sh <<'EOF' 
#!/bin/bash 
read -r -p "Enter your first name and last name: " firstname lastname 
echo "Hello there, $lastname. $firstname $lastname." 
EOF
``` 
* Try your own name, then Eamon de Valera
* Why does it work? 
* `read` breaks the input into words[1], assigns to variables and puts anything left over into the last specified variable 
> It breaks them part based on whitespace. Possible to change this by setting env var IFS

* Useful with herestrings to split variables

### Command substitution 

* Surround commands with $() to execute
> Can also surround with backtics ` (note, not single quote). Better to use $()
 
* Let's run another program and use the result: 
```
cat > homeinfo.sh <<'EOF' 
#!/bin/bash 
numfiles=$(ls ${HOME} | wc -l) 
echo Your '${HOME}' directory is ${HOME} and has
 \ 
 $numfiles visible files and directories 
EOF
``` 
* As with all shell commands, can do this from the command line too: 
* `grep $(whoami) /etc/passwd`

* Can nest commands in other commands 
* Note: output might be multiple lines so be careful with subsequent quoting.  
* E.g. 
```
cat > listprocs
.sh <<'EOF' 
#!/bin/bash 
mytopprocs=$(ps -ef -u $(whoami) | head -5)
echo "Your top processes are:"
echo $mytopprocs 
EOF
```

### Arithmetic

* You can use bash to do simple arithmetic: 
```
cat > add
.sh <<'EOF' 
#!/bin/bash 
a=
${1} 
b=
${2} 
let "c = a + b"
echo "$a + $b
 = $c" 
EOF
``` 
* `./add.sh 5 10` - add the parameters 
* There are other ways to do this
> http://www.tldp.org/LDP/abs/html/arithexp.html


### if statements

* `true` and `false` return exit codes 0 and 1 
* Can flip the results of an exit code using `!: `
* `! true; echo $?` - note, space after `!`
* Turns exit code of 0 to 1, and anything else to 0 
* `if statements` let us specify one set of actions to be carried out only if exit code of condition is 0 
* `if <condition>; then <some statements>; fi `
* `if false; then echo "do this"; fi` echo "do this" executed if exit code of command after if is 0 
* `if true; then echo "do this"; fi`

* Can also specify commands to be executed if exit code is not 0 
* Called an if-else statement 
* `if <condition>; then <some statements>; else <some other statements>; fi`
* `if false; then echo "do this"; else echo "do that"; fi`


### test

* Sometimes need to check things before we proceed 
* test lets you check a variety of conditions
> There is a program called test but also a shell built-in for efficiency. See http://www.tldp.org/LDP/abs/html/internal.html#BUILTINREF
* `test -d dir` - test whether a directory exists 
* `test -f file ` - test where a file exists 
* `test -z str` - test whether a string is of length 0 
* `test -n str` - test whether a string is not empty (could also use ! and -z) 
* `test str1 = str2` - test whether two strings are the same 
* `test str1 != str2` - test whether two strings are not the same 
* `test "${NICKNAME}" = "Bob"; echo $?`
* Can use > and < operators to check alphabetical order 

* Not so neat with integers 
* `test a -eq b` - test whether two integers equal 
* `test a -ne b` - test whether two integers not equal 
* `test a -lt b` - test whether a < b 
* `test a -le b` - test whether a <= b 
* `test a -gt b` - test whether a >b 
* `test a -ge b` - test whether a >= b

* Can test combinations of expressions 
* `test expr1 -a expr2` - test whether expr1 AND expr2 true 
* `test 6 -lt 7 -a 7 -lt 8; echo $?` df 
* `test 6 -lt 7 -a 7 -lt 6; echo $?` df 
* `test expr1 -o expr2` - test whether expr1 OR expr2 true 
* `test 6 -lt 7 -o 7 -lt 6; echo $?`

* Very useful in scripts: 
```
cat > backup1.sh <<'EOF' 
#!/bin/bash 
read -r -p "What directory would you like to back up? " targetdir 
if ! test -d ${targetdir}; then 
    echo "Directory ${targetdir} doesn't exist" 
    exit 1 
fi 
echo "Backing up ${targetdir}" 
EOF
``` 
* Note we set an exit code in the if - check it with `echo $?`
* [ an alias for test, makes it more readable. Needs ] as last param 
* Here's a more reasable version: 
```
cat > backup2.sh <<'EOF' 
#!/bin/bash 
read -r -p "What directory would you like to back up? " targetdir 
if ! [ -d ${targetdir} ]; then 
    echo "Directory ${targetdir} doesn't exist" 
    exit 1 
fi 
echo "Backing up ${targetdir}" 
EOF
```

* If we want to check for a match among several conditions can use an elif-statement 
* if <condition>; then <some statements>; elif <some other statements> else <some other statements>; fi 
* Can have as many elifs as we like 
* But only one else

* Here's an implementation of a simple menu: 
```
cat > simplemenu.sh <<'EOF' 
#!/bin/bash 
echo "b) backup" 
echo "r) restore" 
echo "q) quit" 
read -r -p "Pick an option: " choice 
if [ "b" = ${choice} ]; then 
    echo "Will make backup" 
elif [ "r" = ${choice} ]; then 
    echo "Will restore" 
else 
    # these commands executed for 
q, or any other option  
    echo "Quitting" 
    exit 0 
fi 
EOF
```
> Note that there's another statement, case, for choosing commands based on a variable having one of a variety of alternative values: http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_07_03.html And there's a select statement to make it easy to display menus and read options: http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO-9.html


### for loop

* The for loop lets you iterate over a set of elements executing commands 
* `for <variable> in <set>; do <commands>; done`
* `for val in 1 2 3 elephant moo; do echo $val; done`
* Iterate over the elements in set {1, 2, 3, elephant, moo} 
* At each iteration, variable `val` set to a different element 
* Just prints out the current element 

* Print out all command line parameters: 
```
cat > params2.sh <<'EOF' 
#!/bin/bash 
for param in $@; do 
    echo $param; 
done 
EOF
```
* `./params2.sh {1..10}`
* `./params2.sh {1..10} some more`
* `./params2.sh {1..10} "some more"` - still broken up!

* Quote "$@" to preserve parameters: 
```
cat > params3.sh <<'EOF' 
#!/bin/bash 
for param in "$@"; do 
    echo $param; 
done 
EOF
```
* ./params3.sh {1..10} some more
* ./params3.sh {1..10} "some more" - better

* Looping over the results of another command: 
```
cat > forls.
sh <<'EOF' 
#!/bin/bash 
for f in 
$(ls .); do 
    echo "Item: $f"
done 
EOF
```
* Iterate over the results of `ls .`
* Notice that in scripts you generally indent the commands executed by a for loop, or an if statement. This makes it much easier to read and find bugs
* There is also a C-style for loop which we won't cover


### while loop

* The while loop lets you keep executing a set of commands while a condition is false (exit code != 0) 
* `while <condition>; do <commands>; done`
* Condition evaluated before commands executed, so commands may never execute.
* Looping over the results of another command: 
```
cat > whileread.sh <<'EOF' 
#!/bin/bash 
while read -p "Enter a word: " word; do  
    echo "You typed ${word}" 
done 
EOF
```
* Types words and hit return. Ctrl-D to stop

* Can combine with arithmetic and conditions: 
```
cat > whilemath.sh <<'EOF' 
#!/bin/bash 
i=0 
while [ $i -ne 10 ]; do  
    echo $i 
    let "i = i + 1" 
done 
EOF
```

### until loop

* There's another variation that runs `until` a condition is true 
* `until <condition>; do <commands>; done`
* Condition evaluated after commands executed, so it always runs at least once, unlike while.
* Can combine with arithmetic and conditions: 
```
cat > simplemenu2.sh <<'EOF' 
#!/bin/bash 
choice="" 
until [ "q" = ${choice} ]; do 
    echo "b) backup" 
    echo "r) restore" 
    echo "q) quit" 
    read -r -p "Pick an option: " choice 
    if [ "b" = ${choice} ]; then 
        echo "Will make backup" 
    elif [ "r" = ${choice} ]; then 
        echo "Will restore" 
    fi
    echo "Your last choice was: ${choice}" 
done 
EOF
```

### break and continue

* `break` is used to exit a loop 
* `continue` is used to jump to the next iteration of the loop 
* Notice the infinite loop, so we must have a break somewhere: 
```
cat > simplemenu3.sh <<'EOF' 
#!/bin/bash 
choice="" 
while true; do 
    echo "b) backup" 
    echo "r) restore" 
    echo "q) quit" 
    read -r -p "Pick an option: " choice 
    if [ "b" = ${choice} ]; then 
        echo "Will make backup" 
    elif [ "r" = ${choice} ]; then 
        echo "Will restore" 
    elif [ "q" = ${choice} ]; then 
        echo "Quitting"  
        break 
    else 
        continue 
    fi 
    echo "Your last choice was: ${choice}" 
done 
EOF
```

### source, .profile, cron

* source FILE is used to run the script FILE, in the current shell. That means e.g. any exports in the script will affect the current shell's environment 
* `~/.profile` contains commands to be run when you log in (see also `.bash_profile, .bash_rc, .bash_login`)
> http://www.gnu.org/software/bash/manual/html_node/Bash-Startup-Files.html

* can add new env vars there and add with `source ~/.profile`
* `cron` is a tool used to schedule the program execution
* e.g. you can have your scripts run every hour, or at 2.37am daily, or just on the first day of the month 
* beware, `cron` doesn't run in the user's environment 


### Problem, Solved

Give me a tool that will stop all processes whose names match a particular pattern. I want to be able to call it like this: 
`$ nuke someprogram`
