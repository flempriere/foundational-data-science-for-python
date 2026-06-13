# Chapter 5: Execution Control

- [Notes](#notes)
  - [Compound Statements](#compound-statements)
    - [Compound Statement Structure](#compound-statement-structure)
    - [Evaluating to `True` or `False`](#evaluating-to-true-or-false)
      - [Equality Operations](#equality-operations)
      - [Comparison Operators](#comparison-operators)
      - [Boolean / Logic Operators](#boolean--logic-operators)
      - [Object Evaluation](#object-evaluation)
    - [`if` Statements](#if-statements)
      - [The Walrus Operator](#the-walrus-operator)
      - [`else` Statements](#else-statements)
    - [`While` Loops](#while-loops)
    - [`for` Loops](#for-loops)
    - [`break` and `continue`
      Statements](#break-and-continue-statements)
- [Summary](#summary)
- [Questions](#questions)

## Notes

- Statements can be grouped together to be executed as unit
- By combining these with statements that *control execution* we can
  implement complex behaviours

### Compound Statements

- We’ve seen basic single line statements in [Chapter
  2](../Chapter_02/Chapter_02.qmd)
- Compound statements are sequences of statements grouped together with
  a *control structure* to execute together
  - Execution only occurs when a *control condition* is `True`
- The various *control conditions* include,
  - `for` loops,
  - `while` loops,
  - `if` statements,
  - `try` statements,
  - `with` statements

#### Compound Statement Structure

- A compound statement contains
  1. A control statement

      - Syntax typically follows

        ``` python
         <keyword> <expression>:
        ```

  2. Statements to be executed

      - Indented one level below the control statement

      - Indentation level can be specified by the user but must be
        consistent

        - e.g. if we use four spaces for one level of indentation, all
          remaining must also be four spaces

      - Once the block is complete, we out-dent back to the previous
        level

        ``` python
         <control statement>:
             <controlled statement 1>
             <controlled statement 2>
             <controlled statement 3>
         <start of the next block>
        ```

#### Evaluating to `True` or `False`

- `if`, `while` and `for` loops all test their condition for a `True`
  value
  - Expression needs to be able to be evaluated as `True` or `False`
  - By default, most expressions can be coerced to `True` or `False`

##### Equality Operations

- Python has three forms of equality operators

  1. `==`, the equality operator
      - Compares two objects based on their `__eq__` dunder method
      - Returns `True` or `False`
  2. `!=` the inequality operator
      - Works like the equality but checks that two items are *not*
        equal
  3. `is` the identity operator
      - Compares that two variables reference the *same* memory object

``` python
# Assign values to variables

a, b, c = 1, 1, 2

# check if two values are equal
print("a == b:", a == b)
print("b == c:", b == c)

# check for inequality
print("a != b:", a != b)
print("b != c:", b != c)

# check for identity
print("a == a:", a == a)
print("a is a:", a is a)
print("a is b:", a is b)
```

    a == b: True
    b == c: False
    a != b: False
    b != c: True
    a == a: True
    a is a: True
    a is b: True

- The equality method works when comparing different types of numbers
  - But liable to fail when comparing other types

``` python
print("1 == 1.0:", 1 == 1.0)
print("'1' == 1:", '1' == 1)
```

    1 == 1.0: True
    '1' == 1: False

##### Comparison Operators

- Comparison operators can be used to test the ordering of objects

  - For numbers, it is the natural ordering
  - For strings, it is dictionary ordered (on unicode value of
    characters)

- Comparison operators are,

  1. `<` less than,
  2. `<=` less than or equal,
  3. `>` greater than,
  4. `>=` greater than or equal

``` python
a, b, c = 1, 1, 2

print("a < b:", a < b)
print("a < c:", a < c)

print("a <= b:", a <= b)

print("b > a:", a > b)
print("c > a:", c > a)

print("b >= a:", b >= a)
```

    a < b: False
    a < c: True
    a <= b: True
    b > a: False
    c > a: True
    b >= a: True

- Comparison operators require the correct ordering to be defined
  - Exists for most numeric types and strings
  - Not likely for more complex objects

##### Boolean / Logic Operators

- Act on `True` or `False` expressions to get a final `True` or `False`
  result

- Operators are,

  1. `and`
      - Combines two boolean expressions
      - Evaluates to `True` iff both expressions are `True`
  2. `or`
      - Combines two boolean expressions
      - Evaluates to `True` iff either expression is `True`
  3. `not`
      - Flips the truthfulness of a boolean expression
      - `False` becomes `True`
      - `True` becomes `False` etc.

- `and` and `or` have short-circuiting behaviour

  - Meaning the will stop evaluating sub-expressions once the final
    value has been determined

- If the first expression in `and` is `False` then it will return
  `False`

  - Else it returns the value of the second expression

- If the first expression in `or` is `True` then it will return `True`

  - Else if returns the value of the second expressions

``` python
print(f"False and True:", False and True)
print(f"True or False:", True or False)
```

    False and True: False
    True or False: True

##### Object Evaluation

- Python objects evaluate to `True` or `False`
  - Objects evaluating to `False` are:
    1. `None`
    2. `False` - the boolean object
    3. Any numerical $0$, e.g. `0` or `0.0`
    4. Any container with length $0$ e.g. `[]` or `""`
  - Pretty much anything else evaluates to `True`

``` python
a = ''
b = a or 'default value' # evaluates to second argument since the empty string evaluates `False`
print(b)
```

    default value

#### `if` Statements

- Let code branch based on a control expression

  ``` python
    if <condition>:
        <statements>
  ```

- Code inside the `if` only executes if the control condition is `True`

``` python
# Example use cases for if
if True:
    message = "It's True"
    print(message)

if False:
    message = "It's False"
    print(message)
```

    It's True

##### The Walrus Operator

- When a variable is assigned in python the resulting expression returns
  `None`
- Python 3.8 introduces the walrus operator `:=`
  - When used as part of an expression it both performs the assignment
    and *returns* the assigned value
  - Useful when we want to assign and test in the same sequence

``` python
import re
s = '2020-12-14'
if match := re.search(r"(\d\d\d\d)-(\d\d)-(\d\d)", s)
if match:
    print(f"Matched items: {match.groups(1)}")
else:
    print(f"No match found in {s}")
```

    SyntaxError: expected ':' (2456532456.py, line 3)
      Cell In[7], line 3
        if match := re.search(r"(\d\d\d\d)-(\d\d)-(\d\d)", s)
                                                             ^
    SyntaxError: expected ':'

##### `else` Statements

- `if` statements may have an optional `else` branch
- This is a set of statements that is only executed if an expression
  tests `False`
- For example, consider the basic test if `snack` is contained in a set
  - We might want to print an alternative method if `snack` is not in
    the set

``` python
snack = "apple"
fruit = {"orange", "apple", "pear"}

if snack in fruit:
    print(f"Yeah, {snack} is good!")
```

    Yeah, apple is good!

- Syntax is,

  ``` python
    if <control-expression>:
        <statements-executed-if-true>
    else:
        <statements-executed-if-false>
  ```

- So we might then write

``` python
snack = "apple"
fruit = {"orange", "apple", "pear"}

if snack in fruit:
    print(f"Yeah, {snack} is good!")
```

    Yeah, apple is good!

- `if` and `else` statements can be nested

``` python
balance = 2000.32
account_status = None

if balance > 0:
    account_status = "Positive"
else:
    if balance == 0:
        account_status = "Empty"
    else:
        account_status = "Overdrawn"

print(account_status)
```

    Positive

- If we want to combine multiple branches in the same block, we can use
  an `elif`
  - Shorthand for *else if*
  - Allow us to add sequential tests for if the first `if` (and
    preceding `elif`) tests fail

``` python
fav_num = 13

if fav_num in (3.7):
    print(f"{fav_num} is lucky")
elif fav_num == 0:
    print(f"{fav_num} is evocative")
elif fav_num > 20:
    print(f"{fav_num} is my favourite number too")
else:
    print(f"I have no opinion about {fav_num}")
```

    TypeError: argument of type 'float' is not a container or iterable
    ---------------------------------------------------------------------------
    TypeError                                 Traceback (most recent call last)
    Cell In[11], line 3
          1 fav_num = 13
          2
    ----> 3 if fav_num in (3.7):
          4     print(f"{fav_num} is lucky")
          5 elif fav_num == 0:
          6     print(f"{fav_num} is evocative")

    TypeError: argument of type 'float' is not a container or iterable

#### `While` Loops

- `while` loops run a series of nested statements while their control
  expression evaluates to `True`

  ``` python
    while <control-expression>:
        <statements>
  ```

- For example, counting up to a certain number

  - Each iteration increases `counter` until the loop condition is
    `False` and the `while` loop ends

``` python
counter = 0
while counter = 5:
    print(f"I've counted {counter} so far, I hope there aren't more")
    counter += 1
```

    SyntaxError: invalid syntax. Maybe you meant '==' or ':=' instead of '='? (1051944267.py, line 2)
      Cell In[12], line 2
        while counter = 5:
              ^
    SyntaxError: invalid syntax. Maybe you meant '==' or ':=' instead of '='?

> [!CAUTION]
>
> **Infinite Loops**
>
> If the loop condition for a `while` loop is always `True` then the
> loop will never end. This is commonly referred to as an *infinite
> loop*

#### `for` Loops

- The pattern of running a loop a fixed number of times is quite common

  - e.g. counting up to a certain value, iterating through a collection
    etc.

- We could implement this with `while` loops

  - But the `for` loop is a nice shorthand for this

- The syntax is

  ``` python
    for <variable name> in <iterable>:
        <statements>
  ```

- For example, to iterate a fixed number of times we can use a `for`
  loop with a `range` object

  - i.e. Recreating the `for` loop above

``` python
for i in range(6):
    print(i)
```

    0
    1
    2
    3
    4
    5

- As mentioned we can also iterate over sequences like `list`

``` python
colours = ["Green", "Red", "Blue"]
for colour in colours:
    print(f"My favourite colour is {colour}")
    print("No, wait...")
```

    My favourite colour is Green
    No, wait...
    My favourite colour is Red
    No, wait...
    My favourite colour is Blue
    No, wait...

#### `break` and `continue` Statements

- `break` and `continue` are specialised control statements that can be
  used within `while` and `for` loops
- `break` immediately exits from the current loop
- `continue` immediately goes to the next iteration of the loop
- In general, where possible prefer standard control flow since they
  obfuscate what’s going on
  - But sometimes they are arguably the cleanest way to implement a
    solution
- Using a `break` statement we can stop otherwise apparently infinite
  loops

``` python
fish = ["mackerel", "salmon", "pike"]
beasts = ["salmon", "pike", "bear", "mackerel"]

i = 0
while True:
    beast = beasts[i]
    if beast not in fish:
        print(f"Oh no! It's not a fish, it's a {beast}")
        break
    print(f"I caught a {beast} with my fishing net")
    i += 1
```

    I caught a salmon with my fishing net
    I caught a pike with my fishing net
    Oh no! It's not a fish, it's a bear

- `continue` let’s us move to the next loop iteration early, such as if
  there is an error condition

``` python
for name in ["bob", "billy", "bonzo", "fred", "baxter"]:
    if not name.startswith("b"):
        continue
    print(f"Fine fellow that {name}")
```

    Fine fellow that bob
    Fine fellow that billy
    Fine fellow that bonzo
    Fine fellow that baxter

## Summary

- Compound statements controlled by `if`, `while` or `for` statements
  allow for complex program logic
- They can used to optionally run blocks of code or repeat sections a
  certain number of times

## Questions

1. What is printed by the following code if the variable `a` is set to
    an empty list?

    ``` python
     if a:
         print(f"Hiya {a}")
     else:
         print(f"Biya {a}")
    ```

    - The empty list evaluates to `False`
    - The `else` statement executes
    - The output should be `"Biya []"`

2. What is printed by the previous code if the variable `a` is set to
    the string `"Henry"`?

    - A non-empty string evaluates to `True`
    - The first branch will thus execute
    - The output should be `"Hiya Henry"`

3. Write a `for` loop that prints the numbers from $0$ to $9$, skipping
    $3$, $5$, $7$

    - There are many ways to solve this
    - Using the tools given we to us so far we might write

    ``` python
     for i in range(10):
         if i in [3, 5, 7]:
             continue
         print(i)
    ```

The answers can be seen below

``` python
# Question 1

print("Question 1")
a = []
if a:
    print(f"Hiya {a}")
else:
    print(f"Biya {a}")

print("Question 2")
a = "Henry"
if a:
    print(f"Hiya {a}")
else:
    print(f"Biya {a}")

print("Question 3")
for i in range(10):
    if i in [3, 5, 7]:
        continue
    print(i)
```

    Question 1
    Biya []
    Question 2
    Hiya Henry
    Question 3
    0
    1
    2
    4
    6
    8
    9
