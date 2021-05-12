---
title: "Neural Networks"
categories:
  - blog
tags:
  - Machine Learning
  - Neural Networks
---
<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

### Some Set Ups

In general, a Neural Network will have a basic structure that looks like this:

	Input Layers -> Hidden Layers -> Output Layers

Setting our data\\((X^{(0)})\\) as the input layer, and then send the weighted input(\\(\theta^TX)\\) to the next layer as input\\((X^{(1)})\\). 

The next layer will apply `activation function` and output the result as the input for next layer. 

Finally when we reached output layer, we would obtain our final output.

$$ X^{(0)} \xrightarrow{\theta^{(0)}X^{(0)} = a_0} g^{(0)}(a_0) = X^{(1)}
\xrightarrow{\theta^{(1)}X^{(1)} = a_1} g^{(1)}(a_1) = X^{(2)} \rightarrow ... 
\rightarrow g^{(k)}(a_k) = Y $$ 

Now let's look at Neural Network Models in more detail

#### Input Layer

Suppose now we have M samples of training data and N features for each sample,
then our input matrix\\((X)\\)is a `mx(n+1)` matrix where \\(x_0\\) (biased unit)is a mx1 vector of `1` and \\(x_1,...,x_m)\\) is our N input features.






\\(a_i^{(j)}\\) = activation of unit i in layer j

Depending on if the layer is a hidden layer, or 