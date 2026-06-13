# Chapter 7: NumPy


- [Notes](#notes)
  - [Installing NumPy](#installing-numpy)
  - [Creating Arrays](#creating-arrays)
  - [Indexing and Slicing](#indexing-and-slicing)
  - [Element-by-Element Operations](#element-by-element-operations)
  - [Filtering Values](#filtering-values)
  - [Views vs Copies](#views-vs-copies)
  - [Array Methods](#array-methods)
  - [Broadcasting](#broadcasting)
  - [NumPy Mathematics](#numpy-mathematics)
- [Summary](#summary)
- [Questions](#questions)

## Notes

- NumPy is a numerical computing library for Python
- It underpins many other libraries in the standard python data science
  ecosystem

> [!IMPORTANT]
>
> **Third Party Libraries**s
>
> Python code is organised into packages or libraries. Python itself
> comes with the *standard library*, which is large collection of
> build-in modules and packages that can be imported to add
> functionality to Python’s basic language. Third-party libraries can be
> installed to add functionality further. These libraries often need to
> be installed.

### Installing NumPy

- Numpy can be installed using your python package manager

- For example, with the default `pip`

  ``` shell
    pip install numpy
  ```

- Once installed, Numpy can then be imported to be used

  - A common convention is to import `numpy` under the alias `np`

``` python
import numpy as np
```

### Creating Arrays

- The basic structure of numpy is the `array`
- Simple data structure designed for efficient operations on large
  collections of data
- Arrays can be flexible in the length and number of dimensions
  - Each array is of fixed-type
  - In contrast to Python’s default `list` which supports mixed-types
- Many Python libraries are built around the numpy array including
  - Scipy
    - A scientific computing package
  - Pandas
    - A data science package centred around CSV data
- The numpy array has flexible constructor, they can be created from,
  - Existing sequences
  - `np.zeros`
    - Creates an array of a specified size containing only zeroes
  - `np.ones`
    - Creates an array of a specified size containing only ones
  - `np.empty`
    - Creates an array of a specified size containing arbitrary values
    - Good for creating large arrays efficiently
    - Should only be used when you will override the existing data
  - `np.arange`
    - Creates an array from a numeric range
    - Similar to a built-in range object (takes a beginning, end and
      step)
  - `np.linspace`
    - Creates an array containing evenly spaced points over an interval
  - and more!

``` python
import numpy as np

print(np.array([1, 2, 3])) # Creating from a list

print(np.zeros(3)) # Array of zeros

print(np.ones(3)) # Array of ones

print(np.empty(3)) # Array of arbitrary data

print(np.arange(3)) # Array from a range of numbers

print(np.arange(0, 12, 3)) # Array from a range of numbers

np.linspace(0, 21, 7) # Array over an interval
```

    [1 2 3]
    [0. 0. 0.]
    [1. 1. 1.]
    [1. 1. 1.]
    [0 1 2]
    [0 3 6 9]

    array([ 0. ,  3.5,  7. , 10.5, 14. , 17.5, 21. ])

- Arrays have dimensions, and each dimension is associated with a length
  - For a one-dimensional array this is the number of elements
- Arrays define a number of attributes which can be used to introspect
  their state
  - `dtype`
    - Denotes the type of object stored in the array
    - Note that NumPy defines many of it’s own standard types such as
      fixed-width integer types
  - `size`
    - Number of elements
  - `nbytes`
    - Number of bytes consumed by the array elements
  - `shape`
    - A tuple containing the length of each dimension of the array
  - `ndim`
    - The number of dimensions

``` python
import numpy as np

one_d_array = np.arange(21)

print(one_d_array) # elements

print(one_d_array.dtype) # data type

print(one_d_array.size) # number of elements

print(one_d_array.nbytes) # memory consumption

print(one_d_array.shape) # shape of the array

print(one_d_array.ndim) # number of dimensions

print(type(one_d_array))
```

    [ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20]
    int64
    21
    168
    (21,)
    1
    <class 'numpy.ndarray'>

- Technically the type for a Numpy array is the `ndarray` which is a
  shorthand for *n-dimensional array*
- This means they can also be used to make matrices

``` python
import numpy as np

list_of_lists = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

matrix = np.array(list_of_lists)
print(matrix)

print(matrix.shape)

print(matrix.ndim)
```

    [[1 2 3]
     [4 5 6]
     [7 8 9]]
    (3, 3)
    2

- An array can be restructured into a different shape using the
  `reshape` method
  - Takes a new shape as an argument, i.e. a comma-separated argument
    list specifying the length of each dimension
  - This can be used to add or remove dimensions
- For example, we could instead create a matrix by defining a
  one-dimensional list then reshaping it

``` python
import numpy as np

one_d_array = np.arange(12)
two_d_array = one_d_array.reshape(3, 4)

print(two_d_array)

three_d_array = one_d_array.reshape(2, 2, 3)
print(three_d_array)
```

    [[ 0  1  2  3]
     [ 4  5  6  7]
     [ 8  9 10 11]]
    [[[ 0  1  2]
      [ 3  4  5]]

     [[ 6  7  8]
      [ 9 10 11]]]

- The provided shape must be compatible with the current shape of the
  array
  - i.e. we cannot over or under-specify the number of elements

``` python
import numpy as np

matrix = np.arange(12).reshape(5, 5)
```

    ValueError: cannot reshape array of size 12 into shape (5,5)
    ---------------------------------------------------------------------------
    ValueError                                Traceback (most recent call last)
    Cell In[5], line 3
          1 import numpy as np
          2 
    ----> 3 matrix = np.arange(12).reshape(5, 5)

    ValueError: cannot reshape array of size 12 into shape (5,5)

- Reshaping is especially useful when paired with methods like
  `np.zeros` and `np.ones` to create new arrays of the appropriate shape

### Indexing and Slicing

- Numpy arrays support indexing and slicing like lists
- Each dimension can be sliced or indexed independently of each other

``` python
import numpy as np

one_d_array = np.arange(21)

print("1D Examples:")

print("Indexing:")
print("Accessing index 3:", one_d_array[3])
print("Accessing index -1:", one_d_array[-1])

print("Slicing from 3 to 9")
print(one_d_array[3:9])

print("2D Examples:")
two_d_array = np.arange(21).reshape(3, 7)

print("Indexing:")
print("Accessing row 2:", two_d_array[2])
print("Accessing row 2, column 3:", two_d_array[2, 3])

print("Slicing:")
print("Slicing row 0 to 2:", two_d_array[:2])
print("Accessing column 3 in all rows:", two_d_array[:, 3])
print("Print accessing last 3 columns in row 0 and 1:", two_d_array[:2, -3:])
```

    1D Examples:
    Indexing:
    Accessing index 3: 3
    Accessing index -1: 20
    Slicing from 3 to 9
    [3 4 5 6 7 8]
    2D Examples:
    Indexing:
    Accessing row 2: [14 15 16 17 18 19 20]
    Accessing row 2, column 3: 17
    Slicing:
    Slicing row 0 to 2: [[ 0  1  2  3  4  5  6]
     [ 7  8  9 10 11 12 13]]
    Accessing column 3 in all rows: [ 3 10 17]
    Print accessing last 3 columns in row 0 and 1: [[ 4  5  6]
     [11 12 13]]

- You can assign new values to an array as with a list
  - This includes reassigning to a whole sliced section

``` python
import numpy as np

two_d_array = np.arange(21).reshape(3, 7)

print("Assigning to (0, 0):")
two_d_array[0, 0] = 33
print(two_d_array)

print("Zeroing out the slice [1:, :3]:")
two_d_array[1:, :3] = 0
print(two_d_array)
```

    Assigning to (0, 0):
    [[33  1  2  3  4  5  6]
     [ 7  8  9 10 11 12 13]
     [14 15 16 17 18 19 20]]
    Zeroing out the slice [1:, :3]:
    [[33  1  2  3  4  5  6]
     [ 0  0  0 10 11 12 13]
     [ 0  0  0 17 18 19 20]]

### Element-by-Element Operations

- As mentioned NumPy is designed for fast calculations on large
  collections of data
- One of the ways it implements this is in it’s operator formalism
  - For the built-in Python list operators like `+` and `*` were viewed
    as concatenation operations on the list *itself*
  - In NumPy operations are typically viewed as *element-wise* on the
    array elements themselves
- For example, to perform element-wise multiplication of two lists in
  regular python we could write

``` python
list_1 = list(range(10))
list_2 = list(range(10, 0, -1))

print("List 1:", list_1)
print("List 2:", list_2)

results = []
for i, j in zip(list_1, list_2, strict=True):
    results.append(i * j)
print("results:", results)
```

    List 1: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    List 2: [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
    results: [0, 9, 16, 21, 24, 25, 24, 21, 16, 9]

- The equivalent operation in NumPy becomes,

``` python
import numpy as np

array_1 = np.arange(10)
array_2 = np.arange(10, 0, -1)

print("Array 1:", array_1)
print("Array 2:", array_2)

results = array_1 * array_2
print("Results:", results)
```

    Array 1: [0 1 2 3 4 5 6 7 8 9]
    Array 2: [10  9  8  7  6  5  4  3  2  1]
    Results: [ 0  9 16 21 24 25 24 21 16  9]

### Filtering Values

- A common task given a data structure is filtering out values of
  interest
- NumPy offers built-in support for filtering array elements
  - We can use any boolean condition that is compatible with the shape
    of our array
- For example we could define a `mask` array filled with boolean values
  - Then simply pass it the array as if we were selecting an index

``` python
import numpy as np

two_d_array = np.arange(21).reshape(3, 7)
mask = np.array([[True, False, True, False, True, False, True],
                 [True, False, True, False, True, False, True],
                 [True, False, True, False, True, False, True]])

print(two_d_array[mask])
```

    [ 0  2  4  6  7  9 11 13 14 16 18 20]

- Comparison operators will also naturally create an element-by-element
  mask

``` python
import numpy as np

two_d_array = np.arange(21).reshape(3, 7)
print(mask := (two_d_array < 5))

print(two_d_array[mask])
```

    [[ True  True  True  True  True False False]
     [False False False False False False False]
     [False False False False False False False]]
    [0 1 2 3 4]

- We can also use logic operators to do things such as pair multiple
  array comparisons together to build complex masks

``` python
import numpy as np

two_d_array = np.arange(21).reshape(3, 7)
print(mask := (two_d_array < 5) & (two_d_array % 2 == 0))

print(two_d_array[mask])
```

    [[ True False  True False  True False False]
     [False False False False False False False]
     [False False False False False False False]]
    [0 2 4]

### Views vs Copies

- To preserve efficiency where possible NumPy will return a *view*
  rather than a *copy*
  - This includes when slicing or filtering
- A *view* is a different look at the same block of memory
  - Means this operations don’t incur the cost of copying the underlying
    memory
- The downside is that changing the value in a view propagates that
  change to the original array
  - This includes other views too

``` python
import numpy as np

original_data = np.arange(24).reshape(4, 6)

print("Original data:\n", original_data)

new_data = original_data[:2, 3:]
print("New data view:\n", new_data)

new_data[1, 2] = -1
print("New data after change:\n", new_data)

print("Original data after change:\n", original_data)
```

    Original data:
     [[ 0  1  2  3  4  5]
     [ 6  7  8  9 10 11]
     [12 13 14 15 16 17]
     [18 19 20 21 22 23]]
    New data view:
     [[ 3  4  5]
     [ 9 10 11]]
    New data after change:
     [[ 3  4  5]
     [ 9 10 -1]]
    Original data after change:
     [[ 0  1  2  3  4  5]
     [ 6  7  8  9 10 -1]
     [12 13 14 15 16 17]
     [18 19 20 21 22 23]]

- When used naively this can result in bugs
  - But in the long run results in much better performance
- If you want to make a copy, you can do so explicitly

``` python
import numpy as np

original_data = np.arange(24).reshape(4, 6)

print("Original data:\n", original_data)

new_data = original_data[:2, 3:].copy()
print("New data copy:\n", new_data)

new_data[1, 2] = -1
print("New data after change:\n", new_data)

print("Original data after change:\n", original_data)
```

    Original data:
     [[ 0  1  2  3  4  5]
     [ 6  7  8  9 10 11]
     [12 13 14 15 16 17]
     [18 19 20 21 22 23]]
    New data copy:
     [[ 3  4  5]
     [ 9 10 11]]
    New data after change:
     [[ 3  4  5]
     [ 9 10 -1]]
    Original data after change:
     [[ 0  1  2  3  4  5]
     [ 6  7  8  9 10 11]
     [12 13 14 15 16 17]
     [18 19 20 21 22 23]]

### Array Methods

- There is an extensive selection of built-in methods on NumPy arrays
- This section will canvas a subset of them
  - Includes methods for calculating statistics
- Many methods have an `axis` parameter
  - By default, most of time methods like `max` will calculate over the
    entire array
  - The `axis` parameter allows us to select modify this, e.g. to
    calculate the max in each row
    - An `axis` of `1` denotes to perform the function to each row
    - An `axis` of `0` denotes to perform the function to each column

``` python
import numpy as np

data = np.arange(12).reshape(3, 4)
print(data)

print("data.max():", data.max(), "and data.min():", data.min())

print("data.sum():", data.sum())
print("data.mean():", data.mean() and "data.std():", data.std())

print("data.sum(axis=1):", data.sum(axis=1))
print("data.sum(axis=0):", data.sum(axis=0))
print("data.std(axis=0):", data.std(axis=0))
print("data.std(axis=1):", data.std(axis=1))
```

    [[ 0  1  2  3]
     [ 4  5  6  7]
     [ 8  9 10 11]]
    data.max(): 11 and data.min(): 0
    data.sum(): 66
    data.mean(): data.std(): 3.452052529534663
    data.sum(axis=1): [ 6 22 38]
    data.sum(axis=0): [12 15 18 21]
    data.std(axis=0): [3.26598632 3.26598632 3.26598632 3.26598632]
    data.std(axis=1): [1.11803399 1.11803399 1.11803399]

- NumPy also includes built-in support for matrix operations including
  - Transpose (`.T`)
  - Matrix multiplication (`@`)
  - Dot product (`.dot()`)
  - Retrieve the diagonal (`.diagonal()`)

``` python
import numpy as np

M = np.arange(9).reshape(3,3)
N = np.arange(9).reshape(3,3)

print("M:\n", M)

print("M.T:\n", M.T)

print("N:\n", N)

print("M @ N:\n", M @ N)

print("M.dot(N):\n", M.dot(N))

print("M.diagonal():\n", M.diagonal())
```

    M:
     [[0 1 2]
     [3 4 5]
     [6 7 8]]
    M.T:
     [[0 3 6]
     [1 4 7]
     [2 5 8]]
    N:
     [[0 1 2]
     [3 4 5]
     [6 7 8]]
    M @ N:
     [[ 15  18  21]
     [ 42  54  66]
     [ 69  90 111]]
    M.dot(N):
     [[ 15  18  21]
     [ 42  54  66]
     [ 69  90 111]]
    M.diagonal():
     [0 4 8]

- As mentioned NumPy arrays are designed to hold one fixed type at a
  time
  - Ideally numeric types
- When converting a list NumPy will determine the most appropriate type
  - `np.object` is the fallback object
    - Has limited functionality
- If the data type is not accurately
  - e.g. Because NumPy is defaulting to a larger width integer than
    required this can be overridden via the `dtype` argument at creation
  - Or the `astype` method later
- Attempting to modify an array to an invalid type will generate an
  error
  - A subtle error that can occur is when adding different numeric types
  - For example adding `0.5` to a `int8` numpy array will be coerced
    into `0`

``` python
import numpy as np

simple_array = np.arange(100)
print(simple_array)
print("Type of simple_array elements:", simple_array.dtype)
print("Memory footprint:", simple_array.nbytes)

small_array = np.arange(100, dtype=np.int8)
print(small_array)
print("Type of small_array elements:", small_array.dtype)
print("Memory footprint:", small_array.nbytes)

# Invalid type operations

print("Adding floating point value 0.5 to the array")
small_array[14] += 0.5
print(small_array)

print("Modifying a value to a string")
small_array[14] = "Hello, World!"
```

    [ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23
     24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47
     48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64 65 66 67 68 69 70 71
     72 73 74 75 76 77 78 79 80 81 82 83 84 85 86 87 88 89 90 91 92 93 94 95
     96 97 98 99]
    Type of simple_array elements: int64
    Memory footprint: 800
    [ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23
     24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47
     48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64 65 66 67 68 69 70 71
     72 73 74 75 76 77 78 79 80 81 82 83 84 85 86 87 88 89 90 91 92 93 94 95
     96 97 98 99]
    Type of small_array elements: int8
    Memory footprint: 100
    Adding floating point value 0.5 to the array
    [ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23
     24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47
     48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64 65 66 67 68 69 70 71
     72 73 74 75 76 77 78 79 80 81 82 83 84 85 86 87 88 89 90 91 92 93 94 95
     96 97 98 99]
    Modifying a value to a string

    ValueError: invalid literal for int() with base 10: 'Hello, World!'
    ---------------------------------------------------------------------------
    ValueError                                Traceback (most recent call last)
    Cell In[17], line 20
         16 small_array[14] += 0.5
         17 print(small_array)
         18 
         19 print("Modifying a value to a string")
    ---> 20 small_array[14] = "Hello, World!"

    ValueError: invalid literal for int() with base 10: 'Hello, World!'

### Broadcasting

- As seen NumPy naturally performs it’s operations *element-by-element*
- In-fact more powerfully than that, NumPy naturally *broadcasts* arrays
  when doing operations between arrays of different dimensions / shapes
  - i.e. If the dimension of one of the arrays is lower than the
    dimension of the other NumPy will effectively create *replicates* of
    the array to fill out the additional missing dimensions
  - If the both arrays have a given dimension but the shape of one is
    `1` then the broadcasting also works
- This can be confusing but is best shown with examples

``` python
import numpy as np

M = np.arange(1, 10).reshape(3,3)
N = np.ones(9).reshape(3,3)

print("M\n", M, "\nN:\n", N)

print("Normal element-by-element addition:", M + N)

K = np.arange(1, 4)
print("Broadcasted addition (M + K):", M + K)

print("Broadcasted addition (M + 1):", M + 1)
```

    M
     [[1 2 3]
     [4 5 6]
     [7 8 9]] 
    N:
     [[1. 1. 1.]
     [1. 1. 1.]
     [1. 1. 1.]]
    Normal element-by-element addition: [[ 2.  3.  4.]
     [ 5.  6.  7.]
     [ 8.  9. 10.]]
    Broadcasted addition (M + K): [[ 2  4  6]
     [ 5  7  9]
     [ 8 10 12]]
    Broadcasted addition (M + 1): [[ 2  3  4]
     [ 5  6  7]
     [ 8  9 10]]

- Once you’ve got the hang of it the concept is pretty intuitive
  - Effectively the smaller array is expanded to fit the higher
    dimension
- For example operations between an array of shape `(1, 3, 4, 4)` and
  `(5, 3, 4, 1)` would broadcast to a `(5, 3, 4, 4)` size array

``` python
import numpy as np

print("Demonstrating broadcasting between a (2, 1, 5) and (2, 7, 1) array")

M = np.arange(10).reshape(2, 1, 5)
N = np.arange(14).reshape(2, 7, 1)

print("M:\n", M, "\nN:\n", N)

print(K := M + N)
print(K.shape)
```

    Demonstrating broadcasting between a (2, 1, 5) and (2, 7, 1) array
    M:
     [[[0 1 2 3 4]]

     [[5 6 7 8 9]]] 
    N:
     [[[ 0]
      [ 1]
      [ 2]
      [ 3]
      [ 4]
      [ 5]
      [ 6]]

     [[ 7]
      [ 8]
      [ 9]
      [10]
      [11]
      [12]
      [13]]]
    [[[ 0  1  2  3  4]
      [ 1  2  3  4  5]
      [ 2  3  4  5  6]
      [ 3  4  5  6  7]
      [ 4  5  6  7  8]
      [ 5  6  7  8  9]
      [ 6  7  8  9 10]]

     [[12 13 14 15 16]
      [13 14 15 16 17]
      [14 15 16 17 18]
      [15 16 17 18 19]
      [16 17 18 19 20]
      [17 18 19 20 21]
      [18 19 20 21 22]]]
    (2, 7, 5)

### NumPy Mathematics

- Numpy includes many built-in mathematical functions, includes
  - Trigonometric functions
  - Logarithms
  - Arithmetic
- These also obey the broadcasting rules
- Other libraries can take advantage of these with specialised types
  - For example, NumPy provides specialised types for representing
    polynomials
  - The `poly1d` class represents a one-dimensional polynomial
- A `poly1d` object is created by providing it a tuple of coefficients
  - Printing it shows the polynomial representation

``` python
import numpy as np

p = np.poly1d((4, 5))
print(p)
```

     
    4 x + 5

- Alternatively takes a boolean flag as the second argument to indicate
  the first argument is the polynomial’s roots rather than coefficients

``` python
import numpy as np

p = np.poly1d([4, 3, 2, 1], True)
print(p)
```

       4      3      2
    1 x - 10 x + 35 x - 50 x + 24

- A polynomial can be evaluated using the function call syntax and
  passing the value to evaluate it at

``` python
import numpy as np

p = np.poly1d([4, 3, 2, 1], True)
print(p(5))
```

    24.0

- Can also perform arithmetic operations between polynomials
  - e.g. addition and multiplication
  - Additional built-in methods
    - e.g. for derivatives and integrals

``` python
import numpy as np

p = np.poly1d((2, 3))
print(p)

q = np.poly1d((1, 2, 3))
print(q)

print("p * q =",p * q)

print("Derivative of q is", q.deriv())

print("Integral of p is", p.integ())
```

     
    2 x + 3
       2
    1 x + 2 x + 3
    p * q =    3     2
    2 x + 7 x + 12 x + 9
    Derivative of q is  
    2 x + 2
    Integral of p is    2
    1 x + 3 x

## Summary

- The NumPy library underpins many common scientific and data science
  packages
- NumPy arrays are designed to support multi-dimensional, single-type
  data
- The are designed for efficient calculation and memory consumption even
  with large datasets

## Questions

1.  Name three differences between NumPy arrays and Python lists

    1.  NumPy arrays must be of fixed-type rather than supporting
        mixed-type
    2.  NumPy arrays naturally support element-by-element operations,
        list’s default operations apply to list itself
    3.  NumPy arrays are by default multi-dimensional and provide
        extended syntactic support for multi-dimensional accesses and
        slices, python lists must implement multiple dimensions as a
        list-of-lists construct

2.  Given the following code what would you expect the final value to be
    for `d2`?

    ``` python
     import numpy as np

     d1 = np.array([[0, 1, 3], [4, 2, 9]])
     d2 = d1[:, 1:]
    ```

    - The first slice selects all rows, and the second selects all
      columns from $1$ onwards
    - We would thus expect the final output to be
      `np.array([[1, 3], [2, 9]])`

3.  Given the following code what would you expect the final value of
    `d1[0, 2]`?

    ``` python
     import numpy as np

     d1 = np.array([[0, 1, 3], [4, 2, 9]])
     d2 = d1[:, 1:]
     d2[0, 1] = 0
    ```

    - `d2` is a view over `d1[:, 1 :]` and `d2[0, 1]` thus corresponds
      to `d1[0, 2]`
    - Since the zeroth column of `d2` is the first column of `d1`
    - So `d1[0, 2]` should be `0`, since changes to the view propagate

4.  If you add two arrays of dimensions `(1, 2, 3)` and `(5, 2, 1)`,
    what will the dimensions of the resulting array be?

    - From the broadcasting rules this would be `(5, 2, 3)`

5.  Use the `poly1d` class to model the following polynomial

    $$ 6x^4 + 2x^3 + 5x^2 + x - 10$$

    - We can do this easily by defining the coefficients

      ``` python
         import numpy as np

         g = np.poly1d((6, 2, 5, 1, -10))
      ```

- The answers are repeated below

``` python
import numpy as np

d1 = np.array([[0, 1, 3], [4, 2, 9]])
d2 = d1[:, 1:]

print("Q1:", d2)

d2[0,  1] = 0
print("Q2:", d1)

M = np.arange(6).reshape(1, 2, 3)
N = np.arange(10).reshape(5, 2, 1)

print("Q4:")
print("M.shape:", M.shape)
print("N.shape:", N.shape)
print("(M + N).shape:", (M + N).shape)

print("Q5:")
g = np.poly1d((6, 2, 5, 1, -10))
print(g)
```

    Q1: [[1 3]
     [2 9]]
    Q2: [[0 1 0]
     [4 2 9]]
    Q4:
    M.shape: (1, 2, 3)
    N.shape: (5, 2, 1)
    (M + N).shape: (5, 2, 3)
    Q5:
       4     3     2
    6 x + 2 x + 5 x + 1 x - 10
