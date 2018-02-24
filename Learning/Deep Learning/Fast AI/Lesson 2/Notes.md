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
