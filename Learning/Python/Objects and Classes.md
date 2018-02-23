# Objects and Classes

## Classes and Instances
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
  def __init__(self, first_name, last_name, pay):
    self.first = first
    self.last = last 
    self.pay = pay
    self.email =first + '.' + last + '@company.com'
    
  def full_name(self):
    return '{} {}'.format(self.first, self.last)
# Now we can specify our arguments for each instance in the __init__ method 
emp_1 = Employee('Arif','Alam', '50000')
emp_1 = Employee('John','Doe', '60000')

print(emp_1.email)
print(emp_2.email)

print(emp_1.full_name())
print(Employee.full_name(emp_1))
```
When we create a method in a `class`, it recieves the `instance` as the first argument automatically and by convention we should call the
instance `self`. We can create a `method` within our `class`. We need to put the parentheses while printing the method `full_name` because
it is a method. If we do not do that, it will only print the function name.






