## Generators
`Generators` are good way to improve code performance and are good alternatives to list objects that take up a lot of memory! Lets have a look at an 
example here. Here is a function that takes in `list` as input and gives out the sqaured vaules as another `list`
```python
def find_sqaures(nums):
  squares = []
  for i in nums:
    squares.append(i*i)
  return squares

nums= [1,2,3,4,5]
print(find_squares(nums))
```
Alternatively, what we can do is to create `generator` objects. Have a look at the code below - 
```python
def find_squares(nums):
  for i in nums:
    yield i*i
    
nums= [1,2,3,4,5]
print(find_squares(nums)) # prints <generator_object>

squares = find_squares(nums) 
next(squares)

for el in squares:
  print el 
```
We can also use the `list comprehensions` just that we use parantheses instead of square brackets

```python  
squares = [x*x for x in [1,2,3,4,5]] # gives you a list
squares = (x*x for x in [1,2,3,4,5]) # gives you a generator
```
One of the main advatages of using a generator over a list is its `memory allocation`. To get a sense of that, lets take a look at an
example below -
```python 
import time 
import random
import mem_profile

names = ['arif','neeraj','darsh','david','ali']
profession = ['engineer', 'doctor', 'teacher', 'scientist', 'developer']

# Fucntion that creates a dictionary (as a list)

def person_by_list(range_):
  result =[]
  for i in range(range_):
    person  = {'id': i,
    'name': random.choice(names),
    'profession': random.choice(profession)
    }
    result.append(person)
  return result

# Function that creates a dictionary (as a generator)

def person_by_generator(range_):
  for i in range(range_):
    person  = {'id': i,
    'name': random.choice(names),    
    'profession': random.choice(profession)
    }
    yield person


print('Memory After: {}Mb'.format(mem_profile.memory_usage_resource()))
#t1 = time.clock()
#people = person_by_list(1000000)
#t2 = time.clock()

t1 = time.clock()
people = person_by_generator(1000000)
t2 = time.clock()
print('Memory After: {}Mb'.format(mem_profile.memory_usage_resource()))
print('Time Taken is {}'.format(t2-t1))
```
You will clearly see the difference is terms of `memory allocated` and `time consumed`
