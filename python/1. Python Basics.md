# Further Reading

* References - https://docs.python.org/3.5/index.html
* The Python Tutorial - https://docs.python.org/3.5/tutorial/index.html
* A Byte of Python - https://www.gitbook.com/book/swaroopch/ byte-of-python/details - but beware, only recently converted to Python 3, may have mistakes
* Learn Python The Hard Way - http:// learnpythonthehardway.org/
* The Hitchhiker’s Guide to Python! - http://docs.python-guide.org/ en/latest/
* Video courses - Udacity, Coursera, edX

## Setting Python up

* yum, brew, or apt-get install python3 python-pip pip install package_name
* System or local store of packages
* virtualenv - lets you keep separate stores of packages for different projects, helps avoid depency conflicts

### Python 3

* Python 3 language introduced in 2008, not strictly
* backwards compatible with Python 2 libraries
* The changes are small, but breaking
* Python 2 supported until 2020
* Some libraries not ported to Python 3 yet
* Careful, lots of code online is for Python 2
  * print "some thing %s" % var - Python 2 
  * print("some thing", var) - Python 3

### Python REPL

* REPL - Read Eval Print Loop •7+6
* `python3`
* `7+3`
* Interprets what we type and prints it until we quit Ctrl - D (EOF) to quit
* `Ctrl - D` (EOF) to quit

### Numerical operations

* Most of these are obvious (see Python reference for others)
* `7 - 17`
* `7 * 6`
* `7 / 2` ? Surprising? Huh
* `7 // 2` - integer division, divide and round down
* `-7 // 2` - // always rounds down
* `2 **5`
* `2 ** -5`
* `.5 ** 5`

* Most of these are obvious
* `33 % 5` - 33 modulo 5, remainder after dividing by 5 
> 33 % 5 = 33 - 5 * (33 // 5)

* `6 + 7 * 10` - not 130
* Not evaluated left to right, operators have different precedence 
> https://docs.python.org/3.3/reference/expressions.html#operator-precedence

* If any doubt, or complex expression, use parentheses ()
* `(6+7)*10`
* Even when no doubt, parentheses make intent clear to other

### Variables

* Used to store literals and results of expressions
* `a = 7` - a a variable, 7 a literal
* `b=6`
* `a + b` - a + b an expression, result not stored anywhere
* `c=a+b` 
* `c`
* `c=c+1`
* `c`
* `c += 1`
* `c`

* Use print() function to print output
* `print(c)`
* `print("The value is", c, "and that"s a fact")` - adds spaces
* `d`
* None is a special symbol use to indicate variable does not refer to any value
* `p=None`
* `p`
* `print(p)`
* Note: variable names are case sensitive

### Floating point warning

* Floating point is one that"s not a whole number
* `a=10/9`
* `a`
* `b=0`
* `b += a` - repeat this 9 times
* `a*9`
* `b`
* Be careful when comparing them. Typically check numbers are close enough e.g. is 10 - b < 0.0001 
> https://en.wikipedia.org/wiki/Machine_epsilon

### Strings

* `"hello there"` - this is a string literal
* `"hello" + " " + "there"` - concatenate with +
* `"hip "* 2 + "hooray"`
* `name = "Tom Thumb"`
* `name[0]`
* `name[2:6]` - note character 6 not included
> dir(str) for more operations.

### Quoting

* How do we print the " character?
* Choice of quotes ' or ", if need to print one use the other
* `print("'")`
* `print('"')`
* If need to print both, escape with `\`
* Or use `"""`, also for multiline strings
* `print('He said "It\'s Tom\'s"')`
```
print("""He said "It's Tom's".
She replied, "It is in my hat\"""")
```
* One more way we'll get to later

### Comparisons, conditions

* `6 > 7` - note the answer
* Boolean values are True or False
* `6 == 7`
* `6 != 7`
* combine expressions with `and` and `or`
* `age=21`
* `age < 18 or age > 65` - not of working age 
* `age >= 18 and age <= 65` - working age 
* `18 <= age <= 65` - same, but shorter
* `check=6==7`
* `check`
* `name = "Tom Thumb"` 
* `"T" in name`
* `"Z" in name`
* `"Z" not in name`
* `"a" < "b"`
* `"a" < "B"`

### Types

* `name == "Tom Thumb"`
* `numstr = "7"`
* `numstr == 7`
* `7 and "7"` are different types, not necessarily comparable
* So far we've seen the types bool, int, float, str
* `type()` shows you the type of an object
* `type("7")`
* `type(7)`
* `type(False)`

* We can convert from one type to another
* `num = int(numstr)`
* `num==7`
* Or the other other way
* `numstr == str(7)`
* Works on numeric types too:
* `rough_balance = int(balance)`
* `balance = 22.57`
* And on boolean types:
* `int(True)` - note True maps to 1. Success maps to 0 in the shell 
* `int(name)`

### dir

* `dir()` displays all defined variables
* `dir()`
* dir(type) lists the methods you can call on an object dir (str) - notice find() is there
* Can also call on a literal or variable
* `dir ("Tom Thumb")` - notice find() is there
* `dir(age)`
* Useful later

## Data Structures
* So far only looking at handling single values
* Data Structures let us handle multiple values 
* Different structures better for different purposes
* We'll look at list, tuple, range, dictionary, set 
* Sequence types, elements in order - list, tuple, range 
* Mutable / Immutable - list, dict, set / tuple, range

### Sequence Types

* a list is a mutable sequence of objects
  * `fruits = ["apple", "orange", "banana", "tomato"]`
* a tuple is an immutable sequence of objects
  * `names = ("James", "Bond")`
  * `names = ("James",)` - for single element tuple •
* a range represents an immutable sequence of numbers
  * `working_ages = range(18, 66)`
  * ranges generate the numbers in sequence as required

* These work on all sequence types, but we'll use list They work on strings too, which are also sequences
* `names = ["Ann", "Barry", "Carrie"]`
* `names`
* `names[0]`
* `names[-1]`
  * `names[0:2]` - take a slice. note names[2] not included
* `names[0:]`
* `names[:2]`
* `names[:]`
  * `names[0:3:2]` - can step over objects

* `more_names = ["Derek", "Evie", "Vlad", "Ann"]`
* `all_names = names + more_names` - can concatenate sequences with +
* `names * 2`
* Can count number elements
* `all_names.count("Ann")`
* `all_names.count("Tom")`
* Can find position of an object in the sequence
* `names.index("Barry")`
* `names.index("Tim")` - produces an error. We'll learn how to deal with these later in the course

* Sequence can contail objects of different type
* `mixed_list = (True, "Ann", 0, None)`
* Some operations only work if all objects can be compared to each other
* `max(all_names)`
* `min(all_names)`
  * `max(mixed_list)` - error, can't compare all members
  * Can check whether an object in sequence
* `"Ann" in names`
* `"Tim" in names`
* `"Tim" not in names`

* Can compare sequences of the same type for equality and order
* When checking order, elements at same position
* Like with individual letters in a string compared
* `other_names=['Ann','Barry','Daniel',"Ed"]`
* `names==other_names`
* `names < other_names` - because "Carrie" < "Daniel"

### Mutable Sequences

* These apply to list, but not tuple, range or string
> And others e.g. bytearrays. See https://docs.python.org/3.5/library/stdtypes.html)

* `names[0] = "Anne"`
* `names`
* ... let's make things a bit easier on ourselves 
```
Open PyCharm
Tools -> Python Console
Click icon on left with tooltip "Show Variables"
```
* `names = ["Ann", "Barry", "Carrie"]`
* `names[0] = "Anne"`
* `names.append("Ann")` - changes list, not like +
* `names.remove("Barry")`
* `names.insert(2, "Barry")`
* `names.remove("Bruce")` - error if not present 
* `names.pop(1)` - remove and return particular element
* `name.pop()` - remove and return last element

* Can also insert and delete with slices
* `names[1:1] = ["Fred"]`
* `names[1:2] = []`
* `names[1:2] = ["Alf", "Henry"]`
* Can also delete item at a particular position
* `del names[0]`
* Can delete whole list (or any variable)
* `del names`

* Can sort items, if they are all comparable
* `names.sort()`
* Can reverse too
* `names.reverse()`
* Note, these modify the list. Sometimes you'll want to keep the original too
* `cp_names = names`
* `names.reverse()` - what happened here?

### Diversion - objects and references

* Both variables are references to the same object 
* Changes to the object are visible through both references
```
names    ====> ["Alf", "Anne", "Henry"]
         
cp_names ====> ["Alf", "Anne", "Henry"]
```
* There can be many references to the same object
* `cp_names = []` - makes a new list and makes cp_names refer to it 
* `cp_names = names.copy()` - can also do 
* `cp_names = names[:]`
* `names.append("Richard")` - cp_names unaffected

* Can put data structures inside other data structures
* `fruits = ['orange', 'lemon', 'apple', 'banana']`
* `inventory = ["hammer", "lemonade", fruits]`
* `inventory[2]`
* `inventory[2][2]`
* lists of lists allow us to do multidimensional modelling e.g. we could have chaseboard[row][col]
* `cp_inventory = inventory.copy()`
* `inventory.append("cheese")` - so far so good 
* `inventory[2][2] = "melons"` - what happened here?

* Can put data structures inside other data structures
```
inventory    ============> ['hammer', 'lemonade', * , 'cheese']
					          *====>['orange', 'lemon', 'melons', 'banana']
cp_inventory ============> ['hammer', 'lemonade', * ]
```

* `copy()`  made a new list with references to the same elements 
* This is called a `shallow copy`
* To make a `deep copy`, with copies of each element
> See https://docs.python.org/3.5/library/copy.html

### Sets

* A set is a group of items, with no duplicates, in no particular order. It is mutable. 
* `acting_awards = {"Oscar", "Emmy"} `
* `acting_awards.add("Tony")`
* `acting_awards.add("Oscar")`- no effect 
* `len(acting_awards)` - num of elements 
* `min(acting_awards) `
* `"Oscar" in acting_awards `
* `"Grammy" not in acting_awards`

* The usual mathematical set operations supported 
* `music_awards = { "Grammy" }`
* `showbiz_awards = acting_awards.union(music_awards)`
* `showbiz_awards.intersection(music_awards)`
* `acting_awards.isdisjoint(music_awards)`
* `music_awards.issubset(showbiz_awards)`
* `acting_awards.remove("Tony")`- to remove an element 
* Can compare sets with == but not < or > etc 
* Python supports immutable sets too, with frozensets

### Dictionaries

* A mutable collection of key, value mappings 
* `animal_defs = {"aardvark" : " a snouty creature", "badger" : "a large striped dog"}`
* `animal_defs["badger"] `
* `animal_defs["weasel"]` - gives an error
* `"weasel" in animal_defs`
* Can specify a default to use if not in dictionary 
* `animal_defs.get("weasel", "some creeping, crawling thing")`
* Can add a new item 
* `animal_defs["giraffe"] = "a patchy animal with a long neck"`

* To overwrite an item just assign using same key
* `animal_defs["giraffe"] = "a tall animal"`
* To remove an item use `del`
* `del animal_defs["giraffe"]`
* Can also create a dictionary this way:
* `person = dict(name="Tom", age=15, team="Blue") `
* Dictionaries can be compared using == but not >, < etc

* We've used strings for keys and values 
* Can use `any object` as a value 
* Can use `any hashable` object as a key 
* Typically immutable objects are hashable and mutable ones are not 
* So can e.g. use tuples as keys, dictionaries as values 
* `db = { ("James", "Bond"): { "license": "007", "occupation": "Secret Agent"} }`
* `db[("James", "Bond")]`
* `db[("James", "Bond")]["license"]`
