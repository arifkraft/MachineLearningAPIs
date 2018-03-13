# Tree Ensemble Methods

## Create your first GBM model: [lightgbm](https://github.com/Microsoft/LightGBM/blob/master/examples/python-guide/advanced_example.py)
We use lightgbm to implement our first GBM modelling exercise


#### 1. Load required libraries

```python
#coding: utf-8
#pylint: disable = invalid-name, C0111
import json
import lightgbm as lgb
import pandas as pd
import numpy as np
from sklearn.metrics import mean_squared_error

"""
Pickling” is the process whereby a Python object hierarchy is converted into a byte
stream, and “unpickling” is the inverse operation, whereby a byte stream is converted
back into an object hierarchy. Pickling (and unpickling) is alternatively known as 
“serialization”. 
""" 

try:
    import cPickle as pickle
except:
    import pickle
```
#### 2. Getting data into the system
We create a class that does data input tasks for us. This will generate train and validation samples 

```python
class get_data():
	def __init__(self, path, target, 
```
