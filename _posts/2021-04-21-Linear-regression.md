---
title: "Linear Regression"
categories:
  - blog
tags:
  - Machine Learning
  - Regression
---
<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

### What is Regression

Suppose the distribution of our data points follow a certain function, and the process of fitting such function on our data points is called `Regression`. 
Based on different hypothesis, we have `linear regression` or `logistic regression`. And the process of finding the coefficients are called `Parameter learning`.


### Linear Regression

We suppose there's a linear relationship between our features and our prediction.

- Hyphothesis function: 

$$h_{\theta} (x) = \theta^T x = \theta_0 x_0 + \theta_1 x_1 ... + \theta_n x_n$$ 

where \\(\theta\\) is our parameters and \\( x\\) is our data.


Now that you have your hypothesis function, your sample data points, how do you fit your function to the points? 

#### Parameter Learning

Without knowing the actual function, one thing we can do is to find the best approximation for the right function. To do so we need a direction towards the best fit, and a stepsize.

First you'll need to define a `cost function`, which calculates how far your function's prediction are from the data. This could be the difference, averaged difference, sum of squared distance etc. 

Eventually we want to find the set of parameters that can minimize this cost function, and we can achieve by using following techniques.

##### Gradient Descent

we take derivatives of this cost function with respect to each parameter, aka `gradient`. And the negative direction of gradient will always points to a minima of the function.

Now that we have a direction to minimize our cost function, we determine the stepsize by another parameter called `learning rate`, also denoted as \\(\alpha\\) .

In summary, this is what an update towards parameter that minimize cost function looks like (Suppose we have m samples of data and n features for each data):

$$ \theta_j = \theta_j - \alpha \frac{1}{m} \sum_{i=1}^{m} (h_{\theta}(x^{(i)})-y^{(i)})x_j^{(i)}, j=0,1...n $$ 


We repeat this process untill the convergence, and the steps described aboved are called `gradient descent`.

- Characteristics of Gradient Descent:
	- always converge to a local minimum
	- need to choose alpha/ too large: might not converge, too small: slow convergence
	- needs many iterations
	- works well when n(features) is large
	- Batch Gradient Descent: applieid to the entire dataset
	- Feature scaling: z-scoring features, make gradient descent faster

##### Normal Equation

Alternatively, we can explicitly calulate \\(\theta\\) by taking derivatives on matrix multiplication.

$$ \theta = (X^TX)^{-1}X^TY $$

Note: X is a mxn matrix where each row is a data point. Y is a mx1 vector.

- Characteristics of Normal Equation: 
	- slow if n is large due to \\((X^TX)^{-1},O(n^3)\\)
		- in practice, is n > 10000, use iterative method instead
	- if \\(X^TX\\) is non-invertible: 
		- cause: redundant features/ too many features
		- solution: deleting unneccessary features or use regularization.
