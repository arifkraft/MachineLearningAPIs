# Choosing a learning rate
1. We first create a new learner, since we want to know how to set the learning rate for a new (untrained) model.

```python
learn = ConvLearner.pretrained(arch, data, precompute=True)
lrf=learn.lr_find()
```
2. The below code will plot a graph that helps find optimal Learning Rate. Our `learn` object contains an attribute `sched` that contains our learning rate scheduler, and has some convenient plotting functionality including this one:
```python
learn.sched.plot_lr()
```
## Improving the model
3. Data Augmentation - Horizontal flipping, zooming and rotating
```python
tfms = tfms_from_model(resnet34, sz, aug_tfms=transforms_side_on, max_zoom=1.1)

def get_augs():
  data = ImageClassifierData.from_paths(PATH, bs=2, tfms=tfms, num_workers=1)   
  x,_ = next(iter(data.aug_dl))
  return data.trn_ds.denorm(x)[1]

ims = np.stack([get_augs() for i in range(6)])

plots(ims, rows=2)
```
4. Let's create the a new dataset using the aumented images
```python
data = ImageClassifierData.from_paths(PATH, tfms=tfms)
learn = ConvLearner.pretrained(arch, data, precompute=True)

learn.fit(1e-2, 1)
```
5. By default when we create a learner, it sets all but the last layer to frozen. That means that it's still only updating the weights in the last layer when we call fit
```python
learn.precompute=False
learn.fit(1e-2, 3, cycle_len=1)
```
## Stochastic Gradient Descent with Restarts (SGDR)
What is that cycle_len parameter? What we've done here is used a technique called stochastic gradient descent with restarts (SGDR), 
a variant of learning rate annealing, which gradually decreases the learning rate as training progresses. This is helpful because as we
get closer to the optimal weights, we want to take smaller steps. <br>
However, we may find ourselves in a part of the weight space that isn't very resilient - that is, small changes to the weights may
result in big changes to the loss. We want to encourage our model to find parts of the weight space that are both accurate and stable.
Therefore, from time to time we increase the learning rate (this is the 'restarts' in 'SGDR'), which will force the model to jump 
to a different part of the weight space if the current area is "spikey". Here's a picture of how that might look if we reset the
learning rates 3 times (in this paper they call it a "cyclic LR schedule"):

```python
learn.sched.plot_lr()
learn.save('224_lastlayer')
learn.load('224_lastlayer')
```
## Fine-tuning and Differential Learning Rate annealing
Now that we have a good final layer trained, we can try fine-tuning the other layers. To tell the learner that we want to unfreeze 
the remaining layers, just call (surprise surprise!) `unfreeze()`.<br>
Note that the other layers have already been trained to recognize imagenet photos (whereas our final layers where randomly initialized), 
so we want to be careful of not destroying the carefully tuned weights that are already there.<br>
Generally speaking, the earlier layers (as we've seen) have more general-purpose features. Therefore we would expect them to need less
fine-tuning for new datasets. For this reason we will use different learning rates for different layers: the first few layers will be
at 1e-4, the middle layers at 1e-3, and our FC layers we'll leave at 1e-2 as before. We refer to this as differential learning rates, 
although there's no standard name for this techique in the literature that we're aware of.
```python
learn.unfreeze()

lr=np.array([1e-4,1e-3,1e-2])
learn.fit(lr, 3, cycle_len=1, cycle_mult=2)
```

