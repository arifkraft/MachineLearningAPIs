# Fast AI Lesson 1 useful functions

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
