# Chapter 14: Object-Oriented Programming

- [Notes](#notes)
  - [Grouping State and Function](#grouping-state-and-function)
    - [Classes and Instances](#classes-and-instances)
  - [Private Methods and Attributes](#private-methods-and-attributes)
  - [Class Variables](#class-variables)
  - [Special Methods](#special-methods)
    - [Representation Methods](#representation-methods)
    - [Rich Comparison Methods](#rich-comparison-methods)
    - [Math Operators](#math-operators)
  - [Inheritance](#inheritance)
- [Summary](#summary)
- [Questions and Answers](#questions-and-answers)

## Notes

- Object-Oriented is a common and popular paradigm
- Instead of organising code around functions or procedural state code
  is organised around *objects*
- Objects combine state and the functions that act on that state
- Program flow follows through objects interacting via methods that
  change their internal states

### Grouping State and Function

- As mentioned *objects* in an Object-Oriented sense are seen as those
  items that combine state and functions on that state
  - In contrast to procedural or systems programming where an *object*
    is often a thing that exists in memory
- In Python everything is fundamentally an *object*, combining state and
  methods
- For example, `int` has the `to_bytes` method to convert an integer to
  it’s byte representation

``` python
number = 13
number.to_bytes(8, "little")
import prompt_toolkit.contrib.regular_languages.regex_parser
```

- More complex objects like `string` or `pandas.DataFrame` combine more
  state and functionality

#### Classes and Instances

- Typically in the language of OOP we distinguish between
  1. Classes
      - A definition of an object
      - Type definition
      - e.g. `3` is of type `int`
  2. Object Instance
      - A specific *instantiation* of a class
      - `3` is an *instance* of an `int`
- Classes are defined via the `class` keyword
- Then instantiated by calling the class name as a function e.g.,

``` python
class DoNothing:
    pass


nothing = DoNothing()
print(type(nothing))
```

    <class '__main__.DoNothing'>

- The above defines an empty class
- To check if an object is of a given class dynamically one can use
  `isinstance`
  - Accepts an argument list containing the object to be tested then
    either the class to test against, or a tuple of classes

``` python
class DoNothing:
    pass


nothing = DoNothing()

isinstance(nothing, DoNothing)
```

    True

- Methods are defined on a class indented one level to be inside the
  class scope
- All normal methods must have at least one parameter
  - This first parameter is a reference to the object the method is
    called on
  - By convention this is always called `self`

``` python
class DoSomething:
    def return_self(self):
        return self

do_something = DoSomething()

do_something == do_something.return_self()
```

    True

> [!NOTE]
>
> **The `self` Parameter**
>
> While the `self` parameter must always be specified in the method
> signature, it does not need to actually be passed. It is done so
> implicitly. Once can consider the method call,
>
> ``` python
> do_something.return_self()
> ```
>
> as equivalent to the following standard function call
>
> ``` python
> return_self(do_something)
> ```

- Attributes, or variables attached to a class or object instance can
  also be defined using the `.` syntax

``` python
class AddAttribute:
    def add_score(self):
        self.score = 14  # set's the score attribute, creates it if it does not exist


add_attribute = AddAttribute()
add_attribute.add_score()

add_attribute.score
```

    14

- Method’s can call other methods like any function by referencing the
  target method on the `self` argument

``` python
class InternalMethodCaller:
    def method_one(self):
        print("Calling method one")

    def method_two(self, n):
        print(f"Method two calling method one {n} times")
        for _ in range(n):
            self.method_one()


internal_method_caller = InternalMethodCaller()
internal_method_caller.method_one()
internal_method_caller.method_two(2)
```

    Calling method one
    Method two calling method one 2 times
    Calling method one
    Calling method one

### Private Methods and Attributes

- Methods and attributes on an object are by default accessible by
  anyone who can access the object
  - Referred to as a *public* method or attribute
- Sometimes methods or attributes are defined that are not supposed to
  be accessed by an external caller
  - Called *private*
- Python unlike some languages does not provide strong guarantees or
  support for private variables
- Instead by convention a method or attribute beginning with an `_` is
  regarded as *protected*
  - Should not be accessed outside the class

``` python
class PrivatePublic():
    def _private_method(self):
        print("private")

    def public_method(self):
        self._private_method()

private_public = PrivatePublic()
private_public.public_method()
```

    private

### Class Variables

- Variables associated to a specific instance (e.g. via `self.<name>`)
  are called *instance variables*
  - They can have a unique value for each specific instanceof the
    variable
- Class level variables or *class variables* are defined on the class
  itself
  - Hence exist at the class level
  - Shared across all instances of the class
  - They are defined the same way a method is defined
    - Inside the class scope

``` python
class ClassyVariables:
    class_variable = "Yellow"

    def __init__(self, colour):
        self.instance_variable = colour


red = ClassyVariables("red")
blue = ClassyVariables("blue")

print(f"red.instance_variable:", red.instance_variable)
print(f"red.class_variable:", red.class_variable)
print(f"blue.instance_variable:", blue.instance_variable)
print(f"blue.class_variable:", blue.class_variable)
```

    red.instance_variable: red
    red.class_variable: Yellow
    blue.instance_variable: blue
    blue.class_variable: Yellow

### Special Methods

- Python reserves the names of some methods for special meaning
- These are typically functions that define the standard python
  interfaces
  - e.g. Container types
  - Mathematical operators
  - Object initialisation and construction
- A common one is `__init__`
  - Called every time an object is instantiated
  - Standard mechanism for up initial attributes
- For example, the below defines a new `__init__` method that accepts an
  extra parameter `n`
  - Must be supplied now whenever the object is initialised
  - Is then assigned to an internal attribute `count`

``` python
class Initialised:
    def __init__(self, n):
        self.count = n

    def increment_count(self):
        self.count += 1


initialised = Initialised(2)
print("initialised.count:", initialised.count)

initialised.increment_count()
print("After calling increment_count:", initialised.count)
```

    initialised.count: 2
    After calling increment_count: 3

#### Representation Methods

- The methods `__repr__` and `__str__` control how an object is
  represented
- `__repr__` is supposed to give the technical representation of an
  object
  - Typically such that an object can be reconstructed
  - Invoked when `repr` is called on an object
  - When invoking an object as a statement the `__repr__` method is
    called
- The `__str__` method is intended to provide a friendly-human readable
  string representation
  - Called when `str` is invoked on an object
  - Done automatically when `print` is called on an object

``` python
class Represented:
    def __init__(self, n):
        self.n = n

    def __repr__(self):
        return f"Represented(n={self.n})"

    def __str__(self):
        return "Object demonstrating __str__ and __repr__"


represented = Represented(13)
print("repr(represented):", repr(represented))

# Reconstructing an object from the __repr__
print("Reconstructing `represented` from the `__repr__`")
r = eval(represented.__repr__())

print("type(r):", type(r))
print("r.n:", r.n)

print("str(represented):", str(represented))
print("represented:", represented)
represented
```

    repr(represented): Represented(n=13)
    Reconstructing `represented` from the `__repr__`
    type(r): <class '__main__.Represented'>
    r.n: 13
    str(represented): Object demonstrating __str__ and __repr__
    represented: Object demonstrating __str__ and __repr__

    Represented(n=13)

#### Rich Comparison Methods

- Comparison operators are used to define how an object behaves when
  used with the built-in comparison operators

- The special methods are,

  1. `__lt__(self, other)`
      - Special method for `self < other`
  2. `__le__(self, other)`
      - Special method for `self <= other`
  3. `__eq__(self, other)`
      - Special method for `self == other`
  4. `__gt__(self, other)`
      - Special method for `self > other`
  5. `__ge__(self, other)`
      - Special method for `self >= other`

- We can define the full set of comparables on an example class

  - Here we want a class representing a score and time
  - Objects are ordered in terms of highest score, and lowest time (with
    score taking precedent)

``` python
class CompareMe:
    def __init__(self, score, time):
        self.score = score
        self.time = time

    def __lt__(self, other):
        print("Called __lt__")
        if self.score == other.score:
            return self.time > other.time

    def __le__(self, other):
        print("Called __le__")
        return self.score <= other.score

    def __eq__(self, other):
        print("Called __eq__")
        return (self.score, self.time) == (other.score, other.time)

    def __ne__(self, other):
        print("Called __ne__")
        return (self.score, self.time) != (other.score, other.time)

    def __gt__(self, other):
        print("Called __gt__")
        if self.score == other.score:
            return self.time < other.time
        return self.score > other.score

    def __ge__(self, other):
        print("Called __ge__")
        return self.score >= other.score


high_score = CompareMe(100, 100)
mid_score = CompareMe(50, 50)
mid_score_1 = CompareMe(50, 50)
low_time = CompareMe(100, 25)

print("high_score > mid_score:", high_score > mid_score)
print("high_score >= mid_score:", high_score >= mid_score)
print("high_score == low_time:", high_score == low_time)
print("mid_score == mid_score_l:", mid_score == mid_score_l)
print("low_time == high_score:", low_time == high_score)
```

    Called __gt__
    high_score > mid_score: True
    Called __ge__
    high_score >= mid_score: True
    Called __eq__
    high_score == low_time: False

    NameError: name 'mid_score_l' is not defined
    ---------------------------------------------------------------------------
    NameError                                 Traceback (most recent call last)
    Cell In[11], line 42
         38
         39 print("high_score > mid_score:", high_score > mid_score)
         40 print("high_score >= mid_score:", high_score >= mid_score)
         41 print("high_score == low_time:", high_score == low_time)
    ---> 42 print("mid_score == mid_score_l:", mid_score == mid_score_l)
         43 print("low_time == high_score:", low_time == high_score)

    NameError: name 'mid_score_l' is not defined

- Comparisons don’t have to be defined purely against their own type
  - But in reality they often should be

``` python
class ScoreMatters:
    def __init__(self, score):
        self.score = score

    def __lt__(self, other):
        return self.score < other

    def __eq__(self, other):
        return self.score == other


my_score = ScoreMatters(14)
print("my_score == 14:", my_score == 14)
print("my_score < 15:", my_score < 15)
```

    my_score == 14: True
    my_score < 15: True

- Good advice to follow when overriding operators is the *rule of least
  surprise*
  - Operators should always do what the user is likely to expect
- For example, both demonstration classes are probably *bad* examples
  - The first has counter intuitive differences between `<`, `<=` and
    `eq` for example
  - The second compares different objects (`ScoreMatters` and a number)
- It is possible to do dumb things like defining classes that always
  compare bigger than anything observed

``` python
class AlwaysBigger:
    def __gt__(self, other):
        return True

    def __ge__(self, other):
        return True


bigger = AlwaysBigger()
biggest = AlwaysBigger()

print("bigger > biggest:", bigger > biggest)
print("biggest > bigger:", biggest > bigger)
```

    bigger > biggest: True
    biggest > bigger: True

#### Math Operators

- The same way we can override the comparison operators we can also
  override the mathematical operators

- Their forms are,

  1. `__add__(self, other)`
      - For `self + other`
  2. `__sub__(self, other)`
      - For `self - other`
  3. `__mul__(self, other)`
      - For `self * other`
  4. `__div__(self, other)`
      - For `self \ other`

- An example class is given below

``` python
class MathMe:
    def __init__(self, value):
        self.value = value

    def __add__(self, other):
        return MathMe(self.value + other.value)

    def __sub__(self, other):
        return MathMe(self.value - other.value)

    def __mul__(self, other):
        return MathMe(self.value * other.value)

m1 = MathMe(3)
m2 = MathMe(4)

m3 = m1 + m2
print("m3:", m3.value)

m4 = m1 - m3
print("m4:", m4.value)

m5 = m1 * m3
print("m5:", m5.value)
```

    m3: 7
    m4: -4
    m5: 21

There are an extensive set of special methods covering various different
interfaces for python. They can be found as with all things at the
[docs](https://docs.python.org/3/reference/datamodel.html#special-method-names)

### Inheritance

- A powerful technique in object-oriented programming is inheritance
- In inheritance classes are defined in a *hierarchy*
  - Classes further down the hierarchy can access the methods and
    attributes of their parents
- For example, here we have a simple hierarchy where `Student` is a
  child or *subclass* of `Person`
  - Here we define `__init__` on the person taking a `first_name` and a
    `last_name`
  - `Student` does not define and `__init__` but inherits it from
    `Person` `Student` defines a method `introduce_yourself` which
    accesses the `self.first_name` attribute inherited from the `Person`
    superclass

``` python
class Person:
    def __init__(self, first_name, last_name):
        self.first_name = first_name
        self.last_name = last_name


class Student(Person):
    def introduce_yourself(self):
        print(f"Hello, my name is {self.first_name}")


barb = Student("Barb", "Shilala")  # Student inherits `__init__` from `Person`
print("barb.first_name:", barb.first_name)
barb.introduce_yourself()

print("type(barb):", type(barb))
print("isinstance(barb, Student):", isinstance(barb, Student))
print("isinstance(barb, Person):", isinstance(barb, Person))
```

    barb.first_name: Barb
    Hello, my name is Barb
    type(barb): <class '__main__.Student'>
    isinstance(barb, Student): True
    isinstance(barb, Person): True

- As shown above, `isinstance` obeys the inheritance structure as well
  - A subclass will show as an instance of the parent class
- Inheritance is useful when code needs to be shared across classes
  - For example, a job orchestration system might require all classes to
    have a `run` method
  - In this instance all job types might inherit from a parent `Job`
    class
    - All subclasses them inherit the `run` method

``` python
class Job:
    def run(self):
        print("I'm running")


class ExtractJob(Job):
    def extract(self, data):
        print("Extracting")


class TransformJob(Job):
    def transform(self, data):
        print("Transforming")


job_1 = ExtractJob()
job_2 = TransformJob()

for job in [job_1, job_2]:
    if isinstance(job, Job):
        job.run()
```

    I'm running
    I'm running

- A child that defines a method or attribute already declared by it’s
  parent will use it’s definition in place of the parent’s definition
- For example,

``` python
class Parent:
    def run(self):
        print("I'm the parent running free")


class Child(Parent):
    def run(self):
        print("I am a child running wild")


child = Child()
child.run()
```

    I am a child running wild

- If a child has defined it’s own method yet wishes to invoke the parent
  method this can be done via the `super` built-in function
  - Most commonly this is done to invoke the super’s `__init__` method

``` python
class Person:
    def __init__(self, first_name, last_name):
        self.first_name = first_name
        self.last_name = last_name


class Student(Person):
    def __init__(self, school_name, first_name, last_name):
        self.school_name = school_name
        super().__init__(first_name, last_name)


lydia = Student("boxford", "lydia", "smith")
lydia.last_name
```

    'smith'

- Inheritance can have multiple levels

``` python
class A:
    pass


class B(A):
    pass


class C(B):
    pass


print("isinstance(C(), B):", isinstance(C(), B))
print("isinstance(C(), A):", isinstance(C(), A))
```

    isinstance(C(), B): True
    isinstance(C(), A): True

- Another form of multiple inheritance, is when one class inherits from
  multiple classes simultaneously

``` python
class A:
    def a_method(self):
        print("A's method")

class B:
    def b_method(self):
        print("B's method")

class C(A, B):
    pass

c = C()
c.a_method()
c.b_method()
```

    A's method
    B's method

> [!CAUTION]
>
> **Complex Inheritance**
>
> Inheritance is powerful which also makes it easy to abuse. Be careful
> when using complex inheritance hierarchies and multiple inheritance
> sructures. They can make your program hard to reason about and brittle
> to changes. There is plenty of literature in the wild that covers many
> of the common design patterns and considerations of object-oriented
> design

## Summary

- Object-oriented programming is a design paradigm that organises code
  around objects and the messages between them
  - Objects encapsulate state and the methods that operate on state
    together
- Object’s are also referred to as *instances* of a *class*
  - A class is a form of type definition providing the attributes and
    the methods
- Python’s classes can define special methods to interact with various
  python API’s
  - Including `__str__` and `__repr__` for controlling how objects are
    represented
  - Comparison operators
  - Mathematical operators

## Questions and Answers

1. What does the variable `self` represent in a class definition?

    - `self` is a variable referencing the object on which the method is
      invoked

2. When is the `__init__` special method called?

    - It is called at object construction

3. Given the following class definition

    ``` python
     class Confused:
         def __init__(self, n):
             self.n = n

         def __add__(self, other):
             return self.n - other
    ```

    - What would you expect from the following?

      ``` python
         c = Confused(12)
         c + 12
      ```

          0

    - Looking at the definition this is equivalent to `12 - 12`, so the
      return is `0`

4. What wil be the output of the following code

    ``` python
     class A:
         def say_hello(self):
             print("Hello from A")

         def say_goodbye(self):
             print("Goodbye from A")

     class B(A):
         def say_goodbye(self):
             print("Goodbye from B")

     b = B()
     b.say_hello()
     b.say_goodbye()
    ```

        Hello from A
        Goodbye from B

    - `b` is created as an instance of the class `B` which inherits from
      `A`
      - Calling `say_hello` on `b` first looks in the class definition
        of `B` for the method `say_hello`
        - Since it does not exist, `b` looks on the definition of `A`,
          so prints, `"Hello from A"`
      - Next we invoke `say_goodbye` on `b`. This time `say_goodbye` is
        defined on `B` so we print `Goodbye from B`
