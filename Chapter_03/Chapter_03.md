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
  1. `list`
  2. `tuple`
  3. `string`
  4. `bytes`
  5. ranges

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

  1. The `list` constructor
  2. Wrapping a comma-separated list in `[]`

``` python
list_a = list("a")
print(type(list_a))

list_b = list("d", "e", "f")
print(type(list_b))
```

    <class 'list'>

    TypeError: list expected at most 1 argument, got 3
    ---------------------------------------------------------------------------
    TypeError                                 Traceback (most recent call last)
    Cell In[18], line 4
          1 list_a = list("a")
          2 print(type(list_a))
          3
    ----> 4 list_b = list("d", "e", "f")
          5 print(type(list_b))

    TypeError: list expected at most 1 argument, got 3

- Tuples work similar
  1. The `tuple` constructor
  2. Wrapping a comma-separated list in `()`
      - A tuple of length one requires a terminating comma
  3. Any unwrapped comma-separated list

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
tuple_d = ("f")
print(type(tuple_d))

# Unwrapped comma-separated list

tuple_e = "g", "h"
type(tuple_e)
```

    <class 'tuple'>
    <class 'tuple'>
    <class 'str'>
    <class 'str'>

    tuple

-

## Summary

## Questions
