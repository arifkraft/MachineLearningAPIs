# FastAI Lesson 1 - Looking at Images
## Intial setup for fastAI Image Classification starter kit

```python
# Put these at the top of every notebook, to get automatic reloading and inline plotting
%reload_ext autoreload
%autoreload 2
%matplotlib inline

# This file contains all the main external libs we'll use
from fastai.imports import *

from fastai.transforms import *
from fastai.conv_learner import *
from fastai.model import *
from fastai.dataset import *
from fastai.sgdr import *
from fastai.plots import *

PATH = "data/dogscats/"
sz=224

torch.cuda.is_available()

torch.backends.cudnn.enabled

os.listdir(PATH)
os.listdir(f'{PATH}valid')
files = os.listdir(f'{PATH}valid/cats')[6:10]
files
img = plt.imread(f'{PATH}valid/cats/{files[1]}')
plt.imshow(img)

# Train the model
arch=resnet34
data = ImageClassifierData.from_paths(PATH, tfms=tfms_from_model(arch, sz))
learn = ConvLearner.pretrained(arch, data, precompute=True)
learn.fit(0.009, 4)
```

1. Predictions are in log-scale. So we get the probabilities
```python
log_preds = learn.predict()
log_preds.shape

preds=np.argmax(log_preds)
probs=np.exp(log_preds[:,1])
```
### Analyzing Results: Looking at the pictures
2. Write a function that gives random images (by ids `idxs`) of whether correct classification or not
```python
def rand_by_mask(mask): return np.random.choice(np.where(mask)[0], 4, replace=False)
def rand_by_correct(is_correct): return rand_by_mask((preds == data.val_y)==is_correct)
```
3. Define a function that takes input as `idxs` and plots the desired classification criteria images with its probability

```python

# plotting function 
def plots(ims, figsize=(12,6), rows=1, titles=None):    
	f = plt.figure(figsize=figsize)
	for i in range(len(ims)):
		sp = f.add_subplot(rows, len(ims)//rows, i+1)
		sp.axis('Off')
		if titles is not None: sp.set_title(titles[i], fontsize=16)
		plt.imshow(ims[i])

# Loading the image
def load_img_id(ds, idx): return np.array(PIL.Image.open(PATH+ds.fnames[idx]))

# Plot the images as required
def plot_val_with_title(idxs, title):
	imgs = [load_img_id(data.val_ds,x) for x in idxs]
	title_probs = [probs[x] for x in idxs]
	print(title)
	return plots(imgs, rows=1, titles=title_probs, figsize=(16,8))
```
4. Look at few correct and incorrect labels at random
```python
# 1. A few correct labels at random
plot_val_with_title(rand_by_correct(True), "Correctly classified")

# 2. A few incorrect labels at random
plot_val_with_title(rand_by_correct(False), "Incorrectly classified")
```
5. Write a function that gives random images (by ids `idxs`) of whether it is most correct classification or most incorrect
```python
def most_by_mask(mask, mult):
	idxs = np.where(mask)[0]  
	return idxs[np.argsort(mult * probs[idxs])[:4]]

def most_by_correct(y, is_correct):
	mult = -1 if (y==1)==is_correct else 1
	return most_by_mask(((preds == data.val_y)==is_correct) & (data.val_y == y), mult)
```
6. Evaluate most correct and incorrect images 
```python
plot_val_with_title(most_by_correct(0, True), "Most correct cats")

plot_val_with_title(most_by_correct(1, True), "Most correct dogs")

plot_val_with_title(most_by_correct(0, False), "Most incorrect cats")

plot_val_with_title(most_by_correct(1, False), "Most incorrect dogs")
```
7. look at the most uncertain images
```python
most_uncertain = np.argsort(np.abs(probs -0.5))[:4]
plot_val_with_title(most_uncertain, "Most uncertain predictions")
```
