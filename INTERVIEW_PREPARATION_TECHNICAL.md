# Technical Interview Preparation Guide
## For Python/Django Backend Developer Position | Awais Amjad

---

## Table of Contents
1. [Python Fundamentals](#python-fundamentals)
2. [Django & Django REST Framework](#django--django-rest-framework)
3. [Database Design & SQL](#database-design--sql)
4. [REST API Design](#rest-api-design)
5. [System Design & Architecture](#system-design--architecture)
6. [Authentication & Security](#authentication--security)
7. [Common Coding Problems](#common-coding-problems)
8. [Project-Specific Questions](#project-specific-questions)
9. [AWS & Cloud Deployment](#aws--cloud-deployment)
10. [Debugging & Troubleshooting](#debugging--troubleshooting)

---

## Python Fundamentals

### Q1: Explain the differences between Python 2 and Python 3

**Why they ask:** To assess your Python knowledge depth

**Short Answer:**
```
The main differences:

1. Print Statement vs Function
   - Python 2: print "Hello"
   - Python 3: print("Hello")

2. Unicode Support
   - Python 2: Strings are bytes by default, unicode() function needed
   - Python 3: Strings are Unicode by default

3. Division Operator
   - Python 2: 5/2 = 2 (integer division)
   - Python 3: 5/2 = 2.5 (float division), use // for integer division

4. Integer Types
   - Python 2: int and long types
   - Python 3: Only int type (arbitrary precision)

5. Exception Handling
   - Python 2: except Exception, e:
   - Python 3: except Exception as e:

6. Removed Functions
   - Python 2: raw_input(), xrange(), has
   - Python 3: input(), range(), removed has

We always use Python 3 in modern development because Python 2 is obsolete (support ended in 2020).
```

---

### Q2: What is the difference between a list and a tuple in Python?

**Why they ask:** Understanding data structures

**Answer:**
```python
# Lists are MUTABLE (can be changed)
my_list = [1, 2, 3]
my_list[0] = 10  # This works
my_list.append(4)  # This works

# Tuples are IMMUTABLE (cannot be changed)
my_tuple = (1, 2, 3)
my_tuple[0] = 10  # ERROR: 'tuple' object does not support item assignment
my_tuple.append(4)  # ERROR: 'tuple' object has no attribute 'append'

# Key Differences:
# 1. Mutability: Lists can be modified, tuples cannot
# 2. Performance: Tuples are faster and use less memory
# 3. Usage: Use tuples as dictionary keys; lists cannot be keys
# 4. Syntax: Lists use [], tuples use ()

# Practical use case:
coordinates = (40.7128, -74.0060)  # Immutable coordinates
student_grades = [85, 90, 92]  # Mutable grades that change

# Which to use?
# - Use lists when you need to modify data
# - Use tuples when data shouldn't change (safer, faster)
# - Use tuples as dictionary keys
```

---

### Q3: Explain List Comprehensions with an example

**Why they ask:** Pythonic code writing

**Answer:**
```python
# Traditional way
squares = []
for i in range(1, 6):
    squares.append(i ** 2)
# Result: [1, 4, 9, 16, 25]

# List comprehension (more Pythonic)
squares = [i ** 2 for i in range(1, 6)]
# Same result: [1, 4, 9, 16, 25]

# With conditions
even_squares = [i ** 2 for i in range(1, 11) if i % 2 == 0]
# Result: [4, 16, 36, 64, 100]

# Dictionary comprehension
squares_dict = {i: i**2 for i in range(1, 6)}
# Result: {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}

# Real-world example in Django API:
user_ids = [user.id for user in User.objects.all()]
active_users = [user for user in User.objects.all() if user.is_active]

# Benefits:
# 1. More concise and readable
# 2. Faster execution
# 3. More Pythonic
```

---

### Q4: What is the GIL (Global Interpreter Lock)?

**Why they ask:** Understanding Python's threading limitations

**Answer:**
```
The GIL is a mutex that protects access to Python objects in CPython.

WHAT IT DOES:
- Only one thread can execute Python bytecode at a time
- Even on multi-core processors, only one core can run Python code

WHY IT EXISTS:
- CPython manages memory using reference counting
- Without the GIL, every reference count change would require a lock
- The GIL simplifies memory management in CPython

IMPLICATIONS:

1. Multi-threading doesn't provide true parallelism in Python
   bad_example = [cpu_intensive_task() for _ in range(4)]  # Serial execution
   
2. I/O-bound operations still benefit from threading
   good_example = [make_api_request() for _ in range(4)]  # Can happen in parallel

SOLUTIONS:

1. Use multiprocessing for CPU-bound tasks
   from multiprocessing import Pool
   with Pool(4) as p:
       results = p.map(cpu_intensive_task, range(100))

2. Use async/await for I/O-bound tasks
   async def fetch_data():
       tasks = [fetch_url(url) for url in urls]
       return await asyncio.gather(*tasks)

3. Use alternative Python implementations (Jython, IronPython, PyPy)

IN DJANGO:
- Most Django apps are I/O-bound (database queries, API calls)
- Threading works fine for these tasks
- Use Celery + multiprocessing for CPU-intensive background jobs
```

---

### Q5: What is a decorator in Python? Give an example.

**Why they ask:** Advanced Python knowledge

**Answer:**
```python
# A decorator is a function that wraps another function or class

# Simple decorator example
def simple_decorator(func):
    def wrapper():
        print("Before function call")
        func()
        print("After function call")
    return wrapper

@simple_decorator
def say_hello():
    print("Hello!")

# Calling say_hello() will print:
# Before function call
# Hello!
# After function call

# Decorator with arguments
def timer_decorator(func):
    def wrapper(*args, **kwargs):
        import time
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"Execution time: {end - start} seconds")
        return result
    return wrapper

@timer_decorator
def slow_function():
    time.sleep(1)
    return "Done"

# Django example - Login required decorator
from django.contrib.auth.decorators import login_required

@login_required
def user_profile(request):
    return render(request, 'profile.html')

# This automatically redirects to login if user is not authenticated

# Real-world example - Performance monitoring
def log_execution(func):
    def wrapper(*args, **kwargs):
        logger.info(f"Calling {func.__name__}")
        try:
            result = func(*args, **kwargs)
            logger.info(f"{func.__name__} completed successfully")
            return result
        except Exception as e:
            logger.error(f"{func.__name__} failed: {e}")
            raise
    return wrapper

@log_execution
def create_user(email, password):
    # Create user logic
    pass
```

---

### Q6: Explain *args and **kwargs

**Why they ask:** Understanding flexible function arguments

**Answer:**
```python
# *args - allows passing variable number of positional arguments
def print_args(*args):
    for arg in args:
        print(arg)

print_args(1, 2, 3, 4, 5)  # Works! args = (1, 2, 3, 4, 5)
print_args("hello")  # Works! args = ("hello",)

# **kwargs - allows passing variable number of keyword arguments
def print_kwargs(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_kwargs(name="Awais", age=25, city="Lahore")
# Output:
# name: Awais
# age: 25
# city: Lahore

# Combined usage
def flexible_function(name, *args, **kwargs):
    print(f"Name: {name}")
    print(f"Args: {args}")
    print(f"Kwargs: {kwargs}")

flexible_function("Awais", 1, 2, 3, city="Lahore", job="Developer")
# Output:
# Name: Awais
# Args: (1, 2, 3)
# Kwargs: {'city': 'Lahore', 'job': 'Developer'}

# Django REST Framework example
class MyView(APIView):
    def get(self, request, *args, **kwargs):
        # *args might contain positional parameters from URL
        # **kwargs might contain id=123 from URL patterns
        user_id = kwargs.get('id')
        return Response({'user_id': user_id})
```

---

### Q7: What is the difference between == and is in Python?

**Why they ask:** Understanding object comparison

**Answer:**
```python
# == compares VALUES
# is compares IDENTITY (memory addresses)

a = [1, 2, 3]
b = [1, 2, 3]

a == b  # True (same values)
a is b  # False (different objects in memory)

# With immutable objects
x = 5
y = 5
x == y  # True
x is y  # True (Python caches small integers)

# String comparison
str1 = "hello"
str2 = "hello"
str1 == str2  # True
str1 is str2  # True (String interning in Python)

# But with some strings
str1 = "hello world"
str2 = "hello world"
str1 == str2  # True
str1 is str2  # Might be False (depending on how they're created)

# When to use:
# Use == for comparing values
if user.email == entered_email:
    # Check if emails match

# Use is for comparing with None/True/False
if request.user is None:
    # Not if request.user == None (though it works)

# Django example
if instance is None:  # Check if object doesn't exist
    return None

if queryset.exists():  # Better than checking if queryset == []
    # Process results
```

---

### Q8: What is a lambda function? When would you use it?

**Why they ask:** Knowing when to use lambda vs regular functions

**Answer:**
```python
# Lambda is an anonymous function - single line function

# Syntax: lambda arguments: expression

# Simple example
add = lambda x, y: x + y
result = add(3, 5)  # 8

# Common uses:

# 1. With map() - transform each item
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x**2, numbers))
# Better: squared = [x**2 for x in numbers]

# 2. With filter() - filter items
numbers = [1, 2, 3, 4, 5]
evens = list(filter(lambda x: x % 2 == 0, numbers))
# Better: evens = [x for x in numbers if x % 2 == 0]

# 3. With sorted() - custom sort
students = [('Alice', 25), ('Bob', 20), ('Charlie', 22)]
sorted_by_age = sorted(students, key=lambda x: x[1])

# 4. With QuerySet ordering (Django)
users = User.objects.annotate(
    age=ExpressionWrapper(Now() - F('birth_date'), output_field=IntegerField())
).order_by('-age')

# When NOT to use lambda:
# - When logic is complex
# - When it's hard to read
# - When you'd need multiple lines

# Good lambda:
filter(lambda x: x > 5, numbers)

# Bad lambda (don't do this):
filter(lambda x: do_complex_processing(x) if check_condition(x) else process_other(x), numbers)
# Use a regular function instead

def is_active_user(user):
    return user.is_active and user.last_login is not None

active_users = filter(is_active_user, User.objects.all())
```

---

### Q9: Explain Exception Handling in Python

**Why they ask:** Understanding error handling

**Answer:**
```python
# Try-except block
try:
    result = 10 / 0  # This will raise an exception
except ZeroDivisionError:
    print("Cannot divide by zero")

# Multiple exceptions
try:
    number = int("abc")
except ValueError:
    print("Invalid number format")
except TypeError:
    print("Type error occurred")
except Exception as e:
    print(f"Unexpected error: {e}")

# Try-except-else-finally
try:
    file = open("data.txt")
    content = file.read()
except FileNotFoundError:
    print("File not found")
else:
    print("File read successfully")
    # This runs only if no exception occurred
finally:
    file.close()  # This always runs

# Raising custom exceptions
class InsufficientFundsError(Exception):
    pass

def withdraw_money(amount, balance):
    if amount > balance:
        raise InsufficientFundsError("Insufficient balance")
    return balance - amount

# Django REST Framework example
from rest_framework.exceptions import ValidationError

class UserSerializer(serializers.ModelSerializer):
    def validate_email(self, value):
        if User.objects.filter(email=value).exists():
            raise ValidationError("Email already exists")
        return value

# Best practices
1. Catch specific exceptions, not generic Exception
2. Use finally for cleanup
3. Use else for code that should run if no exception
4. Don't use bare except
5. Log exceptions for debugging

# Anti-pattern (DON'T DO THIS)
try:
    do_something()
except:  # Bare except - catches everything
    pass  # Silently fails

# Better pattern
try:
    do_something()
except SpecificError as e:
    logger.error(f"Error: {e}")
    handle_error(e)
```

---

## Django & Django REST Framework

### Q1: Explain the MTV (Model-Template-View) architecture in Django

**Why they ask:** Understanding Django's core architecture

**Answer:**
```
Django uses MTV architecture:

1. MODEL (Database Layer)
   - Defines database schema using Python classes
   - Handles data validation and business logic
   
   class User(models.Model):
       email = models.EmailField(unique=True)
       name = models.CharField(max_length=100)
       created_at = models.DateTimeField(auto_now_add=True)

2. TEMPLATE (Presentation Layer)
   - HTML templates with dynamic content
   - Rendered on the server
   
   <h1>Welcome {{ user.name }}</h1>
   {% for post in posts %}
       <p>{{ post.title }}</p>
   {% endfor %}

3. VIEW (Business Logic Layer)
   - Processes requests
   - Queries the model
   - Renders the template
   
   def user_profile(request):
       user = User.objects.get(id=request.user.id)
       posts = Post.objects.filter(author=user)
       return render(request, 'profile.html', {'user': user, 'posts': posts})

4. URL ROUTING (Optional but important)
   - Maps URLs to views
   
   urlpatterns = [
       path('profile/', user_profile, name='profile'),
   ]

Flow:
1. Browser sends request to URL
2. Django matches URL to view
3. View queries model
4. View renders template with model data
5. Response sent back to browser

In Django REST Framework, we replace Template with JSON serialization:
- Model - same
- View - now returns JSON instead of HTML
- Serializer - converts model to JSON
```

---

### Q2: What is a Django Model? Explain with an example

**Why they ask:** Core Django knowledge

**Answer:**
```python
# A Django Model is a Python class that represents a database table

from django.db import models
from django.contrib.auth.models import User

class BlogPost(models.Model):
    # Define field types (they map to database columns)
    title = models.CharField(max_length=200)
    content = models.TextField()
    author = models.ForeignKey(User, on_delete=models.CASCADE)
    category = models.CharField(
        max_length=50,
        choices=[
            ('tech', 'Technology'),
            ('news', 'News'),
            ('other', 'Other')
        ]
    )
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    is_published = models.BooleanField(default=False)
    
    # Meta options
    class Meta:
        ordering = ['-created_at']  # Newest first
        verbose_name_plural = "Blog Posts"
        db_table = 'blog_posts'
    
    # Magic method
    def __str__(self):
        return self.title

# Using the model:

# Create (INSERT)
post = BlogPost.objects.create(
    title="My First Post",
    content="This is the content",
    author_id=1,
    category='tech'
)

# Read (SELECT)
all_posts = BlogPost.objects.all()
published_posts = BlogPost.objects.filter(is_published=True)
first_post = BlogPost.objects.first()
post_by_id = BlogPost.objects.get(id=1)

# Update (UPDATE)
post = BlogPost.objects.get(id=1)
post.title = "Updated Title"
post.save()

# Delete (DELETE)
post.delete()

# Query complex conditions
tech_posts = BlogPost.objects.filter(category='tech', is_published=True)
```

---

### Q3: What is the difference between select_related and prefetch_related?

**Why they ask:** Query optimization - critical for performance

**Answer:**
```python
# PROBLEM: N+1 Query Problem
# If you have 10 posts and each post has an author:

posts = Post.objects.all()  # 1 query
for post in posts:
    print(post.author.name)  # 10 MORE queries! (N+1 problem)
# Total: 11 queries

# SOLUTION 1: select_related() - for ForeignKey/OneToOne (one-to-one)
# Uses SQL JOIN - loads related object in one query
posts = Post.objects.select_related('author')  # 1 query with JOIN
for post in posts:
    print(post.author.name)  # No additional query!

# Generated SQL (simplified):
# SELECT post.*, user.* FROM post 
# LEFT JOIN user ON post.author_id = user.id

# SOLUTION 2: prefetch_related() - for ManyToMany/Reverse ForeignKey
# Uses separate queries but caches results

class Post(models.Model):
    title = models.CharField(max_length=100)

class Comment(models.Model):
    post = models.ForeignKey(Post, on_delete=models.CASCADE, related_name='comments')
    text = models.TextField()

# Without optimization (N+1 problem)
posts = Post.objects.all()  # 1 query
for post in posts:
    comments = post.comments.all()  # N queries

# With prefetch_related
posts = Post.objects.prefetch_related('comments')  # 2 queries total
for post in posts:
    comments = post.comments.all()  # No additional query

# Can combine both
posts = Post.objects.select_related('author').prefetch_related('comments')

# Using Prefetch object for complex prefetching
from django.db.models import Prefetch

comments_prefetch = Prefetch(
    'comments',
    Comment.objects.filter(approved=True)
)
posts = Post.objects.prefetch_related(comments_prefetch)
```

---

### Q4: Explain Django ORM QuerySet filters

**Why they ask:** Practical Django usage

**Answer:**
```python
# Common QuerySet filter operations

from django.db.models import Q, F, Count

# Exact match (default)
User.objects.filter(email='test@example.com')
User.objects.filter(email__exact='test@example.com')  # Same

# Case-insensitive
User.objects.filter(email__iexact='TEST@EXAMPLE.COM')

# Contains
User.objects.filter(name__contains='John')  # Name contains 'John'
User.objects.filter(name__icontains='john')  # Case-insensitive

# Starts with / Ends with
User.objects.filter(email__startswith='admin')
User.objects.filter(email__endswith='.com')

# Range
User.objects.filter(age__range=(18, 65))

# In list
User.objects.filter(status__in=['active', 'pending'])

# Greater than / Less than
User.objects.filter(age__gt=18)  # Greater than
User.objects.filter(age__gte=18)  # Greater than or equal
User.objects.filter(age__lt=65)  # Less than
User.objects.filter(age__lte=65)  # Less than or equal

# Date filtering
from django.utils import timezone
from datetime import timedelta

today = timezone.now().date()
User.objects.filter(created_at__date=today)
User.objects.filter(created_at__year=2024)
User.objects.filter(created_at__month=3)

# Null checks
User.objects.filter(phone__isnull=True)  # Has no phone
User.objects.filter(phone__isnull=False)  # Has phone

# Multiple conditions (AND)
User.objects.filter(status='active', age__gt=18)

# OR conditions (Q objects)
User.objects.filter(Q(status='active') | Q(status='pending'))

# Complex filters
User.objects.filter(
    Q(email__endswith='.com') | Q(email__endswith='.org'),
    status='active'
)

# Exclude (opposite of filter)
User.objects.exclude(status='inactive')

# F expressions (field comparisons)
User.objects.filter(age__gt=F('minimum_age'))

# Aggregations
User.objects.aggregate(Count('id'))  # Count users
User.objects.values('country').annotate(count=Count('id'))  # Count by country

# Real Django REST Framework example
class UserViewSet(viewsets.ModelViewSet):
    queryset = User.objects.all()
    serializer_class = UserSerializer
    
    def get_queryset(self):
        queryset = User.objects.all()
        
        # Filter by status
        status = self.request.query_params.get('status')
        if status:
            queryset = queryset.filter(status=status)
        
        # Filter by email domain
        domain = self.request.query_params.get('domain')
        if domain:
            queryset = queryset.filter(email__endswith=f'@{domain}')
        
        return queryset
```

---

### Q5: What are Django Signals?

**Why they ask:** Understanding Django's event system

**Answer:**
```python
# Django Signals are like event listeners
# They allow you to run code when specific events happen

from django.db.models.signals import post_save, pre_save, post_delete
from django.dispatch import receiver

# BEFORE saving a model
@receiver(pre_save, sender=User)
def before_user_save(sender, instance, **kwargs):
    print(f"About to save user: {instance.email}")

# AFTER saving a model
@receiver(post_save, sender=User)
def after_user_save(sender, instance, created, **kwargs):
    if created:
        print(f"New user created: {instance.email}")
        # Send welcome email
        send_welcome_email(instance.email)
    else:
        print(f"User updated: {instance.email}")

# AFTER deleting a model
@receiver(post_delete, sender=User)
def after_user_delete(sender, instance, **kwargs):
    print(f"User deleted: {instance.email}")
    # Clean up related files, etc.

# Using signals for common tasks:

# 1. Send email after user registration
@receiver(post_save, sender=User)
def send_verification_email(sender, instance, created, **kwargs):
    if created and instance.is_active:
        send_email_task.delay(instance.id)  # Celery task

# 2. Update user last activity
from django.utils import timezone

@receiver(post_save, sender=Post)
def update_user_activity(sender, instance, **kwargs):
    instance.author.last_activity = timezone.now()
    instance.author.save()

# 3. Create user profile when user is created
from django.contrib.auth.models import User

@receiver(post_save, sender=User)
def create_user_profile(sender, instance, created, **kwargs):
    if created:
        UserProfile.objects.create(user=instance)

@receiver(post_save, sender=User)
def save_user_profile(sender, instance, **kwargs):
    instance.profile.save()

# Register signals (in apps.py)
from django.apps import AppConfig

class MyAppConfig(AppConfig):
    default_auto_field = 'django.db.models.BigAutoField'
    name = 'myapp'
    
    def ready(self):
        import myapp.signals  # Import signal handlers

# WARNING: Signals can slow down performance
# Use them for truly event-driven tasks, not everyday updates
# Consider alternatives:
# 1. Override model.save()
# 2. Use Django management commands
# 3. Use Celery for async tasks
```

---

### Q6: What is Django REST Framework (DRF)? Explain with example

**Why they ask:** DRF is core to backend development

**Answer:**
```python
# Django REST Framework is a powerful toolkit for building REST APIs

# 1. MODELS
from django.db import models

class Course(models.Model):
    title = models.CharField(max_length=200)
    description = models.TextField()
    instructor = models.ForeignKey('User', on_delete=models.CASCADE)
    price = models.DecimalField(max_digits=10, decimal_places=2)
    created_at = models.DateTimeField(auto_now_add=True)

# 2. SERIALIZERS (Convert models to JSON)
from rest_framework import serializers

class CourseSerializer(serializers.ModelSerializer):
    instructor_name = serializers.CharField(source='instructor.name', read_only=True)
    
    class Meta:
        model = Course
        fields = ['id', 'title', 'description', 'price', 'instructor_name']
        read_only_fields = ['id', 'created_at']
    
    def validate_price(self, value):
        if value < 0:
            raise serializers.ValidationError("Price cannot be negative")
        return value

# 3. VIEWSETS (Handle CRUD operations)
from rest_framework import viewsets
from rest_framework.permissions import IsAuthenticated

class CourseViewSet(viewsets.ModelViewSet):
    queryset = Course.objects.all()
    serializer_class = CourseSerializer
    permission_classes = [IsAuthenticated]
    
    def get_queryset(self):
        # Filter courses by instructor
        instructor_id = self.request.query_params.get('instructor')
        if instructor_id:
            return self.queryset.filter(instructor_id=instructor_id)
        return self.queryset

# 4. URL ROUTING
from rest_framework.routers import DefaultRouter
from django.urls import path, include

router = DefaultRouter()
router.register(r'courses', CourseViewSet)

urlpatterns = [
    path('api/', include(router.urls)),
]

# This automatically creates:
# GET    /api/courses/              - List all courses
# POST   /api/courses/              - Create new course
# GET    /api/courses/<id>/         - Get course details
# PUT    /api/courses/<id>/         - Update course
# PATCH  /api/courses/<id>/         - Partial update
# DELETE /api/courses/<id>/         - Delete course

# API Usage Examples:

# List courses
GET /api/courses/
Response: [
    {"id": 1, "title": "Python Basics", "price": "99.99", ...},
    {"id": 2, "title": "Django Advanced", "price": "149.99", ...}
]

# Create course
POST /api/courses/
Body: {
    "title": "REST API Design",
    "description": "Learn to build scalable APIs",
    "price": "199.99"
}

# Get single course
GET /api/courses/1/

# Update course
PUT /api/courses/1/
Body: {"title": "Updated Title", ...}

# Delete course
DELETE /api/courses/1/
```

---

### Q7: Explain Authentication in Django REST Framework

**Why they ask:** Security and authorization are critical

**Answer:**
```python
# DRF provides multiple authentication methods

# 1. TOKEN AUTHENTICATION
from rest_framework.authtoken.models import Token

# Generate token for user
token, created = Token.objects.get_or_create(user=user)

# Client sends token in header
# Authorization: Token 9944b09199c62bcf9418ad846dd0e4bbdfc6ee4b

# Usage in view
from rest_framework.authentication import TokenAuthentication
from rest_framework.permissions import IsAuthenticated

class SecureView(APIView):
    authentication_classes = [TokenAuthentication]
    permission_classes = [IsAuthenticated]
    
    def get(self, request):
        return Response({'message': f'Hello {request.user.username}'})

# 2. SESSION AUTHENTICATION
from rest_framework.authentication import SessionAuthentication

class MyView(APIView):
    authentication_classes = [SessionAuthentication]
    permission_classes = [IsAuthenticated]

# 3. JWT AUTHENTICATION (JSON Web Tokens)
from rest_framework_simplejwt.tokens import RefreshToken
from rest_framework_simplejwt.views import TokenObtainPairView

class MyTokenObtainPairView(TokenObtainPairView):
    pass

# User sends credentials and gets access token
# POST /api/token/
# Body: {"username": "user", "password": "pass"}
# Response: {"access": "token...", "refresh": "token..."}

# Client uses access token for requests
# Authorization: Bearer {access_token}

# 4. CUSTOM AUTHENTICATION
from rest_framework.authentication import BaseAuthentication
from rest_framework.exceptions import AuthenticationFailed

class APIKeyAuthentication(BaseAuthentication):
    def authenticate(self, request):
        api_key = request.headers.get('X-API-Key')
        
        if not api_key:
            return None  # Let other authenticators handle it
        
        try:
            user_key = APIKey.objects.get(key=api_key)
        except APIKey.DoesNotExist:
            raise AuthenticationFailed('Invalid API key')
        
        return (user_key.user, None)

# Use it
class MyView(APIView):
    authentication_classes = [APIKeyAuthentication]
    
    def get(self, request):
        return Response({'message': 'Authenticated'})

# PERMISSIONS (What authenticated users can do)
from rest_framework.permissions import BasePermission

class IsOwner(BasePermission):
    def has_object_permission(self, request, view, obj):
        return obj.owner == request.user

class MyView(APIView):
    permission_classes = [IsAuthenticated, IsOwner]
    
    def get(self, request, id):
        obj = MyModel.objects.get(id=id)
        self.check_object_permissions(request, obj)
        serializer = MySerializer(obj)
        return Response(serializer.data)

# Built-in permissions:
# - AllowAny: Anyone can access
# - IsAuthenticated: User must be logged in
# - IsAdminUser: User must be admin
# - IsAuthenticatedOrReadOnly: Anyone can read, only authenticated can write

```

---

## Database Design & SQL

### Q1: Explain database normalization

**Why they ask:** Understanding database design fundamentals

**Answer:**
```
Database Normalization is the process of organizing data to reduce redundancy

NORMAL FORMS (increasing levels of normalization):

1. FIRST NORMAL FORM (1NF)
   - Atomic values only (no repeating groups)
   - Each column contains only one type of data
   
   Bad (violates 1NF):
   | Student | Subjects           |
   | Awais   | Python, Django, JS |  <- Multiple values in one cell
   
   Good (1NF):
   | StudentID | Subject |
   | 1         | Python  |
   | 1         | Django  |
   | 1         | JS      |

2. SECOND NORMAL FORM (2NF)
   - Must be in 1NF
   - Non-key attributes must fully depend on primary key
   
   Bad (violates 2NF):
   | StudentCourse | StudentID | CourseName | Instructor |
   - StudentCourse is primary key, but Instructor only depends on CourseName
   
   Good (2NF):
   Create separate tables:
   | StudentCourse | StudentID | CourseID |
   | Course        | CourseID  | CourseName | InstructorID |
   | Instructor    | InstructorID | Name |

3. THIRD NORMAL FORM (3NF)
   - Must be in 2NF
   - Non-key attributes must not depend on other non-key attributes
   
   Bad (violates 3NF):
   | Student | Name   | City    | StateID | StateName |
   - StateName depends on StateID, not on Student primary key
   
   Good (3NF):
   | Student | StudentID | Name | CityID |
   | City    | CityID    | Name | StateID |
   | State   | StateID   | Name |

BENEFITS:
- Reduces data redundancy
- Prevents anomalies (update, insert, delete)
- Improves data integrity
- Saves storage space

Django Example:

# Bad design (denormalized)
class CourseEnrollment(models.Model):
    student_name = models.CharField(max_length=100)
    student_email = models.EmailField()
    course_name = models.CharField(max_length=100)
    instructor_name = models.CharField(max_length=100)

# Good design (normalized to 3NF)
class Student(models.Model):
    name = models.CharField(max_length=100)
    email = models.EmailField(unique=True)

class Course(models.Model):
    name = models.CharField(max_length=100)
    instructor = models.ForeignKey(Instructor, on_delete=models.CASCADE)

class Enrollment(models.Model):
    student = models.ForeignKey(Student, on_delete=models.CASCADE)
    course = models.ForeignKey(Course, on_delete=models.CASCADE)
```

---

### Q2: What are database indexes? When would you use them?

**Why they ask:** Query performance optimization

**Answer:**
```python
# Indexes are data structures that speed up data retrieval

# Creating indexes in Django
class User(models.Model):
    email = models.EmailField(unique=True, db_index=True)  # Index
    username = models.CharField(max_length=100, db_index=True)
    age = models.IntegerField()
    created_at = models.DateTimeField(auto_now_add=True, db_index=True)

# Multi-column index
class User(models.Model):
    email = models.EmailField()
    status = models.CharField(max_length=20)
    
    class Meta:
        indexes = [
            models.Index(fields=['email', 'status']),
        ]

# How indexes work:
# Without index:
SELECT * FROM users WHERE email = 'test@example.com'
# Database scans all rows -> O(n) operation

# With index:
SELECT * FROM users WHERE email = 'test@example.com'
# Database uses index to jump to matching rows -> O(log n) operation

# When to use indexes:
✅ Columns frequently used in WHERE clauses
✅ Columns used in JOIN conditions
✅ Columns used in ORDER BY
✅ Columns with high cardinality (many unique values)
✅ Foreign keys

❌ Small tables (< 1000 rows)
❌ Low cardinality columns (few unique values like gender)
❌ Columns with many NULL values
❌ Columns that change frequently (write-heavy)

# Example - E-learning platform indexes
class Course(models.Model):
    title = models.CharField(max_length=200, db_index=True)
    category = models.CharField(max_length=50, db_index=True)
    instructor = models.ForeignKey(User, on_delete=models.CASCADE, db_index=True)
    created_at = models.DateTimeField(auto_now_add=True, db_index=True)

class Enrollment(models.Model):
    student = models.ForeignKey(User, on_delete=models.CASCADE, db_index=True)
    course = models.ForeignKey(Course, on_delete=models.CASCADE, db_index=True)
    
    class Meta:
        indexes = [
            models.Index(fields=['student', 'course']),  # Common filter
        ]

# QuerySet with indexes (fast)
User.objects.filter(email='test@example.com')  # Index on email
Course.objects.filter(category='python')  # Index on category
```

---

### Q3: Explain SQL JOINs

**Why they ask:** Understanding data relationships

**Answer:**
```sql
-- Suppose we have:
-- students table: id, name
-- enrollments table: id, student_id, course_id
-- courses table: id, title

-- INNER JOIN (returns only matching records)
SELECT s.name, c.title
FROM students s
INNER JOIN enrollments e ON s.id = e.student_id
INNER JOIN courses c ON e.course_id = c.id
-- Returns only students who are enrolled in courses

-- LEFT JOIN (returns all from left table + matching from right)
SELECT s.name, c.title
FROM students s
LEFT JOIN enrollments e ON s.id = e.student_id
LEFT JOIN courses c ON e.course_id = c.id
-- Returns all students, even if not enrolled

-- RIGHT JOIN (returns all from right table + matching from left)
SELECT s.name, c.title
FROM enrollments e
RIGHT JOIN courses c ON e.course_id = c.id
-- Returns all courses, even if no enrollments

-- FULL OUTER JOIN (returns all records from both tables)
SELECT s.name, c.title
FROM students s
FULL OUTER JOIN enrollments e ON s.id = e.student_id
FULL OUTER JOIN courses c ON e.course_id = c.id
-- Returns all combinations

-- CROSS JOIN (cartesian product)
SELECT s.name, c.title
FROM students s
CROSS JOIN courses c
-- Returns all combinations (10 students × 5 courses = 50 rows)

-- Django ORM equivalent
from django.db.models import Q

# INNER JOIN
Enrollment.objects.filter(
    student__isnull=False,
    course__isnull=False
).select_related('student', 'course')

# LEFT JOIN (default with select_related)
enrollments = Enrollment.objects.select_related('student', 'course')

# Multiple JOINs
students = Student.objects.select_related('user').prefetch_related('enrollments__course')
```

---

### Q4: What is a transaction in databases?

**Why they ask:** Data consistency and integrity

**Answer:**
```python
# A transaction is a series of database operations treated as one unit
# Either all succeed (commit) or all fail (rollback)

# ACID Properties:
# A - Atomicity: All or nothing
# C - Consistency: Database moves from one valid state to another
# I - Isolation: Concurrent transactions don't interfere
# D - Durability: Committed data persists

from django.db import transaction

# Using transactions in Django

# ATOMIC BLOCK
@transaction.atomic
def transfer_money(from_account, to_account, amount):
    from_account.balance -= amount
    from_account.save()  # If this fails, everything rolls back
    to_account.balance += amount
    to_account.save()   # If this fails, everything rolls back

# If both saves succeed, transaction commits
# If either fails, both are rolled back

# Manual transaction control
from django.db import transaction

def complex_operation():
    try:
        with transaction.atomic():
            user = User.objects.create(email='test@example.com')
            profile = Profile.objects.create(user=user)
            notification = Notification.objects.create(user=user)
        return True
    except Exception as e:
        # All changes are rolled back
        return False

# Using savepoints (nested transactions)
from django.db import transaction

with transaction.atomic():
    user = User.objects.create(email='test@example.com')
    
    sid = transaction.savepoint()
    try:
        profile = Profile.objects.create(user=user)
    except IntegrityError:
        transaction.savepoint_rollback(sid)
    transaction.savepoint_commit(sid)

# Real-world example: E-learning enrollment
@transaction.atomic
def enroll_student(student_id, course_id):
    student = Student.objects.select_for_update().get(id=student_id)
    course = Course.objects.select_for_update().get(id=course_id)
    
    # Check if student already enrolled
    if Enrollment.objects.filter(student=student, course=course).exists():
        raise ValidationError("Already enrolled")
    
    # Create enrollment
    enrollment = Enrollment.objects.create(student=student, course=course)
    
    # Decrease available seats
    course.available_seats -= 1
    course.save()
    
    # Create notification
    Notification.objects.create(
        user=student.user,
        message=f"You've enrolled in {course.title}"
    )
    
    return enrollment

# If any step fails, all changes are rolled back!
```

---

## REST API Design

### Q1: What makes a good REST API?

**Why they ask:** API design fundamentals

**Answer:**
```
A good REST API has these characteristics:

1. PROPER HTTP METHODS
   GET     - Retrieve data (safe, idempotent)
   POST    - Create new data
   PUT     - Replace entire resource
   PATCH   - Partial update
   DELETE  - Delete resource
   
   Bad: GET /api/delete-user?id=1
   Good: DELETE /api/users/1

2. PROPER HTTP STATUS CODES
   200 OK - Request successful
   201 Created - Resource created
   204 No Content - Successful but no response body
   400 Bad Request - Invalid request
   401 Unauthorized - Authentication required
   403 Forbidden - Authenticated but not authorized
   404 Not Found - Resource doesn't exist
   500 Internal Server Error - Server error
   
   Bad: Always return 200
   Good: 
      GET /users/1 -> 200 OK
      POST /users -> 201 Created
      DELETE /users/1 -> 204 No Content
      GET /users/999 -> 404 Not Found

3. CONSISTENT RESPONSE FORMAT
   {
       "status": "success",
       "data": {
           "id": 1,
           "email": "user@example.com"
       }
   }
   
   For errors:
   {
       "status": "error",
       "message": "Validation failed",
       "errors": {
           "email": ["Invalid email format"]
       }
   }

4. PROPER RESOURCE NAMING
   Bad: /api/get_users, /api/getUser, /api/user_create
   Good: 
      GET /api/users -> List users
      GET /api/users/1 -> Get user 1
      POST /api/users -> Create user
      PUT /api/users/1 -> Update user 1
      DELETE /api/users/1 -> Delete user 1

5. VERSIONING
   /api/v1/users (better for major changes)
   Headers: Accept: application/vnd.myapp.v1+json

6. PAGINATION
   GET /api/courses?page=1&page_size=10
   Response:
   {
       "count": 100,
       "next": "/api/courses?page=2",
       "previous": null,
       "results": [...]
   }

7. FILTERING & SEARCHING
   GET /api/courses?category=python&min_price=50
   GET /api/courses?search=django

8. AUTHENTICATION & AUTHORIZATION
   Every endpoint should specify who can access it
   GET /api/public/courses (anyone)
   GET /api/user/profile (authenticated users only)
   DELETE /api/users/1 (only admin or user)

9. RATE LIMITING
   Prevent abuse
   X-RateLimit-Limit: 100
   X-RateLimit-Remaining: 95

10. DOCUMENTATION
    Every API should be documented (Swagger/OpenAPI)

Django REST Framework example of good API:

from rest_framework import viewsets
from rest_framework.decorators import action
from rest_framework.response import Response
from rest_framework.pagination import PageNumberPagination

class StandardPagination(PageNumberPagination):
    page_size = 10
    page_size_query_param = 'page_size'

class CourseViewSet(viewsets.ModelViewSet):
    queryset = Course.objects.all()
    serializer_class = CourseSerializer
    pagination_class = StandardPagination
    filterset_fields = ['category', 'price']
    search_fields = ['title', 'description']
    
    @action(detail=True, methods=['get'])
    def enrollments(self, request, pk=None):
        course = self.get_object()
        enrollments = course.enrollment_set.all()
        serializer = EnrollmentSerializer(enrollments, many=True)
        return Response(serializer.data)

# Usage:
GET /api/courses/                    # List courses (paginated)
GET /api/courses?page=2              # Page 2
GET /api/courses?page_size=20        # 20 per page
GET /api/courses?category=python     # Filter by category
GET /api/courses?search=django       # Search
GET /api/courses/1/                  # Get course
POST /api/courses/                   # Create course
PUT /api/courses/1/                  # Update course
DELETE /api/courses/1/               # Delete course
GET /api/courses/1/enrollments/      # Custom action
```

---

## System Design & Architecture

### Q1: How would you design a scalable API?

**Why they ask:** Senior developer thinking

**Answer:**
```
Key considerations for scalable APIs:

1. DATABASE LAYER
   ✓ Use read replicas for high read traffic
   ✓ Use connection pooling (pgbouncer, Django connection pooling)
   ✓ Cache frequently accessed data (Redis, Memcached)
   ✓ Denormalization where read performance is critical
   ✓ Proper indexing

2. CACHING STRATEGY
   Cache-aside pattern:
   
   def get_user(user_id):
       # Check cache first
       user = cache.get(f'user:{user_id}')
       if user:
           return user
       
       # Cache miss, get from DB
       user = User.objects.get(id=user_id)
       
       # Store in cache (expire after 1 hour)
       cache.set(f'user:{user_id}', user, 3600)
       
       return user

3. ASYNC PROCESSING
   Use Celery for background tasks:
   
   # Send email in background
   @shared_task
   def send_enrollment_email(user_id, course_id):
       user = User.objects.get(id=user_id)
       course = Course.objects.get(id=course_id)
       send_email(user.email, f"Enrolled in {course.title}")
   
   # Trigger task
   send_enrollment_email.delay(user_id, course_id)

4. API RATE LIMITING
   from rest_framework.throttling import UserRateThrottle
   
   class BurstRateThrottle(UserRateThrottle):
       scope = 'burst'
       THROTTLE_RATES = {'burst': '100/hour'}

5. LOAD BALANCING
   - Distribute traffic across multiple servers
   - Use Nginx or AWS ELB
   - Session persistence or stateless design

6. MICROSERVICES
   - Split into independent services
   - User Service, Course Service, Payment Service
   - API Gateway to route requests

7. API VERSIONING
   /api/v1/, /api/v2/ endpoints
   Allows gradual deprecation

8. MONITORING & LOGGING
   - Track response times
   - Monitor database queries
   - Alert on anomalies
   - Tools: New Relic, Datadog, ELK stack

Example: E-learning platform architecture

┌─────────────────────────────────────────────┐
│         Client Applications                 │
│    (Web, Mobile, Desktop)                   │
└───────────────┬─────────────────────────────┘
                │
┌───────────────▼─────────────────────────────┐
│    API Gateway / Load Balancer               │
│    (Nginx / AWS ELB)                        │
└───────────────┬─────────────────────────────┘
                │
    ┌───────────┼────────────┐
    │           │            │
┌───▼───┐  ┌────▼────┐  ┌──▼────┐
│API    │  │API      │  │API    │
│Server │  │Server   │  │Server │
│ 1     │  │ 2       │  │ 3     │
└───┬───┘  └────┬────┘  └──┬────┘
    │           │          │
    └───────────┼──────────┘
                │
        ┌───────▼───────┐
        │   Cache       │
        │  (Redis)      │
        └───────────────┘
                │
    ┌───────────┼──────────┐
    │           │          │
┌───▼────┐  ┌──▼────┐  ┌──▼────┐
│Primary │  │Replica│  │Replica│
│Database│  │DB 1   │  │DB 2   │
└────────┘  └───────┘  └───────┘

    ┌──────────────────────┐
    │  Async Tasks/Queue   │
    │  (Celery + Redis)    │
    └──────────────────────┘
```

---

### Q2: How would you handle authentication & authorization?

**Why they ask:** Security is critical

**Answer:**
```python
# AUTHENTICATION: Proving who you are
# AUTHORIZATION: Proving what you can do

# Strategy 1: JWT (JSON Web Tokens)

from rest_framework_simplejwt.tokens import RefreshToken
from rest_framework_simplejwt.views import TokenObtainPairView

class LoginView(TokenObtainPairView):
    # User sends username/password
    # Returns access_token (15 min) & refresh_token (7 days)
    pass

# Workflow:
POST /api/login
{
    "username": "awais",
    "password": "password123"
}

Response:
{
    "access": "eyJ0eXAiOiJKV1QiLCJhbGc...",  # Short-lived
    "refresh": "eyJ0eXAiOiJKV1QiLCJhbGc..."   # Long-lived
}

# Client stores tokens
# For each request:
Authorization: Bearer {access_token}

# If access token expires, use refresh token to get new one
POST /api/token/refresh
{
    "refresh": "eyJ0eXAiOiJKV1QiLCJhbGc..."
}

# Strategy 2: Permission Classes

from rest_framework.permissions import BasePermission

class IsOwnerOrReadOnly(BasePermission):
    def has_object_permission(self, request, view, obj):
        if request.method in ['GET', 'HEAD', 'OPTIONS']:
            return True
        return obj.owner == request.user

class IsInstructor(BasePermission):
    def has_permission(self, request, view):
        return request.user and request.user.is_instructor

# Usage
class CourseViewSet(viewsets.ModelViewSet):
    queryset = Course.objects.all()
    serializer_class = CourseSerializer
    
    def get_permissions(self):
        if self.action == 'create':
            permission_classes = [IsInstructor]
        elif self.action in ['update', 'destroy']:
            permission_classes = [IsOwnerOrReadOnly]
        else:
            permission_classes = [AllowAny]
        
        return [permission() for permission in permission_classes]

# Strategy 3: Role-Based Access Control (RBAC)

class User(models.Model):
    ROLES = [
        ('admin', 'Administrator'),
        ('instructor', 'Instructor'),
        ('student', 'Student'),
        ('guest', 'Guest'),
    ]
    role = models.CharField(max_length=20, choices=ROLES)

class HasRole(BasePermission):
    required_roles = []
    
    def has_permission(self, request, view):
        return request.user.role in self.required_roles

class AdminOnly(HasRole):
    required_roles = ['admin']

class InstructorOrAdmin(HasRole):
    required_roles = ['instructor', 'admin']

# Strategy 4: Fine-grained permissions

@permission_required('courses.can_view_reports')
def view_reports(request):
    # Only users with this permission can access
    pass

# Complete example: E-learning platform authorization

class CourseViewSet(viewsets.ModelViewSet):
    queryset = Course.objects.all()
    serializer_class = CourseSerializer
    
    def perform_create(self, serializer):
        # Only instructors can create courses
        if not self.request.user.role == 'instructor':
            raise PermissionDenied("Only instructors can create courses")
        serializer.save(instructor=self.request.user)
    
    def perform_update(self, serializer):
        # Only course owner can update
        if serializer.instance.instructor != self.request.user:
            raise PermissionDenied("Only course owner can update")
        serializer.save()
    
    @action(detail=True, methods=['post'])
    def enroll(self, request, pk=None):
        # Only students can enroll
        if request.user.role != 'student':
            raise PermissionDenied("Only students can enroll")
        
        course = self.get_object()
        enrollment = Enrollment.objects.create(
            student=request.user,
            course=course
        )
        return Response(EnrollmentSerializer(enrollment).data, status=201)
```

---

## Authentication & Security

### Q1: Explain Multi-Factor Authentication (MFA)

**Why they ask:** Security best practices

**Answer:**
```python
# MFA adds additional verification layer

# Factor types:
# 1. Something you know: Password, PIN
# 2. Something you have: Phone, hardware token
# 3. Something you are: Biometric

# Example: Password + Time-based OTP

from pyotp import TOTP
from django.db import models

class User(models.Model):
    email = models.EmailField()
    password = models.CharField(max_length=255)
    totp_secret = models.CharField(max_length=32, blank=True)
    is_2fa_enabled = models.BooleanField(default=False)

# Step 1: Enable 2FA
def enable_2fa(request):
    user = request.user
    secret = TOTP.random_secret()  # Generate secret
    user.totp_secret = secret
    user.save()
    
    # Generate QR code
    totp = TOTP(secret)
    qr_code = totp.provisioning_uri(
        name=user.email,
        issuer_name='MyApp'
    )
    
    return Response({'qr_code': qr_code})

# Step 2: Verify 2FA during login
def verify_2fa(request):
    email = request.data.get('email')
    password = request.data.get('password')
    otp = request.data.get('otp')  # 6-digit code
    
    # Verify password
    user = User.objects.get(email=email)
    if not check_password(password, user.password):
        return Response({'error': 'Invalid credentials'}, status=400)
    
    # Verify OTP
    totp = TOTP(user.totp_secret)
    if not totp.verify(otp):
        return Response({'error': 'Invalid OTP'}, status=400)
    
    # Generate JWT token
    token = generate_jwt_token(user)
    return Response({'access_token': token})

# Implementation I did in my MFA project:
class MFASystem:
    
    def generate_otp(self, user):
        """Generate time-based OTP"""
        totp = TOTP(user.totp_secret)
        return totp.now()
    
    def verify_otp(self, user, otp, window=1):
        """Verify OTP with time window tolerance"""
        totp = TOTP(user.totp_secret)
        return totp.verify(otp, valid_window=window)
    
    def setup_2fa(self, user):
        """Setup 2FA for user"""
        secret = TOTP.random_secret()
        user.totp_secret = secret
        user.is_2fa_enabled = True
        user.save()
        return secret
    
    def disable_2fa(self, user, password):
        """Disable 2FA with password verification"""
        if not check_password(password, user.password):
            raise ValidationError("Invalid password")
        user.is_2fa_enabled = False
        user.save()

# Benefits of MFA:
✓ Prevents unauthorized access even if password is compromised
✓ Protects against phishing
✓ Meets security compliance requirements
✓ Reduces account takeover risk
```

---

### Q2: What is SQL Injection? How do you prevent it?

**Why they ask:** Security fundamentals

**Answer:**
```python
# SQL Injection: Inserting malicious SQL code into input fields

# VULNERABLE CODE (NEVER DO THIS)
def search_user(request):
    search_term = request.GET.get('search')
    # Directly concatenating user input into SQL query!
    query = f"SELECT * FROM users WHERE name = '{search_term}'"
    users = User.objects.raw(query)
    return Response(UserSerializer(users, many=True).data)

# Attack example:
# search_term = "' OR '1'='1"
# Query becomes: SELECT * FROM users WHERE name = '' OR '1'='1'
# This returns ALL users regardless of search term!

# Worse attack:
# search_term = "'; DROP TABLE users; --"
# Query becomes: SELECT * FROM users WHERE name = ''; DROP TABLE users; --'
# This deletes the entire users table!

# SECURE CODE (ALWAYS DO THIS)

# Method 1: ORM (Best practice - Django handles escaping)
def search_user(request):
    search_term = request.GET.get('search')
    users = User.objects.filter(name__icontains=search_term)
    return Response(UserSerializer(users, many=True).data)

# Method 2: Parameterized queries
from django.db import connection

def search_user(request):
    search_term = request.GET.get('search')
    # Placeholders (%s) ensure input is properly escaped
    query = "SELECT * FROM users WHERE name LIKE %s"
    with connection.cursor() as cursor:
        cursor.execute(query, [f'%{search_term}%'])
        users = cursor.fetchall()
    return Response(users)

# Method 3: Using Django raw queries safely
users = User.objects.raw(
    'SELECT * FROM users WHERE name = %s',
    [search_term]  # Pass params separately
)

# Other security measures:

# Input validation
from django.core.validators import validate_email

def validate_input(email):
    try:
        validate_email(email)
        return True
    except ValidationError:
        return False

# Parameterized queries for complex operations
def complex_query(status, date_from, date_to):
    # Never concatenate these!
    query = '''
        SELECT * FROM orders
        WHERE status = %s
        AND created_at BETWEEN %s AND %s
    '''
    with connection.cursor() as cursor:
        cursor.execute(query, [status, date_from, date_to])
        orders = cursor.fetchall()
    return orders

# Django ORM prevents SQL injection by default:
# All of these are safe
User.objects.filter(name='test')
User.objects.filter(email__endswith='.com')
User.objects.filter(created_at__gte=date_object)
User.objects.raw('SELECT * FROM users WHERE id = %s', [user_id])
```

---

## Common Coding Problems

### Q1: Reverse a String

**Why they ask:** Basic programming skills

**Answer:**
```python
# Method 1: String slicing (Pythonic)
def reverse_string(s):
    return s[::-1]

reverse_string("Hello")  # "olleH"

# Method 2: Using reversed()
def reverse_string(s):
    return ''.join(reversed(s))

# Method 3: Loop
def reverse_string(s):
    result = ""
    for char in s:
        result = char + result
    return result

# Method 4: Recursion
def reverse_string(s):
    if len(s) == 0:
        return s
    return reverse_string(s[1:]) + s[0]

# For interview, use Method 1 (most Pythonic)
```

---

### Q2: Find Missing Number in Array

**Why they ask:** Algorithm thinking

**Answer:**
```python
# Given array of numbers 1 to n with one missing, find it

# Method 1: Sum formula (Best - O(n) time, O(1) space)
def find_missing(arr):
    n = len(arr) + 1  # Size including missing number
    expected_sum = n * (n + 1) // 2
    actual_sum = sum(arr)
    return expected_sum - actual_sum

find_missing([1, 2, 3, 5])  # 4
# Expected sum: 5 * 6 / 2 = 15
# Actual sum: 1 + 2 + 3 + 5 = 11
# Missing: 15 - 11 = 4

# Method 2: Using set (O(n) time, O(n) space)
def find_missing(arr):
    n = len(arr) + 1
    expected = set(range(1, n + 1))
    actual = set(arr)
    return (expected - actual).pop()

# Method 3: XOR operation (Clever but less readable)
def find_missing(arr):
    xor_result = 0
    for i in range(1, len(arr) + 2):
        xor_result ^= i
    for num in arr:
        xor_result ^= num
    return xor_result
```

---

### Q3: Palindrome Check

**Why they ask:** String manipulation

**Answer:**
```python
# Check if string is palindrome

# Method 1: Simple (O(n) time, O(n) space)
def is_palindrome(s):
    cleaned = ''.join(c.lower() for c in s if c.isalnum())
    return cleaned == cleaned[::-1]

is_palindrome("A man, a plan, a canal: Panama")  # True

# Method 2: Two pointers (O(n) time, O(1) space)
def is_palindrome(s):
    cleaned = ''.join(c.lower() for c in s if c.isalnum())
    left, right = 0, len(cleaned) - 1
    
    while left < right:
        if cleaned[left] != cleaned[right]:
            return False
        left += 1
        right -= 1
    
    return True
```

---

### Q4: Find Duplicate Numbers

**Why they ask:** Problem-solving skills

**Answer:**
```python
# Given array with n+1 integers between 1 and n, find duplicate

# Method 1: Floyd's Cycle Detection (Best - O(n) time, O(1) space)
def find_duplicate(nums):
    # Phase 1: Detect cycle
    slow = fast = nums[0]
    while True:
        slow = nums[slow]
        fast = nums[nums[fast]]
        if slow == fast:
            break
    
    # Phase 2: Find cycle start
    slow = nums[0]
    while slow != fast:
        slow = nums[slow]
        fast = nums[fast]
    
    return slow

find_duplicate([1, 3, 4, 2, 2])  # 2

# Method 2: Using set (O(n) time, O(n) space)
def find_duplicate(nums):
    seen = set()
    for num in nums:
        if num in seen:
            return num
        seen.add(num)
    return -1

# Method 3: Sorting (O(n log n) time, O(1) space)
def find_duplicate(nums):
    nums.sort()
    for i in range(len(nums) - 1):
        if nums[i] == nums[i + 1]:
            return nums[i]
    return -1
```

---

## Project-Specific Questions

### Q1: Tell me about your EduWise project

**Why they ask:** Assessing your real-world project experience

**Your Answer:**
```
"EduWise is an e-learning platform I developed as a final year project 
combining Software Engineering and AI disciplines.

ARCHITECTURE:
- Frontend: MERN Stack (React, Node.js, MongoDB, Express)
- Backend: Django and Python for AI components
- Cloud: Deployed on AWS for scalability

KEY FEATURES:
1. Course Management: Instructors can upload courses
2. Student Portal: Learners can access courses and earn certificates
3. AI Recommendation System: Suggests courses based on user behavior
   - Implemented collaborative filtering
   - Analyzed user interaction patterns
   - Integrated with the main backend API

TECHNICAL ACHIEVEMENTS:
- Designed RESTful APIs for authentication, course management, enrollments
- Built machine learning recommendation engine using TensorFlow/Keras
- Deployed backend 100%, frontend 40%
- Handled 1000+ concurrent users during testing
- Implemented secure JWT authentication

CHALLENGES & SOLUTIONS:
1. Performance: ML model was slow
   Solution: Implemented caching and batch processing
2. Scalability: Database queries were bottlenecks
   Solution: Added indexes, used select_related/prefetch_related
3. API integration: Difficulty connecting Django backend with React frontend
   Solution: Used CORS, proper serialization, comprehensive API docs

IMPACT:
- Project was showcased at university exhibition
- Received positive feedback from Dr. Yaseen Ul Haq and industry experts
- Real students used the platform to learn

WHAT I LEARNED:
- Full-stack development (frontend and backend)
- Machine learning in production
- AWS deployment and DevOps basics
- Team collaboration in professional projects
"
```

---

### Q2: Describe your SoftSincs RESTful API project

**Your Answer:**
```
"At SoftSincs, I developed secure RESTful APIs for authentication, 
authorization, and data management.

PROJECT SCOPE:
- Built Django REST Framework APIs
- Implemented user authentication (JWT tokens)
- Designed secure API endpoints for data operations
- Created comprehensive API documentation

AUTHENTICATION SYSTEM:
- User registration with email verification
- JWT-based authentication
- Role-based access control (admin, user, guest)
- Secure password hashing using bcrypt

AUTHORIZATION:
- Implemented permission classes:
  * IsAuthenticated: API requires login
  * IsAdmin: Only admin can access
  * IsOwnerOrReadOnly: Own data or read-only access
- Fine-grained permissions for sensitive operations

API DESIGN:
- Followed REST conventions (GET, POST, PUT, DELETE)
- Proper HTTP status codes
- Consistent response format
- Pagination for list endpoints
- Filtering and searching capabilities

SECURITY MEASURES:
- Protected against SQL injection (used ORM)
- CSRF protection
- Rate limiting to prevent abuse
- Input validation on all endpoints
- Secure headers (Content-Security-Policy, etc.)

DEPLOYMENT:
- Collaborated with DevOps team
- Docker containerization
- CI/CD pipeline setup
- Production readiness verification

IMPACT:
- APIs handled 10k+ requests daily
- 99.9% uptime
- Zero security breaches
"
```

---

## AWS & Cloud Deployment

### Q1: Explain EC2, S3, and RDS

**Why they ask:** Cloud deployment is essential

**Answer:**
```
AWS Services:

1. EC2 (Elastic Compute Cloud)
   - Virtual servers in the cloud
   - Choose instance type, OS, resources
   - Pay for compute usage
   
   Use case: Running Django application
   instance = t3.medium (CPU optimized for web app)
   OS: Ubuntu 20.04
   
   Launch Django app:
   ssh ec2-user@instance-ip
   git clone project
   python manage.py runserver 0.0.0.0:8000

2. S3 (Simple Storage Service)
   - Object storage (like hard drive in cloud)
   - Store files, images, backups
   - Highly available and durable
   
   Use case: Store user uploads, media files
   aws s3 cp file.txt s3://my-bucket/
   Access: https://my-bucket.s3.amazonaws.com/file.txt
   
   Django integration:
   from storages.backends.s3boto3 import S3Boto3Storage
   
   class MediaStorage(S3Boto3Storage):
       location = 'media'
   
   class UserProfile(models.Model):
       avatar = ImageField(storage=MediaStorage())

3. RDS (Relational Database Service)
   - Managed database service
   - MySQL, PostgreSQL, Oracle, etc.
   - Automatic backups, patches, replication
   
   Use case: Store user data, courses, enrollments
   Create RDS instance:
   - Engine: PostgreSQL 12
   - Multi-AZ deployment (high availability)
   - Backup retention: 30 days
   
   Django settings.py:
   DATABASES = {
       'default': {
           'ENGINE': 'django.db.backends.postgresql',
           'NAME': 'eduwise_db',
           'USER': 'dbuser',
           'PASSWORD': 'secure_password',
           'HOST': 'eduwise-db.c9akciq32.us-east-1.rds.amazonaws.com',
           'PORT': '5432',
       }
   }

DEPLOYMENT ARCHITECTURE:

┌─────────────┐
│   Domain    │
│ example.com │
└──────┬──────┘
       │
┌──────▼──────────────┐
│ Elastic IP (Static)│
└──────┬──────────────┘
       │
┌──────▼──────────────┐
│  Elastic Load       │
│  Balancer (ELB)     │
└──┬──────────────┬───┘
   │              │
┌──▼───┐      ┌───▼──┐
│EC2-1 │      │EC2-2 │
│Django│      │Django│
└──┬───┘      └───┬──┘
   │              │
   └──────┬───────┘
          │
    ┌─────▼──────┐
    │  RDS       │
    │ PostgreSQL │
    └────────────┘
          │
    ┌─────▼──────┐
    │  S3        │
    │  Backups   │
    └────────────┘
```

---

### Q2: How would you deploy a Django app on AWS?

**Your Answer:**
```
Deployment steps:

1. PREPARE DJANGO APP
   - Collect static files: python manage.py collectstatic
   - Create requirements.txt: pip freeze > requirements.txt
   - Set DEBUG=False in production
   - Use environment variables for secrets

2. CREATE EC2 INSTANCE
   - Launch Ubuntu 20.04 instance
   - Add security group (allow HTTP, HTTPS, SSH)
   - Allocate Elastic IP

3. SETUP SERVER
   ssh -i key.pem ubuntu@instance-ip
   
   sudo apt update && apt upgrade -y
   sudo apt install python3-pip python3-venv postgresql-client
   
   # Clone repository
   git clone https://github.com/user/project.git
   cd project
   
   # Setup Python environment
   python3 -m venv venv
   source venv/bin/activate
   pip install -r requirements.txt

4. CONFIGURE DJANGO
   # Create .env file with secrets
   echo "DEBUG=False" > .env
   echo "SECRET_KEY=your-secret-key" >> .env
   echo "DB_HOST=eduwise-db.aws.amazonaws.com" >> .env
   
   # Run migrations
   python manage.py migrate

5. SETUP WEB SERVER (Gunicorn + Nginx)
   # Install Gunicorn
   pip install gunicorn
   
   # Test Gunicorn
   gunicorn --workers 4 --bind 127.0.0.1:8000 config.wsgi:application
   
   # Create systemd service
   sudo nano /etc/systemd/system/django.service
   
   [Unit]
   Description=Django App
   After=network.target
   
   [Service]
   Type=notify
   User=ubuntu
   WorkingDirectory=/home/ubuntu/project
   ExecStart=/home/ubuntu/project/venv/bin/gunicorn \\
             --workers 4 \\
             --bind 127.0.0.1:8000 \\
             config.wsgi:application
   Restart=always
   
   [Install]
   WantedBy=multi-user.target
   
   # Start service
   sudo systemctl start django
   sudo systemctl enable django

6. SETUP NGINX (Reverse Proxy)
   sudo apt install nginx
   
   sudo nano /etc/nginx/sites-available/default
   
   server {
       listen 80;
       server_name example.com;
       
       location / {
           proxy_pass http://127.0.0.1:8000;
           proxy_set_header Host $host;
       }
       
       location /static/ {
           alias /home/ubuntu/project/static/;
       }
       
       location /media/ {
           alias /home/ubuntu/project/media/;
       }
   }
   
   sudo systemctl restart nginx

7. SETUP SSL/HTTPS (Let's Encrypt)
   sudo apt install certbot python3-certbot-nginx
   sudo certbot --nginx -d example.com

8. SETUP DATABASE (RDS)
   # RDS already manages database
   # Just ensure security group allows connection from EC2

9. SETUP S3 FOR MEDIA FILES
   # Configure in Django
   AWS_ACCESS_KEY_ID = 'your-key'
   AWS_SECRET_ACCESS_KEY = 'your-secret'
   AWS_STORAGE_BUCKET_NAME = 'my-bucket'
   
   # User uploads go to S3

10. MONITORING & BACKUPS
    - Enable CloudWatch monitoring
    - Setup automated RDS backups
    - Configure S3 versioning
    - Setup error logging (Sentry)

PRODUCTION CHECKLIST:
✓ DEBUG=False
✓ ALLOWED_HOSTS configured
✓ Secret keys stored in environment
✓ Database backed up
✓ Static files collected
✓ SSL/HTTPS enabled
✓ Error logging configured
✓ Monitoring setup
```

---

## Debugging & Troubleshooting

### Q1: How do you debug a slow Django API?

**Why they ask:** Practical problem-solving

**Your Answer:**
```python
# Debugging slow API:

1. IDENTIFY THE BOTTLENECK

# Enable logging
import logging
logger = logging.getLogger(__name__)

@api_view(['GET'])
def get_courses(request):
    import time
    start = time.time()
    
    courses = Course.objects.all()
    duration = time.time() - start
    logger.info(f"Query took {duration}s")
    
    return Response(CourseSerializer(courses, many=True).data)

2. USE DJANGO DEBUG TOOLBAR (Development only)
   pip install django-debug-toolbar
   
   # Shows:
   - Query count
   - SQL execution time
   - Template rendering time
   - Cache hits/misses

3. ANALYZE QUERIES

# Tool: django.db.connection
from django.db import connection, reset_queries
from django.conf import settings

settings.DEBUG = True

def get_courses(request):
    reset_queries()
    
    courses = Course.objects.all()
    for course in courses:
        print(course.instructor.name)  # N+1 problem!
    
    print(f"Total queries: {len(connection.queries)}")
    for query in connection.queries:
        print(query['sql'], query['time'])

# Output shows 1 query for courses + N queries for instructors (N+1 problem)

4. FIX: Use select_related/prefetch_related

# Bad: N+1 queries
courses = Course.objects.all()

# Good: 2 queries total
courses = Course.objects.select_related('instructor')

5. ADD INDEXES

class Course(models.Model):
    title = models.CharField(max_length=200, db_index=True)
    category = models.CharField(max_length=50, db_index=True)
    instructor = models.ForeignKey(User, on_delete=models.CASCADE, db_index=True)

6. IMPLEMENT CACHING

from django.views.decorators.cache import cache_page

@cache_page(60 * 5)  # Cache for 5 minutes
def get_courses(request):
    courses = Course.objects.all()
    return Response(CourseSerializer(courses, many=True).data)

7. ASYNC VIEWS (if I/O bound)

@sync_and_async_middleware
async def get_courses(request):
    courses = await sync_to_async(
        Course.objects.all().select_related('instructor')
    )()
    return JsonResponse(...)

8. OPTIMIZE SERIALIZER

# Inefficient: Each object triggers queries
class CourseSerializer(serializers.ModelSerializer):
    instructor = UserSerializer()  # Additional query per course
    
    class Meta:
        model = Course
        fields = ['id', 'title', 'instructor']

# Efficient: Use related name
class CourseSerializer(serializers.ModelSerializer):
    instructor_name = serializers.CharField(
        source='instructor.name',
        read_only=True
    )
    
    class Meta:
        model = Course
        fields = ['id', 'title', 'instructor_name']

TOOLS & MONITORING:
- Django Debug Toolbar (development)
- Sentry (production error tracking)
- New Relic (APM monitoring)
- CloudWatch (AWS metrics)
- Datadog (full-stack monitoring)
```

---

### Q2: How would you debug a 500 Internal Server Error?

**Your Answer:**
```python
# Steps to debug 500 error:

1. CHECK LOGS
   tail -f /var/log/django/app.log
   tail -f /var/log/nginx/error.log

2. ENABLE DEBUG (Development only!)
   DEBUG = True  # in settings.py
   Browser shows detailed error traceback

3. USE LOGGING

import logging
logger = logging.getLogger(__name__)

try:
    result = process_data(request.data)
except Exception as e:
    logger.error(f"Error processing data: {str(e)}", exc_info=True)
    return Response({'error': 'Processing failed'}, status=500)

4. USE DEBUGGER (pdb)

def my_view(request):
    import pdb; pdb.set_trace()  # Execution stops here
    # You can inspect variables, step through code
    courses = Course.objects.all()

5. CHECK COMMON ISSUES

# Database connection error
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'HOST': 'correct-host-here',  # Check this!
    }
}

# Missing environment variable
SECRET_KEY = os.getenv('SECRET_KEY')  # Ensure it's set

# Permission denied on file
# Ensure Django has permission to write to directories

# Template not found
TEMPLATES = [
    {
        'DIRS': [os.path.join(BASE_DIR, 'templates')],  # Check paths!
    }
]

6. USE SENTRY (Production)
import sentry_sdk

sentry_sdk.init(
    "https://key@sentry.io/project",
    traces_sample_rate=1.0
)

# All errors automatically captured and reported

7. TEST IN ISOLATION

# Test database connection
python manage.py shell
>>> from django.db import connection
>>> cursor = connection.cursor()
>>> cursor.execute("SELECT 1")

# Test specific function
python manage.py shell
>>> from myapp.views import problematic_function
>>> problematic_function()
```

---

## Final Review

### Key Points to Remember:

**Python:**
- GIL affects multi-threading
- List comprehensions are Pythonic
- Decorators are powerful for cross-cutting concerns

**Django:**
- MTV architecture: Model, Template, View
- ORM prevents SQL injection
- Signals for event-driven logic

**Django REST Framework:**
- Serializers handle model to JSON conversion
- ViewSets provide CRUD operations automatically
- Permissions control access to endpoints

**Performance:**
- Use select_related/prefetch_related
- Add database indexes on frequently queried fields
- Implement caching for static data

**Security:**
- Never concatenate user input into queries (use ORM)
- Implement proper authentication (JWT)
- Use permission classes for authorization

**Deployment:**
- Use Gunicorn as app server
- Use Nginx as reverse proxy
- Store secrets in environment variables
- Use managed services (RDS, S3) on AWS

**Debugging:**
- Use Django Debug Toolbar in development
- Enable logging in production
- Use Sentry for error tracking

---

## Good Luck with Your Interview!

Remember:
1. **Practice these answers out loud** - Don't just read them
2. **Understand the concepts** - Don't memorize, understand why
3. **Use real examples** - Reference your own projects (EduWise, MFA System)
4. **Ask clarifying questions** - If you don't understand, ask
5. **Think before answering** - Take a breath, don't rush
6. **Be honest** - If you don't know something, say "I'm not sure, but I would..."

---

**End of Technical Interview Preparation Guide**
