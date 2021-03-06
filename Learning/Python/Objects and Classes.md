# Objects and Classes

## 1. Classes and Instances
Classes are used throughout most of the mordern programming languages. There is a good reason for that. They allow us to logically
group data and functions in a way to reuse and also easy to built upon if need be. Data and functions are called 
attributes and methods respectively.
<br>
So lets go ahead and get started -<br>
Say we have an application for our company and we would like to represent our employees in a python code. Each employee is going to 
have specific attributes and methods.<br>
It would be nice if we create a blueprint for each employee so that we don't have to do that manually for each employee individually. 
So lets proceed with a class and see how that would look like - 
```python
class Employee:
	pass 
```
So what is the difference between `class` and a `class instance`. A class is basically a blueprint for creating instances. And each employee 
will be an `instance` of that `class`
```python
# instance 1
emp_1 = Employee()
# instance 2
emp_2 = Employee()
```
Both the `objects` are unique are stored saperately in the memory. `Instance Variables` contain data that is unique to each `instance`.
<br> We are going to use a special `__init__` method. This can be thought of as `constructers`. 
```python
class Employee:
	def __init__(self, first, last, pay):
		self.first = first
		self.last = last 
		self.pay = pay
		self.email =first + '.' + last + '@company.com'
		
	def full_name(self):
		return '{} {}'.format(self.first, self.last)
# Now we can specify our arguments for each instance in the __init__ method 
emp_1 = Employee('Arif','Alam', 50000)
emp_2 = Employee('John','Doe', 60000)

print(emp_1.email)
print(emp_2.email)

print(emp_1.full_name())
print(Employee.full_name(emp_1))
```
When we create a method in a `class`, it recieves the `instance` as the first argument automatically and by convention we should call the
instance `self`. We can create a `method` within our `class`. We need to put the parentheses while printing the method `full_name` because
it is a method. If we do not do that, it will only print the function name.

## 2. Class Variables
`Class variables` are the variables that are shared among all instances of the class. While `instance variables` are unique for each
instance, `class variables` are common for all the instances. For example `Annual Raise` can be a good candidate for a `class variable` in our example above. Let's say we would like to put an annual appraisal of 4% for any `instance` that uses the `Employee class`. We 
define a class variable as shown below. 

```python
class Employee:
raise_amount = 1.04
	def __init__(self, first, last, pay):
		self.first = first
		self.last = last
		self.pay = pay
		self.email =first + '.' + last + '@company.com'
		
	def full_name(self):
		return '{} {}'.format(self.first, self.last)
    
	def apply_raise(self):
		return int(self.pay*self.raise_amount)
    
# Now we can specify our arguments for each instance in the __init__ method 
emp_1 = Employee('Arif','Alam', 50000)
emp_2 = Employee('John','Doe', 60000)

emp1.raise_amount()
```

> Remember : A `class variable` can only be accessed when it is followed by `class name` or the `instance name`. So, if `raise_amount` is `class_variable` then, it can be accessed as `Employee.raise_amount` or `self.raise_amount`.

You will notice that if you check the `namespace` for any instance like `emp_1` by writing 

```python
emp_1.__dict__
```
You will notice that, `raise_amount` class variable is not present. But if you wish to change the `raise_amount` for the instance
`emp_1`, you write:

```python
emp_1.raise_amount= 1.05
```
and you notice that `raise_amount` exists in the instance's namespace. To change the class variable for the entire class, you write:

```python
Employee.raise_amount = 1.06
```

Now lets look at an example where it doesn't make sense to use to use an `instance` class variable. For example to keep a track of
how many employees we have in our employee class

```python
class Employee:
	num_of_emps=0
	raise_amount = 1.04
	def __init__(self, first, last, pay):
		self.first = first
		self.last = last
		self.pay = pay
		self.email =first + '.' + last + '@company.com'
		Employee.num_of_emps += 1 
	def full_name(self):
		return '{} {}'.format(self.first, self.last)
		
	def apply_raise(self):
		return int(self.pay*self.raise_amount)
    
# Now we can specify our arguments for each instance in the __init__ method
emp_1 = Employee('Arif','Alam', 50000)
emp_2 = Employee('John','Doe', 60000)

print(Employee.num_emps)
# This outputs 2 

```
One obvious question now is that if we have `class variables`, do we  also have `class methods`. The answer is yes and we will take a look that in the next section :)
