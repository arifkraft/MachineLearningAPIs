# First Class functions
A fuction that can be assinged as a variable is called a `first-class` function:
```python
def square(num):
	return num*num 
	
f= square

print(f(2))
```
We can also pass functions as `arguments` and `return` functions as result. Such functions are called `Higher order functions`
## `map` function
map function takes two arguments. One is a function and the other is an array. map applies the function arg to each element of the array
```python
# our squares function
def squares(num):
	return num*num

# a custom map function
def map(arg_func, arg_list):	
	for i in arg_list:
		yield i*i 

next(map(squares, [2,3,4,5])


```
