# Lesson 3 Notes
We will look into ways of improving our image  classifier

>When `precompute` is `True`, data augmentation doesn't work because it uses cached non-augmented activations! 

## Making a Submission

1. Have a look at the classes 
```python
data.classes
```
2. Have a look at the names of `test` images
```python
data.test_ds.fnames
```
3. Make predictions on the `test` dataset
```python
log_preds, y = TTA(is_test = True)
probs = np.exp(log_preds)

probs.shape

df = pd.DataFrame(probs)
df.columns = data.classes

df.head()
```
4. Generate the submission file
```python
SUBM = f'{PATH}subm'
os.mkdir(SUBM, exist_ok=True)
df.to_csv(f'{PATH}subm/subm.gz', compresion='gzip', index=False) 

Filelink(f'{PATH}subm/subm.gz')
```

## Making individual predictions 
1. Get the name of the image in directory
```python
fn = data.val_ds.fnames[8]
```
2. Its a good idea to have a look at the image beforehand
```python
PIL.Image.open(PATH+fn)
```
3. The image needs to be transformed. We have seen before `tfms_from_model` and that actually gives two objects `trn_tfms` and `val_tfms`.
Here we'll just use `trn_tfms`
```python
im = trn_tfms(PIL.Image.open(PATH+fn))
```
4. Make predictions
```python
preds = learn.predict_array(im[None])
```
There's a cool trick in `numpy`. `im[None]` converts the image into a tensor ie. `[224x224x3] -> [1x224x224x3]` as it is just one image.

## Theory: Convlutional Neural Networks
Now we will get into little bit of what's going on with all this stuff!<br>
Well here's a very intuitive visual explanation of whats going on with `ConvNet`. [Check this out!](https://www.youtube.com/watch?v=Oqm9vsf_hvU). <br>
**What is an activation?**<br>
An `activation` is just a number. A number that is obtained by finding the sum of matrix multiplication of the portion of the `input_image` (usually 3x3) and `filter stride` (3x3).<br>
`Pytorch` does not store `filters` as separate filters/kernels but as `tensors`. Filters and  Kernels mean pretty much the same thing.
<br>
Also remember that if there are two `filters` it means the `hidden_layer` has a size 2.









