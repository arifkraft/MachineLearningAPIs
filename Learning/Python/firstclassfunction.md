# First Class functions
A fuction that can be assinged as a variable is called a `first-class` function:
```python
def square(num):
	return num*num 
	
f= square

print(f(2))
```
We can also pass functions as `arguments` and `return` functions as result. Such functions are called `Higher order functions`
