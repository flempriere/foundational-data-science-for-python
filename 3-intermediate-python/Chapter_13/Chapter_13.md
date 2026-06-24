# Chapter 13: Functional Programming

- [Notes](#notes)
  - [Introduction to Functional
    Programming](#introduction-to-functional-programming)
    - [Scope and State](#scope-and-state)
    - [Depending on Global Scope](#depending-on-global-scope)
    - [Changing State](#changing-state)
    - [Changing Mutable Data](#changing-mutable-data)
  - [Functional Programming
    Functions](#functional-programming-functions)
    - [`map`](#map)
    - [`reduce`](#reduce)
    - [`filter`](#filter)
  - [List Comprehensions](#list-comprehensions)
    - [List Comprehension Syntax](#list-comprehension-syntax)
    - [Replacing `map` and `filter`](#replacing-map-and-filter)
    - [Multiple Variables](#multiple-variables)
  - [Dictionary Comprehensions](#dictionary-comprehensions)
  - [Generators](#generators)
    - [Generator Expressions](#generator-expressions)
    - [Generator Functions](#generator-functions)
- [Summary](#summary)
- [Questions](#questions)

## Notes

- Functional Programming is a programming *paradigm*
- A *paradigm* is a way of structuring and organising code
- Python is a multi-paradigm language, which means it includes support
  for the functional paradigm

### Introduction to Functional Programming

- Functional programming organises code around *functions*
- In particular, *pure functions*
- Pure functions are those that can be thought of like traditional
  mathematical functions
  - An input is provided and a fixed output is provided
  - Functions should be *side effect free*, in other words every action
    should have the same result every time
- Some functional languages like *Haskell* and *Erlang* adhere to this
  strictly
  - Python lets you mix and in functional elements without fully
    committing

#### Scope and State

- A program’s *state* is the names, definitions, and values existing at
  a given time in a program

- This includes,

  1. Function definitions
  2. Imported modules
  3. variables and their values

- *Scope* refers to where the *state* holds.

  - Scopes follow a hierarchy
    - Nested scopes *inherit* the state from the outer scope
    - Nested scopes are *isolated* from the state of other scopes at the
      same level

- For example, below demonstrates a few different scopes and their
  respective states

``` python
a = "a outer"
b = "b outer"

def scoped_function():
    a = "a inner"
    print(a)
    print(b)

scoped_function()
print(a)
print(b)
```

    a inner
    b outer
    a outer
    b outer

#### Depending on Global Scope

- In Procedural programming code is typically run sequentially

- Each step modifies the state of the previous step

- Function’s that rely on state then can have a different output for a
  given input depending on the state of the system

- For example, a procedural approach to a function `describe_the_wind`
  might be implemented as below

  - Here the function depends on the global state `wind`
  - Calling the same function results in a different outcome as the
    global state changes

``` python
wind = "Southeast"


def describe_the_wind():
    return f"The wind blows from the {wind}"


describe_the_wind()

wind = "Northeast"

describe_the_wind()
```

    'The wind blows from the Northeast'

- A more functional approach (and arguably still the preferred
  procedural implementation) would instead take `wind` as a parameter

``` python
def describe_the_wind(wind):
    return f"The wind blows from the {wind}"


describe_the_wind("Northeast")
```

    'The wind blows from the Northeast'

#### Changing State

- As discussed functions in a functional paradigm should be side-effect
  free

- As mentioned above that means one aspect is not depending on external
  state

- Another aspect is that the function itself should not change an outer
  state

- For example, we might have procedural code that changes the state of
  the wind as follows

``` python
WINDS = ["Northeast", "Northwest", "Southeast", "Southwest"]
wind = WINDS[0]


def change_wind():
    global wind
    wind = WINDS[(WINDS.index(wind) + 1) % 3]


print(wind)

change_wind()

print(wind)

for _ in WINDS:
    print(wind)
    change_wind()
```

    Northeast
    Northwest
    Northwest
    Southeast
    Northeast
    Northwest

- A more functional approach is to move the `WINDS` variable into the
  inner state
- `change_wind` then accepts a parameter indicating which state should
  be retrieved

``` python
def change_wind(wind_index):
    winds = ["Northeast", "Northwest", "Southeast", "Southwest"]
    return winds[wind_index]


print(change_wind(0))

print(change_wind(1))

print(change_wind(2))

print(change_wind(3))
```

    Northeast
    Northwest
    Southeast
    Southwest

#### Changing Mutable Data

- One method of changing state is by modifying mutable objects
  - i.e. those whose contents or attributes can be modified
  - This includes lists, dictionaries etc.
- If a function changes a mutable object, then the state in the scope of
  that object is changed
  - This is not true for *immutable* objects for example,

``` python
b = 1


def foo(a):
    a = 2


foo(b)
print(b)
```

    1

- Here we modify the value of `b` in `foo` but once we leave the scope
  `b` is unchanged
- This is not true for *mutable* objects

``` python
d = {"vehicle": "ship", "owner": "Joseph Bruce Ismay"}

print("Before:", d)


def change_mutable_data(data):
    data["owner"] = "White Star Line"


change_mutable_data(d)
print("After:", d)
```

    Before: {'vehicle': 'ship', 'owner': 'Joseph Bruce Ismay'}
    After: {'vehicle': 'ship', 'owner': 'White Star Line'}

- A way around this is to operate on *copies* of a mutable object
  - By default these are *shallow copies*, that means if a mutable
    object *contains* mutable objects then enclosed objects are *not*
    copies
- A downside of working with copies is that for large data structures
  they can lead to excess memory consumption and increased time

``` python
d = {"vehicle": "ship", "owner": "Joseph Bruce Ismay"}

print("Before:", d)


def change_owner(data):
    new_data = data.copy()
    new_data["owner"] = "White Star Line"
    return new_data


print("Changed:", change_owner(d))
print("After:", d)
```

    Before: {'vehicle': 'ship', 'owner': 'Joseph Bruce Ismay'}
    Changed: {'vehicle': 'ship', 'owner': 'White Star Line'}
    After: {'vehicle': 'ship', 'owner': 'Joseph Bruce Ismay'}

### Functional Programming Functions

- The three fundamental functions of functional programming are,

  1. `map`
  2. `filter`
  3. `reduce`

#### `map`

- Applies a function to a sequence of values
  - Can be any `iterable` type
  - Means something which we can access objects in sequence
- Returns a `map` object
  - This is also an `iterable`
  - Provides a memory-efficient result

``` python
def grow_flowers(d):
    return d * "❀"


gardens = map(grow_flowers, [0, 1, 2, 3, 4, 5])
print(type(gardens))
print(list(gardens))
```

    <class 'map'>
    ['', '❀', '❀❀', '❀❀❀', '❀❀❀❀', '❀❀❀❀❀']

- `map` can accept a function with multiple arguments
  - In that case multiple iterables are supplied
  - Each acts as the input stream for one of the arguments
- The `map` will truncate at the length of the shortest supplied
  iterable

``` python
l1 = [0, 1, 2, 3, 4]
l2 = [11, 19, 9, 8, 7, 6]


def multi(d1, d2):
    return d1 * d2


result = map(multi, l1, l2)
print(list(result))
```

    [0, 19, 18, 24, 28]

#### `reduce`

- `reduce` is supplied via the `functools` built-in library
- `reduce` accepts a function and an iterable
  - Uses these to return a single value
- For example, subtracting an amount from an account balance

``` python
from functools import reduce

initial_balance = 10_000
debits = [20, 40, 300, 3_000, 1, 234]


def minus(a, b):
    return a - b


balance = reduce(minus, debits, initial_balance)
print(balance)
```

    6405

- This is equivalent to the following `for` loop

``` python
initial_balance = 10_000
debits = [20, 40, 300, 3_000, 1, 234]

balance = initial_balance

for debit in debits:
    balance -= debit

print(balance)
```

    6405

- The `operator` built-in module provides functions equivalent to the
  standard mathematical operations
  - These can easily be used for `reduce` rather than having to define
    the functions yourself

``` python
from functools import reduce
import operator

initial_balance = 10_000
debits = [20, 40, 300, 3_000, 1, 234]

balance = reduce(operator.sub, debits, initial_balance)
print(balance)
```

    6405

#### `filter`

- `filter` accepts a function and an iterable as an argument
- The function should map items to `True` or `False`
- Returns an iterable containing only those values for which the
  function returned `True`
- For example, extracting capital letters from a string

``` python
charles = "ChArlesTheBald"


def is_cap(a):
    return a.isupper()


capitals = filter(is_cap, charles)
list(capitals)
```

    ['C', 'A', 'T', 'B']

- In general `map`, `filter` and `reduce` work well with `lambda`
  functions
  - Very useful for performing quick ephemeral calculations rather than
    littering your codebase with small one time functions

``` python
nums = filter(lambda x: x > 3, range(10))
list(nums)
```

    [4, 5, 6, 7, 8, 9]

### List Comprehensions

- We used List Comprehensions (and the other comprehensions) briefly in
  [Chapter 12](../../2-data-science-libraries/Chapter_12/Chapter_12.qmd)
- They are inspired by the [similar Haskell
  construct](https://docs.python.org/3/howto/functional.html)
  - Roughly like a *one-line* loop to create a list

#### List Comprehension Syntax

- The syntax for a list comprehension is roughly

  ``` python
    [<item-returned> for <item-sourced> in <iterable>]
  ```

- They are useful when you want to perform some processing on a list,
  for example normalising a list of strings to use the same case

``` python
names = ["tim", "tiger", "tabassum", "theodora", "tanya"]
titled = [x.title() for x in names]
print(titled)
```

    ['Tim', 'Tiger', 'Tabassum', 'Theodora', 'Tanya']

- This is equivalent to the following `for` loop

``` python
names = ["tim", "tiger", "tabassum", "theodora", "tanya"]

titled = []
for name in names:
    titled.append(name.title())

print(titled)
```

    ['Tim', 'Tiger', 'Tabassum', 'Theodora', 'Tanya']

#### Replacing `map` and `filter`

- In most cases list comprehensions provide a cleaner way to implement
  `map` and `filter` type transformations
- For example, we might want to inject numbers into a string, which we
  could do via `map`

``` python
def count_flower_petals(d):
    return f"{d} petals counted so far"

counts = map(count_flower_petals, range(6))
list(counts)
```

    ['0 petals counted so far',
     '1 petals counted so far',
     '2 petals counted so far',
     '3 petals counted so far',
     '4 petals counted so far',
     '5 petals counted so far']

- The equivalent list comprehension would be

``` python
counts = [f"{d} petals counted so far" for d in range(6)]
print(counts)
```

    ['0 petals counted so far', '1 petals counted so far', '2 petals counted so far', '3 petals counted so far', '4 petals counted so far', '5 petals counted so far']

- To replicate `filter` type functionality we can add a conditional to
  the `for`
  - For example, we can replicate our capital letter filter from
    [before](#filter)

``` python
characters = ["D", "b", "c", "A", "b", "P", "g", "S"]

[x for x in characters if x.isupper()]
```

    ['D', 'A', 'P', 'S']

#### Multiple Variables

- If an iterable’s elements are multi-valued then they can be unpacking
  in the list comprehension

``` python
points = [(12, 3), (-1, 33), (12, 0)]

[f"x: {x} y: {y}" for x, y in points]
```

    ['x: 12 y: 3', 'x: -1 y: 33', 'x: 12 y: 0']

- An alternative for nested structures is to use a nested `for` loop
  structure
  - The syntax can be a little bit confusing due to the ordering going
    from outermost in innermost

``` python
list_of_lists = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

[x for y in list_of_lists for x in y]
```

    [1, 2, 3, 4, 5, 6, 7, 8, 9]

### Dictionary Comprehensions

- The dictionary equivalent of a of a list comprehensions
- Rather than defining list elements we define *key*-*value* pairs
  - Wrapped in the dictionary style `{}` delimiters rather than `[]`

``` python
names = ["James", "Jokubus", "Shaemus"]
scores = [12, 33, 23]

{name: score for name in names for score in scores}
```

    {'James': 23, 'Jokubus': 23, 'Shaemus': 23}

### Generators

- Generators are special iterable objects that return values on the fly
- Contrasts with data types like `list` which store all the values at
  once
- A corollary is that while lists must be of a finite size, generators
  can theoretically be infinite
- Generators are useful since they keep the memory footprint of a
  sequence low

#### Generator Expressions

- Generators can be created through a list comprehension like syntax
  - Delimited by `()` instead of `[]`
- For example, the code below shows a list comprehension and the
  equivalent generator expression

``` python
l_ten = [x**3 for x in range(10)]
g_ten = (x**3 for x in range(10))

print(f"l_ten is a {type(l_ten)}")
print(f"l_ten is prints as: {l_ten}")

print(f"g_ten is a {type(g_ten)}")
print(f"g_ten prints as {g_ten}")
```

    l_ten is a <class 'list'>
    l_ten is prints as: [0, 1, 8, 27, 64, 125, 216, 343, 512, 729]
    g_ten is a <class 'generator'>
    g_ten prints as <generator object <genexpr> at 0x7f0fbc2524d0>

- A generator does not provide it’s contents
- To get values from a generator, you can call `next`

``` python
g_ten = (x**3 for x in range(10))

next(g_ten)
```

    0

- This is done implicitly when one wants to iterate through a generator
  with a `for` loop

``` python
g_ten = (x**3 for x in range(10))

for x in g_ten:
    print(x)
```

    0
    1
    8
    27
    64
    125
    216
    343
    512
    729

- Since generators don’t support arbitrary item access then cannot be
  indexed or sliced

``` python
g_ten = (x**3 for x in range(10))

g_ten[3]
```

    TypeError: 'generator' object is not subscriptable
    ---------------------------------------------------------------------------
    TypeError                                 Traceback (most recent call last)
    Cell In[27], line 3
          1 g_ten = (x**3 for x in range(10))
          2
    ----> 3 g_ten[3]

    TypeError: 'generator' object is not subscriptable

- Since generators return their results one at a time, they have a much
  smaller memory footprint
  - For example, here we contrast the size of a list of one million
    items with the equivalent generator

``` python
import sys

size = 1_000_000

l_big = [x for x in range(size)]
g_big = (x for x in range(size))

print(f"l_big is {sys.getsizeof(l_big)} bytes")
print(f"g_big is {sys.getsizeof(g_big)} bytes")
```

    l_big is 8448728 bytes
    g_big is 200 bytes

#### Generator Functions

- An alternate construction is the use of so called *generator
  functions*
- They can be used to create complex generators beyond what simple
  expressions can provide
- Look like normal functions but rather than `return` a value, they
  `yield` them
- Generator functions can maintain an internal state to determine what
  value to `yield` next

``` python
def square_them(numbers):
    for number in numbers:
        yield number**2


s = square_them(range(5))

print(next(s))
print(next(s))
print(next(s))
```

    0
    1
    4

- As mentioned, generators can be infinite as demonstrated below

``` python
def counter(d):
    while True:
        d += 1
        yield d


c = counter(10)

print(next(c))
print(next(c))
print(next(c))
```

    11
    12
    13

- Generators can be combined and built-up
- This allows complex behaviour to be created from many individual
  generators while still providing the net benefit

``` python
evens = (x * 2 for x in range(10))
three_factors = (x // 3 for x in evens if x % 3 == 0)
titles = (f"This number is {x}" for x in three_factors)

capitalised = (x.title() for x in titles)

print(f"The first call to capitalised: {next(capitalised)}")
print(f"The second call to capitalised: {next(capitalised)}")
print(f"The third call to capitalised: {next(capitalised)}")
```

    The first call to capitalised: This Number Is 0
    The second call to capitalised: This Number Is 2
    The third call to capitalised: This Number Is 4

## Summary

- Functional programming is a paradigm for organising code around
  functions
- Based on the idea of *pure functions*
  - Functions should be side-effect free
  - A function’s state should only change by being called
  - The sample input should always return the same output
- Python provides support for functional programming
  - The basic `map` and `filter` are built-in functions
  - `reduce` is provided by the `functools` built-in library
- Python provides an extensive comprehension syntax for a range of types
  including
  - list
  - set
  - dictionaries
- Generators are a powerful functional approach for sequentially
  accessing data
  - They are useful for providing memory efficient access to sequences

## Questions

1. What would the following code print?

    ``` python
     a = 1
     b = 2

     def do_something(c):
         c = 3
         a = 4
         print(a)
         return c

     b = do_something(b)
     print(a + b)
    ```

        4
        4

    - `do_something` returns `3` and assigns it to `b`
      - It temporarily reassigns `a` to $4$
      - As a side effect it calls `print` on `a`, so prints $4$
    - In the outer scope the value of `a` is once again $1$
    - Then we call the equivalent of `print(3 + 1)` so the final result
      is `4`

2. Use the `map` function to take the string `"omni"` and return the
    list `["oo", "mm", "nn", "ii"]`

    - This is a pretty straightforward exercise once recall that a
      string is something we can iterate over

    - Each iteration returns a sequential character

    - We can then use string multiplication to duplicate the character

    - Last step is to convert the `map` object to a `list`

      ``` python
         word = "omni"
         result = list(map(lambda c : c * 2, word))
         print(result)
      ```

          ['oo', 'mm', 'nn', 'ii']

3. Use the `sum` function which sums the contents of a sequence, with a
    list comprehension to find the summation of the positive even
    numbers below $100$

    - We can do this as a fairly simple one-liner, bypassing the list
      comprehension entirely

      ``` python
         total = sum(range(0, 100, 2))
         print(total)
      ```

          2450

4. Write a generator expression that returns cubed numbers up to
    $1,000$

    - We’ll interpret *cubed numbers up to 1,000* as meaning the number
      to be cubed, not the result

    - This is then again a simple one liner

      ``` python
         cubed_numbers = (x**3 for x in range(1_000))

         for i in range(3):
             print(next(cubed_numbers))
      ```

          0
          1
          8

5. A Fibonacci Sequence starts with $0$ and $1$, and every subsequent
    number is the sum of the previous two numbers. Write a generator
    function that calculates a Fibonacci sequence.

    - For this we need the function to track the previous *two*
      fibonacci results
    - But, we need to have a special case for the first two numbers
    - We can do this by writing two explicit `yield` statements,
      followed by an infinite loop

    ``` python
     def fibonacci_sequence():
         """
         Produces the fibonacci sequence

         Yields
         ======
         int
             The $i$-th call, where $i$ is the number of calls
         """
         x_0 = 0
         x_1 = 1

         while True:
             yield x_0
             x_0, x_1 = x_1, x_0 + x_1

     fib = fibonacci_sequence()
     for i in range(10):
         print(f"{i}-th fibonacci:", next(fib))
    ```

        0-th fibonacci: 0
        1-th fibonacci: 1
        2-th fibonacci: 1
        3-th fibonacci: 2
        4-th fibonacci: 3
        5-th fibonacci: 5
        6-th fibonacci: 8
        7-th fibonacci: 13
        8-th fibonacci: 21
        9-th fibonacci: 34

- The questions are repeated below for use as executable cells

``` python
print("Question 1")

a = 1
b = 2

def do_something(c):
    c = 3
    a = 4
    print(a)
    return c

b = do_something(b)
print(a + b)
```

    Question 1
    4
    4

``` python
print("Question 2")

word = "omni"
result = map(lambda c : c * 2, word)
result = list(result)
print(result)
```

    Question 2
    ['oo', 'mm', 'nn', 'ii']

``` python
print("Question 3")

total = sum(range(0, 100, 2))
print(total)
```

    Question 3
    2450

``` python
print("Question 4")

cubed_numbers = (x**3 for x in range(1_000))

for i in range(3):
    print(next(cubed_numbers))
```

    Question 4
    0
    1
    8

``` python
print("Question 5")

def fibonacci_sequence():
    """
    Produces the fibonacci sequence

    Yields
    ======
    int
        The $i$-th call, where $i$ is the number of calls
    """
    x_0 = 0
    x_1 = 1

    while True:
        yield x_0
        x_0, x_1 = x_1, x_0 + x_1


fib = fibonacci_sequence()
for i in range(10):
    print(f"{i}-th fibonacci:", next(fib))
```

    Question 5
    0-th fibonacci: 0
    1-th fibonacci: 1
    2-th fibonacci: 1
    3-th fibonacci: 2
    4-th fibonacci: 3
    5-th fibonacci: 5
    6-th fibonacci: 8
    7-th fibonacci: 13
    8-th fibonacci: 21
    9-th fibonacci: 34
