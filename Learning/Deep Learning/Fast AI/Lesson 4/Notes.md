# Structured, Time Series and Language models
This chapter we look at these topics from a very high level. Just touching through the topics. More like here is how to use the 
software than like here is what is happening behind the scenes

## Dropout (Only towards the end of the Neural Network)
1. It means randomly throwing away activations from convolutional layers
1. `p=0.5` is the probability of throwing out a randomly selected activation from the `conv` layer. this would mean that you are 
throwing away `50%` of the activations from the conv layer
1. This is a good way to solve the generalization problem and certainlya citical step in creating good deep learning models.

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
accuracy worsens as more epochs continue to run. With dropout it starts descent and continues to improve significatly as the
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

