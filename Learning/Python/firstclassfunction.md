# First Class functions
A fuction that can be assinged as a variable is called a `first-class` function:
```python
def square(num):
	return num*num 
	
f= square

print(f(2))
```
We can also pass functions as `arguments` and `return` functions as result. Such functions are called `Higher order functions`
## How to pass a function: `map` function
map function takes two arguments. One is a function and the other is an array. map applies the function arg to each element of the array
```python
# our squares function
def squares(num):
	return num*num

# a custom map function
def map(arg_func, arg_list):
	for i_ in arg_list:
		yield arg_func(i_)

gen = map(squares,[2,3,4,5])

for el in gen:
	print(el)

def cubes(num):
	return num*num*num

gen = map(cubes, [2,3,4,5])

for el in gen:
	print(el)

```
Notice that we are not adding `parantheses` while passing the functions to the `map` function. This is because we do not want the functions to execute

## How to return a function?
Check this code out!
```python
def html_(tag):
	def wrap_(msg):
		print('<{0}>{1}</{0}>'.format(tag,msg))
	return wrap_

print_html = html_('h1') #1
print_html('Message1') #2

```
`#1` excutes the function `html_` and returns `wrap_` function. Note that the `wrap_` function has not been executed yet and is then assingned as `print_html`.  `print_html` takes `msg` argument and executes. Remember: `html_` will return only one function which is the first one. So if you have more functions below they'll not be returned after executing `html_`
