

Choose a model
Choose a loss function
Fit the model by minimizing average loss


# 1 Prediction vs. Estimation

Estimation is the task of using data to determine model parameters.

Prediction is the task of using a model to predict outputs for unseen data. Once we have estimates for our model’s parameters, we can use our model for prediction.


# 2 Notation


![[image/Pasted image 20250526092725.png]]


![[image/Pasted image 20250526092806.png]]


# 3 Terminology

There are several equivalent terms in the regression context. You should be aware of them.

x
● Feature.
● Covariate.
● Independent variable.
● Explanatory variable.
● Predictor.
● Input.
● Regressor.

---

y 
● Output.
● Outcome.
● Response.
● Dependent variable.



# 4 Loss functions



![[image/Pasted image 20250526093149.png]]


![[image/Pasted image 20250526093217.png]]





## 4.1 MSE (mean squared error )

argmin means “the argument that minimizes the following function. " : fine the optimal paramater of prediction function 

![[image/Pasted image 20250526093722.png]]




we want to the global minimun 

![[image/Pasted image 20250512153323.png]]



### 4.1.1 MSE minimization using calculus 用微积分 用求导 


![[image/Pasted image 20250526101505.png]]

![[image/Pasted image 20250526101513.png]]


![[image/Pasted image 20250526101523.png]]


结果 
![[image/Pasted image 20250526101535.png]]

### 4.1.2 MSE minimization using algebra

![[image/Pasted image 20250526101554.png]]

![[image/Pasted image 20250526101615.png]]


![[image/Pasted image 20250526101623.png]]


结果是 
![[image/Pasted image 20250526101648.png]]


### 4.1.3 Minimum value of MSE is the sample variance

![[image/Pasted image 20250526101710.png]]



## 4.2 MAE mean absolute error


![[image/Pasted image 20250526102535.png]]


### 4.2.1 Median minimizes MAE


optimal paramter ist the median value of y 

![[image/Pasted image 20250526102551.png]]



## 4.3 MSE vs MAE

Mean squared error (optimal parameter for the constant model is the sample mean)
▪ Very smooth. Easy to minimize using numerical methods (coming later in the course).
▪ Very sensitive to outliers, e.g. if we added 1000 to our largest observation, the optimal theta would become 225 instead of 25 

Mean absolute error (optimal parameter for the constant model is the sample median)
▪ Not as smooth – at each of the “kinks,” it’s not differentiable. Harder to minimize.
▪ Robust to outliers! E.g, adding 1000 to our largest observation doesn’t change the median.

It’s not clear that one is “better” than the other.
In practice, we get to choose our loss function!


![[image/Pasted image 20250526103043.png]]



L1 L2 loss 
▪ When we use squared (L2) loss as our loss function, the average loss across our dataset is called mean squared error.
    ▪ “Squared loss” and “mean squared error” are not the exact same thing – one is for a single observation, and one is for an entire dataset.
    ▪ But they are closely related.
▪ A similar relationship holds true between absolute (L1) loss and mean absolute error.
▪ Loss functions and summary statistics you already knew:
    ▪ The sample mean is the value of that minimizes the mean squared error.
    ▪ The sample median is the value of that minimizes the mean absolute error.
▪ “Average loss” and “empirical risk” mean the same thing for our purposes.
    ▪ So far, our empirical risk was either mean squared error, or mean absolute error.
    ▪ But generally, average loss / empirical risk could be the mean of any loss function across our dataset.



# 5 Correlation

Correlation and Causation
Association is not Causation
▪ Correlation only measures association.
▪ Correlation does not imply causation.

Example:
▪ Though the correlation between the weight and the math ability of children in a school district may be positive, that does not mean that doing math makes children heavier or that putting on weight improves the children’s math skills.
▪ Age is a confounding variable: older children are both heavier and better at math than younger children, on average.


---

Limitations of Correlation
▪ Correlation measures only linear association.
    ▪ Variables that have strong non-linear association might have very low correlation.
    ▪ A perfect quadratic relation y=x2 has a correlation equal to 0.
▪ Correlation is Affected by Outliers
    ▪ Outliers can have a big effect on correlation.
▪ Correlation of aggregated values might be misleading

![[image/Pasted image 20250526104112.png]]









## 5.1 correlation coefficient

The correlation coefficient
▪ measures the strength of the linear relationship between two variables.
▪ Graphically, it measures how clustered the scatter diagram is around a straight line.
▪ Short terms: correlation and 𝔯

Properties:
▪ 𝔯 is a number between −1 and 1.
▪ 𝔯 measures the extent to which the scatter plot clusters around a straight line.
▪ 𝔯 = 1 if the scatter diagram is a perfect straight line sloping upwards: Positive correlation
▪ 𝔯 = −1 if the scatter diagram is a perfect straight line sloping downwards.: Negative correlation


𝔯 is the average of the products of the two variables, when both variables are measured in standard units:

![[image/Pasted image 20250526103935.png]]


![[image/Pasted image 20250526104617.png]]



# 6 Regression Line


==解释 correlation coefficient (r) 用于  生成 Regression  Line ==

![[image/Pasted image 20250526103545.png]]



![[image/Pasted image 20250526104755.png]]



![[image/Pasted image 20250526105507.png]]


![[image/Pasted image 20250526105528.png]]


![[image/Pasted image 20250526105542.png]]




# 7 Minimizing MSE for the SLR model

MSE, Mean Squared Error
Simple Linear Regression（简单线性回归）


用 minimize MSE 的   方法 去 得到 SLR 曲线的参数 

![[image/Pasted image 20250526105820.png]]


![[image/Pasted image 20250526112850.png]]


![[image/Pasted image 20250526113151.png]]


![[image/Pasted image 20250526113446.png]]



最终 a, b 的值 
![[image/Pasted image 20250526113502.png]]



# 8 Multiple Linear Regression


## 8.1 Inputs

First, some terminology. For our purposes, all of these terms mean the same thing:
▪ Feature.
▪ Covariate.
▪ Independent variable.
▪ Explanatory variable.
▪ Predictor.
▪ Input.
▪ Regressor.

In the regression context, each of the above things has a “weight” assigned to it, given by the parameter. We also call these weights “coefficients.” For instance, in ො𝑦 = 𝜃0 + 𝜃1𝑥 , we might say the “weight” associated with the constant/intercept term is 𝜃0, and the “weight” associated with the x term is 𝜃1.

![[image/Pasted image 20250526113938.png]]



## 8.2 Multiple linear regression

![[image/Pasted image 20250526114612.png]]


General notation

![[image/Pasted image 20250526114645.png]]




# 9 Evaluating models


What are some ways to determine if our model was a good fit to our data?
▪ Look at MSE or RMSE.
▪ Look at the correlations.
▪ Look at a residual plot.
    ▪ Residuals are defined as being the difference between actual and predicted y values
        ![[image/Pasted image 20250526114904.png]]



# 10 RMSE and Multiple R²

R indicate the correlation coefficient of Regression Line  , 见[[05_01_IntroductionToModeling#5.1 correlation coefficient]]]

RMSE: Root mean squared error,  square root of the mean squared difference 

![[image/Pasted image 20250526115005.png]]

Root mean squared error is defined as being the square root of the mean squared difference between predictions and their true values.
▪ It is the square root of MSE, which is the average loss that we’ve been minimizing to determine optimal model parameters.
▪ ==RMSE is in the same units as y==.
▪ A lower RMSE indicates more “accurate” predictions.
    ▪ Lower average loss across the dataset


---

## 10.1 Comparing RMSEs


• For the constant model with squared loss, RMSE is . .
    • MSE(sample mean) = sample variance.
    • This is a good baseline to compare with.
• Using just the data we trained our model on, it is impossible for RMSE to go up by adding features.
    • If a new feature (e.g. “does a player like the color red?”) we’ve added doesn’t help lower average loss, its weight will just be set to 0.
    • When we start evaluating models on unseen data, this is no longer true.
• Soon, we will look at “training error” and “testing error”. The errors that we look at are RMSEs.

![[image/Pasted image 20250526115727.png]]

为什么 RMSEs 是一个 好的 baseline 

因为：
1. **简单明确**：常数模型不做任何假设、不用特征，只输出一个值。
2. **代表了最简单的情况**：不用模型，也能达到的最优效果。
3. **任何更复杂的模型都至少要比它好**，否则说明你过拟合了、模型没学到任何有用信息。
4. **RMSE（或者 MSE）可以和常数模型的 RMSE 对比，看模型提升了多少性能**



## 10.2 Multiple R²

==R indicate the correlation coefficient of Regression Line  , 见[[05_01_IntroductionToModeling#5.1 correlation coefficient]]]==


▪ When we had just one feature (x), we were able to look at the correlation coefficient r to get a sense of how strong the linear association between x and y was.
▪ The further r was from 0, the stronger the linear association between x and y.
    ▪ Looking at r alone isn’t enough. See Anscombe’s quartet.
▪ Here we have multiple features. We could (and sometimes do!) look at the correlation between each feature and our true y values individually.
▪ However, we are also interested in measuring the strength of the linear association between our actual y and predicted y.
    ▪ We want this relationship to be as close to the line y = x as possible.
![[image/Pasted image 20250526120233.png]]

---

We define the multiple R² value as the square of the correlation between the true y and predicted y. This is also referred to as the coefficient of determination.

Since it is the square of a correlation coefficient (which ranged between -1 and 1), R² ranges between 0 and 1. 

Another way of expressing R², in linear models that have an intercept term, is

Thus, ==we can interpret R² as the proportion of variance in our true y that our fitted values (predictions) capture==, or “the proportion of variance that the model explains.”

![[image/Pasted image 20250526120241.png]]


---

Multiple $R^2$ Example

• As we add more features, our fitted values tend to become closer and closer to our actual y values. Thus, R² increases.
    • The simple model (AST only) explains 45.7% of the variance in the true y.
    • The AST & 3PA model explains 60.9%.
• Adding more features doesn’t always mean our model is better, though!
    • We are a few lectures away from understanding why.
    • “Adjusted R²” accounts for this (see Stat 151A).


![[image/Pasted image 20250526120818.png]]


# 11 Matrix notation

![[image/Pasted image 20250526114420.png]]









