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
type(13.1)
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
bar = x**2 if (x < y) and (y or z) else x // 2 # in-line conditional expression and integer division into variable assignment

print("x:", x ,"y:", y, "z:", z)
```

    NameError: name 'z' is not defined
    ---------------------------------------------------------------------------
    NameError                                 Traceback (most recent call last)
    Cell In[9], line 4
          1 x, y = 5, 6 # Statement hiding a tuple construction (5, 6), then tuple unpacking assignment x, y
          2 bar = x**2 if (x < y) and (y or z) else x // 2 # in-line conditional expression and integer division into variable assignment
          3
    ----> 4 print("x:", x ,"y:", y, "z:", z)

    NameError: name 'z' is not defined

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

    '/home/runner/work/foundational-data-science-for-python/foundational-data-science-for-python/Chapter_02'

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

#### Delete Statements

## Summary

## Questions
