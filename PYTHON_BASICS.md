# Python Basics - Complete Beginner's Guide
## Learn Python Like You're Teaching a Child

---

## Table of Contents
1. [Getting Started](#getting-started)
2. [Basic Syntax](#basic-syntax)
3. [Data Types](#data-types)
4. [Strings](#strings)
5. [Numbers](#numbers)
6. [Variables](#variables)
7. [Operators](#operators)
8. [Control Flow (if/else)](#control-flow)
9. [Loops](#loops)
10. [Lists](#lists)
11. [Tuples](#tuples)
12. [Dictionaries](#dictionaries)
13. [Sets](#sets)
14. [Functions](#functions)
15. [File Handling](#file-handling)
16. [Exception Handling](#exception-handling)
17. [Practice Projects](#practice-projects)

---

## GETTING STARTED

### What is Python?

Think of Python like a recipe for cooking:
- **Recipe** = Program
- **Chef** = Python
- **Ingredients** = Data
- **Instructions** = Code

```python
# This is a simple Python program
print("Hello, World!")
```

| Aspect | Explanation |
|--------|-------------|
| **Easy to Learn** | English-like syntax that's simple |
| **Popular** | Used by millions of developers |
| **Versatile** | Can build websites, games, AI, data analysis |
| **Free** | Download from python.org without paying |
| **Community** | Lots of helpful people online |

### Installing Python

| Step | What to Do |
|------|-----------|
| 1 | Go to python.org |
| 2 | Click "Download" |
| 3 | Run the installer |
| 4 | Check "Add Python to PATH" |
| 5 | Click Install |

### Running Your First Program

```python
# Save as hello.py
print("I am learning Python!")
```

Then run in terminal: `python hello.py`

**Result:** `I am learning Python!`

---

## BASIC SYNTAX

### Comments (Notes in Code)

Comments are like sticky notes - the computer ignores them, but they help humans understand the code.

```python
# This is a single-line comment
print("Hello")  # Comment after code

# Multi-line comment
# This is a multi-line comment
# It can span several lines
# Use it to explain complex code
print("World")

"""
This is a docstring (document string)
It's like a comment but can be used in functions
It uses three quotes on each side
"""
```

| Comment Type | Symbol | Use Case |
|--------------|--------|----------|
| Single-line | `#` | Quick notes |
| Multi-line | `#` multiple times | Explaining blocks |
| Docstring | `""""""` | Function documentation |

### Indentation (Very Important!)

Python uses indentation (spaces at the start of lines) to show which code belongs together.

```python
# Correct indentation
if 5 > 3:
    print("5 is bigger")  # This MUST be indented
    print("Math works")     # This MUST be indented
print("This line is not indented")  # This is outside the if

# Wrong indentation will give an error!
# if 5 > 3:
# print("This will cause an error")  # No indentation!
```

| Indentation Rule | Example |
|------------------|---------|
| **Inside blocks** | Use 4 spaces per level |
| **Consistency** | Use either spaces or tabs, not mixed |
| **Levels** | Nested blocks get more indentation |

---

## DATA TYPES

### What Are Data Types?

Data types are different kinds of information your computer can understand.

Think of it like:
- **Fruits**: Apple, Banana, Orange (different types)
- **Data Types**: Numbers, Text, True/False (different types)

```python
# Let's see different data types
age = 10                    # This is an integer (whole number)
height = 5.5                # This is a float (decimal number)
name = "Alice"              # This is a string (text)
is_happy = True             # This is a boolean (true or false)
nothing = None              # This is None (empty/nothing)
```

| Data Type | What It Is | Example | How to Check |
|-----------|-----------|---------|--------------|
| **int** | Whole numbers | `42`, `-5`, `0` | `type(42)` |
| **float** | Decimal numbers | `3.14`, `-2.5`, `0.0` | `type(3.14)` |
| **str** | Text (strings) | `"Hello"`, `'World'` | `type("Hello")` |
| **bool** | True or False | `True`, `False` | `type(True)` |
| **list** | Collection of items | `[1, 2, 3]` | `type([])` |
| **dict** | Key-value pairs | `{"name": "Bob"}` | `type({})` |
| **tuple** | Unchangeable list | `(1, 2, 3)` | `type(())` |
| **set** | Unique items | `{1, 2, 3}` | `type(set())` |

### Checking Data Types

```python
# Check what type something is
print(type(5))           # <class 'int'>
print(type(5.5))         # <class 'float'>
print(type("Hello"))     # <class 'str'>
print(type(True))        # <class 'bool'>
print(type([1, 2]))      # <class 'list'>
print(type({"a": 1}))    # <class 'dict'>
```

---

## STRINGS

### Understanding Strings

A string is text - like a message or a name.

```python
# Different ways to make strings
greeting1 = "Hello"           # Double quotes
greeting2 = 'Hello'           # Single quotes (same thing)
greeting3 = """Multi
line
string"""                      # Multi-line string
```

### String Basics

```python
# String length
name = "Python"
print(len(name))              # Output: 6

# String indexing (position starts at 0)
word = "Cat"
print(word[0])                # Output: C (first letter)
print(word[1])                # Output: a (second letter)
print(word[2])                # Output: t (third letter)
print(word[-1])               # Output: t (last letter)

# String slicing (getting pieces)
sentence = "Hello World"
print(sentence[0:5])          # Output: Hello (characters 0 to 4)
print(sentence[6:])           # Output: World (from character 6 onwards)
print(sentence[:5])           # Output: Hello (up to character 4)
```

| Operation | Example | Result |
|-----------|---------|--------|
| **Length** | `len("Python")` | `6` |
| **First char** | `"Cat"[0]` | `'C'` |
| **Last char** | `"Cat"[-1]` | `'t'` |
| **Slice** | `"Hello"[1:4]` | `'ell'` |
| **Uppercase** | `"hello".upper()` | `'HELLO'` |
| **Lowercase** | `"HELLO".lower()` | `'hello'` |

### String Methods (Things You Can Do)

```python
text = "  Hello World  "

# Remove spaces from sides
print(text.strip())           # Output: Hello World

# Replace part of string
message = "I like apples"
print(message.replace("apples", "oranges"))  # Output: I like oranges

# Split string into pieces
sentence = "The cat sat on mat"
words = sentence.split()      # Output: ['The', 'cat', 'sat', 'on', 'mat']

# Join pieces together
words = ["Hello", "Python", "World"]
result = " ".join(words)      # Output: Hello Python World

# Check if something is in string
text = "Python is awesome"
print("Python" in text)       # Output: True
print("Java" in text)         # Output: False

# Count occurrences
text = "banana"
print(text.count("a"))        # Output: 3

# Find position
text = "Hello"
print(text.find("l"))         # Output: 2 (first 'l' is at position 2)

# Check types
print("123".isdigit())        # Output: True
print("abc".isalpha())        # Output: True
print("abc123".isalnum())     # Output: True
```

### String Formatting (Putting Values Into Text)

```python
# Method 1: f-strings (easiest - Python 3.6+)
name = "Alice"
age = 10
print(f"My name is {name} and I'm {age} years old")
# Output: My name is Alice and I'm 10 years old

# Method 2: format()
print("I have {} apples and {} oranges".format(5, 3))
# Output: I have 5 apples and 3 oranges

# Method 3: Percentage (old way)
print("Hello %s, you are %d years old" % ("Bob", 12))
# Output: Hello Bob, you are 12 years old

# Format with spacing
number = 3.14159
print(f"Pi is approximately {number:.2f}")  # Limits to 2 decimal places
# Output: Pi is approximately 3.14
```

| Method | Example | Result |
|--------|---------|--------|
| **f-string** | `f"Hi {name}"` | `Hi Alice` |
| **.format()** | `"Hi {}".format("Bob")` | `Hi Bob` |
| **%** | `"Hi %s" % "Charlie"` | `Hi Charlie` |

---

## NUMBERS

### Integers (Whole Numbers)

```python
# Positive and negative numbers
age = 25
temperature = -10
score = 0

# Arithmetic with integers
print(5 + 3)                  # Output: 8 (addition)
print(10 - 4)                 # Output: 6 (subtraction)
print(3 * 4)                  # Output: 12 (multiplication)
print(12 / 3)                 # Output: 4.0 (division - returns float)
print(12 // 3)                # Output: 4 (floor division - returns int)
print(17 % 5)                 # Output: 2 (remainder/modulo)
print(2 ** 3)                 # Output: 8 (exponentiation/power)
```

### Floats (Decimal Numbers)

```python
# Floats with decimals
price = 19.99
temperature = 98.6
percentage = 0.75

# Operations with floats
print(5.5 + 2.3)              # Output: 7.8
print(10.0 - 3.5)             # Output: 6.5
print(2.5 * 4)                # Output: 10.0
print(10.0 / 3)               # Output: 3.3333...

# Rounding floats
pi = 3.14159
print(round(pi))              # Output: 3
print(round(pi, 2))           # Output: 3.14
```

### Math Operations

| Operation | Symbol | Example | Result |
|-----------|--------|---------|--------|
| **Addition** | `+` | `5 + 3` | `8` |
| **Subtraction** | `-` | `10 - 4` | `6` |
| **Multiplication** | `*` | `3 * 4` | `12` |
| **Division** | `/` | `12 / 3` | `4.0` |
| **Floor Division** | `//` | `17 // 5` | `3` |
| **Modulo** | `%` | `17 % 5` | `2` |
| **Power** | `**` | `2 ** 3` | `8` |

### The Math Module

```python
import math

# Square root
print(math.sqrt(16))          # Output: 4.0
print(math.sqrt(2))           # Output: 1.414...

# Powers
print(math.pow(2, 3))         # Output: 8.0

# Rounding
print(math.ceil(4.3))         # Output: 5 (round up)
print(math.floor(4.7))        # Output: 4 (round down)

# Constants
print(math.pi)                # Output: 3.14159...
print(math.e)                 # Output: 2.71828... (Euler's number)

# Absolute value
print(abs(-5))                # Output: 5
print(abs(5))                 # Output: 5
```

### Random Numbers

```python
import random

# Random integer between 1 and 10
number = random.randint(1, 10)
print(number)                 # Output: random number like 7

# Random float between 0 and 1
decimal = random.random()
print(decimal)                # Output: random like 0.823...

# Pick random item from list
colors = ["red", "blue", "green"]
chosen = random.choice(colors)
print(chosen)                 # Output: randomly picks one color
```

---

## VARIABLES

### What Are Variables?

Variables are like boxes that store information. You give each box a name so you can find it later.

```python
# Creating variables
name = "Alice"
age = 10
height = 4.5

# Using variables
print(name)                   # Output: Alice
print(age)                    # Output: 10

# Changing variables
age = 11                      # Now age is 11
print(age)                    # Output: 11
```

### Naming Variables

| ✅ Good Names | ❌ Bad Names | Why? |
|----------------|-------------|------|
| `student_name` | `n` | Clear vs confusing |
| `age` | `a` | Descriptive vs vague |
| `total_price` | `tp` | Easy to read vs hard |
| `is_happy` | `h` | Explains itself vs mysterious |
| `my_list` | `list1` | Descriptive vs generic |

### Variable Rules

```python
# Rules for naming variables:

# ✅ Correct
name = "Bob"
age_of_student = 10
_private = "hidden"
myVar = "camelCase works too"

# ❌ Wrong
# 2name = "Invalid"          # Can't start with number
# my-name = "Invalid"         # Can't use hyphens
# my name = "Invalid"         # Can't use spaces
# class = "Invalid"           # Can't use Python keywords
```

### Multiple Assignment

```python
# Assign multiple variables at once
x, y, z = 1, 2, 3
print(x)                      # Output: 1
print(y)                      # Output: 2
print(z)                      # Output: 3

# Swap variables
a = 5
b = 10
a, b = b, a                   # Now a=10, b=5
print(a, b)                   # Output: 10 5

# Same value to multiple variables
x = y = z = 0
print(x, y, z)                # Output: 0 0 0
```

### Global and Local Variables

```python
# Global variable (can be used anywhere)
total = 100

def show_total():
    print(total)              # Can access global variable
    
show_total()                  # Output: 100

# Local variable (only works inside function)
def calculate():
    result = 5 + 3            # result is local
    print(result)             # Output: 8

calculate()                   # Works
# print(result)               # ERROR - result doesn't exist outside function
```

---

## OPERATORS

### Comparison Operators (Comparing Things)

Comparison operators check if something is true or false.

```python
# Equal to
print(5 == 5)                 # Output: True
print(5 == 3)                 # Output: False

# Not equal to
print(5 != 3)                 # Output: True
print(5 != 5)                 # Output: False

# Greater than
print(10 > 5)                 # Output: True
print(3 > 10)                 # Output: False

# Less than
print(3 < 10)                 # Output: True
print(10 < 5)                 # Output: False

# Greater than or equal
print(10 >= 10)               # Output: True
print(9 >= 10)                # Output: False

# Less than or equal
print(5 <= 10)                # Output: True
print(15 <= 10)               # Output: False
```

| Operator | Meaning | Example | Result |
|----------|---------|---------|--------|
| `==` | Equal to | `5 == 5` | `True` |
| `!=` | Not equal to | `5 != 3` | `True` |
| `>` | Greater than | `10 > 5` | `True` |
| `<` | Less than | `3 < 10` | `True` |
| `>=` | Greater or equal | `10 >= 10` | `True` |
| `<=` | Less or equal | `5 <= 10` | `True` |

### Logical Operators (Combining Conditions)

```python
# AND - both must be true
print(True and True)          # Output: True
print(True and False)         # Output: False
print(False and False)        # Output: False

age = 15
money = 50
# Both conditions must be true
if age >= 13 and money >= 40:
    print("You can buy a game!")  # True because both are true

# OR - at least one must be true
print(True or False)          # Output: True
print(False or False)         # Output: False
print(True or True)           # Output: True

# You can have a pizza if it's Friday OR it's your birthday
is_friday = False
is_birthday = True
if is_friday or is_birthday:
    print("You can have pizza!")  # True because birthday is true

# NOT - opposite
print(not True)               # Output: False
print(not False)              # Output: True

is_raining = False
if not is_raining:
    print("Let's play outside!")  # True because it's NOT raining
```

| Operator | Meaning | Example | Result |
|----------|---------|---------|--------|
| `and` | Both true | `True and True` | `True` |
| `or` | At least one true | `True or False` | `True` |
| `not` | Opposite | `not False` | `True` |

### Assignment Operators (Giving Values)

```python
# Assign value
x = 5

# Add and assign
x += 3                        # Same as x = x + 3, now x = 8

# Subtract and assign
x -= 2                        # Same as x = x - 2, now x = 6

# Multiply and assign
x *= 2                        # Same as x = x * 2, now x = 12

# Divide and assign
x /= 3                        # Same as x = x / 3, now x = 4.0
```

| Operator | Example | Equivalent |
|----------|---------|------------|
| `+=` | `x += 5` | `x = x + 5` |
| `-=` | `x -= 3` | `x = x - 3` |
| `*=` | `x *= 2` | `x = x * 2` |
| `/=` | `x /= 2` | `x = x / 2` |
| `%=` | `x %= 2` | `x = x % 2` |
| `**=` | `x **= 2` | `x = x ** 2` |

---

## CONTROL FLOW

### If Statements (Making Decisions)

If statements let your program make decisions, like: "If it's raining, take an umbrella."

```python
# Simple if
age = 15
if age >= 13:
    print("You are a teenager!")
# Output: You are a teenager!

# If-else (do one thing or another)
score = 45
if score >= 50:
    print("You passed!")
else:
    print("You failed!")
# Output: You failed!

# If-elif-else (multiple choices)
score = 75
if score >= 90:
    print("Grade: A")
elif score >= 80:
    print("Grade: B")
elif score >= 70:
    print("Grade: C")
else:
    print("Grade: F")
# Output: Grade: C
```

### Real-World If Examples

```python
# Example 1: Checking if you can ride a roller coaster
height = 130  # in cm
if height >= 140:
    print("You can ride!")
else:
    print("You're too short. Grow more!")
# Output: You're too short. Grow more!

# Example 2: Checking temperature
temperature = 25
if temperature > 30:
    print("It's hot! Drink water!")
elif temperature > 15:
    print("It's nice weather!")
else:
    print("It's cold! Wear a jacket!")
# Output: It's nice weather!

# Example 3: Checking multiple conditions
age = 16
has_license = True
if age >= 16 and has_license:
    print("You can drive!")
else:
    print("Not yet!")
# Output: You can drive!

# Example 4: Checking if something is in a list
favorite_colors = ["red", "blue", "green"]
my_color = "red"
if my_color in favorite_colors:
    print("That's one of my favorites!")
else:
    print("I don't like that color")
# Output: That's one of my favorites!
```

### Nested If Statements (If inside If)

```python
age = 20
has_money = True

if age >= 18:
    print("You're an adult")
    if has_money:
        print("You can buy a car!")
    else:
        print("Save your money first")
else:
    print("You're not an adult yet")

# Output:
# You're an adult
# You can buy a car!
```

### Ternary Operator (Short If-Else)

```python
# Normal if-else
age = 10
if age >= 13:
    status = "teenager"
else:
    status = "child"

# Same thing in one line (ternary)
age = 10
status = "teenager" if age >= 13 else "child"
print(status)                 # Output: child

# Another example
score = 85
result = "Pass" if score >= 50 else "Fail"
print(result)                 # Output: Pass
```

---

## LOOPS

### While Loops (Repeat While Condition Is True)

A while loop keeps doing something until a condition becomes false.

```python
# Simple while loop
count = 1
while count <= 5:
    print(count)
    count += 1

# Output:
# 1
# 2
# 3
# 4
# 5

# Example: Counting down
number = 5
while number > 0:
    print(number)
    number -= 1
print("Blast off!")

# Output:
# 5
# 4
# 3
# 2
# 1
# Blast off!
```

### For Loops (Repeat A Set Number Of Times)

```python
# Loop through numbers
for i in range(5):           # i goes from 0 to 4
    print(i)

# Output: 0 1 2 3 4

# Loop with start and end
for i in range(1, 6):        # Start at 1, end at 5
    print(i)

# Output: 1 2 3 4 5

# Loop with step
for i in range(0, 10, 2):    # Start at 0, end at 10, step by 2
    print(i)

# Output: 0 2 4 6 8

# Loop through a string
word = "Python"
for letter in word:
    print(letter)

# Output: P y t h o n

# Loop through a list
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)

# Output:
# apple
# banana
# cherry
```

### Break and Continue

```python
# BREAK - exit the loop early
for i in range(10):
    if i == 5:
        break             # Stop the loop
    print(i)

# Output: 0 1 2 3 4

# CONTINUE - skip to next iteration
for i in range(5):
    if i == 2:
        continue          # Skip this iteration
    print(i)

# Output: 0 1 3 4

# Real example: Find a number
numbers = [1, 3, 5, 7, 9, 11]
for num in numbers:
    if num == 7:
        print("Found 7!")
        break
    print(num)

# Output:
# 1
# 3
# 5
# Found 7!
```

### Nested Loops (Loop Inside Loop)

```python
# Multiplication table
for i in range(1, 4):
    for j in range(1, 4):
        print(f"{i} x {j} = {i*j}")

# Output:
# 1 x 1 = 1
# 1 x 2 = 2
# 1 x 3 = 3
# 2 x 1 = 2
# 2 x 2 = 4
# 2 x 3 = 6
# 3 x 1 = 3
# 3 x 2 = 6
# 3 x 3 = 9

# Create a pattern
for i in range(1, 4):
    for j in range(i):
        print("*", end="")
    print()

# Output:
# *
# **
# ***
```

| Loop Type | When to Use | Example |
|-----------|------------|---------|
| **while** | Unknown number of times | `while score < 100:` |
| **for** | Known number of times | `for i in range(5):` |
| **for-in** | Loop through items | `for fruit in fruits:` |

---

## LISTS

### Understanding Lists

A list is like a shopping list - it holds multiple items in order.

```python
# Create a list
fruits = ["apple", "banana", "cherry"]
numbers = [1, 2, 3, 4, 5]
mixed = [1, "hello", 3.14, True]
empty_list = []

# Lists use square brackets
print(fruits)                 # Output: ['apple', 'banana', 'cherry']
```

### Accessing List Items

```python
fruits = ["apple", "banana", "cherry", "date"]

# By position (starts at 0)
print(fruits[0])              # Output: apple (first item)
print(fruits[1])              # Output: banana (second item)
print(fruits[-1])             # Output: date (last item)
print(fruits[-2])             # Output: cherry (second to last)

# Get multiple items (slicing)
print(fruits[0:2])            # Output: ['apple', 'banana']
print(fruits[1:])             # Output: ['banana', 'cherry', 'date']
print(fruits[:2])             # Output: ['apple', 'banana']
print(fruits[::2])            # Output: ['apple', 'cherry'] (every 2nd)

# List length
print(len(fruits))            # Output: 4
```

| Position | Item | Negative Index |
|----------|------|----------------|
| 0 | apple | -4 |
| 1 | banana | -3 |
| 2 | cherry | -2 |
| 3 | date | -1 |

### Changing Lists

```python
fruits = ["apple", "banana", "cherry"]

# Change an item
fruits[0] = "orange"
print(fruits)                 # Output: ['orange', 'banana', 'cherry']

# Add to end
fruits.append("date")
print(fruits)                 # Output: ['orange', 'banana', 'cherry', 'date']

# Add at specific position
fruits.insert(1, "mango")
print(fruits)                 # Output: ['orange', 'mango', 'banana', 'cherry', 'date']

# Remove item
fruits.remove("banana")
print(fruits)                 # Output: ['orange', 'mango', 'cherry', 'date']

# Remove by position
removed = fruits.pop(0)
print(removed)                # Output: orange
print(fruits)                 # Output: ['mango', 'cherry', 'date']

# Clear entire list
fruits.clear()
print(fruits)                 # Output: []

# Add multiple items
items = [1, 2, 3]
items.extend([4, 5, 6])
print(items)                  # Output: [1, 2, 3, 4, 5, 6]
```

### List Methods

```python
numbers = [3, 1, 4, 1, 5, 9]

# Count occurrences
print(numbers.count(1))       # Output: 2

# Find position
print(numbers.index(4))       # Output: 2

# Sort
numbers.sort()
print(numbers)                # Output: [1, 1, 3, 4, 5, 9]

# Reverse
numbers.reverse()
print(numbers)                # Output: [9, 5, 4, 3, 1, 1]

# Copy a list
copy_numbers = numbers.copy()
print(copy_numbers)           # Output: [9, 5, 4, 3, 1, 1]
```

### List Comprehension (Short Way to Create Lists)

```python
# Normal way
squares = []
for i in range(5):
    squares.append(i ** 2)
print(squares)                # Output: [0, 1, 4, 9, 16]

# List comprehension (shorter)
squares = [i ** 2 for i in range(5)]
print(squares)                # Output: [0, 1, 4, 9, 16]

# With condition
even_numbers = [i for i in range(10) if i % 2 == 0]
print(even_numbers)           # Output: [0, 2, 4, 6, 8]

# Transform items
words = ["hello", "world"]
upper_words = [w.upper() for w in words]
print(upper_words)            # Output: ['HELLO', 'WORLD']
```

### Looping Through Lists

```python
fruits = ["apple", "banana", "cherry"]

# Method 1: Just items
for fruit in fruits:
    print(fruit)

# Method 2: With index
for i in range(len(fruits)):
    print(f"Position {i}: {fruits[i]}")

# Method 3: enumerate (position + item)
for index, fruit in enumerate(fruits):
    print(f"Position {index}: {fruit}")

# Output:
# Position 0: apple
# Position 1: banana
# Position 2: cherry
```

| Operation | Code | Result |
|-----------|------|--------|
| **Create** | `[1, 2, 3]` | List created |
| **Access** | `list[0]` | First item |
| **Add** | `list.append(4)` | Add to end |
| **Remove** | `list.remove(1)` | Remove item |
| **Sort** | `list.sort()` | Order items |
| **Length** | `len(list)` | Number of items |

---

## TUPLES

### Understanding Tuples

A tuple is like a list, but you can't change it. Think of it as an unchangeable box.

```python
# Create a tuple
colors = ("red", "green", "blue")
numbers = (1, 2, 3)
single = (42,)                # Tuple with one item needs a comma
empty_tuple = ()              # Empty tuple

print(colors)                 # Output: ('red', 'green', 'blue')
```

### Tuple Features

```python
colors = ("red", "green", "blue")

# Access items (like lists)
print(colors[0])              # Output: red
print(colors[-1])             # Output: blue
print(colors[0:2])            # Output: ('red', 'green')

# Length
print(len(colors))            # Output: 3

# Count occurrences
numbers = (1, 2, 2, 3)
print(numbers.count(2))       # Output: 2

# Find index
print(colors.index("green"))  # Output: 1

# Tuples are immutable (can't change)
# colors[0] = "yellow"        # ERROR - can't change tuple
```

### Why Use Tuples?

| Reason | Explanation | Example |
|--------|-------------|---------|
| **Immutable** | Can't be changed by accident | Storing constants |
| **Faster** | Slightly faster than lists | Used in loops |
| **Dictionary Keys** | Can be used as keys | Unique identifiers |
| **Safety** | Prevents accidental changes | Function returns |

### Unpacking Tuples

```python
# Unpacking - split tuple into variables
colors = ("red", "green", "blue")
color1, color2, color3 = colors
print(color1)                 # Output: red
print(color2)                 # Output: green
print(color3)                 # Output: blue

# With functions (returning multiple values)
def get_person():
    return ("Alice", 10, "NYC")

name, age, city = get_person()
print(f"{name} is {age} and lives in {city}")
# Output: Alice is 10 and lives in NYC
```

---

## DICTIONARIES

### Understanding Dictionaries

A dictionary stores pairs of information - like a phonebook where you look up a name to get a phone number.

```python
# Create a dictionary
person = {
    "name": "Alice",
    "age": 10,
    "city": "New York"
}

print(person)
# Output: {'name': 'Alice', 'age': 10, 'city': 'New York'}
```

### Accessing Dictionary Items

```python
person = {"name": "Alice", "age": 10, "city": "New York"}

# Get value by key
print(person["name"])         # Output: Alice
print(person["age"])          # Output: 10

# Safe way to get (returns None if not found)
print(person.get("name"))     # Output: Alice
print(person.get("job"))      # Output: None
print(person.get("job", "Unknown"))  # Output: Unknown

# Check if key exists
print("name" in person)       # Output: True
print("job" in person)        # Output: False
```

### Changing Dictionaries

```python
person = {"name": "Alice", "age": 10}

# Change value
person["age"] = 11
print(person)                 # Output: {'name': 'Alice', 'age': 11}

# Add new key-value pair
person["city"] = "New York"
print(person)                 # Output: {'name': 'Alice', 'age': 11, 'city': 'New York'}

# Remove item
del person["city"]
print(person)                 # Output: {'name': 'Alice', 'age': 11}

# Remove and get the value
age = person.pop("age")
print(age)                    # Output: 11
print(person)                 # Output: {'name': 'Alice'}

# Clear dictionary
person.clear()
print(person)                 # Output: {}
```

### Dictionary Methods

```python
person = {"name": "Bob", "age": 12, "city": "Boston"}

# Get all keys
print(person.keys())          # Output: dict_keys(['name', 'age', 'city'])

# Get all values
print(person.values())        # Output: dict_values(['Bob', 12, 'Boston'])

# Get all key-value pairs
print(person.items())         # Output: dict_items([('name', 'Bob'), ('age', 12), ('city', 'Boston')])

# Get key with maximum value
scores = {"Alice": 95, "Bob": 87, "Charlie": 92}
top_scorer = max(scores)
print(top_scorer)             # Output: Alice (highest score)

# Update from another dictionary
person.update({"age": 13, "job": "Student"})
print(person)                 # Output: {'name': 'Bob', 'age': 13, 'city': 'Boston', 'job': 'Student'}
```

### Looping Through Dictionaries

```python
person = {"name": "Alice", "age": 10, "city": "NYC"}

# Loop through keys
for key in person:
    print(key)
# Output: name, age, city

# Loop through values
for value in person.values():
    print(value)
# Output: Alice, 10, NYC

# Loop through items
for key, value in person.items():
    print(f"{key}: {value}")
# Output:
# name: Alice
# age: 10
# city: NYC
```

### Nested Dictionaries

```python
# Dictionary inside dictionary
students = {
    "student1": {
        "name": "Alice",
        "age": 10,
        "grades": [90, 85, 92]
    },
    "student2": {
        "name": "Bob",
        "age": 11,
        "grades": [88, 91, 87]
    }
}

# Access nested values
print(students["student1"]["name"])  # Output: Alice
print(students["student1"]["grades"])  # Output: [90, 85, 92]
print(students["student1"]["grades"][0])  # Output: 90

# Loop through nested dictionary
for student_key, student_info in students.items():
    print(f"{student_info['name']} is {student_info['age']} years old")
# Output:
# Alice is 10 years old
# Bob is 11 years old
```

| Operation | Code | Result |
|-----------|------|--------|
| **Create** | `{"key": "value"}` | Dictionary created |
| **Access** | `dict["key"]` | Get value |
| **Add** | `dict["key"] = "value"` | Add item |
| **Remove** | `del dict["key"]` | Remove item |
| **Keys** | `dict.keys()` | All keys |
| **Values** | `dict.values()` | All values |

---

## SETS

### Understanding Sets

A set is a collection of unique items (no duplicates), like a bag of different colored balls.

```python
# Create a set
colors = {"red", "green", "blue"}
numbers = {1, 2, 3}
empty_set = set()             # Can't use {} - that's a dict

print(colors)                 # Output: {'red', 'green', 'blue'}
print(type(empty_set))        # Output: <class 'set'>
```

### Set Operations

```python
# Create sets
fruits1 = {"apple", "banana", "cherry"}
fruits2 = {"banana", "date", "elderberry"}

# Union (all items from both)
all_fruits = fruits1 | fruits2
print(all_fruits)
# Output: {'apple', 'banana', 'cherry', 'date', 'elderberry'}

# Intersection (items in both)
common = fruits1 & fruits2
print(common)                 # Output: {'banana'}

# Difference (items in first but not second)
unique = fruits1 - fruits2
print(unique)                 # Output: {'apple', 'cherry'}

# Symmetric difference (items in either but not both)
different = fruits1 ^ fruits2
print(different)              # Output: {'apple', 'cherry', 'date', 'elderberry'}
```

### Set Methods

```python
colors = {"red", "green"}

# Add item
colors.add("blue")
print(colors)                 # Output: {'red', 'green', 'blue'}

# Add won't add duplicates
colors.add("red")
print(colors)                 # Output: {'red', 'green', 'blue'} (no change)

# Remove item (error if not exists)
colors.remove("green")
print(colors)                 # Output: {'red', 'blue'}

# Discard (no error if not exists)
colors.discard("yellow")      # No error
print(colors)                 # Output: {'red', 'blue'}

# Pop (remove random item)
item = colors.pop()
print(item)                   # Output: 'red' (or 'blue')

# Check membership
print("red" in colors)        # Output: True or False

# Length
numbers = {1, 2, 3, 4}
print(len(numbers))           # Output: 4
```

### Set for Removing Duplicates

```python
# Remove duplicates from list
numbers = [1, 2, 2, 3, 3, 3, 4, 5, 5]
unique_numbers = list(set(numbers))
print(unique_numbers)         # Output: [1, 2, 3, 4, 5] (order may vary)

# Useful for checking unique values
words = ["apple", "apple", "banana", "cherry", "banana"]
unique_words = set(words)
print(unique_words)           # Output: {'apple', 'banana', 'cherry'}
print(len(unique_words))      # Output: 3
```

| Set Operation | Symbol | Example | Result |
|---------------|--------|---------|--------|
| **Union** | `\|` | `{1,2} \| {2,3}` | `{1, 2, 3}` |
| **Intersection** | `&` | `{1,2} & {2,3}` | `{2}` |
| **Difference** | `-` | `{1,2} - {2,3}` | `{1}` |
| **Symmetric Diff** | `^` | `{1,2} ^ {2,3}` | `{1, 3}` |

---

## FUNCTIONS

### Understanding Functions

A function is like a recipe - you give it ingredients (inputs) and it produces a dish (output).

```python
# Define a function
def greet():
    print("Hello!")

# Call (use) the function
greet()                       # Output: Hello!
greet()                       # Output: Hello! (can call multiple times)
```

### Functions with Parameters

```python
# Function with one parameter
def greet(name):
    print(f"Hello, {name}!")

greet("Alice")                # Output: Hello, Alice!
greet("Bob")                  # Output: Hello, Bob!

# Function with multiple parameters
def add(a, b):
    print(f"{a} + {b} = {a + b}")

add(5, 3)                     # Output: 5 + 3 = 8
add(10, 20)                   # Output: 10 + 20 = 30

# Function with default parameters
def greet(name, greeting="Hello"):
    print(f"{greeting}, {name}!")

greet("Alice")                # Output: Hello, Alice!
greet("Bob", "Hi")            # Output: Hi, Bob!
```

### Functions with Return Values

```python
# Function that returns a value
def add(a, b):
    return a + b

result = add(5, 3)
print(result)                 # Output: 8

# Function that returns multiple values
def get_person():
    return "Alice", 10, "NYC"

name, age, city = get_person()
print(name)                   # Output: Alice

# Function with conditional returns
def is_adult(age):
    if age >= 18:
        return True
    else:
        return False

print(is_adult(20))           # Output: True
print(is_adult(15))           # Output: False

# Shorter version
def is_adult(age):
    return age >= 18

print(is_adult(20))           # Output: True
```

### Variable-Length Arguments

```python
# *args - variable number of arguments
def add_all(*numbers):
    total = 0
    for num in numbers:
        total += num
    return total

print(add_all(1, 2, 3))       # Output: 6
print(add_all(1, 2, 3, 4, 5)) # Output: 15

# **kwargs - keyword arguments
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

my_function(1, 2, 3, 4, name="Alice", city="NYC")
# Output:
# a=1, b=2
# args=(3, 4)
# kwargs={'name': 'Alice', 'city': 'NYC'}
```

### Scope

```python
x = "global"                  # Global variable

def my_function():
    x = "local"               # Local variable (only in function)
    print(x)                  # Output: local

print(x)                      # Output: global (outside function)
my_function()                 # Output: local (inside function)
print(x)                      # Output: global (still global)

# Accessing global from inside function
counter = 0

def increment():
    global counter            # Use global keyword
    counter += 1
    print(counter)

increment()                   # Output: 1
increment()                   # Output: 2
```

### Real-World Function Examples

```python
# Example 1: Calculate grade
def get_grade(score):
    if score >= 90:
        return "A"
    elif score >= 80:
        return "B"
    elif score >= 70:
        return "C"
    else:
        return "F"

print(get_grade(85))          # Output: B

# Example 2: Check password strength
def is_strong_password(password):
    if len(password) >= 8:
        return True
    else:
        return False

print(is_strong_password("mypass123"))  # Output: True

# Example 3: Calculate average
def calculate_average(*scores):
    if len(scores) == 0:
        return 0
    total = sum(scores)
    average = total / len(scores)
    return average

print(calculate_average(90, 85, 88, 92))  # Output: 88.75

# Example 4: Process user data
def process_user(name, age, email="unknown"):
    return {
        "name": name,
        "age": age,
        "email": email,
        "is_adult": age >= 18
    }

print(process_user("Alice", 10))
# Output: {'name': 'Alice', 'age': 10, 'email': 'unknown', 'is_adult': False}
```

| Function Part | Example | Explanation |
|---------------|---------|-------------|
| **Definition** | `def add(a, b):` | Creates the function |
| **Parameters** | `a, b` | Inputs to the function |
| **Body** | `return a + b` | What the function does |
| **Return** | `return sum` | Output from function |
| **Call** | `add(3, 5)` | Uses the function |

---

## FILE HANDLING

### Reading Files

```python
# Open and read entire file
with open("myfile.txt", "r") as file:
    content = file.read()
    print(content)

# Read line by line
with open("myfile.txt", "r") as file:
    for line in file:
        print(line.strip())  # strip() removes newline

# Read all lines as list
with open("myfile.txt", "r") as file:
    lines = file.readlines()
    print(lines)            # Output: ['line1\n', 'line2\n', ...]

# Read specific number of characters
with open("myfile.txt", "r") as file:
    first_100 = file.read(100)
    print(first_100)
```

### Writing to Files

```python
# Write to file (overwrites if exists)
with open("myfile.txt", "w") as file:
    file.write("Hello World")
    file.write("\nSecond line")

# Append to file (adds to end)
with open("myfile.txt", "a") as file:
    file.write("\nThird line")

# Write multiple lines
with open("myfile.txt", "w") as file:
    lines = ["Line 1\n", "Line 2\n", "Line 3\n"]
    file.writelines(lines)
```

### File Modes

| Mode | Meaning | What Happens |
|------|---------|--------------|
| `'r'` | Read | Opens file for reading (default) |
| `'w'` | Write | Creates new file or overwrites existing |
| `'a'` | Append | Adds to end of file |
| `'x'` | Create | Creates new file (error if exists) |
| `'b'` | Binary | For non-text files |

### The 'with' Statement

```python
# Wrong way (don't do this)
file = open("myfile.txt", "r")
content = file.read()
file.close()  # Easy to forget!

# Right way (always do this)
with open("myfile.txt", "r") as file:
    content = file.read()
# File automatically closes here

# The 'with' statement ensures file is closed even if error happens
```

### Working with CSV Files

```python
import csv

# Reading CSV
with open("data.csv", "r") as file:
    reader = csv.reader(file)
    for row in reader:
        print(row)  # Each row is a list

# Writing CSV
with open("data.csv", "w", newline="") as file:
    writer = csv.writer(file)
    writer.writerow(["Name", "Age", "City"])
    writer.writerow(["Alice", 10, "NYC"])
    writer.writerow(["Bob", 11, "LA"])
```

### Real File Handling Examples

```python
# Example 1: Count words in file
def count_words(filename):
    with open(filename, "r") as file:
        content = file.read()
        words = content.split()
        return len(words)

print(count_words("myfile.txt"))  # Output: number of words

# Example 2: Search file for specific word
def search_file(filename, search_word):
    found = False
    with open(filename, "r") as file:
        for line in file:
            if search_word.lower() in line.lower():
                print(line.strip())
                found = True
    if not found:
        print("Word not found")

search_file("myfile.txt", "python")

# Example 3: Save list to file
def save_list(filename, items):
    with open(filename, "w") as file:
        for item in items:
            file.write(item + "\n")

save_list("shopping.txt", ["apple", "banana", "milk"])

# Example 4: Load list from file
def load_list(filename):
    items = []
    with open(filename, "r") as file:
        for line in file:
            items.append(line.strip())
    return items

shopping = load_list("shopping.txt")
print(shopping)  # Output: ['apple', 'banana', 'milk']
```

---

## EXCEPTION HANDLING

### Try-Except Blocks

Exception handling helps your program handle errors gracefully instead of crashing.

```python
# Simple try-except
try:
    number = int("hello")    # This will cause an error
    print(number)
except ValueError:
    print("That's not a valid number!")
# Output: That's not a valid number!

# Program continues after error
print("Program continues")
# Output: Program continues

# Without try-except, program would crash!
```

### Multiple Except Blocks

```python
try:
    number = int(input("Enter a number: "))
    result = 10 / number
    print(result)
except ValueError:
    print("That's not a valid number!")
except ZeroDivisionError:
    print("Can't divide by zero!")
```

### Common Exceptions

| Exception | When It Happens | Example |
|-----------|-----------------|---------|
| **ValueError** | Wrong value type | `int("hello")` |
| **TypeError** | Wrong data type | `"5" + 5` |
| **IndexError** | Index out of range | `list[100]` |
| **KeyError** | Key doesn't exist | `dict["missing"]` |
| **ZeroDivisionError** | Divide by zero | `5 / 0` |
| **FileNotFoundError** | File doesn't exist | `open("missing.txt")` |

### Try-Except-Else-Finally

```python
try:
    number = int(input("Enter a number: "))
except ValueError:
    print("Invalid input!")
else:
    print(f"You entered {number}")  # Only if no error
finally:
    print("Done!")  # Always runs, even if error

# Output (if valid):
# You entered 5
# Done!

# Output (if invalid):
# Invalid input!
# Done!
```

### Real Exception Handling Examples

```python
# Example 1: Safe file reading
def read_file_safe(filename):
    try:
        with open(filename, "r") as file:
            return file.read()
    except FileNotFoundError:
        print(f"File {filename} not found!")
        return None

content = read_file_safe("myfile.txt")

# Example 2: Safe user input
def get_valid_number():
    while True:
        try:
            number = int(input("Enter a number: "))
            return number
        except ValueError:
            print("Please enter a valid number!")

number = get_valid_number()
print(f"You entered: {number}")

# Example 3: Safe division
def safe_divide(a, b):
    try:
        return a / b
    except ZeroDivisionError:
        print("Cannot divide by zero!")
        return None

print(safe_divide(10, 2))   # Output: 5.0
print(safe_divide(10, 0))   # Output: Cannot divide by zero! / None

# Example 4: Multiple operations with error handling
def process_data(filename):
    try:
        with open(filename, "r") as file:
            lines = file.readlines()
        
        for line in lines:
            value = int(line.strip())
            print(f"Value: {value}")
    
    except FileNotFoundError:
        print("File not found!")
    except ValueError:
        print("Invalid number in file!")
    except Exception as e:
        print(f"Unexpected error: {e}")
    finally:
        print("Process complete!")

process_data("data.txt")
```

---

## PRACTICE PROJECTS

### Project 1: Calculator Program

```python
def calculator():
    """Simple calculator"""
    print("=== Simple Calculator ===")
    
    try:
        num1 = float(input("Enter first number: "))
        operator = input("Enter operator (+, -, *, /): ")
        num2 = float(input("Enter second number: "))
        
        if operator == "+":
            result = num1 + num2
        elif operator == "-":
            result = num1 - num2
        elif operator == "*":
            result = num1 * num2
        elif operator == "/":
            if num2 == 0:
                print("Cannot divide by zero!")
                return
            result = num1 / num2
        else:
            print("Invalid operator!")
            return
        
        print(f"{num1} {operator} {num2} = {result}")
    
    except ValueError:
        print("Invalid input!")

# Run it
calculator()
```

### Project 2: To-Do List

```python
def todo_list():
    """Simple to-do list"""
    tasks = []
    
    while True:
        print("\n=== To-Do List ===")
        print("1. Add task")
        print("2. View tasks")
        print("3. Remove task")
        print("4. Exit")
        
        choice = input("Choose (1-4): ")
        
        if choice == "1":
            task = input("Enter task: ")
            tasks.append(task)
            print(f"Task added: {task}")
        
        elif choice == "2":
            if not tasks:
                print("No tasks yet!")
            else:
                print("\nYour tasks:")
                for i, task in enumerate(tasks, 1):
                    print(f"{i}. {task}")
        
        elif choice == "3":
            if not tasks:
                print("No tasks to remove!")
            else:
                try:
                    index = int(input("Enter task number to remove: ")) - 1
                    removed = tasks.pop(index)
                    print(f"Removed: {removed}")
                except (ValueError, IndexError):
                    print("Invalid number!")
        
        elif choice == "4":
            print("Goodbye!")
            break
        
        else:
            print("Invalid choice!")

# Run it
todo_list()
```

### Project 3: Guess the Number

```python
import random

def guess_game():
    """Guess the number game"""
    print("=== Guess the Number ===")
    
    number = random.randint(1, 100)
    guesses = 0
    
    while True:
        try:
            guess = int(input("Guess a number (1-100): "))
            guesses += 1
            
            if guess < number:
                print("Too low! Try again.")
            elif guess > number:
                print("Too high! Try again.")
            else:
                print(f"You got it! The number was {number}")
                print(f"It took you {guesses} guesses")
                break
        
        except ValueError:
            print("Please enter a valid number!")

# Run it
guess_game()
```

### Project 4: Grade Calculator

```python
def grade_calculator():
    """Calculate student grades"""
    print("=== Grade Calculator ===")
    
    try:
        # Get grades
        grades = []
        while True:
            try:
                grade = float(input("Enter grade (0-100), or press Ctrl+C to stop: "))
                if 0 <= grade <= 100:
                    grades.append(grade)
                else:
                    print("Grade must be between 0 and 100!")
            except ValueError:
                print("Please enter a valid number!")
        
    except KeyboardInterrupt:
        print()  # New line
    
    if not grades:
        print("No grades entered!")
        return
    
    # Calculate
    average = sum(grades) / len(grades)
    highest = max(grades)
    lowest = min(grades)
    
    # Display results
    print(f"\nAverage: {average:.2f}")
    print(f"Highest: {highest}")
    print(f"Lowest: {lowest}")
    print(f"Total grades: {len(grades)}")

# Run it
grade_calculator()
```

### Project 5: Password Validator

```python
def validate_password(password):
    """Check if password is strong"""
    issues = []
    
    if len(password) < 8:
        issues.append("- Too short (at least 8 characters)")
    
    if not any(c.isupper() for c in password):
        issues.append("- No uppercase letters")
    
    if not any(c.islower() for c in password):
        issues.append("- No lowercase letters")
    
    if not any(c.isdigit() for c in password):
        issues.append("- No numbers")
    
    return issues

def password_checker():
    """Check password strength"""
    print("=== Password Checker ===")
    
    password = input("Enter password: ")
    issues = validate_password(password)
    
    if not issues:
        print("✓ Strong password!")
    else:
        print("✗ Weak password. Issues:")
        for issue in issues:
            print(issue)

# Run it
password_checker()
```

---

## Summary Table: Key Concepts

| Concept | What It Does | Example |
|---------|--------------|---------|
| **Variables** | Store data | `name = "Alice"` |
| **Data Types** | Different kinds of data | `int`, `str`, `list`, `dict` |
| **Operators** | Do operations | `+`, `-`, `==`, `and` |
| **If/Else** | Make decisions | `if x > 5:` |
| **Loops** | Repeat code | `for i in range(5):` |
| **Lists** | Store multiple items | `[1, 2, 3]` |
| **Dictionaries** | Store key-value pairs | `{"name": "Alice"}` |
| **Functions** | Reusable code | `def greet(name):` |
| **Exception Handling** | Handle errors | `try-except` |
| **File Handling** | Read/write files | `with open("file.txt")` |

---

## Beginner Checklist

- [ ] Understand variables and data types
- [ ] Master if/else statements
- [ ] Use for and while loops correctly
- [ ] Work with lists, tuples, and dictionaries
- [ ] Write functions with parameters and returns
- [ ] Handle exceptions with try-except
- [ ] Read and write files
- [ ] Complete all 5 practice projects
- [ ] Experiment with your own code examples
- [ ] Join a Python community for help

---

*This guide covers all the basics a beginner needs! Practice by typing out examples yourself, not just reading them. The more you code, the better you'll get!*

