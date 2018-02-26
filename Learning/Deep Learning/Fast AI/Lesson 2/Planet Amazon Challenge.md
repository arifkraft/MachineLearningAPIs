# Planet: Understanding the Amazon from Space
Use satellite data to track the human footprint in the Amazon rainforest.

### Step 1 - Getting Started with Initial setup
```python
# matplotlib setting
%reload_ext autoreload
%autoreload 2
%matplotlib inline

from fastai.conv_learner import *
PATH = 'data/planets/'
os.makedirs('data/planet/models', exist_ok=True)
ls {PATH}
```
