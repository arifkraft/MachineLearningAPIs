# Recap of Dogs and Cats Classification Problem
Lets have a look at FastAI's code for Dogs and Cats image classification problem 
```python
PATH = "data/dogscats/"
sz=224
arch=resnet34
data = ImageClassifierData.from_paths(PATH, tfms=tfms_from_model(arch, sz))
learn = ConvLearner.pretrained(arch, data, precompute=True)
learn.fit(0.01, 3)
```
`sz` is the size of minibatch. Should lower this one for weaker machines. `arch` is the pretrained NN architecture and is selected as
[resnet34](https://arxiv.org/abs/1512.03385). `ImageClassifierData.from_paths` takes data from `path`. `ConvLearner.pretrained` makes 
necessary settings for training the architecture. `learn.fit` takes two arguments, `learning rate` and `number of epochs` and trains 
the architecture with the data.

## 1. Choosing a Learning Rate 
The thing that most determines how we are going to zoom in or home in on the solution. Were is the "minimum point". How do you find the minimum point?If i was a computer algorithm, how do i found the minimum. The `learning rate` is how big of a jump that we will advance (the size of the arrow in the image below)
![Optimization](http://downhill.readthedocs.org/en/latest/_images/rosenbrock-nag.png)
<br>
What happens if your `learning rate` is too high? It diverges and cannot find the optimum. What happens if the `learning rate` is too
low. It takes up too much memory may also settle for `unwanted` local minima. Will talk about that in `SGD with restarts`

## 2. Data Augmentation
If you try training for more epochs, you'll notice that we start to *overfit*, which means that our model is learning to recognize the specific images in the training set, rather than generalizaing such that we also get good results on the validation set. One way to fix this is to effectively create more data, through *data augmentation*. This refers to randomly changing the images in ways that shouldn't impact their interpretation, such as horizontal flipping, zooming, and rotating.
We can do this by passing `aug_tfms` (*augmentation transforms*) to `tfms_from_model`, with a list of functions to apply that randomly change the image however we wish. For photos that are largely taken from the side (e.g. most photos of dogs and cats, as opposed to photos taken from the top down, such as satellite imagery) we can use the pre-defined list of functions `transforms_side_on`. We can also specify random zooming of images up to specified scale by adding the `max_zoom` parameter.
<br>
Transformantion Library
```python
tfms = tfms_from_model(resnet34, sz, aug_tfms=transforms_side_on, max_zoom=1.1)

def get_augs():    
  data = ImageClassifierData.from_paths(PATH, bs=2, tfms=tfms, num_workers=1)    
  x,_ = next(iter(data.aug_dl))   
  return data.trn_ds.denorm(x)[1]
```
<img src="https://qph.ec.quoracdn.net/main-qimg-e5037e9f5393c6e18a3bb30b4d2098f9-c" width="450">


## 3. Precompute 
Note, we are using a pretrained network. We can take the 2nd last layer and save those `activations`. There is this level of "dog space" "eyeballs" etc. We save these and call these `pre-computed activations`.
> Comment: The augmentation won't doing anything because of precompute<br>
How to make a new classifier from `pre-compute`?<br>
We can quickly train a simple linear model based on these saved precomputed numbers. So the first time you run a model, it will take some time to calculate and compile. Then afterwards, it will train much faster.
```python
data = ImageClassifierData.from_paths(PATH, tfms=tfms)
learn = ConvLearner.pretrained(arch, data, precompute=True)
learn.fit(1e-2, 1)
```
Since we have precomputed the different cat pictures don't help. So we will turn it off.<br>
By default when we create a learner, it sets all but the last layer to frozen. That means that it's still only updating the weights in the last layer when we call fit.
```python
learn.precompute=False
learn.fit(1e-2, 3, cycle_len=1)
```
### 4. Annealing
As we get closer to the minima, we would like to reduce our learning rate. Pick a rate, then drop it 10x, then drop again. Stepwise, very manual. A simpler approach is to choose a functional form such as a line. Turns out that half a `cosine curve` works out well.
<br>
Sometimes one minima will be better than others (based on how well it `generalizes`). So sharply changing the learning rate has the idea that if we suddenly jump up the learning rate, we will get out of "narrow" minimum and find the most "generalized" minimum.

<img src="https://cdn-images-1.medium.com/max/1200/1*S3NpdnSPHmfIURoLU-NuDg.png" width="400">

> Note that annealing is not the same as restarts<br>

### 5. Stochastic Gradient Descent with Restarts
