# BashScript



## Table of Contents

1. [Introduction to Bash Scripting](#introduction-to-bash-scripting)

2. [Understanding Variables in Bash](#understanding-variables-in-bash)

3. [Using `read` to Read Input from the User](#using-read-to-read-input-from-the-user)

4. [Using the `test` Command](#using-the-test-command)

5. [The `if` Statement in Bash](#the-if-statement-in-bash)

6. [Loops in Bash](#loops-in-bash)

7. [Arrays in Bash Scripts](#arrays-in-bash-scripts)

8. [Using `expr` in Bash](#using-expr-in-bash)

9. [Arguments in Bash Scripting](#arguments-in-bash-scripting)

10. [Syntax of `case` Statement in Bash](#syntax-of-case-statement-in-bash)

11. [Syntax of `select` Statement in Bash](#syntax-of-select-statement-in-bash)

12. [Handling Strings in Bash Scripts](#handling-strings-in-bash-scripts)

13. [Syntaxes for Defining Functions in Bash](#syntaxes-for-defining-functions-in-bash)

14. [Understanding `local` Variables in Bash](#understanding-local-variables-in-bash)



## Introduction to Bash Scripting

### What is a Bash Script?

A Bash script is a plain text file containing a series of commands that the Bash shell can execute. Bash, short for "Bourne Again SHell," is a command processor that typically runs in a text window where the user types commands that cause actions. A script automates tasks that could be performed at the command line, saving time and ensuring consistency.

### Why Learn Bash Scripting?

Learning Bash scripting is particularly beneficial for those working with embedded Linux systems for several reasons:

1. **Automation**: Automate repetitive tasks to save time and reduce errors.
2. **Efficiency**: Quickly perform complex operations and handle batch processing.
3. **System Initialization**: Manage and configure startup scripts and services, especially relevant in systems using init systems like System V (`sysvinit`).

### Writing a Bash Script

1. **Create a new file**: Use a text editor like `nano`, `vim`, or `gedit` to create a new file with a `.sh` extension. For example, `myscript.sh`.

2. **Add the Shebang Line**: The first line of the script should be the shebang (`#!`) followed by the path to the Bash interpreter. This tells the system to use Bash to execute the script.

   ```bash
   #!/bin/bash
   ```

3. **Write Your Commands**: Add the commands you want to execute in the script.

   ```bash
   #!/bin/bash
   echo "Hello, World!"
   ```

### Executing a Bash Script

There are several ways to execute a Bash script:

1. **Using the `bash` Command**: You can execute the script by explicitly calling the Bash interpreter.

   ```bash
   bash ./myscript.sh
   ```

2. **Making the Script Executable**: First, make the script executable using the `chmod` command, then run it directly.

   ```bash
   chmod +x myscript.sh
   ./myscript.sh
   ```

3. **Using the `source` Command**: Execute the script in the current shell environment, useful for scripts that modify the environment.

   ```bash
   source myscript.sh
   ```

4. **Using the Dot (`.`) Command**: This is shorthand for the `source` command and has the same effect.

   ```bash
   . myscript.sh
   ```



---



## Understanding Variables in Bash

Variables in Bash are used to store data that can be referenced and manipulated throughout a script. Here’s how to define and use them:

1. **Defining a Variable**: Variables are defined without spaces around the equal sign.

   ```bash
   name=mostafa
   ```

2. **Rules for Variable Names**:

   - Variable names can include letters, numbers, and underscores.
   - Variable names must start with a letter or an underscore.
   - Variable names are case-sensitive.
   - Avoid using special characters and reserved words.

3. **Using Variables**: To access the value of a variable, prefix its name with a dollar sign (`$`).

   ```bash
   echo "My name is $name"
   ```

4. **Curly Braces for Variable Names**: Curly braces `{}` are used to clearly delimit the variable name from surrounding text.

   ```bash
   echo "My full name is ${name}Tera"
   ```

5. **Command Substitution**: Use the `$(command)` syntax to assign the output of a command to a variable.

   ```bash
   var=$(date)
   echo "The date is: $var"
   ```

### Difference Between Double Quotes `"` and Single Quotes `'`

1. **Double Quotes `"`**: Enclose the text in double quotes to allow the evaluation of variables and command substitution.

   ```bash
   echo "My name is $name" # Output will be My name is mostafa
   ```

2. **Single Quotes `'`**: Enclose the text in single quotes to prevent the evaluation of variables and command substitution.

   ```bash
   echo 'My name is $name' # Output will be My name is $name
   ```





---



### Using `read` to Read Input from the User

The `read` command in Bash is used to read input from the user. It reads a line of text from standard input and assigns it to one or more variables.

1. **Basic Usage**:

   ```bash
   read variable_name
   ```

2. **Prompting the User**: Use the `-p` flag to display a prompt before reading the input.

   ```bash
   read -p "Enter your username: " userName
   echo "Your username is: $userName"
   ```

3. **Silent Input**: Use the `-s` flag to hide the input, useful for sensitive information like passwords.

   ```bash
   read -sp "Enter your password: " password
   echo " "
   echo "Your password is saved successfully!"
   ```

4. **Reading Multiple Values**: You can read multiple values into separate variables.

   ```bash
   read var1 var2
   ```



### Example Script

Here is an example script demonstrating the use of the `read` command:

```bash
#!/bin/bash

# ========== Bash script to read the user's username and password ===========

# Read input from user
read -p "Enter your username please: " userName
echo "Your username is: $userName"

read -sp "Enter your password please: " password
echo " "
echo "Your password is saved successfully!"
```



---



## Using the `test` Command

The `test` command is used to evaluate conditional expressions. It checks file types, compares strings and numbers, and more. The `test` command is often used in shell scripts for conditional execution.

1. **Syntax**:

   ```bash
   test expression
   ```

   or equivalently,

   ```bash
   [ expression ]
   ```

   Note: When using `[ ]`, spaces are required around the brackets.

2. **Types of Conditions**:

   - **Numeric Conditions**: Compare numbers.
   - **String Conditions**: Compare strings
   - **File Conditions**: Check file types and attributes.



### Numeric Conditions

| Condition       | Description                                   |
| :-------------- | --------------------------------------------- |
| `num1 -eq num2` | True if the numbers are equal                 |
| `num1 -ne num2` | True if the numbers are not equal             |
| `num1 -lt num2` | True if num1 is less than num2                |
| `num1 -le num2` | True if num1 is less than or equal to num2    |
| `num1 -gt num2` | True if num1 is greater than num2             |
| `num1 -ge num2` | True if num1 is greater than or equal to num2 |

### String Conditions

| Condition      | Description                       |
| -------------- | --------------------------------- |
| `str1 = str2`  | True if the strings are equal     |
| `str1 != str2` | True if the strings are not equal |
| `-z str`       | True if the string is empty       |
| `-n str`       | True if the string is not empty   |

### File Conditions

| Condition | Description                    |
| --------- | ------------------------------ |
| `-e file` | True if file exists            |
| `-f file` | True if file is a regular file |
| `-d file` | True if file is a directory    |
| `-r file` | True if file is readable       |
| `-w file` | True if file is writable       |
| `-x file` | True if file is executable     |

### Logical Operators

To combine multiple conditions, use logical operators:

- **AND**: `&&` or `-a`
- **OR**: `||` or `-o`

### Combining Conditions

There are three syntaxes for combining conditions:

1. **Using `[ ] [ ]`**:

   ```bash
   [ condition1 ] && [ condition2 ]
   [ condition1 ] || [ condition2 ]
   ```

2. **Using `[[ ]]`**:

   ```bash
   [[ condition1 && condition2 ]]
   [[ condition1 || condition2 ]]
   ```

3. **Using `[ ]` with `-a` and `-o`**:

   ```bash
   [ condition1 -a condition2 ]
   [ condition1 -o condition2 ]
   ```



### Examples

#### Syntax 1 

```bash
[ 1 -eq 1 ] && [ 10 -gt 5 ]
echo $?
```

- Explanation:
  - `[ 1 -eq 1 ]` checks if 1 is equal to 1. This is true.
  - `[ 10 -gt 5 ]` checks if 10 is greater than 5. This is true.
  - `&&` is a logical AND operator, meaning both conditions must be true for the whole expression to be true.
  - `echo $?` prints the exit status of the last command executed. An exit status of 0 means the command was successful.
- **Output**: `0`

#### Syntax 2

```bash
[[ 1 -eq 1 && 10 -gt 20 ]]
echo $?
```

- Explanation:
  - `[[ 1 -eq 1 && 10 -gt 20 ]]` uses double brackets, which is another way to perform conditional tests in Bash.
  - `1 -eq 1` is true.
  - `10 -gt 20` is false.
  - `&&` requires both conditions to be true.
  - The whole expression is false because one condition is false.
  - `echo $?` prints the exit status of the last command executed. An exit status of 1 means the command was unsuccessful.
- **Output**: `1`

#### Syntax 3

```bash
[ 1 -eq 1 -a 10 -gt 5 ]
echo $?
```

- Explanation:
  - `[ 1 -eq 1 -a 10 -gt 5 ]` uses the `-a` operator for logical AND.
  - `1 -eq 1` is true.
  - `10 -gt 5` is true.
  - Both conditions are true.
  - `echo $?` prints the exit status of the last command executed. An exit status of 0 means the command was successful.
- **Output**: `0`

#### Test Command with `-a`

```bash
num=1
string="ahmed"

test "$num" -eq 1 -a "$string" = "notAhmed"
echo $?
```

- Explanation:
  - `test "$num" -eq 1 -a "$string" = "notAhmed"` uses the `-a` operator for logical AND.
  - `"$num" -eq 1` checks if the variable `num` is equal to 1. This is true.
  - `"$string" = "notAhmed"` checks if the variable `string` is equal to "notAhmed". This is false.
  - Since one condition is false, the whole expression is false.
  - `echo $?` prints the exit status of the last command executed. An exit status of 1 means the command was unsuccessful.
- **Output**: `1`

#### Using Test Command with `-a`

```bash
num=1
string="ahmed"

test "$num" -eq 1 -a "$string" = "ahmed"
echo $?
```

- Explanation:
  - `test "$num" -eq 1 -a "$string" = "ahmed"` uses the `-a` operator for logical AND.
  - `"$num" -eq 1` checks if the variable `num` is equal to 1. This is true.
  - `"$string" = "ahmed"` checks if the variable `string` is equal to "ahmed". This is true.
  - Both conditions are true.
  - `echo $?` prints the exit status of the last command executed. An exit status of 0 means the command was successful.
- **Output**: `0`

#### Using Test Command with `&&`

```bash
num=1
string="ahmed"

test "$num" -eq 1 && test "$string" = "ahmed"
echo $?
```

- Explanation:
  - `test "$num" -eq 1` checks if the variable `num` is equal to 1. This is true.
  - `&&` is a logical AND operator, meaning both conditions must be true for the whole expression to be true.
  - `test "$string" = "ahmed"` checks if the variable `string` is equal to "ahmed". This is true.
  - Both conditions are true.
  - `echo $?` prints the exit status of the last command executed. An exit status of 0 means the command was successful.
- **Output**: `0`



---

## The `if` Statement in Bash

The `if` statement is used to test a condition and execute a block of code based on whether the condition is true or false. It is a fundamental control structure in Bash scripting.

#### Syntax of `if`

```bash
if [ condition ]; then
    # commands to execute if the condition is true
fi
```

#### Extended Syntax with `else` and `elif`

```bash
if [ condition ]; then
    # commands to execute if the condition is true
elif [ another_condition ]; then
    # commands to execute if the another_condition is true
else
    # commands to execute if none of the above conditions are true
fi
```

#### Points to Note

- Conditions are enclosed in square brackets `[` and `]`, with spaces around the condition.
- The `then` keyword starts the block of code to execute if the condition is true.
- The `fi` keyword ends the `if` block.
- `elif` allows for additional conditions to test if the first condition is false.
- `else` provides a default block of code to execute if none of the conditions are true.

### Explanation of Examples and Outputs

1. **Checking if the string is empty**:

   ```bash
   read -rp "Enter a string: " string
   
   if [ -z "$string" ]; then
       echo "The string is empty"
   else
       echo "The string is: $string"
   fi
   ```

   - If the user inputs an empty string, the output will be "The string is empty."
   - If the user inputs a non-empty string, the output will be "The string is: [user's input]."

2. **Using OR operator**:

   ```bash
   if [[ 1 -eq 1 || $string = "ahmed" ]]; then
       echo "hello tera"
   fi
   ```

   - The condition `1 -eq 1` is true, so the output will be "hello tera."

   ```bash
   if [[ 001 -eq 1 || 10 -gt 5 ]]; then
       echo "hello tera"
   fi
   ```

   - The condition `001 -eq 1` is true (since leading zeros are ignored in numeric comparison), so the output will be "hello tera."

3. **Using AND operator**:

   ```bash
   if [[ 1 = 1 && 10 -gt 5 ]]; then
       echo "hello tera"
   fi
   ```

   - Both conditions are true, so the output will be "hello tera."

4. **Using `test` command with `-a` (logical AND)**:

   ```bash
   num=1
   string="ahmed"
   if test "$num" -eq 1 -a "$string" = "ahmed"; then
       echo "Both conditions are true."
   else
       echo "One or both conditions are false."
   fi
   ```

   - Both conditions are true, so the output will be "Both conditions are true."

5. **Using separate `test` commands with `&&`**:

   ```bash
   num=1
   string="ahmed"
   if test "$num" -eq 1 && test "$string" = "ahmed"; then
       echo "Both conditions are true."
   else
       echo "One or both conditions are false."
   fi
   ```

   - Both conditions are true, so the output will be "Both conditions are true."



---



## Loops in Bash

Loops are fundamental control structures that allow repeated execution of a block of code. Bash supports two main types of loops: `for` loops and `while` loops.

### `for` Loop

The `for` loop iterates over a list of items, executing a block of code for each item.

#### Syntax of `for` Loop

```bash
for variable in list
do
    # commands to execute
done
```

#### Example Scripts and Explanation

##### Example 1: Basic `for` Loop

```bash
#!/bin/bash

# ================= for loops ===================

# --- Example 1 ---
for i in 1 2 3 4 5; do
    echo $i
done
```

- Explanation:

  - The loop iterates over the list `1 2 3 4 5`.
  - For each iteration, the value of `i` is set to the current item in the list.
  - `echo $i` prints the value of `i`.

- Output:

  ```bash
  1
  2
  3
  4
  5
  ```

##### Example 2: Range in `for` Loop

```bash
#!/bin/bash

# --- Example 2 ---
for i in {1..9}; do
    echo $i
done
```

- Explanation:

  - The loop iterates over the range `{1..9}`.
  - For each iteration, the value of `i` is set to the current number in the range.
  - `echo $i` prints the value of `i`.

- Output:

  ```bash
  1
  2
  3
  4
  5
  6
  7
  8
  9
  ```

##### Example 3: Iterating Over Strings

```bash
#!/bin/bash

# --- Example 3 ---
string="mostafa tera"
for i in "mostafa tera" "mohamed" "ahmed"; do
    echo $i
done
```

- Explanation:

  - The loop iterates over the list of strings `"mostafa tera" "mohamed" "ahmed"`.
  - For each iteration, the value of `i` is set to the current string in the list.
  - `echo $i` prints the value of `i`.

- Output:

  ```bash
  mostafa tera
  mohamed
  ahmed
  ```

### `while` Loop

The `while` loop executes a block of code as long as a specified condition is true.

#### Syntax of `while` Loop

```bash
while [ condition ]
do
    # commands to execute
done
```

#### Example Script and Explanation

##### Example 1: Basic `while` Loop

```bash
#!/bin/bash

# ================= While loops ===================

# --- Example 1 ---
x=1
y=3

while [[ $x != 6 && $y -ge 0 ]]; do
    echo "x= $x"
    echo "y= $y"
    ((x++))
    ((y--))
done
```

- Explanation:

  - `x=1` initializes `x` to 1.
  - `y=3` initializes `y` to 3.
  - `while [[ $x != 6 && $y -ge 0 ]]` checks if `x` is not equal to 6 and `y` is greater than or equal to 0.
  - The loop continues as long as both conditions are true.
  - `echo "x= $x"` prints the value of `x`.
  - `echo "y= $y"` prints the value of `y`.
  - `((x++))` increments `x` by 1.
  - `((y--))` decrements `y` by 1.

- Output:

  ```bash
  x= 1
  y= 3
  x= 2
  y= 2
  x= 3
  y= 1
  x= 4
  y= 0
  ```



---



## Arrays in Bash Scripts

Arrays in Bash allow you to store multiple values in a single variable. Bash supports both indexed arrays (numerically indexed) and associative arrays (key-value pairs).

#### 1. Indexed Arrays

Indexed arrays use sequential numbers as indexes starting from 0.

##### Example 1: Create and Access Arrays

```bash
#!/bin/bash

# ---- Example 1: Create and Access Arrays ----
echo "1. Create and Access Arrays"
cities=("New York" "Los Angeles" "Chicago" "Houston" "Phoenix")
echo "First city: ${cities[0]}"
echo "Last city: ${cities[4]}"
```

- Explanation:

  - `cities=("New York" "Los Angeles" "Chicago" "Houston" "Phoenix")` initializes an indexed array named `cities`.
  - `${cities[0]}` accesses the first element of the array (`"New York"`).
  - `${cities[4]}` accesses the fifth element of the array (`"Phoenix"`).

- Output:

  ```bash
  1. Create and Access Arrays
  First city: New York
  Last city: Phoenix
  ```

##### Example 2: Modify Array Elements

```bash
# ---- Example 2: Modify Array Elements ----
cities=("New York" "Los Angeles" "Chicago" "Houston" "Phoenix")
echo "2. Modify Array Elements"
cities[2]="San Francisco"
echo "Cities: ${cities[@]}"
```

- Explanation:

  - `cities[2]="San Francisco"` modifies the third element of the `cities` array to `"San Francisco"`.
  - `${cities[@]}` prints all elements of the array.

- Output:

  ```bash
  2. Modify Array Elements
  Cities: New York Los Angeles San Francisco Houston Phoenix
  ```

##### Example 3: Add Elements to an Array

```bash
# ---- Example 3: Add Elements to an Array ----
cities=("New York" "Los Angeles" "Chicago" "Houston" "Phoenix")
echo "3. Add Elements to an Array"
cities[6]="Atlanta"

for city in "${cities[@]}"
do
    echo "$city"
done
```

- Explanation:

  - `cities[6]="Atlanta"` adds `"Atlanta"` as the seventh element to the `cities` array.
  - The `for` loop iterates through all elements of the `cities` array and prints each city.

- Output:

  ```bash
  3. Add Elements to an Array
  New York
  Los Angeles
  Chicago
  Houston
  Phoenix
  Atlanta
  ```

##### Example 4: Get the Length of an Array

```bash
# ---- Example 4: Get the Length of an Array ----
cities=("New York" "Los Angeles" "Chicago" "Houston" "Phoenix")
echo "4. Get the Length of an Array"
echo "Number of cities: ${#cities[*]}"
```

- Explanation:

  - `${#cities[*]}` gives the number of elements in the `cities` array.

- Output:

  ```bash
  4. Get the Length of an Array
  Number of cities: 5
  ```

##### Example 5: Loop Through an Array

```bash
# ---- Example 5: Loop Through an Array ----
cities=("New York" "Los Angeles" "Chicago" "Houston" "Phoenix")
echo "5. Loop Through an Array"
for city in "${cities[@]}"; do
    echo $city
done
```

- Explanation:

  - The `for` loop iterates through each element of the `cities` array and prints it.

- Output:

  ```bash
  5. Loop Through an Array
  New York
  Los Angeles
  Chicago
  Houston
  Phoenix
  ```

##### Example 6: Index Looping

```bash
# ---- Example 6: Index Looping ----
cities=("New York" "Los Angeles" "Chicago" "Houston" "Phoenix")
echo "6. Index Looping"
for index in "${!cities[@]}"; do
    echo "Index: $index, City: ${cities[$index]}"
done
```

- Explanation:

  - `${!cities[@]}` retrieves all indices of the `cities` array.
  - `${cities[$index]}` accesses each element using its index.

- Output:

  ```bash
  6. Index Looping
  Index: 0, City: New York
  Index: 1, City: Los Angeles
  Index: 2, City: Chicago
  Index: 3, City: Houston
  Index: 4, City: Phoenix
  ```

##### Example 7: Array Slicing

```bash
# ---- Example 7: Array Slicing ----
cities=("New York" "Los Angeles" "Chicago" "Houston" "Phoenix")
echo "7. Array Slicing"
echo "Slice of cities: ${cities[@]:1:3}"
```

- Explanation:

  - `${cities[@]:1:3}` slices the `cities` array starting from index 1 and includes 3 elements.

- Output:

  ```bash
  7. Array Slicing
  Slice of cities: Los Angeles Chicago Houston
  ```

#### 2. Associative Arrays

Associative arrays use arbitrary strings as keys to access values.

##### Example 8: Associative Arrays

```bash
# ---- Example 8: Associative Arrays ----
echo "8. Associative Arrays"
declare -A country_capitals
country_capitals[USA]="Washington D.C."
country_capitals[France]="Paris"
country_capitals[Japan]="Tokyo"
echo "Capital of France: ${country_capitals[France]}"
```

- Explanation:

  - `declare -A country_capitals` declares an associative array named `country_capitals`.
  - `country_capitals[USA]="Washington D.C."` assigns a value to the key `USA`.
  - `${country_capitals[France]}` accesses the value associated with the key `France`.

- Output:

  ```bash
  8. Associative Arrays
  Capital of France: Paris
  ```

##### Example 9: Loop Through Associative Arrays

```bash
# ---- Example 9: Loop Through Associative Arrays ----
declare -A country_capitals
country_capitals[USA]="Washington D.C."
country_capitals[France]="Paris"
country_capitals[Japan]="Tokyo"
echo "9. Loop Through Associative Arrays"
for country in "${!country_capitals[@]}"; do
    echo "Country: $country, Capital: ${country_capitals[$country]}"
done
```

- Explanation:

  - `${!country_capitals[@]}` retrieves all keys of the `country_capitals` array.
  - `${country_capitals[$country]}` accesses each value using its corresponding key.

- Output:

  ```bash
  9. Loop Through Associative Arrays
  Country: USA, Capital: Washington D.C.
  Country: France, Capital: Paris
  Country: Japan, Capital: Tokyo
  ```

##### Example 10: Delete Array Elements

```bash
# ---- Example 10: Delete Array Elements -----
cities=("New York" "Los Angeles" "Chicago" "Houston" "Phoenix")
echo "10. Delete Array Elements"
unset cities[1]
echo "Cities after deletion: ${cities[@]}"

declare -A country_capitals
country_capitals[USA]="Washington D.C."
country_capitals[France]="Paris"
country_capitals[Japan]="Tokyo"
unset country_capitals[France]
echo "Countries and capitals after deletion:"
for country in "${!country_capitals[@]}"; do
    echo "Country: $country, Capital: ${country_capitals[$country]}"
done
```

- Explanation:

  - `unset cities[1]` deletes the element at index 1 in the `cities` array.
  - `unset country_capitals[France]` deletes the key-value pair with key `France` in the `country_capitals` associative array.

- Output:

  ```bash
  10. Delete Array Elements
  Cities after deletion: New York Chicago Houston Phoenix
  Countries and capitals after deletion:
  Country: USA, Capital: Washington D.C.
  Country: Japan, Capital: Tokyo
  ```



---



## Using `expr` in Bash

The `expr` command in Bash is used for evaluating expressions. It performs arithmetic operations and can also be used for string comparison. Here's how `expr` works and some examples of its usage:

#### Example Script and Explanation

```bash
#!/bin/bash

x=10
y=2

# Increment the variable x by 1 and print the result
# The ((...)) syntax allows for C-style increment operators
((x++))
echo "x is equal to $x"

# Increment the variable x by 5 and print the result
((x += 5))
echo "x is equal to $x"

# Decrement the variable x by 5 and print the result
((x -= 5))
echo "x is equal to $x"

# Multiply the variable x by 5 and print the result
((x *= 5))
echo "x is equal to $x"

# Divide the variable x by 5 and print the result
((x /= 5))
echo "x is equal to $x"

# Arithmetic expansion allows us to perform operations directly on variables
z=$(($x - $y))
echo "z is equal to $z"

# Perform the same arithmetic operation using arithmetic expansion $((...)) and print the result
z=$((1 + 2))
echo "z is equal to $z"

# Using 'expr' command to perform arithmetic operations
# Note: expr requires spaces between operators and operands
z=$(expr 1 + 2)
echo "z is equal to $z"
```

#### Explanation

- **Increment and Decrement Operations**:
  - `((x++))`: Increments `x` by 1 using the C-style increment operator.
  - `((x += 5))`: Increments `x` by 5.
  - `((x -= 5))`: Decrements `x` by 5.
  - `((x *= 5))`: Multiplies `x` by 5.
  - `((x /= 5))`: Divides `x` by 5.
- **Arithmetic Expansion** (`$((...))`):
  - `z=$(($x - $y))`: Performs arithmetic subtraction using variables `x` and `y`.
  - `z=$((1 + 2))`: Direct arithmetic operation within double parentheses.
- **Using `expr` Command**:
  - `expr 1 + 2`: Performs addition using `expr` command. Note the spaces around the operator (`+`).
  - `z=$(expr 1 + 2)`: Stores the result of `expr` operation in variable `z`.

#### Output

Running the script will produce the following output:

```bash
x is equal to 16
x is equal to 21
x is equal to 16
x is equal to 80
x is equal to 16
z is equal to 14
z is equal to 3
z is equal to 3
```

- Explanation of Output:
  - `x` undergoes several arithmetic operations as shown in the script.
  - `z` holds the results of various arithmetic operations (`$x - $y`, `1 + 2`, and `expr 1 + 2`).





## Arguments in Bash Scripting

In Bash scripting, arguments allow you to pass information to a script when it is executed. Here's an explanation of how arguments work in Bash scripts along with examples:

#### Example Script and Explanation

```bash
#!/bin/bash

# Display script name
echo "Script name: $0"

# Display total number of arguments
echo "Total number of arguments: $#"

# Display all arguments as separate values
echo "All arguments (separate): "
for arg in "$@"; do
    echo "- $arg"
done

# Display all arguments as a single string
echo "All arguments (together): $*"

# Accessing specific arguments
if [ $# -ge 1 ]; then
    echo "First argument: $1"
fi

if [ $# -ge 2 ]; then
    echo "Second argument: $2"
fi

# Example of using the exit status variable
ls
if [ $? -eq 0 ]; then
    echo "ls command executed successfully."
else
    echo "Error: ls command failed."
fi
```

#### Explanation

- **`$0`**: Represents the script name itself.
- **`$#`**: Indicates the total number of arguments passed to the script.
- **`$@`**: Represents all arguments passed to the script as separate values.
- **`$*`**: Represents all arguments passed to the script as a single string.
- **Loop through arguments**: The `for arg in "$@"` loop iterates through each argument and prints them separately.
- **Accessing specific arguments**: Conditional statements (`if [ $# -ge 1 ]`) check if enough arguments are passed before accessing `$1`, `$2`, etc., which represent the first and second arguments, respectively.
- **Exit status (`$?`)**: After executing the `ls` command, `$?` holds the exit status of the last executed command (0 indicates success, non-zero values indicate failure).

#### Output

When running the script with various arguments, it will produce output similar to:

```bash
Script name: ./script.sh
Total number of arguments: 3
All arguments (separate): 
- arg1
- arg2
- arg3
All arguments (together): arg1 arg2 arg3
First argument: arg1
Second argument: arg2
ls command executed successfully.
```

- Explanation of Output:
  - The script `script.sh` is executed with three arguments (`arg1`, `arg2`, `arg3`).
  - It displays the script name, total number of arguments, lists all arguments separately, lists all arguments together as a single string, and accesses specific arguments (`$1` and `$2`).
  - Finally, it demonstrates checking the exit status of a command (`ls` in this case) using `$?`.



---

## Syntax of `case` Statement in Bash

The `case` statement in Bash allows you to match the value of a variable against multiple patterns. It's useful for executing different blocks of code based on different conditions. Here's the syntax and explanation of how `case` works:

#### Syntax

```bash
case variable in
    pattern1)
        # Commands to execute if variable matches pattern1
        ;;
    pattern2 | pattern3)
        # Commands to execute if variable matches pattern2 or pattern3
        ;;
    pattern4)
        # Commands to execute if variable matches pattern4
        ;;
    *)
        # Default commands if no patterns match
        ;;
esac
```

#### Explanation

- **`case`**: Starts the `case` statement.
- **`variable`**: The variable whose value will be checked against patterns.
- **`in`**: Marks the beginning of patterns to match against `variable`.
- **Patterns (`pattern1`, `pattern2`, ...)**:
  - Each pattern can be a literal value or a wildcard expression.
  - `pattern1`: Checks if `variable` matches `pattern1`.
  - `pattern2 | pattern3`: Checks if `variable` matches either `pattern2` or `pattern3`.
  - `pattern4`: Checks if `variable` matches `pattern4`.
- **Commands (`# Commands to execute`)**:
  - Each block of commands executes when the corresponding pattern matches `variable`.
  - Use `;;` to indicate the end of each block of commands within a pattern.
- **Default (`*`)**:
  - If no patterns match, the `*` pattern (wildcard) acts as a default case.
  - Commands under `*)` execute when none of the specified patterns match `variable`.
- **`esac`**: Ends the `case` statement.

#### Example Explained

```bash
#!/bin/bash

echo "Select an option:"
echo "1) Display date and time"
echo "2) List files in current directory"
echo "3) Display current disk usage"
echo "4) Display system uptime"
echo "5) Exit"

read -p "Enter your choice: " choice

case $choice in
    1|date)
        echo "Current date and time:"
        date
        ;;
    2)
        echo "Files in current directory:"
        ls
        ;;
    3)
        echo "Current disk usage:"
        df -h
        ;;
    4)
        echo "System uptime:"
        uptime
        ;;
    5)
        echo "Goodbye!"
        ;;
    *)
        echo "Invalid option! Please select a valid option."
        ;;
esac
```

- Explanation of Example:
  - The script prompts the user to enter a choice (`$choice`).
  - The `case` statement checks the value of `$choice` against patterns (`1|date`, `2`, `3`, `4`, `5`).
  - Depending on the value of `$choice`, it executes the corresponding block of commands.
  - If `$choice` doesn't match any patterns (`1`, `2`, `3`, `4`, `5`), the default case (`*`) displays an error message.



---

## Syntax of `select` Statement in Bash

The `select` statement in Bash provides an easy way to create interactive menus for users to select options. It iterates through a list of items and allows the user to choose one. Here's the syntax and explanation of how `select` works:

#### Syntax

```bash
select name [in list]
do
    # Commands to execute based on user selection
    case $name in
        pattern1)
            # Commands to execute if user selects pattern1
            ;;
        pattern2)
            # Commands to execute if user selects pattern2
            ;;
        pattern3)
            # Commands to execute if user selects pattern3
            ;;
        *)
            # Default commands if user selects an invalid option
            ;;
    esac
done
```

#### Explanation

- **`select`**: Starts the `select` statement.
- **`name`**: Variable that stores the user's selection from the menu.
- **`in list`**: Optional list of items for the user to choose from.
- **Commands (`do` and `done`)**:
  - Executes the loop for each item in the list.
  - Prompts the user to select an option and stores it in `$name`.
- **`case` Statement** (`case $name in ... esac`):
  - Checks the value of `$name` against different patterns (`pattern1`, `pattern2`, ...).
  - Each `pattern` corresponds to an option in the menu.
  - Executes the corresponding block of commands if `$name` matches a `pattern`.
- **Default (`*`)**:
  - Executes if the user selects an option that doesn't match any defined `pattern`.
  - Provides error handling or a default action for invalid selections.

#### Example Explained

```bash
#!/bin/bash

# ------------- Example 1 ----------------

PS3="Please enter your choice: "
select color in red blue orange 
do
    echo "You chose $color"
    # Optional: Add break statement to exit loop after selection
    # break
done
```

- Explanation of Example 1:
  - Sets the prompt message for user input (`PS3="Please enter your choice: "`).
  - Uses `select` to create a menu (`select color in red blue orange`).
  - Prompts the user to choose from options (`red`, `blue`, `orange`).
  - Stores the user's choice in the variable `$color`.
  - Prints the selected color (`echo "You chose $color"`).
  - Optionally, you can use `break` to exit the `select` loop after the user makes a selection.

#### More Complex Example (Example 2)

```bash
#!/bin/bash

# ------------- Example 2 ----------------

echo "Select your favorite fruit:"

select fruit in "Apple" "Banana" "Cherry" "Date" "Exit"
do
    case $fruit in
        "Apple")
            echo "You selected Apple."
            ;;
        "Banana")
            echo "You selected Banana."
            ;;
        "Cherry")
            echo "You selected Cherry."
            ;;
        "Date")
            echo "You selected Date."
            ;;
        "Exit")
            echo "Exiting."
            break
            ;;
        *)
            echo "Invalid option. Please select a number between 1 and 5."
            ;;
    esac
done
```

- Explanation of Example 2:
  - Displays a prompt (`echo "Select your favorite fruit:"`).
  - Uses `select` to present a menu of options (`select fruit in ...`).
  - User selects a fruit (`Apple`, `Banana`, `Cherry`, `Date`, `Exit`).
  - Executes a `case` statement ( `case $fruit in ... esac `) based on the user's selection:
    - Displays a message corresponding to the selected fruit.
    - Handles special case for `Exit` to break out of the loop and exit the script.
    - Provides default handling for invalid selections (`*`).



---

## Handling Strings in Bash Scripts

Strings are fundamental in Bash scripting for storing and manipulating textual data. Here's an overview of how to work with strings in Bash, along with examples illustrating various operations:

#### String Initialization

```bash
#!/bin/bash

# String initialization
string1="Hello"
string2="World"
string3="iti"
string4=""

echo "String 1: $string1"
echo "String 2: $string2"
echo "String 3: $string3"
echo "String 4: $string4"
echo ""
```

- Explanation:
  - Defines several strings (`string1`, `string2`, `string3`, `string4`).
  - Prints each string to verify initialization.

#### Example 1: String Comparison

```bash
if [ "$string1" = "$string2" ]; then
    echo "'$string1' is equal to '$string2'"
else
    echo "'$string1' is not equal to '$string2'"
fi
```

- Explanation:
  - Checks if `string1` is equal to `string2`.
  - Uses `[ ... ]` for string comparison.

#### Example 2: Checking Length Difference

```bash
if [[ ${#string1} -ne ${#string2} ]]; then
    echo "The strings have a different number of characters."
else
    echo "The strings have the same number of characters."
fi
```

- Explanation:
  - Uses `${#string}` to get the length of strings.
  - Compares the length of `string1` and `string2`.

#### Example 3: Checking Content Difference

```bash
if [[ "$string1" != "$string2" ]]; then
    echo "$string1 is not equal to $string2"
fi
```

- Explanation:
  - Checks if `string1` is not equal to `string2`.

#### Example 4: Comparison with Length Check

```bash
if [[ "${#string1}" -gt "${#string2}" ]]; then
    echo "$string1 has more characters than $string2"
else
    echo "$string1 does not have more characters than $string2"
fi
```

- Explanation:
  - Compares the lengths of `string1` and `string2`.
  - Outputs which string has more characters.

#### Example 5: Check if a String is Empty

```bash
if [ -z "$string4" ]; then
    echo "string4 is empty"
fi
```

- Explanation:
  - Uses `-z` to check if `string4` is empty.

#### Example 6: Check if a String is Not Empty

```bash
if [[ -n "$string1" ]]; then
    echo "string1 is not empty"
fi
```

- Explanation:
  - Uses `-n` to check if `string1` is not empty.

#### Example 7: String Concatenation

```bash
concatenated="$string1 $string2"
echo "Concatenated String: $concatenated"
```

- Explanation:
  - Concatenates `string1` and `string2` with a space in between.

#### Example 8: String Length

```bash
length=${#concatenated}
echo "Length of '$concatenated' is $length"
```

- Explanation:
  - Uses `${#variable}` to get the length of `concatenated`.

#### Example 9: Substring Extraction

```bash
substring=${concatenated:6:5}
echo "Substring of '$concatenated' from index 6 with length 5 is '$substring'"
```

- Explanation:

  - Uses `${variable:start:length}` to extract a substring from `concatenated`.

    

---

## Syntaxes for Defining Functions in Bash

#### Syntax 1: Basic Function Definition

```bash
#!/bin/bash

# Example 1: Function defined without 'function' keyword
say_hello() {
    echo "Hello, World!"
}

# Call the function
say_hello
```

- Explanation:
  - Defines a function `say_hello` using the basic syntax without the `function` keyword.
  - The function body (`echo "Hello, World!"`) is enclosed within `{}`.
  - Calls the `say_hello` function to output "Hello, World!".

#### Syntax 2: Using the `function` Keyword

```bash
#!/bin/bash

# Example 2: Function defined using 'function' keyword
function say_hello {
    echo "Hello, World!"
}

# Call the function
say_hello
```

- Explanation:
  - Defines a function `say_hello` using the `function` keyword.
  - The function body (`echo "Hello, World!"`) follows after the function name without `{}`.
  - Calls the `say_hello` function to output "Hello, World!".

#### Syntax 3: Using `function` Keyword with Parentheses

```bash
#!/bin/bash

# Example 3: Function defined using 'function' keyword and parentheses
say_hello () {
    echo "Hello, World!"
}

# Call the function
say_hello
```

- Explanation:
  - Defines a function `say_hello` using the `function` keyword followed by parentheses `()`.
  - The function body (`echo "Hello, World!"`) is enclosed within `{}`.
  - Calls the `say_hello` function to output "Hello, World!".

### Additional Examples

#### Function with Arguments

```bash
#!/bin/bash

# Example 4: Function with arguments
DisplayArguments() {
    echo "Script name: $0"
    echo "First argument: $1"
    echo "Second argument: $2"
}

# Call the function with arguments provided to the script
DisplayArguments "$1" "$2"
```

- Explanation:
  - Defines a function `DisplayArguments` that prints the script name (`$0`) and its first two arguments (`$1` and `$2`).
  - Calls the `DisplayArguments` function with arguments provided to the script.

#### Getting the Return Value of a Function

```bash
#!/bin/bash

# Example 5: Function to add two numbers and return the result
add() {
    sum=$(($1 + $2))
    echo $sum
}

# Call the function and capture the return value
result=$(add 5 5)
echo "Result of addition: $result"
```

- Explanation:
  - Defines a function `add` that calculates the sum of two numbers (`$1` and `$2`) and echoes (`echo`) the result.
  - Calls the `add` function with arguments `5` and `5`.
  - Captures the return value (`echo` output) of the function in the variable `result`.
  - Prints the result using `echo`.



---

## Understanding `local` Variables in Bash

In Bash scripting, variables by default are global, meaning they are accessible from anywhere within the script. However, there are situations where you may want to restrict the scope of a variable to a specific function. This is where `local` variables come into play.

#### Example Without `local`:

```bash
#!/bin/bash

VAR="Global Variable"

fun()
{
     VAR="Local Variable"
     echo $VAR
 }

 echo $VAR        # Outputs: Global Variable
 fun              # Outputs: Local Variable
 echo $VAR        # Outputs: Local Variable
```

- Explanation:
  - Initially, `VAR` is declared as a global variable with the value "Global Variable".
  - The `fun` function modifies `VAR` to "Local Variable" within its scope.
  - When `fun` is called, it prints the local value of `VAR`.
  - After `fun` finishes, `echo $VAR` outside the function also prints the modified value of `VAR`, which is now "Local Variable".

#### Example With `local`:

```bash
#!/bin/bash

VAR="Global Variable"

fun()
{
    local VAR="Local Variable"
    echo $VAR
}

echo $VAR        # Outputs: Global Variable
fun              # Outputs: Local Variable
echo $VAR        # Outputs: Global Variable
```

- Explanation:
  - `VAR` is initially set as a global variable with the value "Global Variable".
  - Inside the `fun` function, `local VAR="Local Variable"` declares a new variable `VAR` that is local to the function scope.
  - When `fun` is called, it prints the local value of `VAR`, which is "Local Variable".
  - After `fun` finishes, `echo $VAR` outside the function prints the global value of `VAR`, which remains "Global Variable" and was not affected by the local assignment inside `fun`.