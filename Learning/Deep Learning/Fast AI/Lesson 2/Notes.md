# Recap of our Dogs and Cats Classification Problem
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




