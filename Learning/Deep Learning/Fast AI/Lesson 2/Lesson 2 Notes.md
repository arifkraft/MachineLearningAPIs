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
Comment: The augmentation won't doing anything because of precompute<br>
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

What is that `cycle_len` parameter? What we've done here is used a technique called *stochastic gradient descent with restarts (SGDR)*, a variant of *learning rate annealing*, which gradually decreases the learning rate as training progresses. This is helpful because as we get closer to the optimal weights, we want to take smaller steps.<br>
However, we may find ourselves in a part of the weight space that isn't very resilient - that is, **small changes to the weights may result in big changes to the loss.** We want to encourage our model to find parts of the weight space that are both accurate and stable. Therefore, from time to time we increase the learning rate (this is the 'restarts' in 'SGDR'), which will force the model to **jump to a different part of the weight space if the current area is "spikey".** Here's a picture of how that might look if we reset the learning rates 3 times (in this paper they call it a "cyclic LR schedule"):

<img src="https://cdn-images-1.medium.com/max/1002/1*9Fca3kpx3pVW8SaYz2pjpw.png" width=600>

The number of epochs between resetting the learning rate is set by `cycle_len`, and the number of times this happens is refered to as the *number of cycles*, and is what we're actually passing as the 2nd parameter to `fit()`. So here's what our actual learning rates looked like:

<img src= "http://forums.fast.ai/uploads/default/original/2X/2/2b56caa0a5f87fb17429eb961fa2adf77f299547.png" width=400>
Good tip, Save as you go!

```python
learn.save('224_lastlayer')
```

### 6. Differential Learning Rate Annealing
Now that we have a good final layer trained, we can try fine-tuning the other layers. To tell the learner that we want to unfreeze the remaining layers, just call (surprise surprise!) `unfreeze()`.
```python
learn.unfreeze()
```
Note that the other layers have *already* been trained to recognize imagenet photos (whereas our final layers where randomly initialized), so we want to be careful of not destroying the carefully tuned weights that are already there.
Generally speaking, the earlier layers (as we've seen) have more general-purpose features. Therefore we would expect them to need less fine-tuning for new datasets. For this reason we will use different learning rates for different layers: the first few layers will be at 1e-4, the middle layers at 1e-3, and our FC layers we'll leave at 1e-2 as before. We refer to this as *differential learning rates*, although there's no standard name for this techique in the literature that we're aware of.
#### Specifying learning rates
We are going to specify 'differential learning rates' for different layers. We are grouping the blocks (RESNET blocks) in different areas and assigning different learning rates.
Reminder: we unfroze the layers and now we are retraining the whole set. The learning rate is smaller for early layers and making them larger for the ones farther away
```python
lr=np.array([1e-4,1e-3,1e-2])
# 3 is the number of cycles
# 3 cycles of 2 epochs
learn.fit(lr, 3, cycle_len=1, cycle_mult=2)
```
`cycle_mult` doubles the cycle length after each `cycle`
Take a look at the following chart, and see if you can figure out what the parameter is doing:

```python
learn.sched.plot_lr()
```
### 7. Test Time Augmentation
We are going to do. Use **test time augmentation** we are going to take 4 random data augmentation. Move them around and flip and mix with the prediction. We are going to average all the predictions of the original + permutation. Ideally the rotating + zoom will get it in the right orientation <br>
TTA() - makes predictions not only on the originals but also on the random augmented generated
```python
log_preds,y = learn.TTA()
accuracy(log_preds,y)
```
