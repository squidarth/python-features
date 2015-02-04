# Intermediate to advanced Python features

### Passing functions as first-class objects

Just as numbers and strings can be passed to functions as arguments, functions themselves can be passed as well!

```python
def double(x):
    return x * 2
    
def copy_list_with_operation(operation, ls):
    new_ls= []
    for item in ls:
        new_ls.append(operation(item))
    return new_ls
    
copy_list_with_operation(double, [1,2,3,4])
```

### Lambda

In the above example, I called `copy_list_with_operation` with the name of the function that we want to apply to every element of the list.  `lambda` is a python keyword that allows you create a function quickly without a name.

```python
def copy_list_with_operation(operation, ls):
    new_ls= []
    for item in ls:
        new_ls.append(operation(item))
    return new_ls
    
copy_list_with_operation(lambda x: x * 2, [1,2,3,4])
```

### Decorators

Ever seen the `@` sign in Python code? These refer to what are called "decorators", and when added to the top of a function as follows:

```python
@cached
def my_hard_func(x):
    sum = 0
    for i in range(x):
        sum += i
```

adds some behavior to the function.  In the example I will give, we will build a decorator that takes the result of a function and adds it to a dictionary, where the argument to the function is the key.  When the function is called again with that argument, pull it from the dictionary instead of executing the body of the function.

```python
def cached(func):
    cache = {}
    def decorated_function(arg):
        if arg not in cache:
            cache[arg] = func(arg)
        return cache[arg]

    return decorated_function

def my_hard_func(x):
    sum = 0
    for i in range(x):
        sum += i
        
decorated_function = cached(my_hard_func)

# The following is equivalent

@cached
def my_hard_func(x):
    sum = 0
    for i in range(x):
        sum += i
```

Decorators can also be passed arguments, which makes things a little more confusing:

```python
def cached(limit):
    def decorator(func):
        cache = {}
        def decorated_function(arg):
            if arg in cache:
                return cache[arg]
            else:
                ret = func(arg)
                if len(cache) < limit
                    cache[arg] = ret
                return ret
        return decorated_function
    return decorator
            

@cached(limit=100)
def my_hard_func(x):
    sum = 0
    for i in range(x):
        sum += i
```

### Iterators and Generators

#### Iterators

In Python, I'm sure that you've seen the following pattern:

```
ls = [1,2,3,4]

for item in ls:
    print item
```

You can do this lists, as I showed above, you can also do this with dictionaries:

```
properties = {'name': "Sid", "location": "south san francisco"}
for property, value in properties.iteritems():
    print("%s %s" % (property, value))
```

This isn't some magic, built in thing in python that only belongs to dictionaries and lists. You too can write your own iterators!

```python
class numbers(object):
    def __init__(self):
        self.initial_number = 0
    
    def __iter__(self):
        return self
    
    def next(self):
        return self.initial_number
        self.initial_number += 1

number_iterator = numbers()
for i in number_iterator:
    print i
```

All you have to do is define an appropriate `next()` function for the iterator.

#### Generators

Generators are similar to iterators, except they can only be used once, and are much more complicated.

Simple example:

```python
def simple_generator():
    yield 1
    yield 2
    yield 3

gen = simple_generator()

for item in gen:
    print item
    
# Prints:
# 1
# 2
# 2
```

The `yield` keyword is very similar to return, except that it "pauses" the function and keeps its state. On each of iteration of the generator, it moves the next `yield` statement.

### with

The `with` statement is used as follows:

```python
# Open a file and write the string "hello" to it
with open('filename.txt') as f:
    f.write('hello')
```

This keyword allows you to specify some code that you would like to be executed as a pair, with stuff that happens in the middle.  In this case, that means opening and closing a file (we wouldn't want a case in which a file is opened without it being closed).  This is also written such that if anything within the `with` statement throws an exception, it still closes the file.

`open`, in this case, could have been written in a couple different ways:

```python
# As a class, assuming that file handlers are created with some function called `os.open`.
# Note that THIS DOES ACTUALLY EXIT
class open(object):
    def __init__(self, filename):
        self.filename = filename
    def __enter__(self):
         self.file_handler = os.open(self.filename)
    def __exit__(self):
         self.file_handler.close()

# As a contextmanager
from contextlib import contextmanager

# Remember what this "@" does?
@contextmanager
def open(filename):
    file_handler = os.open(self.filename)
    yield file_handler
    file_handler.close()
```
