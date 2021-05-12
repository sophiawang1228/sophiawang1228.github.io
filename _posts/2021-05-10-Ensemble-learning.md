---
title: "Ensemble Learning: Bagging and Boosting"
categories:
  - blog
tags:
  - Ensemble Learning
---

Ensemble learning is the process by which multiple models, such as classifiers or experts, are strategically generated and combined to solve a particular computational intelligence problem. Ensemble learning is primarily used to improve the (classification, prediction, function approximation, etc.


Bagging (Bootstrap Aggregating)

Majority Voting.

That is, different training data subsets are randomly drawn – with replacement – from the entire training dataset. Each training data subset is used to train a different classifier of the same type. Individual classifiers are then combined by taking a simple majority vote of their decisions. For any given instance, the class chosen by most number of classifiers is the ensemble decision. Since the training datasets may overlap substantially, additional measures can be used to increase diversity, such as using a subset of the training data for training each classifier, or using relatively weak classifiers (such as decision stumps). 

- Lower variance
- Random Forest

Boosting

 In boosting, resampling is strategically geared to provide the most informative training data for each consecutive classifier. In essence, each iteration of boosting creates three weak classifiers: the first classifier C1 is trained with a random subset of the available training data. The training data subset for the second classifier C2 is chosen as the most informative subset, given C1 . Specifically, C2 is trained on a training data only half of which is correctly classified by C1 , and the other half is misclassified. The third classifier C3 is trained with instances on which C1 and C2 disagree. The three classifiers are combined through a three-way majority vote. 

- Lower bias
- Adaboost
- Gradient Boosting Machine(GBM)
- XGBoost
- Light GBM

Dataset:
- Bagging: with replacement,
- Boosting: same set for each iteration, given different weights based on weak classifier's results

Sample Weights:
- Bagging: same weights for all samples
- Boosting: give misclassified samples more weights

Classifier Weights:
- Bagging: same for all
- Boosting: each weak classifier has own weights, 

Parallel calculation:
- Bagging: Yes,parallel computation on each classifier
- Boosting: No, later classifier depends on results ffrom previou classifier



