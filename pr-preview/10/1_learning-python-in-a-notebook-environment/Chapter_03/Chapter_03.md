# Chapter 3: Sequences


- [Notes](#notes)
  - [Shared Operations](#shared-operations)
    - [Testing Membership](#testing-membership)
    - [Indexing](#indexing)
    - [Slicing](#slicing)
    - [Interrogation](#interrogation)
  - [Mathematical Operations](#mathematical-operations)
  - [Lists and Tuples](#lists-and-tuples)
    - [Creating Lists and Tuples](#creating-lists-and-tuples)
    - [Adding and Removing Items](#adding-and-removing-items)
    - [Unpacking](#unpacking)
    - [Sorting Lists](#sorting-lists)
  - [Strings](#strings)
  - [Ranges](#ranges)
- [Summary](#summary)
- [Questions](#questions)

## Notes

- Python’s *duck typing* defines a number of *interface types* which
  describe the kind of methods and functionality they support
- *Collection* types, are types which *collect* a number of different
  objects, think arrays or lists
- Python offers a number of different types of collections, once being
  the *sequence* type
  - These are collections that are finite and ordered
- Python has a number of different concrete sequence implementations
  1.  `list`
  2.  `tuple`
  3.  `string`
  4.  `bytes`
  5.  `range`

### Shared Operations

- As mentioned, python uses *duck typing*
  - Meaning if an object supports the methods of an interface, then it
    *is* that interface
- This holds true for sequence types too
  - There are a number of common sequence methods that can be used
    regardless of the concrete sequence implementation

#### Testing Membership

- The `in` operator can be used to test if an object is included in a
  sequence
  - This uses the equality test of the object type
  - `in` returns `True` if the object is in the sequence else `False`

``` python
sequence = ['first', 'second', 'third'] # A list
print('first' in sequence)
print('fourth' in sequence)
```

    True
    False

- To test for the absence of an object we can either use `in` and take
  the negation of the result via the `not` operator

``` python
sequence = ['first', 'second', 'third'] # A list
print('first' not in sequence)
print('fourth' not in sequence)
```

    False
    True

#### Indexing

- Sequences support arbitrary access via indexing operators `[]`
- This allows a program to select a specific index in an sequence by
  referencing the index of the object
- Indices start at $0$ and go to $n - 1$ for a sequence of length $n$
- To access the $i$-th element, we write `[i]`

``` python
name = "Ignatius"
print(f"The first character is {name[0]}")
print(f"The fourth character is {name[4]}")
```

    The first character is I
    The fourth character is t

- Negative numbers can be used to index a sequence in reverse
  - Useful for patterns like accessing the last character in a sequence
    of unknown length

``` python
name = "Ignatius"
print(f"The last character is {name[-1]}")
```

    The last character is s

#### Slicing

- The slice operator can be used to create a copy of a subsequence of
  items from an existing sequence
- Syntax is similar to the indexing operator `[start:stop:step]`
  - `start` is inclusive
    - i.e. the slice will include the `start` index
  - `stop` is exclusive
    - i.e. the slice will not include the `stop` index
  - Each component is optional
    - Omitting the start, slices from the start of the sequence
    - Omitting the stop, slices to the end of the sequence
    - Omitting the `:step` implies a step of one, i.e. every element

``` python
name = "Ignatius"

# slicing a subsequence
print(name[2:5])

# slicing from the start
print(name[:5])

# slicing to the end
print(name[4:])

# slicing from the end of a sequence
print(name[-3:])
```

    nat
    Ignat
    tius
    ius

- Now if we want to use the `step` parameter

``` python
scores = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

# Taking every second number
print(scores[::2])

# Counting backwards and steping
print(scores[9:0:-4])
```

    [0, 2, 4, 6, 8]
    [9, 5, 1]

- In general, doing complicated slicing and steping in the same
  operation can be unintuitive and hard to get right
- Prefer slicing and stepping as two distinct steps
  - For example to reverse a list taking every second item in the last
    five items

``` python
scores = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

# reverse
backwards = scores[::-1]
trim = backwards[-5:]
print(trim[::2])
```

    [4, 2, 0]

#### Interrogation

- There are a number of python built-in functions designed to work on
  sequence types
- `len` returns the length (or number of elements) in a sequence

``` python
names = ["Ignatius", "Hal"]
print(f"length of names is {len(names)}, length of names[1] is {len(names[1])}")
```

    length of names is 2, length of names[1] is 3

- `min` and `max` return the minimum and maximum value in a sequence
  respectively

``` python
scores = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
print(f" min is {min(scores)}, max is {max(scores)}")
```

     min is 0, max is 9

- These methods rely on the types in a sequence being mutually
  comparable to form an ordering
  - This often breaks for sequences containing mixed type values

``` python
mixed_list = ["Free", 2, object()]

print(max(mixed_list))
```

    TypeError: '>' not supported between instances of 'int' and 'str'
    ---------------------------------------------------------------------------
    TypeError                                 Traceback (most recent call last)
    Cell In[10], line 3
          1 mixed_list = ["Free", 2, object()]
          2 
    ----> 3 print(max(mixed_list))

    TypeError: '>' not supported between instances of 'int' and 'str'

- There are also a number of common methods that sequence types must
  implement
  - Accessed via the `.` operator
- `count(value)` returns the number of occurences of `value` in the
  given sequence

``` python
name = "Ignatius"
print(name.count("a"))
```

    1

- To get the specific index associated with an item use the `index`
  method

``` python
name = "Ignatius"
print(name.index("a"))
```

    3

- `index` can be combined with a slice to slice up to that item

``` python
name = "Ignatius"
print(name[:name.index("a")])
```

    Ign

### Mathematical Operations

- Sequences define the `+` and `*` operator to mean concatenation
  - `+` can concatenate two sequences together *once*
  - `*` concatenates a sequence with itself $n$ times where $n$ is the
    supplied argument

``` python
a = [1]
b = [2]

# Concatenation with the + operator
print(a + b)

# Concatenation with the * operator
print(a * 5)
```

    [1, 2]
    [1, 1, 1, 1, 1]

- Concatenation operations can be useful for reshaping data or setting
  up sequences with default values

``` python
num_participants = 10
scores = [0] * num_participants
print(scores)
```

    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]

### Lists and Tuples

- Lists and tuples can hold any type
  - The contents can be of mixed types too
- Lists are created / denoted by delimiting `[]` whereas tuples use `()`
- Lists are *mutable*, meaning their contents can be changed
  - Items can be added, removed, swapped, replaced etc.
- Tuples are *immutable*
  - Once they are defined their contents are fixed
  - Attempting to change an *immutable* object will lead to runtime
    errors

> [!TIP]
>
> **Using the constructor vs delimiter**
>
> Why should you use `list` over `[]` or vice-versa? They have different
> use cases. `[]` lets you write out a list of an arbitrary length.
> `list` on the otherhand accepts either zero arguments where it returns
> an empty list, or an existing *iterable* which is converted to a
> `list`. The same happens for `tuple`. In general which to use should
> generally be obvious based on the situation. For example if trying to
> convert a string `"Hello"` to a list of it’s characters than we would
> use `list("Hello")`, `["Hello"]` creates a list holding the string
> `"Hello"`
>
> ``` python
> string = "Hello"
>
> # Using list as the constructor
> print(list(string))
>
> # Using the delimiter string
> print([string])
> ```
>
>     ['H', 'e', 'l', 'l', 'o']
>     ['Hello']

``` python
list_a = ["a", "b", "c"]
tuple_b = ("d", "e", "f")

# Mutating a list
list_a[0] = "-a"
print(list_a)

# Attempting to mutate a tuple
tuple_b[0] = "g"
print(tuple_b)
```

    ['-a', 'b', 'c']

    TypeError: 'tuple' object does not support item assignment
    ---------------------------------------------------------------------------
    TypeError                                 Traceback (most recent call last)
    Cell In[17], line 9
          5 list_a[0] = "-a"
          6 print(list_a)
          7 
          8 # Attempting to mutate a tuple
    ----> 9 tuple_b[0] = "g"
         10 print(tuple_b)

    TypeError: 'tuple' object does not support item assignment

#### Creating Lists and Tuples

- Lists are created a few ways,

  1.  The `list` constructor
  2.  Wrapping a comma-separated list in `[]`

``` python
list_a = list("a")
print(type(list_a))

list_b = ["d", "e", "f"]
print(type(list_b))
```

    <class 'list'>
    <class 'list'>

- Tuples work similar
  1.  The `tuple` constructor
  2.  Wrapping a comma-separated list in `()`
      - A tuple of length one requires a terminating comma
  3.  Any unwrapped comma-separated list

``` python
# using the tuple constructor
tuple_a = tuple("a")
print(type(tuple_a))

# Using parentheses
tuple_b = ("c", "d")
print(type(tuple_b))

# Incorrect usage of parentheses for a tuple of size 1
tuple_c = ("e")
print(type(tuple_c))

# Correct usage of parentheses for a tuple of size 1
tuple_d = ("f",)
print(type(tuple_d))

# Unwrapped comma-separated list
tuple_e = "g", "h"
print(type(tuple_e))
```

    <class 'tuple'>
    <class 'tuple'>
    <class 'str'>
    <class 'tuple'>
    <class 'tuple'>

#### Adding and Removing Items

- List’s can be extended or removed
  - `append` adds an item to the end of the list
    - This is the most efficient way to do so
  - `insert` can be used to add an item at a specific index
    - Every subsequent item is shuffled down one
    - This is more expensive than an `append` due to the shuffle
    - This difference is only really significant when doing a large
      number of `insert` operations and/or on big lists

``` python
flavours = ["Chocolate", "Strawberry"]
print(flavours)

# appending an item
flavours.append("Vanilla")
print(flavours)

# inserting an item
flavours.insert(1, "Caramel")
print(flavours)
```

    ['Chocolate', 'Strawberry']
    ['Chocolate', 'Strawberry', 'Vanilla']
    ['Chocolate', 'Caramel', 'Strawberry', 'Vanilla']

- The `pop` method removes an item
  - By default the last item is removed
  - Optionally accepts an index for the item to remove
- The item that is removed is returned by the `pop` method
  - Subsequent items will be shuffled down

``` python
flavour = ["Chocolate", "Caramel", "Strawberry", "Vanilla"]

# Default `pop` call
print(flavour.pop())

# `pop` with an index
print(flavour.pop(1))
```

    Vanilla
    Caramel

- To merge two lists there are two methods
  1.  Use the concatenation operator `+`
      - Returns a new list
  2.  Use the `extend` method
      - Modifies a list in place
      - Returns `None`

``` python
a = ["a", "b", "c"]
b = ["d", "e", "f"]
# Using the concatenation technique
print(f"a + b =", a + b)
print(f"a = {a}\nb = {b}")

# Using the `extend` method
print(f"a.extend(b)=", a.extend(b))
print(f"a = {a}\nb = {b}")
```

    a + b = ['a', 'b', 'c', 'd', 'e', 'f']
    a = ['a', 'b', 'c']
    b = ['d', 'e', 'f']
    a.extend(b)= None
    a = ['a', 'b', 'c', 'd', 'e', 'f']
    b = ['d', 'e', 'f']

> [!CAUTION]
>
> **Nested Lists**
>
> Consider the problem of trying to initialise a series of nested lists.
> You might be tempted to use list multiplication like,
>
> ``` python
> lists = [[]] * 4
> print(lists)
> ```
>
>     [[], [], [], []]
>
> This looks like we’ve created a list containing four sub-lists but if
> we try to modify one of the sub-lists we find,
>
> ``` python
> lists = [[]] * 4
> lists[-1].append(4)
> print(lists)
> ```
>
>     [[4], [4], [4], [4]]
>
> We can see that *all* of the sub-lists are modified! Why is this? This
> is because, using multiplication doesn’t repeat the *construction* of
> the list, but merely duplicates the *reference* to the list. Each
> sub-list is therefore a reference to the same underlying list object.
> This will happen with any *mutable* type in Python.
>
> A way around this is to use a technique called a list comprehension,
> (which tends to be the more natural way to generate lists)
>
> ``` python
> lists = [[] for _ in range(4)]
> lists[-1].append(4)
> print(lists)
> ```
>
>     [[], [], [], [4]]
>
> We’ll discuss list comprehensions in more detail later.

#### Unpacking

- You can assign multiple values from a sequence to variables using the
  *unpacking* syntax

``` python
a, b, c = (1, 3, 4)

print(f"a is {a}\nb is {b}\n c is {c}")
```

    a is 1
    b is 3
     c is 4

- The unpacking operator `*` can be combined with variables to assign
  *all* leftover values to a specific variable following the capture
  pattern
- For example, to capture all but the last two elements in `first`

``` python
*first, middle, last = ["horse", "carrot", "swan", "burrito", "fly"]
print(f"first is: {first}\nmiddle is: {middle}\nlast is: {last}")
```

    first is: ['horse', 'carrot', 'swan']
    middle is: burrito
    last is: fly

- If we want to capture the first and the last, and discard the rest

``` python
first, *middle, last = ["horse", "carrot", "swan", "burrito", "fly"]
print(f"first is {first}\nmiddle is {middle}\nlast is: {last}")
```

    first is horse
    middle is ['carrot', 'swan', 'burrito']
    last is: fly

#### Sorting Lists

- The ordering of lists can be changed via the `sort` and `reverse`
  methods
  - Like `min` and `max` they require values to be comparable
  - Liable to break for mixed-type lists

``` python
name = "Ignatius"
letters = list(name)
print("Unsorted:", letters)

letters.sort()
print("Sorted:", letters)

letters.reverse()
print("Reversed:", letters)
```

    Unsorted: ['I', 'g', 'n', 'a', 't', 'i', 'u', 's']
    Sorted: ['I', 'a', 'g', 'i', 'n', 's', 't', 'u']
    Reversed: ['u', 't', 's', 'n', 'i', 'g', 'a', 'I']

### Strings

- A string is a sequence of text characters
- Python strings are by definition Unicode
- As mentioned Python strings are delimited by `'` or `"`
  - Both are equivalent

``` python
str_a = 'Here is a string'
print(str_a)

print("Here is a string" == 'Here is a string')
```

    Here is a string
    True

- To include a quote in a string, delimit it with the other type of
  quote

``` python
# This works
print('Here "is" a string')

# This doesn't, since the double-quote delimiters results in the string being split
print("Here "is" a string")
```

    Here "is" a string
    False

    <>:5: SyntaxWarning: "is" with 'str' literal. Did you mean "=="?
    <>:5: SyntaxWarning: "is" with 'str' literal. Did you mean "=="?
    /tmp/ipykernel_2989/2287492867.py:5: SyntaxWarning: "is" with 'str' literal. Did you mean "=="?
      print("Here "is" a string")

- To write multiple line strings use a *triple-quoted* string
  - Either `'''` or `"""`

``` python
a_very_large_phrase = """
Wikipedia is hosted by the Wikimedia Foundation,
a non-profit organisation that also hosts a range of other projects
"""
print(a_very_large_phrase)
```


    Wikipedia is hosted by the Wikimedia Foundation,
    a non-profit organisation that also hosts a range of other projects

- Some special characters are represented using an *escape sequence*
  - This include tabs and newlines for examples
  - These are denoted by a backslash `\` followed by a letter for the
    given character, e.g. `\t` is for tab
- When representing a windows path `\` is also the path separator
  - Means we need to use a double `\\` to escape the `\` to make it a
    literal backslash

``` python
# broken path
windows_path = "c:\row\the\boat\now"
print(f"Broken path:\n{windows_path}")

# fixed path
fixed_windows_path = "c:\\row\\the\\boat\\now"
print(f"Fixed path:\n{fixed_windows_path}")
```

    Broken path:
    c:
    ow  heoat
    ow
    Fixed path:
    c:\row\the\boat\now

- The alternative is to use a *raw* string
  - All characters are then interpreted literally
  - Done by prepending the string delimiters with `r`

``` python
raw_path = r"c:\row\the\boat\now"
print(raw_path)
```

    c:\row\the\boat\now

- `str` contains a number of helper methods for converting between
  typography forms

``` python
captain = "Patrick Taylor"

print("Default:", captain)
print("Capitalised:", captain.capitalize())
print("Lowercase:", captain.lower())
print("Uppercase:", captain.upper())
print("Swapped case:", captain.swapcase())
print("Title case:", captain.title())
```

    Default: Patrick Taylor
    Capitalised: Patrick taylor
    Lowercase: patrick taylor
    Uppercase: PATRICK TAYLOR
    Swapped case: pATRICK tAYLOR
    Title case: Patrick Taylor

- Formatted strings or *f-strings* were introduced in Python 3.6 to
  simply the process of injecting variable values into a string
  - Prepended by `f`
  - Variables are injected by the syntax `{expression}` within a string
    - `{}` denotes a replacement field
    - `expression` is an expression to be evaluated for the value to
      display
    - These fields can also have additional *format specifiers* which
      control how the value is displayed

``` python
strings_count = 5
frets_count = 24
formatted_str = f"Noam Pikelny's banjo has {strings_count} strings and {frets_count} frets"
print(formatted_str)
```

    Noam Pikelny's banjo has 5 strings and 24 frets

- As mentioned, the injected value can be an expression rather than a
  variable

``` python
a = 12
b = 32
result_str = f"{a} times {b} equals {a * b}"
print(result_str)
```

    12 times 32 equals 384

- One can even inject elements for example

``` python
players = ["Tony Trischka", "Bill Evans", "Alan Munde"]
formatted_str = f"Performances will be held by {players[1]}, {players[0]}, and {players[2]}"
print(formatted_str)
```

    Performances will be held by Bill Evans, Tony Trischka, and Alan Munde

### Ranges

- Ranges are an efficient way to represent an ordered sequence of values
- Useful for specifying loop bounds
- Like [slices](#slicing) ranges take a `start`, `stop` and `step`
  parameter
  - `start` is optional, by default $0$
    - `start` is included in the range
  - `stop` is required
    - `stop` is excluded from the range
  - `step` is optional
    - $1$ by default
    - Can use negative step values to count down

``` python
# Range with optional start excluded
r_1 = range(10) # interval [0, 10)
print(r_1) # Range object
print("list(range(10)):", list(r_1)) # Converted to a list

# Range with optional start included
r_2 = range(1, 10) # interval [1, 10)
print("list(range(1, 10)):", list(r_2)) # Converted to a list

# Range with optional step included
r_3 = range(0, 10, 2)
print("list(range(0, 10, 2))", list(r_3))

# Range with negative step
r_4 = range(10, 0, -2)
print("list(range(10 ,0, -2))", list(r_4))
```

    range(0, 10)
    list(range(10)): [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    list(range(1, 10)): [1, 2, 3, 4, 5, 6, 7, 8, 9]
    list(range(0, 10, 2)) [0, 2, 4, 6, 8]
    list(range(10 ,0, -2)) [10, 8, 6, 4, 2]

- `range` objects generate and return their numbers one at a time
  - Means there is a reduced memory footprint over a list for large
    ranges

## Summary

- A sequence type in python represents an ordered, finite collection of
  values
- Concrete implementations of sequences include `list`, `tuple`, `str`
  and `range`
  - List and Tuple are arbitrary length, mixed value collections
    - Lists are *mutable* meaning their items can be added or removed
    - Tuples are *immutable* meaning their contents are fixed once
      created
  - Strings are sequences representing unicode text characters
  - Ranges are a memory efficient description of a range of integer
    sequences
- Sequence types are very commonly used in python

## Questions

1.  How would you test whether `a` is in the list `my_list`

    - We use the `in` operator, as `a in my_list`
    - This returns `True` if `a` is contained in `my_list`

2.  How would you find out how many times `b` appears in a string named
    `my_string`?

    - We can use the `count` method on the string type
      e.g. `my_string.count("b")`

3.  How would you add `a` to the end of the list `my_list`

    - We can use the `append` method on a list,
      e.g. `my_list.append("a")`

4.  Are the strings `'superior'` and `"superior"` equal?

    - Yes, Python strings can be delimited by `'` or `"` but the string
      is considered the same regardless

5.  How would you make a range going from $3$ to $13$?

    - We would write `range(3, 14)`
    - This is because ranges are *inclusive* in the `start` argument,
      and *exclusive* in the `end` argument

- We can see the results below,

``` python
# Question 1
a = "Hello,"
my_list = list("World!")

print(" a is in my_list", a in my_list)

# Question 2
my_string = "bee"
print(f"b appears {my_string.count("b")} times in my_string")

# Question 3
my_list.append(a)
print("my_list after calling my_list.append(a) is:", my_list)

# Question 4
print("The strings  'superior' and \"superior\" are equal:", 'superior' == "superior")

# Question 5
print("The numbers generated by range(3, 14) are:", list(range(3, 14)))
```

     a is in my_list False
    b appears 1 times in my_string
    my_list after calling my_list.append(a) is: ['W', 'o', 'r', 'l', 'd', '!', 'Hello,']
    The strings  'superior' and "superior" are equal: True
    The numbers generated by range(3, 14) are: [3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13]
