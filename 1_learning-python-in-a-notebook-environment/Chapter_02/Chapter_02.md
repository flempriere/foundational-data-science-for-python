# Chapter 2: Fundamentals of Python

- [Notes](#notes)
  - [Basic Types](#basic-types)
  - [High Level vs Low Level](#high-level-vs-low-level)
  - [Statements](#statements)
    - [Multiple Statements](#multiple-statements)
    - [Expression Statements](#expression-statements)
    - [Assert Statements](#assert-statements)
    - [Assignment Expression](#assignment-expression)
    - [Pass Statements](#pass-statements)
    - [Delete Statements](#delete-statements)
    - [Return Statements](#return-statements)
    - [Yield Statements](#yield-statements)
    - [Raise Statements](#raise-statements)
    - [Break Statements](#break-statements)
    - [Continue Statements](#continue-statements)
    - [Import Statements](#import-statements)
    - [Global Statements](#global-statements)
    - [Nonlocal Statements](#nonlocal-statements)
    - [Print Statements](#print-statements)
  - [Performing Basic Mathematics](#performing-basic-mathematics)
  - [Using Classes and Objects with Dot
    Notation](#using-classes-and-objects-with-dot-notation)
- [Summary](#summary)
- [Questions](#questions)

## Notes

### Basic Types

- Python represents different data formats as different types
- For example integers, floating point numbers, strings, etc. all
  classify different forms of data
- For example to represent whole numbers, we use integers

``` python
type(13)
```

    int

- Numbers with a decimal point are represented as floats

``` python
type(4.1)
```

    float

- To ensure a whole number is treated as a floating point, ensure we use
  a dot point

``` python
type(13.0)
```

    float

- Boolean values (`True` or `False`) are represented by the `bool` type

``` python
type(True)
```

    bool

``` python
type(False)
```

    bool

- A string represents text
  - Represented by delimiting `""` or `''`

``` python
type("Hello")
```

    str

- The absence of a value is represented by `None`, which has it’s own
  type

``` python
type(None)
```

    NoneType

### High Level vs Low Level

- Languages are sometimes referred to as *high-level* or *low level*
  depending on how much they abstract from the computer machine model
- Python is regarded as a high level language because it’s syntax and
  semantics provide highly abstract operations divorced from the reality
  of how to implement them
  - For contrast, C is regarded as a relatively low-level language since
    memory must be handled explicitly and data structures typically
    implemented by hand

### Statements

- Python programs are made of statements and expressions
- Each can be seen as an instruction to be executed
- A statement is any executable line of python

``` python
print("hello") # A statement
```

    hello

- Expressions are sub-statements that return a value
- A statement can be made of a mix of a expressions and statements

``` python
x, y = 5, 6 # Statement hiding a tuple construction (5, 6), then tuple unpacking assignment x, y
z = 0
bar = x**2 if (x < y) and (y or z) else x // 2 # in-line conditional expression and integer division into variable assignment

print("x:", x ,"y:", y, "z:", z)
```

    x: 5 y: 6 z: 0

#### Multiple Statements

- Python executes each statement in the order it is written
- State is saved between statements

``` python
x = 23 // 3 # integer division
y = x ** 2 # power
print(f"x is {x} and y is {y}") # Combines x and y
```

    x is 7 and y is 49

#### Expression Statements

- Expression statements are statements that consist only of expressions
  (See [Statements](#statements))
- Uncaptured values form expression statements are typically lost
- However, in Notebooks if the last statement is an expression statement
  the value is normally printed
  - This behaviour is also seen in the python REPL

``` python
23 * 42
```

    966

``` python
"Hello"
```

    'Hello'

``` python
import os
os.getcwd()
```

    '/home/runner/work/foundational-data-science-for-python/foundational-data-science-for-python/1_learning-python-in-a-notebook-environment/Chapter_02'

#### Assert Statements

- An `assert` is a runtime check that an expression evaluates `True`

- Values that check as `False` include,

  1. Any explicit `False` reference such as a `bool` variable
  2. Expressions returning `False`
  3. Expressions returning `None`
  4. The integer and float `0`, `0.0`
  5. Empty containers e.g. (`[]`)
  6. Empty strings e.g. (`""`)

- Anything that is not `False` is `True`

- The below throws an error

``` python
assert(False)
print("Passed assert!")
```

    AssertionError:
    ---------------------------------------------------------------------------
    AssertionError                            Traceback (most recent call last)
    Cell In[14], line 1
    ----> 1 assert(False)
          2 print("Passed assert!")

    AssertionError:

- The above will proceed past the `assert`

``` python
assert(True)
print("Passed assert!")
```

    Passed assert!

- `assert` statements are useful for enforcing that runtime assumptions
  hold
- They are useful for debugging

> [!WARNING]
>
> **Disabling** `assert`
>
> You may see some advice recommending that `assert` statements should
> be disabled when not debugging. This can be done for example by using
> the optimise flag when running a python script.
>
> ``` shell
> python -o script.py
> ```
>
> However, you should never do this! Many community packages use
> `assert` statements to ensure that they function correctly and by
> doing so you can break these packages. The runtime overhead of
> `assert` is very small and almost never worth this micro-optimisation.

#### Assignment Expression

- Variables in python act as named labels to a block of data
- Multiple variables can reference the same block of data
- Variables are not typed in python
  - They can be freely reassigned to new values and different types
- An assignment statement assigns a data object to a variable
  - This syntax is `variable = value`

``` python
x = 12
y = "Hello"

print(f"x is {x}, y is {y}")
```

    x is 12, y is Hello

- Variables can be substituted for values by referencing them in place
  of the value

``` python
x = 12
y = "Hello"
z = x - 3

print(f"{y} Jeff, the answer is {z}")
```

    Hello Jeff, the answer is 9

- Multiple assignment can be done using the tuple-unpacking syntax
  - i.e. A comma separated list of variables assigned to a
    comma-separated list of values

``` python
x, y, z = 1, "a", 3.0

print(f"x is {x}, y is {y}, z is {z}")
```

    x is 1, y is a, z is 3.0

- Best practice for variable naming follow are few heuristics

  1. Make variables names meaningful
      - This helps make readers understand the code
      - `x` is less meaningful than `first_name` or
        `weight_in_kilograms`
  2. Use `snake_case` to merge words
  3. Variable name lengths should be proportional to their importance
      - A temporary list indexing variable can be named `i`
        - Common mathematical convention
        - Short, indicates not an important variable in the whole
          program
      - A temporary list indexing variable called
        `index_number_in_the_list`
        - Conveys what the variable is
        - But, overly detailed
        - Makes the variable standout and look more important then it is
      - A global configuration variable called
        `configuration_management`
        - Name is meaningful
        - Longer name denotes it’s global importance over something like
          `config` which looks unimportant

#### Pass Statements

- `pass` is an empty placeholder statement
- Do nothing
- Can be used in places where python expects a statement but you don’t
  want to put something there yet
  - e.g. Abstract methods in classes
  - Placeholder functions

``` python
def func():
    pass # a placeholder function
```

#### Delete Statements

- `del` followed by the object to delete
- Delete statements delete objects
- This frees the associated object and it’s memory
- It can no longer be referenced

``` python
polly = "parrot"
del polly
print(polly)
```

    NameError: name 'polly' is not defined
    ---------------------------------------------------------------------------
    NameError                                 Traceback (most recent call last)
    Cell In[19], line 3
          1 polly = "parrot"
          2 del polly
    ----> 3 print(polly)

    NameError: name 'polly' is not defined

> [!NOTE]
>
> **Python and Memory Management**
>
> Python has automatic memory management handled by a garbage collector.
> This means that any unreferenced memory will be freed and returned to
> the operating system for reallocation later. Given this generally one
> will not need to use the `del` operator. However it is still useful to
> know especially in memory intensive applications where waiting for the
> garbage collector to clean-up can cause problems.

#### Return Statements

- `return` followed by a value
- Define the *return value* of a function
- i.e. what value we receive when we call a function

``` python
def func():
    return "foo"

print(func())
```

    foo

#### Yield Statements

- `yield` followed by a value
- The analogy to `return` for generator functions
  - Generators are functions which can progressively return a series of
    values
  - They are useful for performance and memory management

``` python
def generator_func():
    yield "foo"
    yield "bar"

print(generator_func())
print(generator_func())
```

    <generator object generator_func at 0x7fc3f61a1d80>
    <generator object generator_func at 0x7fc3f61a1d80>

#### Raise Statements

- `raise` followed by an *exception* type
- Exceptions indicate errors that arise during program runtime
- Exceptions interrupt regular program control flow
- Exceptions can be *handled* or *caught* but if not they will crash a
  program
- `raise` statements can be used to re-raise caught exceptions or to
  raise new ones
- There are a wide range of built-in exceptions provided by python for
  various scenarios ([See the
  docs](https://docs.python.org/3/library/exceptions.html#bltin-exceptions))

``` python
# Raising an exception
try:
    raise NotImplementedError("An example exception") # raising a new exception
except NotImplementedError as e:
    print(f"Caught {e!r}")
    raise # re-raising a handled exception
```

    Caught NotImplementedError('An example exception')

    NotImplementedError: An example exception
    ---------------------------------------------------------------------------
    NotImplementedError                       Traceback (most recent call last)
    Cell In[22], line 6
          2 try:
          3     raise NotImplementedError("An example exception") # raising a new exception
          4 except NotImplementedError as e:
          5     print(f"Caught {e!r}")
    ----> 6     raise # re-raising a handled exception

    NotImplementedError: An example exception

#### Break Statements

- Just the keyword `break`
- Used to exit a loop outside of it’s loop condition

``` python
i = 0
while (i < 5):
    print(i)
    i += 1
    if i > 3:
        break # early exit the loop
```

    0
    1
    2
    3

#### Continue Statements

- Just the keyword `continue`
- Immediately go to the next loop iteration

``` python
i = 0
while(i < 5):
    i += 1
    if i == 3:
        continue
    print(i)
```

    1
    2
    4
    5

#### Import Statements

- Used to *import* existing code from a different file, module or
  package
  - A module is nothing more than a python file
  - A package is usually used to refer to a set of related modules or
    files
- Python has an extensive built-in library of packages and modules that
  offer specialised features over the basic language syntax
  - There is also an extensive ecosystem of external community packages
    and tools
- For example, the `os` built-in module provides features for
  interacting with the host operating system
- Libraries, packages and modules are loaded via the `import name`
  syntax
  - Functions, variables and classes in a package are then accessed via
    the namespace e.g. `module.function()`

``` python
import os
os.listdir()
```

    ['Chapter_02.html',
     'Chapter_02_files',
     'Chapter_02.quarto_ipynb',
     'Chapter_02.qmd',
     'Chapter_02.ipynb',
     'Examples',
     'Chapter_02.md']

- Most community modules are located at [PyPI](https://pypi.org/) the
  python package index and are installed via `pip`
- For example, to install the `pandas` package

``` shell
pip install pandas
```

- Which can then be used in code
- We can re-alias an import with the `as` keyword
- For pandas a common idiom is to import `pandas` as `pd`

``` python
import pandas as pd

df = pd.read_csv("Examples/01_Pandas/hello.csv")
print(df)
```

    Empty DataFrame
    Columns: [Hello,  World!]
    Index: []

- Specific functions, classes or modules can be imported via the `from`
  keyword
  - These objects are imported into the importing namespace
  - This means they don’t need to prefixed with their module namespace

``` python
from os import path
print(path)
```

    <module 'posixpath' (frozen)>

#### Global Statements

- `global` followed by the reference to connect to
- Python programs consist of several different scopes
- Scopes refer to the regions were certain definitions such as variables
  or objects are accessible
- A program may only reference things available to the current scope
- A `global` statement is used to connect to the global scope
  - This is a scope that can be accessed from anywhere
- It is commonly used to resolve cases where it is ambiguous is we are
  referring to a global scope or creating a new variable in an inner
  scope

``` python
global_value = 3

print(f"global_value is {global_value}")
def func():
    local_value = 2 # defining a local value
    global_value = 4 # shadowing the global value
    print(f"local_value is {local_value}, global_value is {global_value}")

func()
print(f"global_value is {global_value}")

def func_2():
    local_value = 2
    global global_value # connect to the global value
    global_value = 4 # change the global value
    print(f"local_value is {local_value}, global_value is {global_value}")

func_2()
print(f"global_value is {global_value}")

# We cannot access `local_value` at this scope
print(local_value)
```

    global_value is 3
    local_value is 2, global_value is 4
    global_value is 3
    local_value is 2, global_value is 4
    global_value is 4

    NameError: name 'local_value' is not defined
    ---------------------------------------------------------------------------
    NameError                                 Traceback (most recent call last)
    Cell In[28], line 22
         18 func_2()
         19 print(f"global_value is {global_value}")
         20
         21 # We cannot access `local_value` at this scope
    ---> 22 print(local_value)

    NameError: name 'local_value' is not defined

#### Nonlocal Statements

- `nonlocal` works the same as `global` but can be used to connect
  objects from an inner scope to an enclosing scope
  - Especially useful when defining closures over functions

``` python
def outer_function():

    inner_called = 0

    def inner_function():

        nonlocal inner_called
        inner_called += 1
        print(f"Called {inner_called} times")

    return inner_function

f = outer_function()
# Calling `f` three times
f()
f()
f()
```

    Called 1 times
    Called 2 times
    Called 3 times

#### Print Statements

- print statements are used to print output

- Use the `print` built-in function

- Useful when we want to format output or merge several data objects
  together into some form of output

- `print` is quite a complex function that provides a lot of ways to
  control the output

- Some simple use cases,

- Passing a value to `print` prints it out

``` python
print(1)
print('a')
```

    1
    a

- Passing multiple arguments will result in all the arguments being
  printed
  - By default a space is printed between each argument

``` python
print(1, "b")
```

    1 b

- You can modify the default separator between shared arguments with the
  `sep` keyword argument

``` python
print(1, "b", sep="->")
```

    1->b

### Performing Basic Mathematics

- Python has built-in operator level support for most common
  mathematical operations
- This includes addition, subtraction, multiplication, division and
  exponentiation

``` python
2 + 3
```

    5

``` python
5 - 6
```

    -1

``` python
3 * 4
```

    12

``` python
9 / 3
```

    3.0

``` python
2**3
```

    8

- Division in Python is floating-point by default
  - This is different to many other programming languages where division
    between integers returns an integer
- To use integer division we can use the double-slash notation `//`

``` python
5 // 2
```

    2

- The modulo operator `%` returns the remainder of integer division

``` python
5 % 2
```

    1

- Modulo can be used to test if one number is divisible by another
  - If $a$ is divisible by $b$ then $a \% b = 0$

``` python
print(14 % 7 == 0)
```

    True

### Using Classes and Objects with Dot Notation

- A class is typically a way of defining a collection of data, and the
  associated operations on that data as one object
  - A concrete instance of a class is typically called an *object
    instance* or a *class instance*
- Methods and attributes on a class are accessed via the `.` operator
  - Methods are still called via the `()` operator

``` python
complex_number = 4 + 5j

# Accessing an attributes
print(f"The real part of the number is {complex_number.real}")


# Calling a method
print(f"The conjugate is {complex_number.conjugate()}")
```

    The real part of the number is 4.0
    The conjugate is (4-5j)

## Summary

- Python converts Python statements into computer instructions
- Each statement can be considered an action
  - A program is then a series of statements or actions
- Python represents data in a program through a number of different
  types
  - Types can be *built-in* or defined by other programs
  - An object’s type defines what attributes, methods and operations
    they expose
- Classes are types that combine data and methods on that data
  - Their attributes and methods can be accessed via the `.` operator

## Questions

1. With Python, what is the output of `type(12)`?

    - `12` is a whole number, so the type is `int`

2. When using Python, what is the effect of using `assert(True)` on the
    statements that follow it?

    - Nothing, an `assert(True)` statement does nothing
    - An `assert(False)` statement will raise an `AssertionError`
      exception

3. How would you use Python to invoke the exception `LastParamError`

    - We would write `raise LastParamError`

4. How would you use Python to print the string `"Hello"`?

    - We would write `print("Hello")`

5. How do you use Python to compute $2^{3}$?

    - We would write `2**3`

- We can see below,

``` python
# 1
print(type(12))

# 2
assert(True)
print("Hello, World!")

# 3
class LastParamError(Exception):
    pass

try:
    raise LastParamError
except LastParamError as e:
    print(f"Caught {e!r}")

# 4
print("Hello")

# 5
print(2 ** 3)
```

    <class 'int'>
    Hello, World!
    Caught LastParamError()
    Hello
    8
