---
title: "Logistic Regression and Regularizaiton"
categories:
  - blog
tags:
  - Machine Learning
  - Logistic Regression
  - Regularization
---
<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

We have seen in the previous post that linear regression outputs the actual predicted value for any new data points. However, in some cases when we encounter a classification problem, we want our function to give us a probability instead, probability of the given dataset fall in to specific category (y=1 in most case).

### Logistic Regression 

Instead of predicting on the actual values, suppose we only want the answer as `Yes` or `No`, we are now dealing with a model that will give us a binary output that's either 1 or 0. Generally, we called the y=1 case being the positive class and y=0 being negative class.

Therefore, to optimize this new model, we would want a new hypothesis function that will give us an answer between 0  and 1.


#### Sigmoid function

$$ f(x) = \frac{1}{1+e^{-x}} $$

Before we dive into our new hypothesis function, first let's talk about what's a sigmoid function.

- A sigmoid function is monotonic, and has a first derivative which is bell shaped. 
- Conversely, the `integral of the cumulative distribution functions(CDF) for many common probability distributions` are sigmoidal. One such example is the `error function`, which is related to the CDF of a normal distribution.
- A sigmoid function is constrained by a pair of horizontal asymptotes as \\(x\rightarrow \pm \infty \\).
- A sigmoid function is convex for values less than 0, and it is concave for values greater than 0.
- The active region of a sigmoid ranges from -5 to 5.

#### Hypothesis function (Logistic function)

$$ h_{\theta}(x) = \frac{1}{1+e^{-\theta^Tx}} $$

Numerically, we are treating \\(\theta^Tx\\) as our error function, and taking an interval of the CDF of a normal distribution. Therefore, outputs from this logistic function actually represents the `probability that the our output is 1`.


#### Cost function

$$ J(\theta) = -\frac{1}{m} \sum_{i=1}^{m}[y^{(i)}log(h_{\theta}(x^{(i)})) -(1-y^{(i)})log(1-h_{\theta}(x^{(i)}))] $$

Vectorized:

$$ J(\theta) = \frac{1}{m}(-Y^Tlog(h)-(1-Y)^Tlog(1-h)) $$


#### Gradient Descent

Stepwise:

$$ \theta_j = \theta_j - \frac{\alpha}{m} \sum_{i=1}^{m} (h_\theta(x^{(i)})-y^{(i)})x_j^{(i)}, j=0,1,....n $$

Vectorized:

$$ \theta = \theta - \frac{\alpha}{m}X^T(g(X\theta)-Y) $$

where \\(g(X\theta)\\) is the sigmoid function.


Alternatively, you can use more complicated and optimized algorithm to calculate cost function and it's gradient

- More optimization algorithms: Conjugate descent, BFGS, L-BFGS

### Multiclass Classification

What if instead of binary output, we want the output to be \\(y \in {0,1,...,n}\\)?

We will solve this multiclass problem by converting it to `n+1` binary class problem.
Basically, we will be choosing one class and predicting it against rest of the class. Repeat the process for each class, and the final outcome is the class that has the maximum probabiliy.


#### Some problems you might encounter

- underfitting: high bias, did not fit well with train data
- overfitting: high variance, did not predict well with test data
	- reduce features: manually select/ model selection
	- regularization: reduce maginitude of features(works well with lots of slightly useful features)

#### Regularization:

In the case of overfitting, we might have some polynomial term that improves the accuracy of our hypothesis function on the sample data, but blows out significantly as we encounter new data (increase variance).

In this case instead of simply removed the polynomial terms, we can limit it's influence on our prediction during the process of regression by adding more cost to it.


If we have overfitting from our hypothesis function, we can reduce the weight that some of the terms in our function carry by increasing their cost.

$$ J(\theta) = Cost(h_{\theta}(x),y) + \lambda \sum_{j=1}^{n} \theta_j^2 $$

The λ, or lambda, is the `regularization parameter`. It determines how much the costs of our theta parameters are inflated. 


NOTE: If lambda is chosen to be too large, it may smooth out the function too much and cause underfitting. If λ=0 or is too small, there might be litter to no impact on the function.



##### Regularized Linear Regression

Gradient Descent:

$$ \theta_j = \theta_j - \alpha [(\frac{1}{m} \sum_{i=1}^{m} (h_\theta(x^{(i)})-y^{(i)})x_j^{(i)}) + \frac{\lambda}{m} \theta_j, j=1,....n $$

Another form:
$$ \theta_j = \theta_j(1-\alpha\frac{\lambda}{m}-\frac{\alpha}{m} \sum{i=1}^{m} (h_\theta(x^{(i)})-y^{(i)})x_j^{(i)})) $$

Normal Equation:

$$\theta = (X^TX +\lambda L)^{-1}X^TY $$

where L is the (n+1)x(n+1) identity matrix.

##### Regularized Logistic Regression

Cost function:

$$ J(\theta) = -\frac{1}{m} \sum_{i=1}^{m}[y^{(i)}log(h_{\theta}(x^{(i)})) -(1-y^{(i)})log(1-h_{\theta}(x^{(i)}))] \frac{\lambda}{m} \sum_{j=1}^{n} \theta_j^2$$

Gradient Descent:
$$ \theta_j = \theta_j - \alpha[\frac{1}{m} \sum_{i=1}^{m} (h_\theta(x^{(i)})-y^{(i)})x_j^{(i)} + \frac{\lambda}{m}\theta_j], j=1,....n $$
