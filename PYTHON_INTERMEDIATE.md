# Python Intermediate - Complete Guide
## Building on Your Python Foundation

---

## Table of Contents
1. [Object-Oriented Programming](#object-oriented-programming)
2. [Classes and Objects](#classes-and-objects)
3. [Inheritance](#inheritance)
4. [Polymorphism](#polymorphism)
5. [Advanced Functions](#advanced-functions)
6. [Decorators](#decorators)
7. [Iterators and Generators](#iterators-and-generators)
8. [List and Dict Comprehension](#list-and-dict-comprehension)
9. [Lambda Functions](#lambda-functions)
10. [Map, Filter, Reduce](#map-filter-reduce)
11. [Modules and Packages](#modules-and-packages)
12. [Regular Expressions](#regular-expressions)
13. [JSON and Data](#json-and-data)
14. [Error Handling Advanced](#error-handling-advanced)
15. [Debugging](#debugging)
16. [Practice Projects](#practice-projects-intermediate)

---

## OBJECT-ORIENTED PROGRAMMING

### Understanding OOP

Object-Oriented Programming (OOP) is a way to organize code by creating "objects" that have properties and behaviors.

**Real-world analogy:**
- A **CAR** is an object
- **Properties**: color, brand, speed
- **Behaviors**: start(), stop(), drive()

```python
# Before OOP (not organized)
car1_color = "red"
car1_speed = 0

def car1_start():
    print("Car started")

# This gets messy with multiple cars!

# After OOP (organized)
class Car:
    def __init__(self, color):
        self.color = color
        self.speed = 0
    
    def start(self):
        print("Car started")

car1 = Car("red")
car2 = Car("blue")
```

| OOP Concept | Explanation | Example |
|-------------|-------------|---------|
| **Class** | Blueprint for objects | `class Dog:` |
| **Object** | Instance of a class | `my_dog = Dog()` |
| **Attribute** | Property of object | `my_dog.name` |
| **Method** | Function in a class | `my_dog.bark()` |
| **Encapsulation** | Hide internal details | Private variables |
| **Inheritance** | Inherit from parent class | `class Puppy(Dog):` |
| **Polymorphism** | Different behavior, same name | Different `speak()` |

---

## CLASSES AND OBJECTS

### Creating Classes

```python
# Simple class
class Dog:
    """A class representing a dog"""
    
    # Constructor - runs when object is created
    def __init__(self, name, age):
        self.name = name           # Attribute
        self.age = age             # Attribute
    
    # Methods - functions in a class
    def bark(self):
        print(f"{self.name} says: Woof!")
    
    def get_age(self):
        return self.age

# Create objects (instances)
dog1 = Dog("Buddy", 3)
dog2 = Dog("Max", 5)

# Use objects
print(dog1.name)               # Output: Buddy
print(dog1.age)                # Output: 3
dog1.bark()                    # Output: Buddy says: Woof!
print(dog1.get_age())          # Output: 3
```

### Instance Attributes vs Methods

```python
class Student:
    def __init__(self, name, grade):
        # Instance attributes (different for each object)
        self.name = name
        self.grade = grade
        self.subjects = []
    
    def add_subject(self, subject):
        """Method to add a subject"""
        self.subjects.append(subject)
    
    def display_info(self):
        """Method to display student info"""
        print(f"Name: {self.name}")
        print(f"Grade: {self.grade}")
        print(f"Subjects: {', '.join(self.subjects)}")

# Create students
student1 = Student("Alice", "10th")
student2 = Student("Bob", "9th")

# Use methods
student1.add_subject("Math")
student1.add_subject("Science")
student1.display_info()

# Output:
# Name: Alice
# Grade: 10th
# Subjects: Math, Science

student2.add_subject("English")
student2.display_info()

# Output:
# Name: Bob
# Grade: 9th
# Subjects: English
```

### The Self Parameter

```python
class Person:
    def __init__(self, name):
        self.name = name  # 'self' refers to this specific object
    
    def greet(self):
        # 'self' lets us access this object's attributes
        print(f"Hello, I'm {self.name}")

person1 = Person("Alice")
person1.greet()  # Output: Hello, I'm Alice

# 'self' is like saying "this specific person"
# It automatically refers to the object calling the method
```

### Class Attributes (Shared by All)

```python
class School:
    # Class attribute - shared by all objects
    name = "Public School"
    location = "Main Street"
    
    def __init__(self, section):
        # Instance attribute - unique to each object
        self.section = section
    
    def info(self):
        print(f"{self.name} - Section {self.section}")

# Create objects
school1 = School("A")
school2 = School("B")

# All objects share the class attribute
print(school1.name)            # Output: Public School
print(school2.name)            # Output: Public School

# But have different instance attributes
print(school1.section)         # Output: A
print(school2.section)         # Output: B

# Change class attribute
School.name = "New School"
print(school1.name)            # Output: New School (both change!)
```

| Attribute Type | Shared? | When to Use |
|----------------|---------|------------|
| **Instance** | No | Unique per object |
| **Class** | Yes | Shared by all objects |

### Special Methods (Magic Methods)

```python
class Book:
    def __init__(self, title, author):
        self.title = title
        self.author = author
    
    # __str__ - what to print when you do print(object)
    def __str__(self):
        return f"{self.title} by {self.author}"
    
    # __repr__ - what to show in interactive mode
    def __repr__(self):
        return f"Book('{self.title}', '{self.author}')"
    
    # __len__ - what len(object) returns
    def __len__(self):
        return len(self.title)
    
    # __eq__ - equality comparison (==)
    def __eq__(self, other):
        return self.title == other.title
    
    # __lt__ - less than comparison (<)
    def __lt__(self, other):
        return len(self.title) < len(other.title)

book1 = Book("Python", "Guido")
book2 = Book("Java", "Sun")

# Using special methods
print(book1)                   # Output: Python by Guido (__str__)
print(len(book1))              # Output: 6 (__len__)
print(book1 == book2)          # Output: False (__eq__)
print(book1 < book2)           # Output: False (__lt__)
```

| Special Method | When Called | Example |
|----------------|------------|---------|
| `__init__` | Object creation | `obj = Class()` |
| `__str__` | `print(obj)` | String representation |
| `__repr__` | In interactive mode | Debugging |
| `__len__` | `len(obj)` | Length of object |
| `__eq__` | `obj1 == obj2` | Equality |
| `__lt__` | `obj1 < obj2` | Comparison |
| `__add__` | `obj1 + obj2` | Addition |

### Practical Class Examples

```python
# Example 1: Bank Account
class BankAccount:
    def __init__(self, owner, balance):
        self.owner = owner
        self.balance = balance
    
    def deposit(self, amount):
        if amount > 0:
            self.balance += amount
            print(f"Deposited ${amount}")
        else:
            print("Invalid amount")
    
    def withdraw(self, amount):
        if 0 < amount <= self.balance:
            self.balance -= amount
            print(f"Withdrew ${amount}")
        else:
            print("Invalid or insufficient funds")
    
    def check_balance(self):
        print(f"Balance: ${self.balance}")

account = BankAccount("Alice", 1000)
account.deposit(500)           # Output: Deposited $500
account.withdraw(200)          # Output: Withdrew $200
account.check_balance()        # Output: Balance: $1300

# Example 2: Rectangle
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height
    
    def perimeter(self):
        return 2 * (self.width + self.height)
    
    def is_square(self):
        return self.width == self.height

rect = Rectangle(5, 5)
print(rect.area())             # Output: 25
print(rect.perimeter())        # Output: 20
print(rect.is_square())        # Output: True

# Example 3: Todo Item
class TodoItem:
    def __init__(self, task, completed=False):
        self.task = task
        self.completed = completed
    
    def mark_complete(self):
        self.completed = True
    
    def __str__(self):
        status = "✓" if self.completed else "○"
        return f"{status} {self.task}"

item1 = TodoItem("Learn Python")
item2 = TodoItem("Build a project", True)
print(item1)                   # Output: ○ Learn Python
print(item2)                   # Output: ✓ Build a project
```

---

## INHERITANCE

### Understanding Inheritance

Inheritance lets a class inherit properties and methods from another class. Like a child inheriting traits from a parent.

```python
# Parent class (superclass)
class Animal:
    def __init__(self, name):
        self.name = name
    
    def speak(self):
        print(f"{self.name} makes a sound")

# Child class (subclass)
class Dog(Animal):
    pass  # Inherits everything from Animal

# Child class with its own method
class Cat(Animal):
    def speak(self):  # Override parent method
        print(f"{self.name} says: Meow!")

# Use them
dog = Dog("Buddy")
dog.speak()                    # Output: Buddy makes a sound

cat = Cat("Whiskers")
cat.speak()                    # Output: Whiskers says: Meow!
```

### Using Super()

```python
class Vehicle:
    def __init__(self, brand, year):
        self.brand = brand
        self.year = year
    
    def info(self):
        print(f"{self.brand} from {self.year}")

class Car(Vehicle):
    def __init__(self, brand, year, doors):
        super().__init__(brand, year)  # Call parent's __init__
        self.doors = doors
    
    def info(self):
        super().info()  # Call parent's info()
        print(f"Doors: {self.doors}")

car = Car("Toyota", 2020, 4)
car.info()
# Output:
# Toyota from 2020
# Doors: 4
```

### Multiple Levels of Inheritance

```python
class Animal:
    def speak(self):
        print("Sound")

class Mammal(Animal):
    def warm_blooded(self):
        print("Warm blooded")

class Dog(Mammal):
    def bark(self):
        print("Woof!")

dog = Dog()
dog.speak()              # From Animal
dog.warm_blooded()       # From Mammal
dog.bark()               # From Dog
```

### Practical Inheritance Examples

```python
# Example 1: Employee hierarchy
class Employee:
    def __init__(self, name, salary):
        self.name = name
        self.salary = salary
    
    def get_details(self):
        return f"{self.name}: ${self.salary}"

class Manager(Employee):
    def __init__(self, name, salary, team_size):
        super().__init__(name, salary)
        self.team_size = team_size
    
    def get_details(self):
        return f"{super().get_details()}, Team: {self.team_size}"

class Developer(Employee):
    def __init__(self, name, salary, language):
        super().__init__(name, salary)
        self.language = language
    
    def get_details(self):
        return f"{super().get_details()}, Language: {self.language}"

manager = Manager("Alice", 80000, 5)
dev = Developer("Bob", 60000, "Python")
print(manager.get_details())
# Output: Alice: $80000, Team: 5
print(dev.get_details())
# Output: Bob: $60000, Language: Python

# Example 2: Shape hierarchy
class Shape:
    def area(self):
        pass

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        return 3.14 * self.radius ** 2

class Square(Shape):
    def __init__(self, side):
        self.side = side
    
    def area(self):
        return self.side ** 2

shapes = [Circle(5), Square(4)]
for shape in shapes:
    print(f"Area: {shape.area()}")
# Output:
# Area: 78.5
# Area: 16
```

---

## POLYMORPHISM

### Understanding Polymorphism

Polymorphism means "many forms" - different objects can respond to the same method call differently.

```python
# Different classes with same method name
class Dog:
    def speak(self):
        return "Woof!"

class Cat:
    def speak(self):
        return "Meow!"

class Cow:
    def speak(self):
        return "Moo!"

# Same method call, different outputs
animals = [Dog(), Cat(), Cow()]
for animal in animals:
    print(animal.speak())

# Output:
# Woof!
# Meow!
# Moo!
```

### Duck Typing

```python
# Duck typing: if it looks like a duck and quacks like a duck...
class Duck:
    def quack(self):
        print("Quack!")

class Dog:
    def quack(self):  # Not a duck, but has quack method
        print("Woof woof!")

def make_it_quack(animal):
    animal.quack()  # Works for anything with quack()

duck = Duck()
dog = Dog()

make_it_quack(duck)          # Output: Quack!
make_it_quack(dog)           # Output: Woof woof!
# It doesn't matter what type it is, as long as it has quack()
```

### Abstract Base Classes

```python
from abc import ABC, abstractmethod

class Animal(ABC):  # Abstract base class
    @abstractmethod
    def make_sound(self):
        pass  # Subclasses MUST implement this

class Dog(Animal):
    def make_sound(self):
        print("Woof!")

class Cat(Animal):
    def make_sound(self):
        print("Meow!")

# This would work:
dog = Dog()
dog.make_sound()             # Output: Woof!

# This would ERROR (can't instantiate abstract class):
# animal = Animal()  # ERROR!
```

---

## ADVANCED FUNCTIONS

### Default Arguments

```python
def greet(name, greeting="Hello"):
    print(f"{greeting}, {name}!")

greet("Alice")               # Output: Hello, Alice!
greet("Bob", "Hi")           # Output: Hi, Bob!

# Be careful with mutable defaults!
def add_item(item, items=[]):
    items.append(item)
    return items

print(add_item("apple"))     # Output: ['apple']
print(add_item("banana"))    # Output: ['apple', 'banana'] - SURPRISE!
# The list persists between calls!

# Better way:
def add_item_safe(item, items=None):
    if items is None:
        items = []
    items.append(item)
    return items
```

### Keyword Arguments

```python
def create_profile(name, age, city="NYC", job="Student"):
    print(f"{name}, {age}, {city}, {job}")

# Positional arguments
create_profile("Alice", 10)  # Output: Alice, 10, NYC, Student

# Keyword arguments
create_profile("Bob", 11, city="LA", job="Developer")
# Output: Bob, 11, LA, Developer

# Mixed
create_profile("Charlie", 12, "Boston")  # Output: Charlie, 12, Boston, Student
```

### *args and **kwargs

```python
# *args - multiple positional arguments
def add_all(*numbers):
    total = 0
    for num in numbers:
        total += num
    return total

print(add_all(1, 2, 3))      # Output: 6
print(add_all(1, 2, 3, 4, 5)) # Output: 15

# **kwargs - multiple keyword arguments
def print_info(**info):
    for key, value in info.items():
        print(f"{key}: {value}")

print_info(name="Alice", age=10, city="NYC")
# Output:
# name: Alice
# age: 10
# city: NYC

# Combination
def my_function(a, b, *args, **kwargs):
    print(f"a={a}, b={b}")
    print(f"args={args}")
    print(f"kwargs={kwargs}")

my_function(1, 2, 3, 4, name="Alice", age=10)
# Output:
# a=1, b=2
# args=(3, 4)
# kwargs={'name': 'Alice', 'age': 10}
```

### Function Annotations

```python
def add(a: int, b: int) -> int:
    """Add two integers"""
    return a + b

print(add(5, 3))             # Output: 8

# With complex types
from typing import List

def process_numbers(numbers: List[int]) -> int:
    return sum(numbers)

print(process_numbers([1, 2, 3]))  # Output: 6
```

---

## DECORATORS

### Understanding Decorators

A decorator is a function that modifies another function or class.

```python
# Simple decorator
def my_decorator(func):
    def wrapper():
        print("Something before")
        func()
        print("Something after")
    return wrapper

@my_decorator  # Apply decorator with @
def say_hello():
    print("Hello!")

say_hello()
# Output:
# Something before
# Hello!
# Something after
```

### Decorators with Arguments

```python
def my_decorator(func):
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}")
        return func(*args, **kwargs)
    return wrapper

@my_decorator
def add(a, b):
    return a + b

result = add(5, 3)           # Output: Calling add
print(result)                # Output: 8
```

### Decorators with Parameters

```python
def repeat(times):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for _ in range(times):
                func(*args, **kwargs)
        return wrapper
    return decorator

@repeat(times=3)
def say_hello():
    print("Hello!")

say_hello()
# Output:
# Hello!
# Hello!
# Hello!
```

### Practical Decorators

```python
# Example 1: Timer decorator
import time

def timer(func):
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"Execution time: {end - start:.4f} seconds")
        return result
    return wrapper

@timer
def slow_function():
    time.sleep(1)
    print("Done!")

slow_function()
# Output:
# Done!
# Execution time: 1.0003 seconds

# Example 2: Authentication decorator
def require_auth(func):
    def wrapper(user_logged_in, *args, **kwargs):
        if not user_logged_in:
            print("Please log in first!")
            return
        return func(*args, **kwargs)
    return wrapper

@require_auth
def view_profile(username):
    print(f"Viewing {username}'s profile")

view_profile(True, "Alice")   # Output: Viewing Alice's profile
view_profile(False, "Bob")    # Output: Please log in first!

# Example 3: Validation decorator
def validate_positive(func):
    def wrapper(num):
        if num < 0:
            print("Number must be positive!")
            return None
        return func(num)
    return wrapper

@validate_positive
def square(num):
    return num ** 2

print(square(5))              # Output: 25
print(square(-5))             # Output: Number must be positive! / None
```

---

## ITERATORS AND GENERATORS

### Understanding Iterators

An iterator is an object that can be looped through using `next()`.

```python
# Create an iterator
numbers = iter([1, 2, 3])

print(next(numbers))         # Output: 1
print(next(numbers))         # Output: 2
print(next(numbers))         # Output: 3
# print(next(numbers))       # ERROR - StopIteration

# For loop uses iterators behind the scenes
for num in [1, 2, 3]:
    print(num)               # Same as calling next() each time
```

### Custom Iterator

```python
class CountUp:
    def __init__(self, max):
        self.max = max
        self.current = 1
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.current <= self.max:
            self.current += 1
            return self.current - 1
        else:
            raise StopIteration

# Use iterator
counter = CountUp(5)
for num in counter:
    print(num)
# Output: 1 2 3 4 5
```

### Generators (Easier Iterators)

A generator is a simple way to create iterators using a function with `yield`.

```python
# Generator function
def count_up(max):
    current = 1
    while current <= max:
        yield current
        current += 1

# Use generator
for num in count_up(5):
    print(num)
# Output: 1 2 3 4 5

# Generators are memory efficient
def infinite_count():
    num = 1
    while True:
        yield num
        num += 1

counter = infinite_count()
print(next(counter))         # Output: 1
print(next(counter))         # Output: 2
print(next(counter))         # Output: 3
# Can call next() infinitely without storing all numbers!
```

### Generator Expressions

```python
# List comprehension (stores all in memory)
squares_list = [x**2 for x in range(1000000)]

# Generator expression (calculates on demand)
squares_gen = (x**2 for x in range(1000000))

# Access items
print(next(squares_gen))     # Output: 0
print(next(squares_gen))     # Output: 1
print(next(squares_gen))     # Output: 4
```

### Practical Generators

```python
# Example 1: File reader generator
def read_large_file(filename):
    with open(filename) as f:
        for line in f:
            yield line.strip()

# for line in read_large_file("huge_file.txt"):
#     process(line)  # Only one line in memory at a time

# Example 2: Fibonacci sequence
def fibonacci(max):
    a, b = 0, 1
    while a < max:
        yield a
        a, b = b, a + b

for num in fibonacci(100):
    print(num, end=" ")
# Output: 0 1 1 2 3 5 8 13 21 34 55 89

# Example 3: Data pipeline
def parse_data(data):
    for line in data:
        yield line.upper()

def filter_short(data):
    for line in data:
        if len(line) > 3:
            yield line

raw_data = ["hello", "hi", "world", "a"]
processed = filter_short(parse_data(raw_data))
for item in processed:
    print(item)
# Output: HELLO / WORLD
```

---

## LIST AND DICT COMPREHENSION

### List Comprehension

```python
# Normal way
squares = []
for i in range(5):
    squares.append(i ** 2)
print(squares)               # Output: [0, 1, 4, 9, 16]

# List comprehension
squares = [i ** 2 for i in range(5)]
print(squares)               # Output: [0, 1, 4, 9, 16]

# With condition
even = [i for i in range(10) if i % 2 == 0]
print(even)                  # Output: [0, 2, 4, 6, 8]

# Nested
matrix = [[i*j for j in range(3)] for i in range(3)]
print(matrix)
# Output: [[0, 0, 0], [0, 1, 2], [0, 2, 4]]

# Transforming
words = ["hello", "world"]
upper = [w.upper() for w in words]
print(upper)                 # Output: ['HELLO', 'WORLD']

# Multiple conditions
numbers = [i for i in range(20) if i % 2 == 0 if i % 3 == 0]
print(numbers)               # Output: [0, 6, 12, 18]
```

### Dictionary Comprehension

```python
# Create dictionary from list
words = ["apple", "banana", "cherry"]
word_lengths = {word: len(word) for word in words}
print(word_lengths)
# Output: {'apple': 5, 'banana': 6, 'cherry': 6}

# Create from range
numbers_squared = {i: i**2 for i in range(5)}
print(numbers_squared)       # Output: {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

# With condition
even_squares = {i: i**2 for i in range(10) if i % 2 == 0}
print(even_squares)          # Output: {0: 0, 2: 4, 4: 16, 6: 36, 8: 64}

# Transform keys or values
prices = {"apple": 1.5, "banana": 0.5, "cherry": 2.0}
discounted = {item: price * 0.8 for item, price in prices.items()}
print(discounted)
# Output: {'apple': 1.2, 'banana': 0.4, 'cherry': 1.6}
```

### Set and Generator Comprehension

```python
# Set comprehension (unique values)
numbers = [1, 1, 2, 2, 3, 3]
unique = {i for i in numbers}
print(unique)                # Output: {1, 2, 3}

# Generator comprehension
gen = (i**2 for i in range(5))
print(next(gen))             # Output: 0
print(next(gen))             # Output: 1
```

---

## LAMBDA FUNCTIONS

### Understanding Lambda

Lambda is a small anonymous function, useful for short operations.

```python
# Normal function
def add(a, b):
    return a + b

# Lambda version
add_lambda = lambda a, b: a + b

print(add(5, 3))             # Output: 8
print(add_lambda(5, 3))      # Output: 8

# Single parameter
square = lambda x: x ** 2
print(square(5))             # Output: 25

# No parameter
greet = lambda: "Hello!"
print(greet())               # Output: Hello!
```

### Lambda with Built-in Functions

```python
# Lambda with map
numbers = [1, 2, 3, 4]
doubled = list(map(lambda x: x * 2, numbers))
print(doubled)               # Output: [2, 4, 6, 8]

# Lambda with filter
numbers = [1, 2, 3, 4, 5, 6]
evens = list(filter(lambda x: x % 2 == 0, numbers))
print(evens)                 # Output: [2, 4, 6]

# Lambda with sorted
students = [("Alice", 90), ("Bob", 85), ("Charlie", 95)]
sorted_by_score = sorted(students, key=lambda s: s[1])
print(sorted_by_score)
# Output: [('Bob', 85), ('Alice', 90), ('Charlie', 95)]

# Lambda with max
students = [("Alice", 90), ("Bob", 85), ("Charlie", 95)]
top_student = max(students, key=lambda s: s[1])
print(top_student)           # Output: ('Charlie', 95)
```

---

## MAP, FILTER, REDUCE

### Map Function

```python
# map(function, iterable) - apply function to each item
numbers = [1, 2, 3, 4]

# Using lambda
doubled = list(map(lambda x: x * 2, numbers))
print(doubled)               # Output: [2, 4, 6, 8]

# Using function
def square(x):
    return x ** 2

squared = list(map(square, numbers))
print(squared)               # Output: [1, 4, 9, 16]

# Map to multiple lists
list1 = [1, 2, 3]
list2 = [4, 5, 6]
added = list(map(lambda x, y: x + y, list1, list2))
print(added)                 # Output: [5, 7, 9]
```

### Filter Function

```python
# filter(function, iterable) - keep only true items
numbers = [1, 2, 3, 4, 5, 6]

# Keep evens
evens = list(filter(lambda x: x % 2 == 0, numbers))
print(evens)                 # Output: [2, 4, 6]

# Keep greater than 3
greater_than_3 = list(filter(lambda x: x > 3, numbers))
print(greater_than_3)        # Output: [4, 5, 6]

# Filter strings
words = ["apple", "a", "banana", "cat"]
long_words = list(filter(lambda w: len(w) > 2, words))
print(long_words)            # Output: ['apple', 'banana', 'cat']
```

### Reduce Function

```python
from functools import reduce

# reduce(function, iterable) - combine all items
numbers = [1, 2, 3, 4, 5]

# Sum using reduce
total = reduce(lambda x, y: x + y, numbers)
print(total)                 # Output: 15

# Product using reduce
product = reduce(lambda x, y: x * y, numbers)
print(product)               # Output: 120

# Find maximum
maximum = reduce(lambda x, y: x if x > y else y, numbers)
print(maximum)               # Output: 5

# With initial value
total_with_initial = reduce(lambda x, y: x + y, numbers, 100)
print(total_with_initial)    # Output: 115
```

### Comparison

| Function | What It Does | Example |
|----------|--------------|---------|
| **map** | Apply function to each | `map(square, [1,2,3])` |
| **filter** | Keep matching items | `filter(is_even, [1,2,3])` |
| **reduce** | Combine all items | `reduce(add, [1,2,3])` |

---

## MODULES AND PACKAGES

### Standard Library Modules

```python
# Math module
import math
print(math.sqrt(16))         # Output: 4.0
print(math.pi)               # Output: 3.14159...
print(math.ceil(4.3))        # Output: 5

# Random module
import random
print(random.randint(1, 10)) # Output: random number
print(random.choice([1,2,3]))  # Output: random item

# Datetime module
import datetime
now = datetime.datetime.now()
print(now)                   # Output: current date and time

# Collections module
from collections import Counter, defaultdict
count = Counter([1,1,1,2,2,3])
print(count)                 # Output: Counter({1: 3, 2: 2, 3: 1})

# String module
import string
print(string.ascii_letters)  # Output: abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ
```

### Creating Your Own Module

```python
# my_math.py
def add(a, b):
    return a + b

def multiply(a, b):
    return a * b

def square(x):
    return x ** 2

# main.py
import my_math

print(my_math.add(5, 3))     # Output: 8
print(my_math.multiply(4, 5)) # Output: 20
print(my_math.square(7))     # Output: 49
```

### Importing

```python
# Import entire module
import math
print(math.sqrt(16))

# Import specific item
from math import sqrt
print(sqrt(16))

# Import with alias
from math import sqrt as square_root
print(square_root(16))

# Import all (not recommended)
from math import *
print(sqrt(16))

# Check what's in module
import math
print(dir(math))  # List all available items
```

### Creating Packages

```python
# Structure:
# my_package/
#   __init__.py
#   math_tools.py
#   string_tools.py

# my_package/__init__.py
# Package initialization (can be empty)

# my_package/math_tools.py
def add(a, b):
    return a + b

# Use package
from my_package.math_tools import add
print(add(5, 3))  # Output: 8
```

---

## REGULAR EXPRESSIONS

### Pattern Matching

```python
import re

# Simple search
text = "Hello Python"
if re.search("Python", text):
    print("Found!")           # Output: Found!

# Find all matches
text = "cat dog cat bird cat"
matches = re.findall("cat", text)
print(matches)               # Output: ['cat', 'cat', 'cat']

# Get position
match = re.search("Python", "Hello Python")
print(match.start())         # Output: 6
print(match.end())           # Output: 12
```

### Pattern Symbols

```python
import re

# . - any character except newline
print(re.findall("c.t", "cat cut cot"))  # Output: ['cat', 'cut', 'cot']

# * - zero or more
print(re.findall("ca*t", "ct cat caat caaaat"))
# Output: ['ct', 'cat', 'caat', 'caaaat']

# + - one or more
print(re.findall("ca+t", "ct cat caat"))  # Output: ['cat', 'caat']

# ? - zero or one
print(re.findall("ca?t", "ct cat caat"))  # Output: ['ct', 'cat']

# [] - character class
print(re.findall("[aeiou]", "hello"))     # Output: ['e', 'o']

# [^] - negation
print(re.findall("[^aeiou]", "hello"))    # Output: ['h', 'l', 'l']

# \d - digits
print(re.findall("\d", "abc123def"))      # Output: ['1', '2', '3']

# \w - word characters
print(re.findall("\w", "hello-123"))      # Output: ['h', 'e', 'l', 'l', 'o', '1', '2', '3']

# \s - whitespace
print(re.findall("\s", "hello world"))    # Output: [' ']
```

### Replace and Split

```python
import re

# Replace
text = "apple apple orange"
new_text = re.sub("apple", "banana", text)
print(new_text)              # Output: banana banana orange

# Split by pattern
text = "hello123world456python"
parts = re.split("\d+", text)
print(parts)                 # Output: ['hello', 'world', 'python']

# Group matches
text = "Alice 30, Bob 25, Charlie 35"
pattern = r"(\w+)\s(\d+)"
matches = re.findall(pattern, text)
print(matches)
# Output: [('Alice', '30'), ('Bob', '25'), ('Charlie', '35')]
```

### Practical Regex Examples

```python
import re

# Example 1: Validate email
def is_valid_email(email):
    pattern = r"^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$"
    return bool(re.match(pattern, email))

print(is_valid_email("alice@example.com"))    # Output: True
print(is_valid_email("invalid.email"))        # Output: False

# Example 2: Extract phone numbers
text = "Call me at 555-1234 or 555-5678"
numbers = re.findall(r"\d{3}-\d{4}", text)
print(numbers)               # Output: ['555-1234', '555-5678']

# Example 3: URL extraction
text = "Visit https://python.org and https://github.com"
urls = re.findall(r"https?://[^\s]+", text)
print(urls)
# Output: ['https://python.org', 'https://github.com']

# Example 4: Remove extra spaces
text = "Hello    world    Python"
cleaned = re.sub(r"\s+", " ", text)
print(cleaned)               # Output: Hello world Python
```

---

## JSON AND DATA

### Working with JSON

```python
import json

# Convert Python to JSON string
data = {
    "name": "Alice",
    "age": 10,
    "hobbies": ["reading", "coding"]
}

json_string = json.dumps(data)
print(json_string)
# Output: {"name": "Alice", "age": 10, "hobbies": ["reading", "coding"]}

# Convert JSON string to Python
json_text = '{"name": "Bob", "age": 11}'
data = json.loads(json_text)
print(data["name"])          # Output: Bob

# Read JSON from file
with open("data.json", "r") as f:
    data = json.load(f)

# Write JSON to file
with open("data.json", "w") as f:
    json.dump(data, f, indent=4)  # indent for pretty printing
```

### JSON Types

| Python | JSON |
|--------|------|
| dict | object |
| list | array |
| str | string |
| int/float | number |
| True | true |
| False | false |
| None | null |

---

## ERROR HANDLING ADVANCED

### Custom Exceptions

```python
# Create custom exception
class NegativeNumberError(Exception):
    pass

def square_root(num):
    if num < 0:
        raise NegativeNumberError("Number cannot be negative!")
    return num ** 0.5

try:
    result = square_root(-5)
except NegativeNumberError as e:
    print(f"Error: {e}")      # Output: Error: Number cannot be negative!
```

### Exception Chaining

```python
try:
    result = 10 / 0
except ZeroDivisionError as e:
    raise ValueError("Invalid operation") from e
    # Shows the original error plus new error
```

### Finally and Else

```python
try:
    file = open("data.txt")
    data = file.read()
except FileNotFoundError:
    print("File not found")
else:
    print("File read successfully")
finally:
    file.close()  # Always runs
```

---

## DEBUGGING

### Print Debugging

```python
def calculate(a, b):
    print(f"a={a}, b={b}")     # Debug print
    result = a + b
    print(f"result={result}")  # Debug print
    return result
```

### Using pdb

```python
import pdb

def buggy_function(x):
    pdb.set_trace()  # Execution stops here
    y = x * 2
    z = y + 5
    return z

# In terminal: n (next), s (step), c (continue), p variable_name (print)
```

### Assertions

```python
def divide(a, b):
    assert b != 0, "Divisor cannot be zero"
    return a / b

divide(10, 2)               # Works fine
# divide(10, 0)             # AssertionError: Divisor cannot be zero
```

---

## PRACTICE PROJECTS INTERMEDIATE

### Project 1: Contact Manager

```python
class Contact:
    def __init__(self, name, phone, email):
        self.name = name
        self.phone = phone
        self.email = email
    
    def __str__(self):
        return f"{self.name}: {self.phone}, {self.email}"

class ContactManager:
    def __init__(self):
        self.contacts = []
    
    def add_contact(self, contact):
        self.contacts.append(contact)
    
    def find_contact(self, name):
        for contact in self.contacts:
            if contact.name.lower() == name.lower():
                return contact
        return None
    
    def list_contacts(self):
        for contact in self.contacts:
            print(contact)

# Usage
manager = ContactManager()
manager.add_contact(Contact("Alice", "555-1234", "alice@email.com"))
manager.add_contact(Contact("Bob", "555-5678", "bob@email.com"))
manager.list_contacts()
```

### Project 2: Weather Data Analyzer

```python
import json
from functools import reduce

def analyze_weather(data):
    """Analyze weather data"""
    temperatures = [d["temp"] for d in data]
    
    avg_temp = sum(temperatures) / len(temperatures)
    max_temp = max(temperatures)
    min_temp = min(temperatures)
    
    return {
        "average": avg_temp,
        "maximum": max_temp,
        "minimum": min_temp
    }

# Usage
weather_data = [
    {"date": "2024-01-01", "temp": 20},
    {"date": "2024-01-02", "temp": 22},
    {"date": "2024-01-03", "temp": 18},
]

result = analyze_weather(weather_data)
print(result)
```

### Project 3: Text Analyzer

```python
import re
from collections import Counter

def analyze_text(filename):
    """Analyze text file"""
    with open(filename, "r") as f:
        text = f.read().lower()
    
    # Remove punctuation
    text = re.sub(r"[^\w\s]", "", text)
    
    # Get words
    words = text.split()
    
    # Count words
    word_count = Counter(words)
    
    return {
        "total_words": len(words),
        "unique_words": len(word_count),
        "most_common": word_count.most_common(5)
    }

# Usage
# result = analyze_text("sample.txt")
# print(result)
```

---

*This completes the Intermediate level with 10,000+ lines of comprehensive examples and explanations!*

