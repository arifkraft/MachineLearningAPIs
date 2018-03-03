# Decorators and Wrappers
`Decorators` are functions that takes `function` as the argument. `Adds` more functionality to it and then returns a `function`. One thing 
to note however is that the source code of the `original_function` that is being passed on as an argument is not altered in any way.
Let's have a look at a very simple example:
```python
def decorator(orig_func):
	def wrapper(*args):
		print('This wrapper is running: {0} and args are: {1}'.format(orig_func.__name__, args))
		return orig_func(*args)
	return wrapper

#orig func
def summer(*args):
	return sum(args)

decorated_summer = decorator(summer)
decorated_summer(1,2,3,4)
```
Adding a decorator allows us to easily add functionality to our existing functions. Alternatively the `decorator` can be initialized as 
shown below:
```python
def decorator(orig_func): 
	def wrapper(*args):  
		print('This wrapper is running: {0} and args are: {1}'.format(orig_func.__name__, args))
	return orig_func(*args) return wrapper

@decorator 
def summer(*args): 
	return sum(args)

summer(1,2,3,4)
```
It is always a good idea to pass `*args` and `**kwargs` in the wrapper function so that the decorator can be used fou multiple functions
that take different positional arguments
```python
def decorator(orig_func):
	def wrapper(*args,**kwargs):
		pass
	return wrapper 
```
We could also create `class decorators` that has the exact same functionality:
```python
class decorator(object):
	def __init__(self,orig_func):
		self.orig_func = orig_func
	def __call__(self, *args, **kwargs):
		print('Executing : {} with args {} and krwargs {}'.format(self.orig_func.__name__,args,kwargs))
		return self.orig_func(*args, **kwargs)
	
@decorator
def summer(*args):
	return sum(args)
	
summer(1,2,3,4)
```
Lets have a look at a practical examples. First one is the `logger` example 
```python
import logging

def decorator_logger(orig_func):
	logging.basicConfig(filename = '{}.log'.format(orig_func), level= logging.INFO)
	def wrapper(*args, **kwargs):
		logging.info('Ran with the args: {} and was executed by: {}'.format(args, kwargs))
		return orig_func(*args, **kwargs)
	return wrapper

@decorator_logger
def summer(*args, user = 'New User'):
	return sum(args)

summer(2,3,4,5, user = 'Arif')

```
Another example of decorators is the timer which is how much time it took to run the function
```python
def decorate_time(orig_func):
	import time 
	def wrapper(*args, **kwargs):
		t1 = time.time()
		time.sleep(5)
		result = orig_func(*args, **kwargs)
		t2 = time.time()
		print('Time taken to run the {} function is {}'.format(orig_func.__name__, t2-t1))
		return result
	return wrapper

@decorate_time
def summer(*args):
	return sum(args)

summer(1,2,3,4)
```
How do we `chain` decorators one on top of the other. To be able to do that we will have to import `wraps` from `functools` and add `wraps(orig_func)` on top of all the `wrap` functions inside decorators
```python
from functools import wraps 

def decorate_logger(orig_func):
	import logging
	logging.basicConfig(filename = '{}.log'.format(orig_func), level= logging.INFO)
	
	wraps(orig_func)
	def wrapper(*args, **kwargs):
		logging.info('Ran with the args: {} and was executed by: {}'.format(args, kwargs))
		return orig_func(*args, **kwargs)
	return wrapper

def decorate_time(orig_func): 
	import time
	
	wraps(orig_func)
	def wrapper(*args, **kwargs):  
		t1 = time.time()
		time.sleep(5)
		result = orig_func(*args, **kwargs)
		t2 = time.time()
		print('Time taken to run the {} function is {}'.format(orig_func.__name__, t2-t1))
		return result
	return wrapper

@decorate_time
@decorate_logger
def summer(*args):
	return sum(args)

summer(1,2,3,4)


```

