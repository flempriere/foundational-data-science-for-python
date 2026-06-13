# Chapter 4: Other Data Structures


- [Notes](#notes)
  - [Dictionaries](#dictionaries)
    - [Creating Dictionaries](#creating-dictionaries)
    - [Accessing, Adding and Updating
      Keys](#accessing-adding-and-updating-keys)
    - [Removing Items from a
      Dictionary](#removing-items-from-a-dictionary)
    - [Dictionary Views](#dictionary-views)
    - [Checking to See if a Dictionary has a
      Key](#checking-to-see-if-a-dictionary-has-a-key)
      - [The `get` Method](#the-get-method)
    - [Valid Key Types](#valid-key-types)
      - [The `hash` Method](#the-hash-method)
  - [Sets](#sets)
    - [Set Operations](#set-operations)
      - [Disjoint](#disjoint)
      - [Subset](#subset)
      - [Proper Subsets](#proper-subsets)
      - [Supersets and Proper
        Supersets](#supersets-and-proper-supersets)
    - [Union](#union)
    - [Intersection](#intersection)
    - [Difference](#difference)
      - [Symmetric Difference](#symmetric-difference)
      - [Updating Sets](#updating-sets)
    - [Frozensets](#frozensets)
- [Summary](#summary)
- [Questions](#questions)

## Notes

- Python contains a number of other data structures that are not
  [sequence types](../Chapter_03/Chapter_03.qmd)
- Dictionaries and sets are two common alternative data structures in
  Python

### Dictionaries

- Dictionaries store key/value pairs
  - You can think of this as like a list, but rather than being indexed
    by their position in the list we use *key* value
- Using key-based look-ups makes dictionaries very quick at testing for
  if it contains a specific key
  - Compared to lists which can be slow to look for a specific value
    since they might need to iterate over all the elements
- For example we might use a dictionary to associate a student, their
  height and a GPA
- Dictionaries are declared using the
  `{key_1 :value_1, key_2 : value_2, ...}` syntax
  - i.e. a `{}` delimited, comma-separated list of `key:value` pairs
    - They key and the value are separated by a `:`

``` python
person = {"name": "Betty", "height": 62, "gpa": 3.6}
print(person)
```

    {'name': 'Betty', 'height': 62, 'gpa': 3.6}

- The above dictionary contains
  1.  The keys `"name"`, `"height"` and `"gpa"`,
  2.  The values `"Betty"`, `62` and `3.6`
- Dictionary values can be of *any* type
- Dictionary keys have some restrictions
  - Fundamental types like `int` and `str` will work

#### Creating Dictionaries

- As seen dictionaries can be created with the a brace-delimited comma
  separated list
- To create an empty dictionary, simply pass an empty list like `{}`

``` python
dictionary = {}
print(f"dictionary is of type {type(dictionary)} and has value {dictionary}")
```

    dictionary is of type <class 'dict'> and has value {}

- The alternative is to call the `dict` method
  - An empty call to `dict` also creates an empty list
  - Optionally can pass a comma-separated list of *keyword* arguments
  - Or, optionally accept key, value pairs as a sequence of sequences

``` python
# empty `dict` call
dictionary = dict()
print(f"Dictionary created by the call dict(): {dictionary}")

# Using `dict` to create a non-empty `dict` via kwargs
subject_1 = dict(name="Paula", height=64, gpa=3.8, ranking=1)
print("Subject 1:", subject_1)

# Using `dict` to create non-empty dict via nested sequences
subject_2 = dict([["name", "Paula"], ["height", 64], ["gpa", 3.8], ["ranking", 1]])
print("Subject 2:", subject_2)
```

    Dictionary created by the call dict(): {}
    Subject 1: {'name': 'Paula', 'height': 64, 'gpa': 3.8, 'ranking': 1}
    Subject 2: {'name': 'Paula', 'height': 64, 'gpa': 3.8, 'ranking': 1}

#### Accessing, Adding and Updating Keys

- Dictionary keys act like indices in a list
  - Can be used to access their associated value

``` python
student = {"name": "Paula", "height": 64, "gpa": 3.8}

# key access
print(student["name"])
print(student["height"])
print(student["gpa"])
```

    Paula
    64
    3.8

- Assigning to a non-existent key will create the key
- If the key does exist then the old value is overwritten

``` python
student = {"name": "Paula", "height": 64, "gpa": 3.8}

# defining a new key
student["applied"] = "2019-10-31"
print(student)

# overwriting a key
student["gpa"] = 3.0
print(student["gpa"])

# overwriting a key using the inplace addition operator
student["gpa"] += 1.0
print(student["gpa"])
```

    {'name': 'Paula', 'height': 64, 'gpa': 3.8, 'applied': '2019-10-31'}
    3.0
    4.0

#### Removing Items from a Dictionary

- Sometimes dictionary values need to be removed or masked out
  - For example removing personally identifiable information in a study
- One option is to overwrite the value with something meaningless like
  `None`

``` python
student = {"name": "Paula", "height": 64, "gpa": 3.8}

# Removing name
student["name"] = None
print(student)
```

    {'name': None, 'height': 64, 'gpa': 3.8}

- The `key:value` pair can be removed directly by the `del` operator

``` python
student = {"name": "Paula", "height": 64, "gpa": 3.8}

# Deleting the key
del student["name"]
print(student)

# Trying to access the removed key
print(student["name"])
```

    {'height': 64, 'gpa': 3.8}

    KeyError: 'name'
    ---------------------------------------------------------------------------
    KeyError                                  Traceback (most recent call last)
    Cell In[7], line 8
          4 del student["name"]
          5 print(student)
          6 
          7 # Trying to access the removed key
    ----> 8 print(student["name"])

    KeyError: 'name'

- The last technique is to use a variation of the `pop` method
  - `pop` returns the value associated with a key
    - Optionally accepts a `default` value argument to return if the key
      is missing
  - `popitem` returns the most recent `key:value` pair

``` python
student = {"name": "Paula", "height": 64, "gpa": 3.8}

# Using `popitem`
print("popitem returned:", student.popitem())

# Using `pop`
print("pop('name') returned:", student.pop("name"))

# Using pop with a default value to handle missing key
print("pop('id', default=0):", student.pop("id", None))
```

    popitem returned: ('gpa', 3.8)
    pop('name') returned: Paula
    pop('id', default=0): None

#### Dictionary Views

- *dictionary views* are special objects that let us look at a
  dictionary contents through different perspectives

- There are three views

  1.  `dict_keys`
      - Obtained by the `keys` dictionary method
      - Contains the dictionary’s current keys
  2.  `dict_values`
      - Obtained by the `values` dictionary method
      - Contains the dictionary’s current values
  3.  `dict_items`
      - Obtained by the `items` dictionary method
      - Contains the `key:value` mappings as tuples

``` python
student = {"name": "Paula", "height": 64, "gpa": 3.8}

# Keys view
keys = student.keys()
print("Keys:", keys)

# Values view
values = student.values()
print("Values:", values)

# Items
items = student.items()
print("Items:", items)
```

    Keys: dict_keys(['name', 'height', 'gpa'])
    Values: dict_values(['Paula', 64, 3.8])
    Items: dict_items([('name', 'Paula'), ('height', 64), ('gpa', 3.8)])

- You can test a view for membership with the `in`

``` python
student = {"name": "Paula", "height": 64, "gpa": 3.8}

# Keys view
keys = student.keys()
print("'ranking' in keys:", "ranking" in keys)

# Values view
values = student.values()
print("64 in values:", 64 in values)

# Items
items = student.items()
print('("height", 64) in items:', ("height", 64) in items)
```

    'ranking' in keys: False
    64 in values: True
    ("height", 64) in items: True

- If working in `Python 3.8+` then views are dynamic
  - If dictionary is updated so is the corresponding view

``` python
student = {"name": "Paula", "height": 64, "gpa": 3.8}

# Keys view
keys = student.keys()
print("keys:", keys)

student["id"] = 0
print("keys after updating dict:", keys)
```

    keys: dict_keys(['name', 'height', 'gpa'])
    keys after updating dict: dict_keys(['name', 'height', 'gpa', 'id'])

- View’s also support the `len` built-in function

``` python
student = {"name": "Paula", "height": 64, "gpa": 3.8}

# Keys view
keys = student.keys()
print("len(keys):", len(keys))
```

    len(keys): 3

- Since `Python 3.8` they can be reversed via the `reversed` built-in

``` python
student = {"name": "Paula", "height": 64, "gpa": 3.8}

# Keys view
keys = student.keys()
print("reversed(keys):", list(reversed(keys)))
```

    reversed(keys): ['gpa', 'height', 'name']

- `dict_key` objects are *set-like*
  - They support set operations

``` python
admission_record = {
    "first": "Julia",
    "last": "Brown",
    "id": "ax012E4",
    "admitted": "2020-03-14",
}

student_record = {
    "first": "Julia",
    "last": "Brown",
    "id": "ax012E4",
    "gpa": 3.8,
    "major": "Data Science",
    "minor": "Mathematics",
    "advisor": "Pickerson",
}

# Testing equality of keys
print("Testing key equality:", admission_record.keys() == student_record.keys())

# Symmetric Difference
print("Symmetric Difference:", admission_record.keys() ^ student_record.keys())

# Intersection
print("Intersection:", admission_record.keys() & student_record.keys())

# Difference
print("Difference:", admission_record.keys() - student_record.keys())

# Union
print("Union:", admission_record.keys() | student_record.keys())
```

    Testing key equality: False
    Symmetric Difference: {'admitted', 'advisor', 'gpa', 'major', 'minor'}
    Intersection: {'first', 'last', 'id'}
    Difference: {'admitted'}
    Union: {'first', 'admitted', 'advisor', 'last', 'gpa', 'major', 'minor', 'id'}

- `dict_items` views are useful for iterating over the `key:value` pairs
  in dictionary
  - We can use `dict_keys` and `dict_values` if we only want to iterate
    over keys or values

``` python
student = {"name": "Paula", "height": 64, "gpa": 3.8}

for k, v in student.items():
    print(f"{k} => {v}")
```

    name => Paula
    height => 64
    gpa => 3.8

#### Checking to See if a Dictionary has a Key

- `dict_keys` views can be combined with `in` to check for a key
  - This is implicitly done by just using the `in` operator on the
    dictionary directly

``` python
student = {"name": "Paula", "height": 64, "gpa": 3.8}

keys = student.keys()

print("Using `in` on a `dict_keys` object:", "name" in keys)
print("Using `in` on a dictionary directly:", "name" in student)
```

    Using `in` on a `dict_keys` object: True
    Using `in` on a dictionary directly: True

- Another technique is to try and access a key directly
  - A `KeyError` is thrown if the key doesn’t exist
  - Can use a `try/except` block to handle this

``` python
student = {"name": "Paula", "height": 64, "gpa": 3.8}

try:
    print(student["id"])
except KeyError:
    print("ID not found")
```

    ID not found

##### The `get` Method

- `get` is a dictionary method
- Is used to retrieve a dictionary value associated with a key
  - Returns a `default` value if a key is missing
  - If not supplied the `default` value is `None`

``` python
student = {"name": "Paula", "height": 64, "gpa": 3.8}

# Existing key
print("name:", student.get("name"))

# Unspecified default
print("ID:", student.get("id"))

# Specified default
print("major:", student.get("major", "Undeclared"))
```

    name: Paula
    ID: None
    major: Undeclared

#### Valid Key Types

- Dictionary keys can only be *immutable*
  - These are types that cannot be modified
  - This includes `int`, `str`, `range`, `byte` and `tuple`

> [!CAUTION]
>
> **Tuple Immutability**
>
> Tuples are regarded as immutable their contents cannot be changed,
> i.e. we can’t redefine an index. However, if a tuple contains
> *mutable* types, those objects can still be mutated by their
> reference. For example,
>
> ``` python
> immutable_tuple = ("a", ["b", "c"])
>
> print("tuple before mutation:", immutable_tuple)
>
> # mutating the list inside the tuple
> immutable_tuple[1].append("d")
> print("tuple after mutation:", immutable_tuple)
> ```
>
>     tuple before mutation: ('a', ['b', 'c'])
>     tuple after mutation: ('a', ['b', 'c', 'd'])
>
> This means that in these cases the overall object is *not immutable*.
> This means that such objects cannot be used for example as dictionary
> keys
>
> ``` python
> dictionary = {("a", ["b", "c"]): 1}
> print(dictionary)
> ```
>
>     TypeError: cannot use 'tuple' as a dict key (unhashable type: 'list')
>     ---------------------------------------------------------------------------
>     TypeError                                 Traceback (most recent call last)
>     Cell In[20], line 1
>     ----> 1 dictionary = {("a", ["b", "c"]): 1}
>           2 print(dictionary)
>
>     TypeError: cannot use 'tuple' as a dict key (unhashable type: 'list')

``` python
dictionary = {1: 1, "a": 2, 2.3: 3, ("b", "c"): 4, b"bytes": 5, range(3): 6}
print("A dict showing a range of key types:", dictionary)

# Invalid key type
print("Trying to use a list as a key")
list_keyed_dictionary = {["a", "b"]: 1}
print("A dict using a list as a key:", list_keyed_dict)
```

    A dict showing a range of key types: {1: 1, 'a': 2, 2.3: 3, ('b', 'c'): 4, b'bytes': 5, range(0, 3): 6}
    Trying to use a list as a key

    TypeError: cannot use 'list' as a dict key (unhashable type: 'list')
    ---------------------------------------------------------------------------
    TypeError                                 Traceback (most recent call last)
    Cell In[21], line 6
          2 print("A dict showing a range of key types:", dictionary)
          3 
          4 # Invalid key type
          5 print("Trying to use a list as a key")
    ----> 6 list_keyed_dictionary = {["a", "b"]: 1}
          7 print("A dict using a list as a key:", list_keyed_dict)

    TypeError: cannot use 'list' as a dict key (unhashable type: 'list')

##### The `hash` Method

- Technically, the definition of a valid dictionary key is something
  that implements the `__hash__` dunder method
  - `__hash__` converts the object to a number to act as an index via a
    *hash function*
  - It can only be implemented on *immutable* objects because it must be
    *stable* for a given object reference

``` python
print("Calling __hash__ on string:", "abc".__hash__())

print("Calling __hash__ on a list:", list("abc").__hash__())
```

    Calling __hash__ on string: 6462868499547590877

    TypeError: 'NoneType' object is not callable
    ---------------------------------------------------------------------------
    TypeError                                 Traceback (most recent call last)
    Cell In[22], line 3
          1 print("Calling __hash__ on string:", "abc".__hash__())
          2 
    ----> 3 print("Calling __hash__ on a list:", list("abc").__hash__())

    TypeError: 'NoneType' object is not callable

> [!TIP]
>
> **Dictionary Ordering**
>
> Since Python 3.7 for CPython (and made mandatory for Python 3.8+),
> dictionaries store their keys in *insertion order*. This is useful for
> creating reproducible tests.

### Sets

- A `set` works like the mathematical object
- It is an unordered collection of unique items
  - Use case tends to be more niche than `list`, `dict`, or `tuple` but
    it is powerful
- A `set` can only contain hashable values ([See
  above](#the-hash-method))
- A set is constructed either via
  1.  The `set` constructor call
      - Converts an existing sequence to a set
  2.  A comma-separated list delimited by `{}`
      - Note, the lack of a `:` separator is used to distinguish between
        a dictionary construction and a set construction

``` python
empty_set_a = set()
empty_set_b = {}
print("Empty set via `set()`:", empty_set_a)
print("Empty set via {}:", empty_set_b)

set_a = set("abc")
print("Set constructed via `set` called on a string:", set_a)

tuple_b = "a", "a", "a", "b", "c"
set_b = set(tuple_b)
print("Set constructed via `set` called on a tuple:", set_b)

set_c = {1, 1, 1, 2, 2, 3}
print("Set constructed via {1, 1, 1, 2, 2, 3}:", set_c)
```

    Empty set via `set()`: set()
    Empty set via {}: {}
    Set constructed via `set` called on a string: {'b', 'c', 'a'}
    Set constructed via `set` called on a tuple: {'b', 'c', 'a'}
    Set constructed via {1, 1, 1, 2, 2, 3}: {1, 2, 3}

- As mentioned, sets do not work with mutable types

``` python
broken_set = {["a", "b"], "c"}
print(broken_set)
```

    TypeError: cannot use 'list' as a set element (unhashable type: 'list')
    ---------------------------------------------------------------------------
    TypeError                                 Traceback (most recent call last)
    Cell In[24], line 1
    ----> 1 broken_set = {["a", "b"], "c"}
          2 print(broken_set)

    TypeError: cannot use 'list' as a set element (unhashable type: 'list')

- `set` supports most of the standard sequence operations
- Can test for membership with `in` or `not in`

``` python
unique_numbers = {1, 1, 1, 2, 2, 3}

print("3 in unique_numbers:", 3 in unique_numbers)
print("3 not in unique_numbers:", 3 not in unique_numbers)
```

    3 in unique_numbers: True
    3 not in unique_numbers: False

- `len` returns the cardinality of a set

``` python
unique_numbers = {1, 1, 1, 2, 2, 3}

print("len(unique_numbers):", len(unique_numbers))
```

    len(unique_numbers): 3

- `set` contents can be modified
  - The `add` method adds an item to a set
  - `pop` removes an arbitrary item
    - Raises a `KeyError` if the set is empty
  - `remove` removes a specified element
    - Raises a `KeyError` if the element is not in the set
  - `discard` removes a specified element
    - Fails silently if the element does not exist
  - `clear` removes the entire contents of a set

``` python
unique_numbers = {1, 1, 1, 2, 2, 3}

# Adding an item
unique_numbers.add(4)
print("Adding a item:", unique_numbers)

# Removing an item
unique_numbers.remove(1)
print("Removed 1 using `remove`:", unique_numbers)

# Discarding an item
unique_numbers.discard(2)
print("Discarded 2 using `discard`:", unique_numbers)

# Popping an arbitrary item
popped = unique_numbers.pop()
print("Popped:", popped)
print("set is now:", unique_numbers)

# Discarding a non-existent item
unique_numbers.discard(5)
print("Attempting to discard non-existent 5 using `discard`:", unique_numbers)

# Attempting to remove a non-existent item
print("Attempting to remove non-existent 5 using `remove`")
unique_numbers.remove(5)
```

    Adding a item: {1, 2, 3, 4}
    Removed 1 using `remove`: {2, 3, 4}
    Discarded 2 using `discard`: {3, 4}
    Popped: 3
    set is now: {4}
    Attempting to discard non-existent 5 using `discard`: {4}
    Attempting to remove non-existent 5 using `remove`

    KeyError: 5
    ---------------------------------------------------------------------------
    KeyError                                  Traceback (most recent call last)
    Cell In[27], line 26
         22 print("Attempting to discard non-existent 5 using `discard`:", unique_numbers)
         23 
         24 # Attempting to remove a non-existent item
         25 print("Attempting to remove non-existent 5 using `remove`")
    ---> 26 unique_numbers.remove(5)

    KeyError: 5

#### Set Operations

- Python has native support for standard mathematical set operations
  - Come in both method call and operator variants
  - Methods can be used between a `set` and any other `iterable`
  - Operators only work between `set` and `frozenset` objects

##### Disjoint

- Disjoint set’s share no common elements
- Can be verified via the `isdisjoint` method
  - Returns `True` if two sets are disjoint, else `False`

``` python
one_to_three = {1, 2, 3}
four_to_six = {4, 5, 6}
even = {2, 4, 6}

print("one_to_three is disjoint from four_to_six:", one_to_three.isdisjoint(four_to_six))
print("one_to_three is disjoint from even:", one_to_three.isdisjoint(even))
```

    one_to_three is disjoint from four_to_six: True
    one_to_three is disjoint from even: False

##### Subset

- A set $A$ is a subset of another subset $B$ if all the elements
  contained in $A$ are also contained in $B$
  - Can test for this via the `issubset` method
  - Alternatively can use the `<=` operator

``` python
one_to_three = {1, 2, 3}
one_to_six = {1, 2, 3, 4, 5, 6}
evens = {2, 4, 6}

print("one_to_three is a subset of one_to_six:", one_to_three.issubset(one_to_six))
print("one_to_three is a subset of evens:", one_to_three.issubset(evens))

print("Repeating with the <= operator")
print(one_to_three <= one_to_six)
print(one_to_three <= evens)
```

    one_to_three is a subset of one_to_six: True
    one_to_three is a subset of evens: False
    Repeating with the <= operator
    True
    False

- Remember that the method approach will work with non-set objects
  - Operator approach will not

``` python
one_to_three = {1, 2, 3}

print("one_to_three is a subset of range(6):", one_to_three.issubset(range(6)))
print("one_to_three <= range(6):", one_to_three <= range(6))
```

    one_to_three is a subset of range(6): True

    TypeError: '<=' not supported between instances of 'set' and 'range'
    ---------------------------------------------------------------------------
    TypeError                                 Traceback (most recent call last)
    Cell In[30], line 4
          1 one_to_three = {1, 2, 3}
          2 
          3 print("one_to_three is a subset of range(6):", one_to_three.issubset(range(6)))
    ----> 4 print("one_to_three <= range(6):", one_to_three <= range(6))

    TypeError: '<=' not supported between instances of 'set' and 'range'

##### Proper Subsets

- Set $A$ is a proper subset of another set $B$ if $A$ is a subset of
  $B$ and $B$ contains an element not contained in $A$
  - Equivalent to saying $A$ is a subset of $B$ but not equal to $B$
- The `<` operator can be used to test for proper subsets between sets

``` python
one_to_three = {1, 2, 3}
one_to_six = {1, 2, 3, 4, 5, 6}

print("one_to_three is a proper subset of one_to_six:", one_to_three < one_to_six)
print("one_to_three is a proper subset of one_to_three:", one_to_three < one_to_three)
```

    one_to_three is a proper subset of one_to_six: True
    one_to_three is a proper subset of one_to_three: False

##### Supersets and Proper Supersets

- A *superset* is a reverse of a *subset*
- $A$ is a superset of $B$ if $A$ contains every element in $B$
  - If $A$ is not equal to $B$, i.e. contains at least one element not
    also in $B$ it is a proper superset
- We can test for supersets with the `issuperset` method or the `>=`
  operator
  - To test for a proper superset we use the `>` operator

``` python
one_to_three = {1, 2, 3}
one_to_six = {1, 2, 3, 4, 5, 6}
evens = {2, 4, 6}

print("one_to_six is a superset of one_to_three:", one_to_six.issuperset(one_to_three))
print("evens is a superset of one_to_three:", evens.issuperset(one_to_three))

print("Repeating with the >= operator")
print(one_to_six >= one_to_three)
print(evens >= one_to_three)

print("one_to_six is a proper superset of one_to_three:", one_to_six > one_to_three)
print("one_to_six is a proper superset of one_to_six:", one_to_six > one_to_six)
```

    one_to_six is a superset of one_to_three: True
    evens is a superset of one_to_three: False
    Repeating with the >= operator
    True
    False
    one_to_six is a proper superset of one_to_three: True
    one_to_six is a proper superset of one_to_six: False

#### Union

- The union of set $A$ and set $B$ is the set containing all elements of
  $A$ *or* $B$
- Can be performed via the `union` method or the `|` operator

``` python
evens = {2, 4, 6}
odds = {1, 3, 5}

print("Using the `union` method:")
one_to_six = odds.union(evens)
print(one_to_six)

print("Using the `|` operator:")
print(evens | odds)
```

    Using the `union` method:
    {1, 2, 3, 4, 5, 6}
    Using the `|` operator:
    {1, 2, 3, 4, 5, 6}

#### Intersection

- The intersection of sets $A$ and $B$ is the set containing all
  elements in *both* $A$ and $B$
- Performed via the `intersection` method or the `&` operator

``` python
one_to_three = {1, 2, 3}
evens = {2, 4, 6}

print("Using the `intersection` method:")
two = one_to_three.intersection(evens)
print(two)

print("Using the `&` operator:")
print(one_to_three & evens)
```

    Using the `intersection` method:
    {2}
    Using the `&` operator:
    {2}

#### Difference

- The difference $A - B$ of sets $A$ and $B$ is the set of all elements
  in $A$ that are not contained in $B$
- Performed via the `difference` method or the `-` operator

``` python
one_to_three = {1, 2, 3}
evens = {2, 4, 6}

print("Using the `difference` method to calculate one_to_three - evens:", one_to_three.difference(evens))
print("Using the `difference` method to calculate evens - one_to_three:", evens.difference(one_to_three))

print("Repeated using the `-` operator:")
print(one_to_three - evens)
print(evens - one_to_three)
```

    Using the `difference` method to calculate one_to_three - evens: {1, 3}
    Using the `difference` method to calculate evens - one_to_three: {4, 6}
    Repeated using the `-` operator:
    {1, 3}
    {4, 6}

##### Symmetric Difference

- The symmetric difference of sets $A$ and $B$ is the set of elements
  contained in exactly one of either $A$ or $B$
  - i.e. it is the opposite of the intersection
- Can be calculated via the `symmetric_difference` method or the `^`
  operator

``` python
one_to_three = {1, 2, 3}
evens = {2, 4, 6}

print("Symmetric difference of one_to_three and evens via method call:", one_to_three.symmetric_difference(evens))
print("Symmetric difference via `^` operator:", one_to_three ^ evens)
```

    Symmetric difference of one_to_three and evens via method call: {1, 3, 4, 6}
    Symmetric difference via `^` operator: {1, 3, 4, 6}

##### Updating Sets

- We’ve already seen that set’s [can be modified](#sets) after creation
- A powerful technique is in-place modifying a set with another
  *iterable*
  - This can be done via the `update` method
    - Equivalent to a `union_update`
    - For a set can be equivalently done with the `|=` operator

``` python
unique_numbers = {1, 2, 3}
print("Set before update:", unique_numbers)

unique_numbers.update(range(6))
print("Set after `update`:", unique_numbers)

extra_numbers = {5, 6, 7, 8, 9}
unique_numbers |= extra_numbers
print("Set after |=:", unique_numbers)
```

    Set before update: {1, 2, 3}
    Set after `update`: {0, 1, 2, 3, 4, 5}
    Set after |=: {0, 1, 2, 3, 4, 5, 6, 7, 8, 9}

- Set’s can also be updated in-place based on the set operations
  discussed above
  - `intersection_update` in-place updates the set to the intersection
    with the provided set (or iterable)
    - Comes in operator form `&=`
  - `difference_update` in-place updates the set to the difference with
    the provided set (or iterable)
    - Comes in operator form `-=`
  - `symmetric_difference_update` in-place updates the set to the
    symmetric difference with the provided set (or iterable)
    - Comes in operator form `^=`

``` python
unique_numbers = {1, 2, 3}
evens = {0, 2, 4, 6}

# Update using intersection
r0 = range(1, 8, 2)
unique_numbers.intersection_update(r0)
print("unique numbers after intersection update:", unique_numbers)

unique_numbers = {1, 2, 3}
unique_numbers &= evens
print("unique numbers after &= update:", unique_numbers)

# Update using the difference
r = range(3, 6)
unique_numbers.difference_update(r)
print("unique numbers after difference update:", unique_numbers)

# Repeating but with an operator
unique_numbers = {1, 2, 3}
unique_numbers -= evens
print("Unique numbers after |= update:", unique_numbers)

# Update using the symmetric difference
unique_numbers = {1, 2, 3}
r2 = range(2, 8, 2)
unique_numbers.symmetric_difference_update(r2)
print("Unique numbers after symmetric difference update:", unique_numbers)

# Repeating but with an operator
unique_numbers = {1, 2, 3}
unique_numbers ^= evens
print("Unique numbers after ^= update:", unique_numbers)
```

    unique numbers after intersection update: {1, 3}
    unique numbers after &= update: {2}
    unique numbers after difference update: {2}
    Unique numbers after |= update: {1, 3}
    Unique numbers after symmetric difference update: {1, 3, 4, 6}
    Unique numbers after ^= update: {0, 1, 3, 4, 6}

#### Frozensets

- The python `set` as seen is *mutable*
  - Mean’s it cannot be used as a dictionary key
  - Also can’t construct sets of sets
- The `frozenset` is an *immutable* set-like object (i.e implements most
  of the same behaviours)
  - Is pretty much a drop-in replacement for `set`

``` python
frozen = frozenset(range(10))

print(frozen)

print("Testing proper subset with a frozenset")
print(frozen < set(range(21)))

print("Taking the intersection with a frozenset")
print(frozen & set(range(5, 15)))

print("Taking the symmetric difference with a frozenset")
print(frozen ^ set(range(5, 15)))

print("Taking the union with a frozenset")
print(frozen | set(range(5, 15)))
```

    frozenset({0, 1, 2, 3, 4, 5, 6, 7, 8, 9})
    Testing proper subset with a frozenset
    True
    Taking the intersection with a frozenset
    frozenset({5, 6, 7, 8, 9})
    Taking the symmetric difference with a frozenset
    frozenset({0, 1, 2, 3, 4, 10, 11, 12, 13, 14})
    Taking the union with a frozenset
    frozenset({0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14})

## Summary

- Python provides specialised data structures beyond the sequence types
- A common one is the `dict` or dictionary
  - Provides a mutable `key:value` mapping
- `set` implements mathematical sets and their associated operations
  - `frozenset` is a specialised *immutable* implementation
- Both provide highly efficient structures for unordered data

## Questions

1.  What are three ways to create a dictionary with the following key,
    value pairs?

    ``` python
     {"name": "Smuah", "height": 62}
    ```

    1.  `kwargs` to the `dict` constructor
        `dict(name="Smuah", height=62)`
    2.  A list of key-value pairs to the `dict` constructor
        `dict([("name", "Smuah"), ("height", 62)])`
    3.  A `{}` delimited comma-separated list of `key:value` pairs
        `{"name": "Smuah", "height": 62}`

2.  How would you update the value associated with the key `"gpa"` in
    the dictionary `student` to be `4.0`?

    - We would write `student["gpa"] = 4.0`

3.  Given the dictionary `data` how would you safely access the value
    for the key `settings` if that key might be missing?

    - The best way is generally to use the `get` method via
      `data.get("settings")`
      - This will return a default of `None` if the key is missing
        rather than raising an exception which might crash the program
      - Or wasting time with the `in` check

4.  What is the difference between a mutable object and immutable
    object?

    - A mutable object can be modified after creation while an immutable
      object cannot

5.  How would you create a set from the string `"lost and lost again"`?

    - We can use the `set` constructor which will automatically
      deconstruct an iterable into a `set`
    - e.g. `set("lost and lost again")`

The answers can be seen below

``` python
# Q1

print("Question 1:")
print("Method 1:", dict(name="Smuah", height=62))
print("Method 2:", dict([("name", "Smuah"), ("height", 62)]))
print("Method 3:", {"name": "Smuah", "height": 62})

# Q2

print("Question 2:")
student = {"name": "Paula", "height": 64, "gpa": 3.8}
print("Before modification:", student)
student["gpa"] = 4.0
print("After modification:", student)

# Q3

print("Question 3:")
data = {"config": True, "active": False}
print("data.get(\"settings\"):", data.get("settings"))

# Q5
print("Question 5:")
print(set("lost and lost again"))
```

    Question 1:
    Method 1: {'name': 'Smuah', 'height': 62}
    Method 2: {'name': 'Smuah', 'height': 62}
    Method 3: {'name': 'Smuah', 'height': 62}
    Question 2:
    Before modification: {'name': 'Paula', 'height': 64, 'gpa': 3.8}
    After modification: {'name': 'Paula', 'height': 64, 'gpa': 4.0}
    Question 3:
    data.get("settings"): None
    Question 5:
    {'i', 's', 'l', 'o', ' ', 'g', 't', 'd', 'n', 'a'}
