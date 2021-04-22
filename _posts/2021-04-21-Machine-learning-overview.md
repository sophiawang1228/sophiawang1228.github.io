---
title: "Machine Learning Overview"
date: 2021-04-21
categories:
  - blog
tags:
  - Machine Learning
---

<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

# Definition of Machine Learning

> A computer program is said to learn from experience E with respect to some class of tasks T and performance measure P, if its performance at tasks in T, as measured by P, improves with experience E." -- Tom Mitchell


# Types of Machine Learning

In general, any machine learning problem can be assigned to one of two broad classifications: 


- Supervised Learning:
	We are told what are the right answers and we want the machine to predict on these samples.

	Example of Supervised Learning:

	- Linear/Logistic regression: predict on continuous variable output(Square cost functions)
	- Classification: discrete value output
	- Support Vector Machine (SVM): solve problems with infinite features
	- Naive Bayes (suppose normal distribution)
	- Decision Tree
	- K-NN(k nearest neighbors)


- Unsupervised Learning:
	Find structure from unlabelled data, and we are not given the correct answers.

	Sample usage of Unsupervised Learning: Social Network Analysis, Market Segmentation, Astronomial Data Analysis

	- Clustering: finding a structure or pattern in a collection of uncategorized data
		- Hierarchical clustering
		- K-means clustering
		- Principal Component Analysis (PCA)
		- Singular Value Decomposition (SVD)
		- Independent Component Analysis