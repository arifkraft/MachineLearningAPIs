# Structured, Time Series and Language models
This chapter we look at these topics from a very high level. Just touching through the topics. More like here is how to use the 
software than like here is what is happening behind the scenes
With deep learning on columnar data, we get state of the art results but with much less effort compared to GBM

## Dropout 
1. It means randomly throwing away activations from convolutional layers
1. `p=0.5` is the probability of throwing out a randomly selected activation from the `conv` layer. This would mean that you are 
throwing away `50%` of the activations from the conv layer
1. This is a good way to solve the generalisation problem and certainly a critical step in creating good deep learning models.

#### Q1. Is there anything we need to do in order to compensate for the loss incurred as a result of throwing so many activations away?

We don't have to worry about that. `Pytorch` behind the scenes does that for us. It doubles the remaining activations to keep the
average unchanged
```python
learn = ConvLearner.pretrained(arch, data, ps=0.5, precompute=True)
```
`ps=0.5` is the keyword for dropout
#### Q2. Why do we take `logsoftmax` and not `softmax` as the last layer?
This for a minor numerical precision reasons 
<br>
#### Q3. What are the implications of using or not using dropout?
If you do not use dropout, you could expect better validation accuracy in the initial epochs however the improvement in validation
accuracy worsens as more epochs continue to run. With dropout it starts descent and continues to improve significantly as the
epochs progress. 
#### Q4. What is an ideal dropout rate?
The fastAI library creates two dropout layers at the end of the network: 1st one has `ps=0.25` and the second one has `ps=0.5`
This should give you descent results. Only if you decide to use a larger NN like the `Resnet50` and above you would want to 
increase the dropout rate because they have too many parameters and are more likely to overfit!
```
(0)-BatchNorm
(1)-Dropout (p = 0.25) <----
(2)-Linear
(3)-ReLU
(4)-BatchNorm
(5)-Dropout (p = 0.5) <----
(6)-Linear 
(7)-LogSoftMax
```

## Kaggle's Rossman Challenge
Please check out `.ipynb` version of the Rossman Challenge inside this direcrtory!

Some pointers to note -

1. Catergorical variables are converted to contiguous. The reason is that finally these variables will be converted to matrix
and you dont want them to be `2014` long when it can be just `2` long
1. One key new concept we need to learn about is called `embeddings`. This is good way to deal with categorical vars in deep
neural nets. It creates sort of matrix for the the variable. Have a look below:

| Cardinals        | Rand1           | Rand2  | Rand3     |   Rand4    |
| ------------- |:-------------:| -----:| -------- | --------- |
| Sunday            | 87687    |78687      |88967     |897987    |
| Monday | 87687 |78687|88967 |897987 |
| ..... | 87687 |78687|88967 |897987 |
| Friday | 87687 |78687|88967 |897987 |

So this looks like a `[4x7]` weight tensor. There are 4 random weights for each level. The weights will be adjusted accordingly as the training progresses. This is called embeddings.
1. What is the advantage of using embeddings over just doing a one-hot encoding?
<br>
Well, when you do one-hot encoding, your cat vars are just behaving very linearly. For example 'Sunday' would be just one thing on its own. If we let Sunday vary in multiple dimensions, it might be the case that Sunday might show certain characteristics that are similar to Saturday. So embeddings allow categorical vars to take care of some kinds of  interactions that may exist among levels of categorical vars.

1. We need to get rid of the last layer of softmax too
1. Why do embeddings when you can get away with just one-hot encoding? The reason we are doing embedding is that it just prevents the levels of the variable to act very linearly. Adding embeddings adds more dimensions in which the levels can covary. So Sunday and Saturday need not be fully exclusive now as they can have one of the embeddings as very high or something. 
1. So how are date categorical vars are taken care of?  Well, there happens to be pandas function `date_addpart` which converts the date into new features like `day of week`, `is_end_month` etc. Now that helps our NN model to also learn some cyclic aspects of the time series. It is therefore necessary that we include such cyclic variables in our models to make sure sure it is getting the right kind of information to generate better time series predictions


# Language models
The definition seems to be quite simple. Given a specific set of words, can you tell what the next word wold be.! Keep in mind language models have a very specific meaning which is this def.


