# FastAI Lesson 1 - Analyzing Results

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
def plot_val_with_title(idxs,title):
  imgs = np.stack([data.val_ds[x][0] for x in idxs])
  title_probs = [probs[x] for x in idxs]
  print(title)
  return plots(data.val_ds.denorm(imgs), rows=1, titles=title_probs)
```
