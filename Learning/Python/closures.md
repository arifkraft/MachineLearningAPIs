# Closures
In simple terms, a `closure` is an `inner_function` that remembers and has access to to all variables that are created within its scope, 
even after the `outer_function` has finished executing.
<br> A very simple example of closures 
```python
def outer_func_(msg):
	def inner_func_():
		print(msg)
	return inner_func_
	
my_func_ = outer_func_('Hi')
my_func_()
```
Here's a slightly complex `closure` example:
```python
import logging

logging.basicConfig(filename = 'example_log', level = logging.INFO)

def logger(func):
	def log_func_(*args):
		logging.info('Running {0} with arguments {1}'.format(func.__name__, args))
		print(func(*args))
	return log_func_
	
def adder(*args):
	return sum(args)

add_func = logger(adder)
add_func(1,2,3,4,5)

```
