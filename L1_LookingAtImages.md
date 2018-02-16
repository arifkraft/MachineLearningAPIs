# FastAI Lesson 1 - Looking at Images

1. Put these at the top of every notebook, to get automatic reloading and inline plotting
```python
%reload_ext autoreload
%autoreload 2
%matplotlib inline 
```
2. Check if `torch` cuda and cuDNN are available
```python
torch.cuda.is_available()
torch.backends.cudnn.enabled
```
3. Looking at directories directly from jupyter
```python
PATH = "data/dogscats/"
os.listdir(PATH)
os.listdir(f'{PATH}valid')
files = os.listdir(f'{PATH}valid/cats')[:5]
files
```
4. Plotting the images
```python
img = plt.imread(f'{PATH}valid/cats/{files[2]}')
plt.imshow(img)
```
5. Predictions are in log-scale
```python
log_preds = learn.predict()
log_preds.shape

preds=np.argmax(log_preds)
probs=np.exp(log_preds[:,1])
```
### Analyzing Results: Looking at the pictures
7. Write a function that gives random images (by ids `idxs`) of whether correct classification or not
```python
def rand_by_mask(mask): return np.random.choice(np.where(mask)[0], 4, replace=False)
def rand_by_correct(is_correct): return rand_by_mask((preds == data.val_y)==is_correct)
```
8. Define a function that takes input as `idxs` and plots the desired classification criteria images with its probability

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
9. Look at few correct and incorrect labels at random
```python
# 1. A few correct labels at random
plot_val_with_title(rand_by_correct(True), "Correctly classified")

# 2. A few incorrect labels at random
plot_val_with_title(rand_by_correct(False), "Incorrectly classified")
```
10. Write a function that gives random images (by ids `idxs`) of whether it is most correct classification or most incorrect
```python
def most_by_mask(mask, mult):
  idxs = np.where(mask)[0]  
  return idxs[np.argsort(mult * probs[idxs])[:4]]

def most_by_correct(y, is_correct):
  mult = -1 if (y==1)==is_correct else 1
  return most_by_mask(((preds == data.val_y)==is_correct) & (data.val_y == y), mult)
```
11. Evaluate most correct and incorrect images 
```python
plot_val_with_title(most_by_correct(0, True), "Most correct cats")

plot_val_with_title(most_by_correct(1, True), "Most correct dogs")

plot_val_with_title(most_by_correct(0, False), "Most incorrect cats")

plot_val_with_title(most_by_correct(1, False), "Most incorrect dogs")
```
12. look at the most uncertain images
```python
most_uncertain = np.argsort(np.abs(probs -0.5))[:4]
plot_val_with_title(most_uncertain, "Most uncertain predictions")
```
