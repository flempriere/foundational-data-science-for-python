# Chapter 6: Functions


- [Notes](#notes)
  - [Defining Functions](#defining-functions)
  - [Control Statement](#control-statement)
  - [Docstrings](#docstrings)
  - [Parameters](#parameters)
  - [`return` Statements](#return-statements)
  - [Scope in Functions](#scope-in-functions)
  - [Decorators](#decorators)
  - [Anonymous Functions](#anonymous-functions)
- [Summary](#summary)
- [Questions](#questions)

## Notes

- Functions are a way of creating repeatable code blocks

### Defining Functions

- A function definition creates a function object
  - Wraps the block of code
  - Does not execute it
- Describes how a function is called
  - Name
  - What arguments it receives
  - What is executed when invoked
  - What is returned

### Control Statement

- The definition of a function takes the form of a control statement

  ``` python
    def function_name(function_arguments):
        <statements>
  ```

- `def` indicates a function definition

- We then provide the function name

- Then a comma-separated list of arguments the function accepts

  - Surrounded by parentheses
  - Note no space between the name and the list

- Finally a colon

- The most basic function we can write would be

``` python
def func():
    pass

print(func)
```

    <function func at 0x7fd2746a1bc0>

- This defines a function `func` that accepts no arguments, does nothing
  and returns nothing
- `pass` in python is a special statement that does nothing
- Python style guides state that functions should follow `snake_case`

### Docstrings

- Functions definitions are optionally followed by a *docstring*
- This is a special string that provides the documentation describing a
  function and it’s usage
- Docstrings can be single-line or multi-line
  - For multi-line strings we have to use the triple-delimit format
    `"""string"""`
- Docstrings can be expected in code by the `help` function

``` python
def do_nothing(not_used):
    """This is a function doing nothing

    Parameters:
        not_used - an unused parameter
    """

    pass

help(do_nothing)
```

    Help on function do_nothing in module __main__:

    do_nothing(not_used)
        This is a function doing nothing

        Parameters:
            not_used - an unused parameter

### Parameters

- Functions can define *parameters* that can be passed into them
  - The actual values passed are referred to as *arguments*
- Parameters act like local variables for a function
  - Enable functions to communicate between each other
- By default arguments are passed in definition order
  - Alternatively can pass them by keyword, i.e. using the
    `parameter=argument` syntax
  - We can mix the two, but all positional arguments must *precede* the
    keyword arguments

``` python
def parameterised_function(first, second, third):
    """Prints out the parameters"""

    print(f"First: {first}")
    print(f"Second: {second}")
    print(f"Third: {third}")

# passing using positional arguments
parameterised_function(1, 2, 3)

# passing using keyword arguments
parameterised_function(first=1, second=2, third=3)

# passing using mixed
parameterised_function(1, 2, third=3)

# incorrect use of mixed
parameterised_function(second=2, 1, 3)
```

    SyntaxError: positional argument follows keyword argument (3539635243.py, line 18)
      Cell In[3], line 18
        parameterised_function(second=2, 1, 3)
                                             ^
    SyntaxError: positional argument follows keyword argument

- If you want to force certain parameters to *only* be passed by keyword
  you can indicate that by the `*` placeholder in the argument list
  - All parameters that come after *must* be keyword-specified
- If we want to force something to be supplied *only* positionally then
  we can use the `/` placeholder
  - All parameters *before* must be positional only

``` python
def fixed_position_and_keyword(first, /, second, *, third):
    """Prints out the parameters"""

    print(f"First: {first}")
    print(f"Second: {second}")
    print(f"Third: {third}")

# using pos, pos, kwarg
fixed_position_and_keyword(1, 2, third=3)

# using pos, kwarg, kwarg
fixed_position_and_keyword(1, second=2, third=3)

# trying fully positional
fixed_position_and_keyword(1, 2, 3)
```

    First: 1
    Second: 2
    Third: 3
    First: 1
    Second: 2
    Third: 3

    TypeError: fixed_position_and_keyword() takes 2 positional arguments but 3 were given
    ---------------------------------------------------------------------------
    TypeError                                 Traceback (most recent call last)
    Cell In[4], line 15
         11 # using pos, kwarg, kwarg
         12 fixed_position_and_keyword(1, second=2, third=3)
         13 
         14 # trying fully positional
    ---> 15 fixed_position_and_keyword(1, 2, 3)

    TypeError: fixed_position_and_keyword() takes 2 positional arguments but 3 were given

- Parameters can be assigned optional default values
  - Used if no other value is supplied

``` python
def does_default(first, second, third=3):
    """Prints parameters"""

    print(f"First: {first}")
    print(f"Second: {second}")
    print(f"Third: {third}")

does_default(1, 2)
```

    First: 1
    Second: 2
    Third: 3

- Like passing keyword arguments to a function, we once we have defined
  a default value for a parameter, all subsequent parameters must also
  have default values

- A common pitfall is *when* the default value is defined

  - It is defined when the function is defined
  - This means that if we define a mutable type like a `list` or
    `dictionary` it is shared between all calls to the function

``` python
def does_default_list(my_list=[]):
    """Uses a list as a default value"""

    my_list.append(1)
    print(my_list)

does_default_list()
does_default_list()
```

    [1]
    [1, 1]

- In general avoid using mutable types for the default
  - If you need default mutables a work-around is to use `None` as the
    default value and then check

``` python
def fixed_default_list(my_list=None):
    """Defaults to an  empty list on every call"""

    if my_list is None:
        my_list = []
    my_list.append(1)
    print(my_list)

fixed_default_list()
fixed_default_list()
```

    [1]
    [1]

- To accept an arbitrary number of positional arguments or
  keyword-arguments we can use the `*args` or `**kwargs` arguments
  - `args` becomes a list of all the supplied position arguments not
    mapped to an existing parameter
  - `kwargs` works similar but is a dictionary mapping argument names to
    values

``` python
def args_and_kwargs(*args, **kwargs):
    """Accepts and prints arbitrary positional and keyword arguments"""

    print(args)
    print(kwargs)

args_and_kwargs(1, 2, animals=["Donkey", "Hippo"], name="foo")
```

    (1, 2)
    {'animals': ['Donkey', 'Hippo'], 'name': 'foo'}

### `return` Statements

- `return` statements cause a function to exit and optionally return a
  value
  - Otherwise a function terminates when it reaches the end of it’s
    statements and returns a value of `None`

``` python
def adds_one(some_number):
    """increments the supplied number by one

    Parameters
    some_number - number to be incremented

    Returns
    some_number + 1"""

    return some_number + 1

print(adds_one(0))
```

    1

### Scope in Functions

- As discussed *scope* refers to what and where objects can be seen
- The global scope refers to objects that can be accessed from anywhere
- Functions define their own *local* scope
  - Anything defined in a function is only accessible in that function’s
    scope
  - Functions can redefine variables in their scope using the name of an
    outer scope variable
    - This is called shadowing and should generally be avoided
    - The outer variable is untouched

``` python
outer = "Global scope"

def shows_scope():
    """Demonstrates local variable"""

    inner = "inner scope"
    print("Outer inside function:", outer)
    print("Inner inside function:", inner)

shows_scope()
print("Outer outside function:", outer)
print("Inner outside function:", inner)
```

    Outer inside function: Global scope
    Inner inside function: inner scope
    Outer outside function: Global scope

    NameError: name 'inner' is not defined
    ---------------------------------------------------------------------------
    NameError                                 Traceback (most recent call last)
    Cell In[10], line 12
          8     print("Inner inside function:", inner)
          9 
         10 shows_scope()
         11 print("Outer outside function:", outer)
    ---> 12 print("Inner outside function:", inner)

    NameError: name 'inner' is not defined

### Decorators

- Decorators are special functions that *wrap* other functions
  - Useful for adding additional functionality to an existing function
    e.g. logging
- Function’s can be referenced like any other object
  - Just have to pass their name around like any other variable
  - `()` denotes *calling* a function
- For example, to assign the function `add_one` to another variable
  `my_func` we write,

``` python
def add_one(n):
    """Adds one to a number"""

    return n + 1

my_func = add_one
print(my_func)
print(my_func(2))
```

    <function add_one at 0x7fd2746a26c0>
    3

- Since functions can be treated as any other object or variable they
  can be passed to functions or stored in data structures

``` python
def add_one(n):
    """Adds one to a number"""

    return n + 1

def add_two(n):
    """Adds two to a number"""

    return n + 2

functions = [add_one, add_two]

for func in functions:
    print(func(1))
```

    2
    3

- Python also allows functions to be defined *inside* another function
  - Sometimes referred to as a *nested function*
  - Useful for making things like closures which are functions with some
    captured state
  - They can even be returned from a function

``` python
def call_nested():
    """Calls a nested function"""

    print("outer")

    def nested():
        """Prints a message"""

        print("nested")

    return nested

my_func = call_nested()
my_func()
```

    outer
    nested

- Functions can call other functions
  - Allows for defining nesting and building up behaviours

``` python
def add_one(n):
    """Adds one to a number"""

    print("Adding one")
    return n + 1

def wrapper(number):
    """Wraps the `add_one` function"""

    print("Before calling function")
    retval = add_one(number)
    print("After calling function")

    return retval

wrapper(1)
```

    Before calling function
    Adding one
    After calling function

    2

- We can also accept functions as parameters
  - We can build up all these features into quite complex constructs
  - For example, we can define a function which accepts a function,
    applies a wrapper and returns the wrapped function

``` python
def add_one(n):
    """Adds one to a number"""

    print("Adding one")
    return n + 1

def wrap_function(func):
    """Applies a wrapper to a function"""

    def wrapper(number):
        """Wraps the `add_one` function"""

        print("Before calling function")
        retval = func(number)
        print("After calling function")
        return retval

    return wrapper

wrapped = wrap_function(add_one)
wrapped(1)
```

    Before calling function
    Adding one
    After calling function

    2

- Decorators are a form of syntactic sugar
  - Allow this wrapping behaviour without having to manage all the
    set-up and storage
- They use the `@decorator` syntax
- To recreate our wrapping function above as a decorator we would write

``` python
def wrap_function(func):
    """Applies a wrapper to a function"""

    def wrapper(number):
        """Wraps the `add_one` function"""

        print("Before calling function")
        retval = func(number)
        print("After calling function")
        return retval

    return wrapper

@wrap_function
def add_one(n):
    """Adds one to a number"""

    print("Adding one")
    return n + 1

add_one(1)
```

    Before calling function
    Adding one
    After calling function

    2

### Anonymous Functions

- Sometime’s it’s useful to define small *anonymous* functions for
  sort-lived or ephemeral tasks
- These functions are sometimes called *lambdas*
- In Python they are defined by the syntax
  `lambda <parameters>: <statements>`
  - `parameters` are the input parameters to a `lamba`, while
    `statements` are the statements to execute
- In general, avoid using `lamba` except where they are needed
  - Normal functions are clearer

``` python
def apply_to_list(data, func):
    """Applies the function to the elements in data"""
    for item in data:
        print(f"{func(item)}")

apply_to_list([1, 2, 3], lambda x: x + 1)
```

    2
    3
    4

## Summary

- Functions are a cornerstone of programs
- They enable one to define named, reusable code blocks
- Functions can be documented with *docstrings* a standard format for
  documenting code
- Functions can accept parameters
  - Via position or keyword argument
  - Parameters may optionally have default values
- Functions use `return` statements to return control to the caller and
  optionally supply a value
- Python treats functions as first class objects meaning they can be
  treated like any other variable or object
- Decorators are a special syntax for simplifying the process of
  creating wrapped functions

## Questions

Refer to the code example below for the first three questions and
question five.

``` python
def add_prefix(word, prefix="before-"):
    """Prepend a word."""
    return f"{prefix}{word}"

def return_one():
    return 1

def wrapper():
    print("a")
    retval = return_one()
    print("b")
    print(retval)
```

1.  What would be the output of the following?

    ``` python
     add_prefix("nighttime", "after-")
    ```

    - The output should be `"after-nighttime"`

2.  What would be the output of the following?

    ``` python
     add_prefix("nighttime")
    ```

    - The output should be `"before-nighttime"`

3.  What would be the output of the following call?

    ``` python
     add_prefix()
    ```

    - We would get an error as we have failed to supply the required
      `word` parameter

4.  What line should you put above a function definition to decorate it
    with the function `standard_logging`?

    - The decorator syntax is `@standard_logging`

5.  What would be printed by the following call?

    ``` python
     wrapper()
    ```

    - The output should be

      ``` shell
         a
         b
         1
      ```

As we can see,

- For Questions one through three

``` python
# Questions 1 - 3
def add_prefix(word, prefix="before-"):
    """Prepend a word."""
    return f"{prefix}{word}"

def return_one():
    return 1

def wrapper():
    print("a")
    retval = return_one()
    print("b")
    print(retval)

print(add_prefix("nighttime", "after-"))
print(add_prefix("nighttime"))
print(add_prefix)
```

    after-nighttime
    before-nighttime
    <function add_prefix at 0x7fd2746a2e50>

- and for four and five,
  - Where we’ve added a demo of `simple_logging`

``` python
# Questions 1 - 3
def add_prefix(word, prefix="before-"):
    """Prepend a word."""
    return f"{prefix}{word}"

def return_one():
    return 1

def simple_logging(func):

    def wrapped():
        print("logged")
        func()

    return wrapped

@simple_logging
def wrapper():
    print("a")
    retval = return_one()
    print("b")
    print(retval)

wrapper()
```

    logged
    a
    b
    1
