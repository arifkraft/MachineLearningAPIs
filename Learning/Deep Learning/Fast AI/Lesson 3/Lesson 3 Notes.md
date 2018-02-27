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
Well here's a very intuitive visual explanation of whats going on with `ConvNet`. [Check this out!](https://www.youtube.com/watch?v=Oqm9vsf_hvU). 

#### What is an activation?
An `activation` is just a number. A number that is obtained by finding the sum of matrix multiplication of the portion of the `input_image` (usually 3x3) and `filter stride` (3x3).
<br>
`Pytorch` does not store `filters` as separate filters/kernels but as `tensors`. Filters and  Kernels mean pretty much the same thing.
Also remember that if there are two `filters` it means the `hidden_layer` has a size 2.

#### What to do when you got less number of `channels` for a pre-trained network?
Some strategies you might wanna try -
1. 1 channel  - Make copies of the single channel into three
1. 2 channels - Create a third channel as the average of the the existing two or create a copy of one of the two

#### What about more than 3 channels?
Jeremy shares his experience where he used satellite images which had 4-channels (4th is the infra-red band). What he did then was to add an extra level/filter to the `convolutional kernel` that were all zeros.

#### What are activation functions?
`Activation Functions` are the functions that we apply to the activations `ReLU` is `MAX(0, act)` is an activation function. `sigmoid` 
is used to convert activations into `probabilities` is an activation function. Takes in one number and spits out another number.
<br>
**Well, Get the hang of `log` and `exp`. Do some experiments/plots to understand better**

> These activation function add non-linearity to the architecture. Because if you stack up a bunch of linear layers together you get
a linear layer in the end

#### How does `soft_max` work?

Softmax is an activation function that lets us get more intuitive outputs from our architecture. ie. if you have 5 classes, you need an output that is between (0,1) for each class and whose some adds up to 1 just as probabilities. So technically this is how it is done:

|Label   |Output  |Exp      |Softmax  |
|--------|--------|---------|---------|
|cat     |-1.63   |0.20     |0.15     |
|dog     |-0.05   |0.95     |0.74     |
|plane   |-4.75   |0.01     |0.01     |
|fish    |-3.54   |0.03     |0.02     |
|building|-2.38   |0.09     |0.07     |
|        |        |**1.28** |**1.00** |

Softmax is only done for a `single-label` classification problem. That's an important thing to keep in mind! 

#### Multi-label classification
This is where a sample can fall into mulitple classes. The `planet` competition is one such example. Look into the `lesson2` planet notebook.
In `multi-label`, the  last layer instead of `softmax` is actually a `sigmoid`. This is an important distinction. 

$$ Softmax: \frac{e^x_i}{sum(e^x)}$$

$$ Sigmoid: \frac{e^x_i}{1-e^x_i}$$

<br>
In Pytorch the `data` object that you create has two things, `ds` is the dataset and `dl` is the data loader which gives you the minibatch

```python
data = get_data(256)
x,y = next(iter(data.val_dl))
print(y)
```
So, if you remember from `Otavia's Video`, at the end of the process, the letters are compared with `actuals`. However, `Pytorch` does the comparison through indexing. Have a look below -

|Label   |Output  |Exp      |Softmax  |Actuals |Index   |
|--------|--------|---------|---------|--------|--------|
|cat     |-1.63   |0.20     |0.15     |0       |1       |
|dog     |-0.05   |0.95     |0.74     |1       |2       |
|plane   |-4.75   |0.01     |0.01     |0       |3       |
|fish    |-3.54   |0.03     |0.02     |0       |4       |
|building|-2.38   |0.09     |0.07     |0       |5       |
|        |        |**1.28** |**1.00** |        |        |

Behind the scenes, Pytorch does convert to `one-hot-encoding` and calculate `loss` and stuff!<br>
For the `Planet` Competition, You need to start with small image size `sz=64` because this is nothing like `ImageNet`. We dont do small image sizes for `Cats and Dogs` we don't want to screw up the `weights` as Cats and Dogs is more like `ImageNet`.
<br>
In the planet competition, you will notice the piece of code `data.resize` and you might be wondering what that does!

```python
data = data.resize(int(sz*1.3), 'tmp')
```
If you have really big images coming in it will save you a lot of time. Its like taking in `1000x1000` and then converting to `64x64` is indeed a massive task!
<br>
Also, the `evaluation metric` used for Planet competition is `fbeta`. So, have a look at that too!


