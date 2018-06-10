# collaborative Filtering: Inside the training loop

We will not create a neural network per se. Instead we will do someting along the lines of matrix vectorization. Think about
movie lens problem -
1. Create a cross-tab Movie Id vs. User Id
1. Initialize random weights for User Id and Movie Id
1. Do a dot product of weights 
1. Minimize RMSE of the entire crosstab

We can do collaborative filtering easily using the fastAI library without any problem at all.
<br>
If the data is too big, you might wanna do some workaround with bcolz array or dask dataFrame. That will make things faster.
<br>
If you want to use a training loop for any custom made PyTorch Model, just use FastAI's `fit` funtion. Have a look at this 
week's notebook. 
<br>
Just doing matrix dot product doen't bring down the loss very much, gotta add user bias and movie bias too. That seems to work
just fine. Try it out on the notebook.
<br>
Have a look at what broadcasting is! (In Numpy or PyTorch context)


