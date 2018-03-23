## 0. What is the difference between inference and prediction? Bias and Variance
For example, in a real estate setting, one may seek to relate values of homes to inputs such as crime rate, zoning, distance from a river, air quality, schools, income level of community, size of houses, and so forth. In this case one might be interested in how the individual input variables affect the prices—that is, how much extra will a house be worth if it has a view of the river? This is a inference problem. Alternatively, one may simply be interested in predicting the value of a home given its characteristics: is this house under- or over-valued? This is a prediction problem. <br>

<br>
Bias vs. Variance:

1. Low Bias: Suggests less assumptions about the form of the target function. RF, SVM etc.
1. High-Bias: Suggests more assumptions about the form of the target function. Regression, LR etc.

1. Low Variance: Suggests small changes to the estimate of the target function with changes to the training dataset.
1. High Variance: Suggests large changes to the estimate of the target function with changes to the training dataset.

## 1. What is the intuition behind regression?
## 2. What are the assumptions in Linear Regression?
1. Linear relationship
1. Multivariate normality
1. No or little multicollinearity
1. No auto-correlation
1. Homoscedasticity

## 3. What is null hypothesis in a linear regression?
## 4. Explain Heteroscadaticity? Why is homoscadasticity essential?
## 5. Explain Forward Selection, Backward Elimination and Stepwise regression?
## 6. Explain multicollinearity? What happens if multicollinearity is ignored?
Multicollinearity (also collinearity) is a phenomenon in which one predictor variable in a multiple regression model can be linearly predicted from the others with a substantial degree of accuracy.
<br>
The idea is that you can change the value of one independent variable and not the others. However, when independent variables are correlated, it indicates that changes in one variable are associated with shifts in another variable. The stronger the correlation, the more difficult it is to change one variable without changing another. It becomes difficult for the model to estimate the relationship between each independent variable and the dependent variable independently because the independent variables tend to change in unison.

## 7. What are interaction variables?
The presence of a significant interaction indicates that the effect of one predictor variable on the response variable is different at different values of the other predictor variable. 
## 8. How would you test normality of residuals?
QQ Plot. 
## 9. How is r-squared calculated?
R-squared is a statistical measure of how close the data are to the fitted regression line. R-squared = Explained variation / Total variation
## 10. Any other tests you would conduct in order to evaluate model?
1. Regression Diagnostics
1. Unusual and Influential data
1. Tests on Normality of Residuals
1. Tests on Nonconstant Error of Variance
1. Tests on Multicollinearity
1. Tests on Nonlinearity
1. Model Specification
1. Issues of Independence

## 11. Intuition behind logistic regression?
## 12. How do you interpret odds-ratio estimates in logistic regression?
## 13. Loss in Logistic regression? And what is RMSE?
RMSE can be interpreted as the standard deviation of the unexplained variance, and has the useful property of being in the same units as the response variable.
## 15. Can you explain MLE?
## 16. Have you heard of cross-entropy?
-(ylog(p) + (1-y)log(1-p))

## 17. Chi-Square test in logistic regression?


## 18. Can you explain how ROC curves are created?
## 19. Any other evaluation metric that you want to talk about?
## 20. What would you do if you have very small number of actual events?
## 21. What is AIC? What happens to AIC values when more variables are added?
## 22. Have you heard of binning? Any advantages?
## 23. What is cardinality of a variable?
## 24. Intuitively explain what decision trees are?
## 25. Explain the difference between bagging and boosting.
## 26. How is gradient boosting different from Adaboost?
## 27. Can ensemble models overfit? Why or why not?
## 28. Explain cross-validation.
## 29. Difference between Random Forest and Gradient Boosting -
Random Forest

| Conceptual                                                                        | Practical                                        | 
|-----------------------------------------------------------------------------------|--------------------------------------------------|
| Combine multiple trees. Each fit to a random sample of data                       | Strengths: Easy to use and few parameters        | 
| Randomly selects rows and columns. Reduces variance with minimal increase in bias | Weakness: Slow to score and Lack of transparency |

Gradient Boosting -

| Conceptual                                                                        | Practical                                        | 
|-----------------------------------------------------------------------------------|--------------------------------------------------|
| Fits consecutive trees where each solves for the net loss of the prior trees      | Strengths: Robust, Directly optimizes cost fn    | 
| Results of new trees are applied partially to the entire dataset                  | Weakness: Overfits and has several hyperparams   |




















