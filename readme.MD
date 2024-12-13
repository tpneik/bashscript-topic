# Bashscript learning notes

## Syntax

### Shebang

```bash
#!/usr/bin/bash
```
This is the first line every bash script. This mean that which environment the bash gonna run in? Ex: /bin/bash or /bin/sh or ...

### Each command starts on a new line, or after a semicolon

Like this: 

```bash
echo "This is the first command"; echo "This is the second command"
```
### Variable declaration:

```bash
variable="Some string"
```

and also it is not this
```bash
variable = "Some string"
```
### To use the variable

#### Treat variable as tring
```bash
echo "$variable" # => Print the valuable of the variable "variable"
echo '$variable' # => Print the string "$variable"
```
The "  **'**  " is will not help the $variable to expand to the variable's value.

If you use:

```bash
echo $variable 
```
It will treat all the element seperated by space individually. How is it?
```bash
[1] [tpnei@localhost Desktop]$ file_name="my important document.txt"
# Your string is seperated by space.
[2] [tpnei@localhost Desktop]$ echo "file_name"
file_name
# If you print it as normal, you gonna see no differences between it anymore!
[3] [tpnei@localhost Desktop]$ echo "$file_name"
my important document.txt
# If you print it as the variable's value, you gonna see in here I use the '$' before it.
[4] [tpnei@localhost Desktop]$ mv $file_name $file_name.bak
mv: target 'document.txt.bak' is not a directory
```
What happens with it? Because our file is intended to have a name as *"my important document.txt"* (with space in side). Look at the **mv** command, It have a syntax as 
```bash
mv file1 file2 filen ... target_place
```
See how we use the command:

The first argument in [4] is $file_name, as I mention before, the $file_name will treat the element inside seperately by the space. So the argument will be treat as this 

```bash
mv my important document.txt my important document.txt.bak
```
Yas, of couse the **document.txt.bak** no exist anymore! So the result is error.

Now look at the right thing!

```bash
[tpnei@localhost Desktop]$ touch "$file_name"
[tpnei@localhost Desktop]$ ls
'my important document.txt'
[tpnei@localhost Desktop]$ mv "$file_name" "$file_name".bak
'my important document.txt.bak'
```



#### Parameter expansion: 
**Taking account to the term** expansion :
In the context of shell scripting, "expansion" means taking a variable, expression, or pattern and converting it into its actual/final value. Think of it like "expanding" a compressed file or "expanding" an abbreviation into its full form.

```bash
echo "${variable}"
```
For a simpler thing, you could understand it as you can execute things in side the **{ }**

```bash
[tpnei@localhost Desktop]$ name="John Smith"
# We declare the variable "name"
[tpnei@localhost Desktop]$ echo "${#name}"
# We can understand it as bash will execute the # on the variable name
# In this situation it is count the characters of the variable contained.

## The output:
10
```

#### Another example:

```bash
[tpnei@localhost Desktop]$ variable="Some string"
[tpnei@localhost Desktop]$ echo "${variable/Some/A}"
# In this situation, this execution will be take the first "Some" of the variable, and replace with the A, like the above
A string

[tpnei@localhost Desktop]$ echo "${variable/Some/'Ahaha hheeheh'}"
# You also can replace with the string with space, put it into the ''
Ahaha hheeheh string
```

#### Another with Substring from a variable, the syntax is
```bash
${variable:start_index:end_index}
```

```bash
 variable="Some string"
# declare length with the value is 7
length=7
[tpnei@localhost Desktop]$ echo "${variable:0:length}"
Some st

# You also can strip the string reversively (Remember to have the space)
[tpnei@localhost Desktop]$ echo "${variable: -5}"
tring
```

#### Indirect expansion:
```bash
# First variable contains value
variable="Hello World"

# Second variable contains first variable's name
other_variable="variable"

# Using ${!other_variable} to get value through name
echo ${!other_variable}   # Output: Hello World
```
#### Set default value for variable:

```bash
[tpnei@localhost Desktop]$ foo="hheehThisIsFoo"
[tpnei@localhost Desktop]$ echo "$foo"
hheehThisIsFoo
# Ex: The scenario is to have the foo as empty value
[tpnei@localhost Desktop]$ unset foo
[tpnei@localhost Desktop]$ echo "$foo"
## It printed nothing!
[tpnei@localhost Desktop]$
# Use the :-"The thing we want to be as Default"
[tpnei@localhost Desktop]$ echo "${foo:-"ThisIsFvckinDefaultValues"}"
ThisIsFvckinDefaultValues
```


#### Declare an array

```bash
# array=(one two three four five six)
[tpnei@localhost Desktop]$ array=(one two three four five six)

# To use one of it, we must use the element, put it into string expansion ${}
[tpnei@localhost Desktop]$ echo "${array[0]}"
one

# You also can work as this 
[tpnei@localhost Desktop]$ array=(one two three four five "six nine six nine")
[tpnei@localhost Desktop]$ echo "${array[5]}"
six nine six nine
```

Also can:

```bash
array=(one two three four five six)
# Print the first element:
echo "${array[0]}" # => "one"
# Print all elements:
echo "${array[@]}" # => "one two three four five six"
# Print the number of elements:
echo "${#array[@]}" # => "6"
# Print the number of characters in third element
echo "${#array[2]}" # => "5"
# Print 2 elements starting from fourth:
echo "${array[@]:3:2}" # => "four five"
# Print all elements each of them on new line.
for item in "${array[@]}"; do
    echo "$item"
done

```
We also can take the output of the things to array, like this:
```bash
[tpnei@localhost Desktop]$ myfolder=($(ls))
[tpnei@localhost Desktop]$ echo "${myfolder[0]}"
aa
[tpnei@localhost Desktop]$ echo "${myfolder[@]}"
aa bb cc
[tpnei@localhost Desktop]$ 
```
#### Comparision

##### Interger Comparision

## Use case

### Use case with loop over folder

```bash
# I have a folder like this 
[tpnei@localhost Desktop]$ ls
aa  bb  cc  check_if_filename.sh

# I use this bash script
########################
dir="."
for item in "$dir"/*; do
    echo "$item"
done
########################
# Execute:
[tpnei@localhost Desktop]$ ./check_if_filename.sh 
./aa
./bb
./cc
./check_if_filename.sh

# If I change 
########################
dir="./aa"
for item in "$dir"/*; do
    echo "$item"
done
########################
[tpnei@localhost Desktop]$ ./check_if_filename.sh 
./aa/a_aa
./aa/my important document.txt.bak
```
Or
```bash
find "$search_dir" -type f -o -type d
```

**find**: The command to search for files
"$search_dir": The directory to start searching from
-type f: Find files (f means regular file)
-o: Logical OR operator
-type d: Find directories (d means directory)


#### You have a file and want to read this file line by line
```bash
value="hehe_k.txt"
while read -r line; do
    echo "$line"
done < "$value"
```
#### List all file and folder, print out folder and file
```bash
find "$dir" -type f -o -type d | while read -r line; do
    if [ -d "$line" ]; then
        echo "$line is dir"
    fi
    if [ -f "$line" ]; then
        echo "$line is file"
    fi
done
```

The output:
```bash
[tpnei@localhost Desktop]$ ./check_if_filename.sh 
. is dir
./aa is dir
./aa/my important document.txt.bak is file
./aa/a_aa is dir
./bb is dir
./bb/file_b.txt is file
./cc is dir
./check_if_filename.sh is file
./hehe_k.txt is file
```
#### String comparision

```bash
# Basic equality
string1="hello"
string2="hello"
string3="world"

# Equal to
[[ "$string1" == "$string2" ]]  # true
[[ "$string1" = "$string2" ]]   # true (single = also works)

# Not equal to
[[ "$string1" != "$string3" ]]  # true

# Empty string check
emptyString=""
[[ -z "$emptyString" ]]         # true if empty
[[ -n "$string1" ]]             # true if not empty

# Alphabetical comparison
[[ "abc" < "def" ]]             # true
[[ "def" > "abc" ]]             # true

# Pattern matching
[[ "hello.txt" == *.txt ]]      # true
[[ "hello" == h* ]]             # true
[[ "hello" == h??? ]]          # true (? matches single character)

# Regular expression matching
[[ "hello123" =~ ^[a-z]+[0-9]+$ ]]  # true
[[ "hello" =~ ^h.+o$ ]]             # true

# Examples with if statements
if [[ "$string1" == "$string2" ]]; then
    echo "Strings are equal"
fi

if [[ -z "$emptyString" ]]; then
    echo "String is empty"
fi

if [[ "file.txt" == *.txt ]]; then
    echo "This is a text file"
fi
```

#### Check if a file appear is the dir
```bash
find '.' -type f -o -type d | grep 'a_aa' || echo "true"
```

#### Think of when you have a need to write a function to return a value, you basically echo the value and stop doing anything, it is equivalent to return in python. 

#### Get input 

```bash
echo "What's your name?"
read name
# Note that we didn't need to declare a new variable.
echo "Hello, $name!"
```

#### cut and head
The cut -d command is used to extract sections from each line of files using a delimiter.
. Let me explain with examples:
```bash
cut -d' ' -f2
```
This mean the delimiter is space ' ' and take the '2' second element. I have the data like this, I used curl:

```bash
[tpnei@localhost Desktop]$ x=$(curl -I -s https://www.google.com)
[tpnei@localhost Desktop]$ echo "$x"

HTTP/2 200  <-----------The target line

content-type: text/html; charset=ISO-8859-1
content-security-policy-report-only: object-src 'none';base-uri 'self';script-src 'nonce-IHcKZqc4CUYx1IVV1lBRHw' 'strict-dynamic' 'report-sample' 'unsafe-eval' 'unsafe-inline' https: http:;report-uri https://csp.withgoogle.com/csp/gws/other-hp
accept-ch: Sec-CH-Prefers-Color-Scheme
p3p: CP="This is not a P3P policy! See g.co/p3phelp for more info."
date: Mon, 09 Dec 2024 04:57:28 GMT
server: gws
x-xss-protection: 0
x-frame-options: SAMEORIGIN
expires: Mon, 09 Dec 2024 04:57:28 GMT
cache-control: private
set-cookie: AEC=AZ6Zc-Ug73b_CG8kxe3y0Dbkn9BuuamNvhX9jObVTe7958IlRXnarIf68A; expires=Sat, 07-Jun-2025 04:57:28 GMT; path=/; domain=.google.com; Secure; HttpOnly; SameSite=lax
set-cookie: NID=519=dQgrMwaL4bDOkUnD3fEdyIuogJ2JBgHHmji7CwD3Y6S6Ef3FrsexTxy78fhKko6FOJLaqr8gFBO-OxeObMnrd5sJsS07BRDU1cs9M96HB5DF1dalquzwVfF7Sa5WFTOGeLai_X8sTWv5aNQx2FiPTq41GbfGD6GOkf1vrWpTT2z-9ZwN2rRoT2TRoE8BNEA; expires=Tue, 10-Jun-2025 04:57:28 GMT; path=/; domain=.google.com; HttpOnly
alt-svc: h3=":443"; ma=2592000,h3-29=":443"; ma=2592000
```
If I want to get the "HTTP/2 200", there is many way to do:

- Get the first line, and get the second element of the string, deliminated by space ' '
    ```bash
    # echo "$( echo "$x" | head -n 1 | cut -d' ' -f2)"
    [tpnei@localhost Desktop]$ echo "$(echo "$x"| head -n 1 | cut -d' ' -f2)"
    200
    ```
- If you want to get the second line, or third line, or n line, and the m element by space ' ' 
    ```bash
    # echo "$x"| head -n *n* | tail -n 1 | cut -d' ' -f*m*
    # If you want to get the 3rd line and get the 3rd element it would be
    echo "$( echo "$x" | head -n 3 | tail -n 1 | cut -d' ' -f3)"
    ```
- Other wise, you also can get the other delimiter like '**,**' or '**:**' or any


#### 

## Regex in Bash 

## Code template

## References
[Use Bash Strict Mode](http://redsymbol.net/articles/unofficial-bash-strict-mode/)

[Learn X in Y minutes](https://learnxinyminutes.com/bash/)

[Bashscript cheatsheet](https://devhints.io/bash)