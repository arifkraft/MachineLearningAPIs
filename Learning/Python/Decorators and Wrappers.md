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
Lets have a look at a practical examples:
