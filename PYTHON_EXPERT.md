# Python Expert - Enterprise and Specialized Knowledge
## Master Advanced Systems, Architecture, and Specialized Domains

---

## Table of Contents
1. [CPython Architecture](#cpython-architecture)
2. [Advanced Meta-programming](#advanced-meta-programming)
3. [C Extensions and FFI](#c-extensions-and-ffi)
4. [Distributed Systems](#distributed-systems)
5. [Advanced Async Patterns](#advanced-async-patterns)
6. [Performance Engineering](#performance-engineering)
7. [Advanced Concurrency](#advanced-concurrency)
8. [Database Advanced](#database-advanced)
9. [Web Frameworks Deep Dive](#web-frameworks-deep-dive)
10. [Security Advanced](#security-advanced)
11. [Microservices Architecture](#microservices-architecture)
12. [Machine Learning Integration](#machine-learning-integration)
13. [DevOps and Deployment](#devops-and-deployment)
14. [Expert Projects](#expert-projects)

---

## CPYTHON ARCHITECTURE

### Python Virtual Machine

```python
import sys
import dis

# Python code gets compiled to bytecode
code = compile("x = 5; y = 10; z = x + y", "<string>", "exec")

# View bytecode instructions
dis.dis(code)

# The CPython VM executes bytecode instructions
# Understanding the stack machine model:

def add_numbers():
    x = 5
    y = 10
    return x + y

# LOAD_CONST 5
# STORE_FAST x
# LOAD_CONST 10
# STORE_FAST y
# LOAD_FAST x
# LOAD_FAST y
# BINARY_ADD
# RETURN_VALUE
```

### Reference Counting and Garbage Collection

```python
import gc
import sys

# Every object has reference count
obj = []
print(sys.getrefcount(obj))  # 2 (obj + function parameter)

ref = obj
print(sys.getrefcount(obj))  # 3

del ref
print(sys.getrefcount(obj))  # 2

# Cyclic garbage collection
class Node:
    def __init__(self, value):
        self.value = value
        self.next = None

# Circular reference
a = Node(1)
b = Node(2)
a.next = b
b.next = a  # Circular!

# CPython's ref counting handles most
# But cycles need GC

stats = gc.get_stats()
print(f"Collections: {stats}")

# Force collection
collected = gc.collect()
print(f"Objects collected: {collected}")
```

### Object Structure and Optimization

```python
import sys
from types import SimpleNamespace

# CPython objects have overhead
empty_dict = {}
print(f"Empty dict size: {sys.getsizeof(empty_dict)} bytes")

# String interning optimization
s1 = "hello"
s2 = "hello"
print(s1 is s2)  # Output: True (same object in memory)

# Integer caching
a = 256
b = 256
print(a is b)    # Output: True (cached)

c = 257
d = 257
print(c is d)    # Output: False (not cached)

# Slots for memory efficiency
class Regular:
    def __init__(self, x):
        self.x = x

class Optimized:
    __slots__ = ['x']
    def __init__(self, x):
        self.x = x

print(f"Regular: {sys.getsizeof(Regular(5))} bytes")
print(f"Optimized: {sys.getsizeof(Optimized(5))} bytes")
```

### Peephole Optimization

```python
import dis

# CPython optimizes simple expressions
def with_constants():
    return 2 + 3  # Optimized at compile time to 5

def with_variables(a, b):
    return a + b  # Cannot optimize

dis.dis(with_constants)
# Shows LOAD_CONST 5 (optimized!)

dis.dis(with_variables)
# Shows BINARY_ADD
```

---

## ADVANCED META-PROGRAMMING

### AST (Abstract Syntax Tree) Manipulation

```python
import ast
import inspect

# Parse code into AST
code = """
def greet(name):
    return f"Hello {name}"
"""

tree = ast.parse(code)
print(ast.dump(tree, indent=2))

# Find all function definitions
class FunctionFinder(ast.NodeVisitor):
    def __init__(self):
        self.functions = []
    
    def visit_FunctionDef(self, node):
        self.functions.append(node.name)
        self.generic_visit(node)

finder = FunctionFinder()
finder.visit(tree)
print(finder.functions)  # Output: ['greet']

# Transform AST
class DebugTransformer(ast.NodeTransformer):
    def visit_Call(self, node):
        # Add print before each function call
        self.generic_visit(node)
        return node

# Compile modified AST
modified = DebugTransformer().visit(tree)
code_obj = compile(modified, '<transformed>', 'exec')
```

### Dynamic Code Generation

```python
# Generate functions at runtime
def create_getter(attr_name):
    code = f"""
def getter(obj):
    return obj.{attr_name}
"""
    namespace = {}
    exec(code, namespace)
    return namespace['getter']

# Create getters dynamically
class User:
    def __init__(self, name, age):
        self.name = name
        self.age = age

get_name = create_getter('name')
get_age = create_getter('age')

user = User("Alice", 10)
print(get_name(user))  # Output: Alice
print(get_age(user))   # Output: 10

# Code generation with templates
def create_data_class(name, fields):
    field_list = ", ".join(fields)
    assign_list = "\n        ".join([f"self.{f} = {f}" for f in fields])
    
    code = f"""
class {name}:
    def __init__(self, {field_list}):
        {assign_list}
    
    def __repr__(self):
        return f"{name}({field_list})"
"""
    namespace = {}
    exec(code, namespace)
    return namespace[name]

# Create class dynamically
Person = create_data_class('Person', ['name', 'age', 'email'])
person = Person("Alice", 10, "alice@example.com")
print(person)  # Output: Person(name, age, email)
```

### Runtime Code Introspection

```python
import inspect
from typing import get_type_hints

def complex_function(a: int, b: str, c: list = None) -> dict:
    """Complex function documentation"""
    return {"a": a, "b": b, "c": c}

# Get signature
sig = inspect.signature(complex_function)
print(f"Signature: {sig}")

# Get parameters
for param_name, param in sig.parameters.items():
    print(f"{param_name}: {param.annotation}")

# Get type hints
hints = get_type_hints(complex_function)
print(f"Type hints: {hints}")

# Get source code
source = inspect.getsource(complex_function)
print(f"Source:\n{source}")

# Get documentation
doc = inspect.getdoc(complex_function)
print(f"Doc: {doc}")

# Get line number
line = inspect.getsourcelines(complex_function)[1]
print(f"Defined at line: {line}")
```

---

## C EXTENSIONS AND FFI

### Using ctypes for C Libraries

```python
import ctypes
import ctypes.util

# Load C library
libc = ctypes.CDLL(ctypes.util.find_library("c"))

# Use C functions
# Example: strlen function
libc.strlen.argtypes = [ctypes.c_char_p]
libc.strlen.restype = ctypes.c_size_t

result = libc.strlen(b"hello")
print(f"Length: {result}")  # Output: 5

# Working with structures
class Point(ctypes.Structure):
    _fields_ = [("x", ctypes.c_int), ("y", ctypes.c_int)]

point = Point(10, 20)
print(f"Point: ({point.x}, {point.y})")

# Callbacks
CALLBACK_TYPE = ctypes.CFUNCTYPE(ctypes.c_int, ctypes.c_int)

def callback_func(n):
    return n * 2

callback = CALLBACK_TYPE(callback_func)
# Pass to C function expecting callback
```

### CFFI (C Foreign Function Interface)

```python
# More Pythonic than ctypes

from cffi import FFI

ffi = FFI()

# Define C functions
ffi.cdef("""
    int add(int a, int b);
    char* greet(const char* name);
""")

# Compile or load library
# C = ffi.verify(source_code)

# Use like Python
# result = C.add(5, 3)
# greeting = ffi.string(C.greet(b"Alice"))
```

### Building Python C Extensions

```cython
# setup.py for extension
from setuptools import setup
from Cython.Build import cythonize

setup(
    name='my_extension',
    ext_modules=cythonize("my_module.pyx")
)

# my_module.pyx
def fast_sum(int n):
    """Sum using Cython (type declarations make it fast)"""
    cdef int total = 0
    cdef int i
    for i in range(n):
        total += i
    return total

# Build: python setup.py build_ext --inplace
# Use: from my_module import fast_sum
```

---

## DISTRIBUTED SYSTEMS

### Message Queues with RabbitMQ

```python
import pika
import json

# Producer
def publish_message(message):
    connection = pika.BlockingConnection(
        pika.ConnectionParameters('localhost')
    )
    channel = connection.channel()
    
    channel.queue_declare(queue='tasks', durable=True)
    
    channel.basic_publish(
        exchange='',
        routing_key='tasks',
        body=json.dumps(message),
        properties=pika.BasicProperties(delivery_mode=2)
    )
    
    connection.close()

# Consumer
def consume_messages():
    connection = pika.BlockingConnection(
        pika.ConnectionParameters('localhost')
    )
    channel = connection.channel()
    
    channel.queue_declare(queue='tasks', durable=True)
    
    def callback(ch, method, properties, body):
        message = json.loads(body)
        print(f"Received: {message}")
        ch.basic_ack(delivery_tag=method.delivery_tag)
    
    channel.basic_qos(prefetch_count=1)
    channel.basic_consume(queue='tasks', on_message_callback=callback)
    
    channel.start_consuming()

# Usage
publish_message({"task": "process", "data": [1, 2, 3]})
consume_messages()
```

### Kafka Streaming

```python
from kafka import KafkaProducer, KafkaConsumer
import json

# Producer
producer = KafkaProducer(
    bootstrap_servers=['localhost:9092'],
    value_serializer=lambda v: json.dumps(v).encode('utf-8')
)

for i in range(10):
    producer.send('my-topic', {"id": i, "value": i * 2})

producer.flush()

# Consumer
consumer = KafkaConsumer(
    'my-topic',
    bootstrap_servers=['localhost:9092'],
    value_deserializer=lambda m: json.loads(m.decode('utf-8')),
    group_id='my-group'
)

for message in consumer:
    print(f"Received: {message.value}")
```

### Redis for Caching

```python
import redis
import json

# Connect to Redis
r = redis.Redis(host='localhost', port=6379, db=0)

# Set and get
r.set('mykey', 'myvalue')
print(r.get('mykey'))  # Output: b'myvalue'

# With expiration
r.setex('session', 3600, json.dumps({'user_id': 1}))

# Incrementing
r.incr('counter')

# Lists
r.rpush('mylist', 'a', 'b', 'c')
print(r.lrange('mylist', 0, -1))

# Sets
r.sadd('myset', 'member1', 'member2')
print(r.smembers('myset'))

# Hash maps
r.hset('user:1', 'name', 'Alice')
r.hset('user:1', 'age', '10')
print(r.hgetall('user:1'))

# Pub/Sub
def publisher():
    r.publish('channel', 'Hello from publisher')

def subscriber():
    sub = r.pubsub()
    sub.subscribe('channel')
    for message in sub.listen():
        print(message)
```

---

## ADVANCED ASYNC PATTERNS

### Event Loop Control

```python
import asyncio

# Create and run custom event loop
async def main():
    await asyncio.sleep(1)
    print("Done")

# Method 1: asyncio.run (recommended)
asyncio.run(main())

# Method 2: Manual event loop
loop = asyncio.new_event_loop()
asyncio.set_event_loop(loop)
try:
    loop.run_until_complete(main())
finally:
    loop.close()

# Method 3: Get running loop
async def nested():
    loop = asyncio.get_running_loop()
    print(f"Running in: {loop}")

asyncio.run(nested())
```

### Async Context Managers

```python
import asyncio

class AsyncResource:
    async def __aenter__(self):
        print("Acquiring resource")
        await asyncio.sleep(0.1)
        return self
    
    async def __aexit__(self, exc_type, exc_val, exc_tb):
        print("Releasing resource")
        await asyncio.sleep(0.1)

async def main():
    async with AsyncResource() as resource:
        print("Using resource")

asyncio.run(main())
```

### Concurrent.futures Integration

```python
import asyncio
from concurrent.futures import ThreadPoolExecutor
import time

def blocking_io(x):
    """CPU bound or blocking operation"""
    time.sleep(2)
    return x * 2

async def main():
    loop = asyncio.get_event_loop()
    
    # Run blocking function in thread pool
    with ThreadPoolExecutor() as executor:
        result = await loop.run_in_executor(executor, blocking_io, 5)
        print(f"Result: {result}")

asyncio.run(main())
```

### Task Cancellation and Timeouts

```python
import asyncio

async def worker():
    try:
        for i in range(10):
            print(f"Working {i}")
            await asyncio.sleep(1)
    except asyncio.CancelledError:
        print("Task cancelled!")
        raise

async def main():
    # Create task
    task = asyncio.create_task(worker())
    
    # Cancel after 3 seconds
    try:
        await asyncio.wait_for(task, timeout=3)
    except asyncio.TimeoutError:
        print("Task timed out")
        task.cancel()

asyncio.run(main())
```

---

## PERFORMANCE ENGINEERING

### Profiling with Flame Graphs

```python
import cProfile
import pstats
import io

def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

# Profile the code
profiler = cProfile.Profile()
profiler.enable()

for i in range(30):
    fibonacci(i)

profiler.disable()

# Analyze results
s = io.StringIO()
ps = pstats.Stats(profiler, stream=s).sort_stats('cumulative')
ps.print_stats(20)
print(s.getvalue())

# For flame graphs, use py-spy:
# pip install py-spy
# py-spy record -o profile.svg python script.py
```

### Vectorization with NumPy

```python
import numpy as np
import time

# Slow: Python loop
def slow_sum(n):
    total = 0
    for i in range(n):
        total += i
    return total

# Fast: NumPy vectorization
def fast_sum(n):
    return np.arange(n).sum()

# Benchmark
n = 1000000

start = time.time()
slow_sum(n)
print(f"Python: {time.time() - start:.4f}s")

start = time.time()
fast_sum(n)
print(f"NumPy: {time.time() - start:.4f}s")
```

### Jit Compilation with Numba

```python
from numba import jit
import time

@jit(nopython=True)
def fibonacci_jit(n):
    """JIT compiled version"""
    if n < 2:
        return n
    return fibonacci_jit(n-1) + fibonacci_jit(n-2)

def fibonacci_python(n):
    """Pure Python"""
    if n < 2:
        return n
    return fibonacci_python(n-1) + fibonacci_python(n-2)

# Benchmark
n = 35

start = time.time()
fibonacci_python(n)
print(f"Python: {time.time() - start:.4f}s")

start = time.time()
fibonacci_jit(n)
print(f"JIT: {time.time() - start:.4f}s")
```

---

## ADVANCED CONCURRENCY

### Lock-Free Programming Concepts

```python
import threading
import time
from queue import Queue

# Lock-free queue (thread-safe)
q = Queue()

def producer():
    for i in range(100):
        q.put(i)

def consumer():
    while True:
        try:
            item = q.get(timeout=1)
            print(f"Consumed: {item}")
        except:
            break

# Run
threads = []
threads.append(threading.Thread(target=producer))
threads.append(threading.Thread(target=consumer))

for t in threads:
    t.start()
for t in threads:
    t.join()
```

### Advanced Synchronization

```python
import threading

# Barrier: wait for N threads to reach point
barrier = threading.Barrier(3)

def worker(name):
    print(f"{name} waiting")
    barrier.wait()
    print(f"{name} proceeding")

threads = [threading.Thread(target=worker, args=(f"T{i}",)) for i in range(3)]
for t in threads:
    t.start()
for t in threads:
    t.join()

# Semaphore: limit resource access
sem = threading.Semaphore(2)

def limited_resource():
    with sem:
        print("Using resource")
        import time
        time.sleep(1)

threads = [threading.Thread(target=limited_resource) for _ in range(5)]
for t in threads:
    t.start()
for t in threads:
    t.join()
```

---

## DATABASE ADVANCED

### SQLAlchemy ORM Advanced

```python
from sqlalchemy import create_engine, Column, Integer, String, ForeignKey
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import relationship, sessionmaker
from sqlalchemy import event

Base = declarative_base()

class User(Base):
    __tablename__ = 'users'
    
    id = Column(Integer, primary_key=True)
    name = Column(String)
    posts = relationship('Post', back_populates='author')

class Post(Base):
    __tablename__ = 'posts'
    
    id = Column(Integer, primary_key=True)
    title = Column(String)
    user_id = Column(Integer, ForeignKey('users.id'))
    author = relationship('User', back_populates='posts')

# Create engine
engine = create_engine('sqlite:///db.sqlite')
Base.metadata.create_all(engine)

# Session
Session = sessionmaker(bind=engine)
session = Session()

# Query with relationships
users = session.query(User).all()
for user in users:
    print(f"{user.name}: {len(user.posts)} posts")

# Lazy vs eager loading
from sqlalchemy import joinedload
users = session.query(User).options(joinedload(User.posts)).all()
```

### Raw SQL with Parameterization

```python
from sqlalchemy import text, create_engine

engine = create_engine('sqlite:///db.sqlite')

# Parameterized query (safe from SQL injection)
with engine.connect() as conn:
    result = conn.execute(
        text("SELECT * FROM users WHERE name = :name"),
        {"name": "Alice"}
    )
    for row in result:
        print(row)
```

### NoSQL with MongoDB

```python
from pymongo import MongoClient
from datetime import datetime

# Connect
client = MongoClient('mongodb://localhost:27017/')
db = client['myapp']
collection = db['users']

# Insert
collection.insert_one({
    'name': 'Alice',
    'email': 'alice@example.com',
    'created': datetime.now()
})

# Query
user = collection.find_one({'name': 'Alice'})

# Aggregation pipeline
pipeline = [
    {'$match': {'age': {'$gte': 18}}},
    {'$group': {'_id': '$city', 'count': {'$sum': 1}}},
    {'$sort': {'count': -1}}
]
results = collection.aggregate(pipeline)
```

---

## WEB FRAMEWORKS DEEP DIVE

### FastAPI with Dependencies

```python
from fastapi import FastAPI, Depends, HTTPException
from sqlalchemy.orm import Session

app = FastAPI()

# Dependency injection
def get_db():
    db = Session()
    try:
        yield db
    finally:
        db.close()

# Path parameters with validation
@app.get("/users/{user_id}")
async def get_user(user_id: int, db: Session = Depends(get_db)):
    user = db.query(User).filter(User.id == user_id).first()
    if not user:
        raise HTTPException(status_code=404, detail="Not found")
    return user

# Request body with validation
from pydantic import BaseModel

class UserCreate(BaseModel):
    name: str
    email: str

@app.post("/users")
async def create_user(user: UserCreate, db: Session = Depends(get_db)):
    db_user = User(name=user.name, email=user.email)
    db.add(db_user)
    db.commit()
    db.refresh(db_user)
    return db_user

# Middleware
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

### Django Signals and Middleware

```python
from django.db.models.signals import post_save, post_delete
from django.dispatch import receiver
from django.utils.decorators import method_decorator
from django.views.decorators.cache import cache_page

# Signal receivers
@receiver(post_save, sender=User)
def user_created(sender, instance, created, **kwargs):
    if created:
        print(f"New user: {instance.name}")

@receiver(post_delete, sender=User)
def user_deleted(sender, instance, **kwargs):
    print(f"User deleted: {instance.name}")

# Middleware
class LoggingMiddleware:
    def __init__(self, get_response):
        self.get_response = get_response
    
    def __call__(self, request):
        print(f"Request: {request.path}")
        response = self.get_response(request)
        print(f"Response: {response.status_code}")
        return response

# Caching
class CachedListView(APIView):
    @method_decorator(cache_page(60 * 5))
    def get(self, request):
        # Cached for 5 minutes
        return Response(data)
```

---

## SECURITY ADVANCED

### Cryptography

```python
from cryptography.fernet import Fernet
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.kdf.pbkdf2 import PBKDF2

# Symmetric encryption
key = Fernet.generate_key()
cipher = Fernet(key)

plaintext = b"Secret message"
ciphertext = cipher.encrypt(plaintext)
decrypted = cipher.decrypt(ciphertext)

# Key derivation from password
password = b"mypassword"
salt = b'salt_value'  # Should be random
kdf = PBKDF2(
    algorithm=hashes.SHA256(),
    length=32,
    salt=salt,
    iterations=100000,
)
key = kdf.derive(password)

# Asymmetric encryption (RSA)
from cryptography.hazmat.primitives.asymmetric import rsa

private_key = rsa.generate_private_key(
    public_exponent=65537,
    key_size=2048,
)
public_key = private_key.public_key()

message = b"Confidential"
ciphertext = public_key.encrypt(message, rsa.OAEP())
plaintext = private_key.decrypt(ciphertext, rsa.OAEP())
```

### JWT Authentication

```python
import jwt
from datetime import datetime, timedelta

# Create token
payload = {
    'user_id': 1,
    'email': 'alice@example.com',
    'exp': datetime.utcnow() + timedelta(hours=24)
}

token = jwt.encode(
    payload,
    'secret_key',
    algorithm='HS256'
)

# Verify token
try:
    decoded = jwt.decode(
        token,
        'secret_key',
        algorithms=['HS256']
    )
    print(f"User: {decoded['user_id']}")
except jwt.InvalidTokenError:
    print("Invalid token")
```

---

## MICROSERVICES ARCHITECTURE

### Service Communication

```python
# Service 1: User Service
from flask import Flask, jsonify

app1 = Flask(__name__)

@app1.route('/users/<int:user_id>')
def get_user(user_id):
    return jsonify({'id': user_id, 'name': 'Alice'})

# Service 2: Order Service
from flask import Flask
import requests

app2 = Flask(__name__)

@app2.route('/orders/<int:user_id>')
def get_orders(user_id):
    # Call User Service
    response = requests.get(f'http://user-service:5000/users/{user_id}')
    user = response.json()
    
    return jsonify({
        'user': user,
        'orders': [{'id': 1, 'total': 100}]
    })

# Use service discovery (Consul, Eureka)
# Or API Gateway (Kong, Ambassador)
```

### Event-Driven Architecture

```python
# Event sourcing pattern
class EventStore:
    def __init__(self):
        self.events = []
    
    def append(self, event):
        self.events.append(event)
    
    def get_events(self, aggregate_id):
        return [e for e in self.events if e['aggregate_id'] == aggregate_id]

# Commands and events
class UserCreatedEvent:
    def __init__(self, user_id, name, email):
        self.aggregate_id = user_id
        self.type = 'UserCreated'
        self.data = {'name': name, 'email': email}

class UserNameChangedEvent:
    def __init__(self, user_id, new_name):
        self.aggregate_id = user_id
        self.type = 'UserNameChanged'
        self.data = {'name': new_name}

# Saga pattern for distributed transactions
class CreateOrderSaga:
    async def execute(self, order):
        # Step 1: Create order
        await self.order_service.create(order)
        
        try:
            # Step 2: Reserve inventory
            await self.inventory_service.reserve(order.items)
            
            # Step 3: Process payment
            await self.payment_service.charge(order.amount)
        
        except Exception as e:
            # Compensating transactions (rollback)
            await self.inventory_service.release(order.items)
            await self.order_service.cancel(order)
            raise
```

---

## MACHINE LEARNING INTEGRATION

### Scikit-learn Pipeline

```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score

# Create pipeline
pipeline = Pipeline([
    ('scaler', StandardScaler()),
    ('classifier', RandomForestClassifier())
])

# Load data
X = [[1, 2], [2, 3], [3, 4], [4, 5]]
y = [0, 0, 1, 1]

# Split data
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2
)

# Train
pipeline.fit(X_train, y_train)

# Predict
predictions = pipeline.predict(X_test)
accuracy = accuracy_score(y_test, predictions)
print(f"Accuracy: {accuracy}")

# Hyperparameter tuning
from sklearn.model_selection import GridSearchCV

params = {
    'classifier__n_estimators': [10, 50, 100],
    'classifier__max_depth': [5, 10, None]
}

grid_search = GridSearchCV(pipeline, params, cv=5)
grid_search.fit(X_train, y_train)
print(f"Best params: {grid_search.best_params_}")
```

### TensorFlow Integration

```python
import tensorflow as tf
from tensorflow import keras

# Create model
model = keras.Sequential([
    keras.layers.Dense(128, activation='relu', input_shape=(784,)),
    keras.layers.Dropout(0.2),
    keras.layers.Dense(64, activation='relu'),
    keras.layers.Dense(10, activation='softmax')
])

# Compile
model.compile(
    optimizer='adam',
    loss='sparse_categorical_crossentropy',
    metrics=['accuracy']
)

# Train
# model.fit(X_train, y_train, epochs=10, batch_size=32)

# Predict
# predictions = model.predict(X_test)

# Save/Load
model.save('model.h5')
loaded_model = keras.models.load_model('model.h5')
```

---

## DEVOPS AND DEPLOYMENT

### Docker for Python

```dockerfile
# Dockerfile
FROM python:3.11-slim

WORKDIR /app

# Copy requirements
COPY requirements.txt .

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy code
COPY . .

# Expose port
EXPOSE 8000

# Run application
CMD ["python", "app.py"]
```

### Docker Compose

```yaml
# docker-compose.yml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://user:pass@db:5432/app
    depends_on:
      - db
    
  db:
    image: postgres:14
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=pass
      - POSTGRES_DB=app
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

### CI/CD with GitHub Actions

```yaml
# .github/workflows/test.yml
name: Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v2
    
    - name: Set up Python
      uses: actions/setup-python@v2
      with:
        python-version: 3.11
    
    - name: Install dependencies
      run: |
        pip install -r requirements.txt
        pip install pytest
    
    - name: Run tests
      run: pytest
    
    - name: Upload coverage
      uses: codecov/codecov-action@v2
```

---

## EXPERT PROJECTS

### Project 1: Distributed Task Queue

```python
import celery
from celery import Celery

app = Celery(
    'tasks',
    broker='redis://localhost:6379',
    backend='redis://localhost:6379'
)

@app.task
def process_image(image_path):
    """Long-running task"""
    # Process image
    return f"Processed {image_path}"

# Usage
task = process_image.delay('image.jpg')
print(task.id)

# Check status
print(task.status)
print(task.result)

# Scheduled tasks
from celery.schedules import crontab

app.conf.beat_schedule = {
    'daily-cleanup': {
        'task': 'tasks.cleanup_old_files',
        'schedule': crontab(hour=2, minute=0),
    },
}
```

### Project 2: Real-time Chat with WebSockets

```python
from fastapi import FastAPI, WebSocket
from fastapi.responses import HTMLResponse

app = FastAPI()

# Store connections
connections = []

@app.websocket("/ws/{client_id}")
async def websocket_endpoint(websocket: WebSocket, client_id: int):
    await websocket.accept()
    connections.append(websocket)
    
    try:
        while True:
            data = await websocket.receive_text()
            
            # Broadcast to all
            for connection in connections:
                await connection.send_text(f"Client {client_id}: {data}")
    
    except Exception:
        connections.remove(websocket)
```

### Project 3: Monitoring and Logging System

```python
import logging
from logging.handlers import RotatingFileHandler
import time
from prometheus_client import Counter, Histogram, start_http_server

# Setup logging
logger = logging.getLogger(__name__)
handler = RotatingFileHandler('app.log', maxBytes=10000, backupCount=5)
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
handler.setFormatter(formatter)
logger.addHandler(handler)

# Prometheus metrics
request_count = Counter('requests_total', 'Total requests')
request_duration = Histogram('request_duration_seconds', 'Request duration')

@request_duration.time()
def process_request(data):
    request_count.inc()
    logger.info(f"Processing request: {data}")
    time.sleep(1)
    logger.info("Request completed")

# Start metrics server
start_http_server(8000)

# Usage
process_request({'id': 1})
```

---

## Summary: Expert Checklist

| Category | Key Concepts |
|----------|--------------|
| **CPython** | Bytecode, GC, reference counting, optimization |
| **Meta-programming** | AST, code generation, introspection |
| **FFI** | ctypes, CFFI, C extensions |
| **Distributed** | Message queues, event sourcing, sagas |
| **Async** | Event loops, task control, integration |
| **Performance** | Profiling, vectorization, JIT |
| **Database** | ORM, raw SQL, NoSQL |
| **Security** | Cryptography, JWT, secure coding |
| **DevOps** | Docker, CI/CD, monitoring |
| **ML/AI** | Scikit-learn, TensorFlow integration |

---

*This completes the Expert level - the pinnacle of Python knowledge for building enterprise systems and specialized applications!*

