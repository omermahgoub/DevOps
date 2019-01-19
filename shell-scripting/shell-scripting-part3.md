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









