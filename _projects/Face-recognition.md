---
title: "Face Recognition with Deep Learning Models"
categories:
  - blog
tags:
  - Machine Learning
  - Neural Network
  - Face Recognition
toc: true
toc_sticky: true
---
<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>


## 1. Intro

In this project, we will be dealing with a classic face recognition problem.
Throughout the process, we will be implementing different methodogy to 
improve our model, ranging from transforming inputs, to fine tuning the
archetecture. Also, we will be evaluating our models by using numerous
metrics.

## 2. Background

### Data
The dataset contains of 200 identities and over 30030 entries in total. 
Images are taken from the google search results and filtered with haar cascade
face detector. After filtering process each category is left with enough item
(mean 162, std: 62). The images are then resized to (224,224).

As for test set, we have 3236 entries

### Training

For this project, we will be using the following techniques for our training process.
- Mini-batch: our batchsize is set for 32 for all of the training and evaluation process. Given over 30000 training samples and 200 output class, mini-batch is our best choice.

- Early Stopping: the maximum number of epochs is 50, but we don't want to run 50 epochs each time training one model. We set up early stopping for training such that when validation error goes up, we will stop the training to prevent overfitting.

- 4 fold cross-validation: In order to achieve a more representitive evaluation of our model, we uses cross-validation during training process. We record the training loss and
validation loss for the four models, and report their average scorings. Then we will be 
using the best model to perform evaluation on test set.


### Metrics

For each model, we will be reporting it's performance from two perspectives

Overall model performance: we will be reporting the final training loss and validation loss averaged across four cross-validation rounds for each model. This is the most intuitive metrics on how well the model is performing.

Per class evaluation: 
From the 4 rounds of cross-validation, we will pick the model with the best
performance to evaluate. And since we have 200 classes, overall evaluation is not 
as informative as per class evaluation. We will be looking at
`Precision`, `Recall` and `BCR(Balanced Classification Rate` per class instead.

Moreover, we will also calculate averaged `Accuracy`, `Precision`, `Recall` and `BCR` over
class.

- Accuraccy: \\(\frac{\text{total correct predictions}}{\text{total predictions}}\\)

- Precision: \\(\frac{TP}{TP+FP}\\), precision indicates percentage of
true positives among all positive predictions. 

- Recall: \\(\frac{TP}{TP+FN}\\), recall indicates percentage of 
true positives among all correct predictions of this class.

- BCR: \\(\frac{\text{Precision+Recall}}{2
}\\), Balanced Classification Rate
indicates an overall performance on each class.

Note: Every model for this specific problem generally achievess 99% 
overall accuracy. One would assume that our model has learned 200 identities completely. But by looking at 
other scoring metrics, we realize that this is not the case.

A high overall accuracy is due to the high True Negative values. Since
we are predicting from 200 classes, for each prediction there are at least 
198 True Negative classes, 199 if it's a correct prediction.

Therefore, we should pay more attention to scores like Precision and Recall instead.



## 3. Methods


### 3.1 Baseline Model
In the begining, we start with a bareboned implementation of the
convolution network. The archetecture are as follows

Archetecture

| Layers | Dimension (In/Out/kernel/stride/padding)| Activation Function  |
| ------ |  :-----------------------------------:  |----------------------|
| 1      |  Conv2d: 3, 21, 3, 2, 1                 | ReLU                 |
| 2      |  Conv2d: 21, 20, 3, 2, 1                | BatchNorm(20), ReLU  |
| 3      |  Conv2d: 20, 15, 3, 2, 1                | BatchNorm(15), ReLU  |
| 4      |  Conv2d: 15, 7, 5, 2, 1                 | BatchNorm(7), ReLU   |
| 5      |  Linear: 1575, 201                      | ReLU                 |
| 6      |  Linear: 201, 201                       | (Softmax)            |


Hyperparameters

Batchsize: 32

Input Transformation: resize to 256

Learning rate: 0.001

Loss function: Cross Entropy Loss

Optimizer: Adam Optimizer

Cross-validation: 4

Initial weights: W ~ N(0,0.01)


### 3.2 Alexnet

As an important rule in Deep Learning, deeper nets generalize better as they
can learn all intermediate representation of the inputs. On the other hand
wider nets memorize better as they can store more information into their
units. 

Apparently our previous baseline model is not deep enough for this task, we 
need a more complicated archetecture for it. Alexnet is our choice.

Archetecture:

| Layers | Dimension (In/Out/kernel/stride/padding)| Activation Function  |
| ------ |  :-----------------------------------:  |----------------------|
| 1      |  Conv2d: 3, 96, 11, 4, 0                | ReLU, MaxPool2d(3,2) |
| 2      |  Conv2d: 96, 256, 5, 2, 0               | ReLU, MaxPool2d(3,2) |
| 3      |  Conv2d: 256, 384, 3, 1, 1              | ReLU                 |
| 4      |  Conv2d: 384, 384, 3, 1, 1              | ReLU                 |
| 6      |  Conv2d: 384, 256, 3, 1, 1              | ReLU, MaxPool2d(3,2) |
| 7      |  Linear: 9216, 4096                     | ReLU                 |
| 8      |  Linear: 4096, 4096                     | ReLU                 |
| 9      |  Linear: 4096, 201                      | (Softmax)            |


Hyperparameters:

Batchsize: 32

Input Transformation: resize to 256 -> crop to 227 -> normalize with mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225]

Learning rate: 0.0003

Loss function: Cross Entropy Loss

Optimizer: Adam Optimizer

Cross-validation: 4

Initial weights: W ~ N(0,0.01),bias = 1


### 3.3 Resampling and Cropping

Class Imbalance:

To address class imbalance, there're generally two directions to fix the problems.

- Modify input data set:
	- Oversampling: add more data in the minority class. You can achieve this
	either by collecting more data, or by replicating current data.

	Advantages: get's more data, more information!
	Disadvantages: replicated data might lead to overfitting model

	- Undersampling: reduce data from the most represented class.
	Advantages: easy to acheive
	Disadvantage: not suitable for all cases. When data is not sufficient,
	reduce data leads to lower accuracy.

- Modify loss function:
	- Instead of actually modifying our data, we can also address class imbalance
	by using weighted loss. Simply put, we put more penalty on mistake made on underrepresented class, forcing the network to treat them more importantly.

In this project, we address class imbalance by using oversampling on the minority class,
forcing all class to have at least 90 samples.


Archetecture: Same as Alexnet


Hyperparameter:

Batchsize: 32

Input Transformation: resize to 350 -> crop to 227 -> normalize with mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225]

Learning rate: 0.0003

Loss function: Cross Entropy Loss

Optimizer: Adam Optimizer

Cross-validation: 4

Initial weights: Xavier Normalization

### 3.4 Transfer Learning

Pretrained model:

Our pretrained ResNet(50) model is obtained from PyTorch. An additional linear(1000,201) layer was added 
before output in this case. We will be implementing finetuning methods since our dataset is still
relatively large.

Also, we will be implementing oversampling and resizing from previous method.

Hyperparameter:

Batchsize: 32

Input Transformation: resize to 350 -> crop to 224 -> normalize with mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225]

Learning rate: 0.001

Loss function: Cross Entropy Loss

Optimizer: Adam Optimizer

Cross-validation: 0

Initial weights: from pretrain model


## 4. Results

### 4.1 Baseline Model:

Model Performance

- Training Loss: 1.41
- Validation loss: 2.03

Performance weighted by class:

- Accuracy: 51.36%
- Precision: 54.79%
- Recall: 51.36%
- BCR: 53.07%


Analysis

Since our model only has about 50% accuracy, there's not much to analysis
on the classification results. Our model didn't learn enough features.

### 4.2 AlexNet

Model Performance:

- Training Loss: 1.26 
- Validation Loss: 1.72 (15% improvement)

Performance weighted by class:
- Accuracy: 61.19% (19% improvement)
- Precision: 65.47%
- Recall: 61.19%
- BCR: 63.33% (18% improvement)

Top 5/Bottom 5 classes for each category:

BCR: 

Top 3:  157[94.12%], 199[92.86%], 195[90.91%]

Bottom 3: 96[8.89%], 70[10.56%], 61[12.88%] 

Precision: 

Top 3:  160[100.00%], 112[100.00%], 2[100.00%]

Bottom 3: 96[6.67%], 70[10.00%], 115[15.38%]

Recall: 

Top 3:  157[100.00%], 113[100.00%], 199[100.00%]

Bottom 3: 61[9.09%], 70[11.11%], 96[11.11%]

Analysis:

Not surprisingly, our new Alexnet achieves 61.19% accuracy, which is 10% increase from 
baseline model. Now that we have a much more consistant performance, we can start analyzing test results. Let's see some examples from top 3 classes and bottom 3 classes
of BCR.

Top 3:
{% include figure image_path="/assets/images/face_recogn/class_157.png"  caption="94.12% BCR" %}

{% include figure image_path="/assets/images/face_recogn/class_199.png"  caption="92.86% BCR" %}

{% include figure image_path="/assets/images/face_recogn/class_195.png"  caption="90.91% BCR" %}

Bottom 3:

{% include figure image_path="/assets/images/face_recogn/class_96.png"  caption="8.89% BCR" %}

{% include figure image_path="/assets/images/face_recogn/class_70.png"  caption="10.56% BCR" %}

{% include figure image_path="/assets/images/face_recogn/class_61.png"  caption="12.88% BCR" %}

Noticed that for top classes, these pictures all have similar color distribution, 
high contrast hair color and skin color. And more importantly, these identites pretty much kept the same hair color and hair style
throughout different photos. Sometimes even their expressions stay similar.


For bottom classes, it seems that the different backgound color is interfering with our 
predictions. As for class 96 and 80, they have only 86 and 59 training samples as well. And plus that their appearance varies a lot due to drastic change in hairstyle and hair color.
Also, noted that there's only 93 and 54 training samples respectively,
class imbalance might play a role here as well.



### 4.3 Resampling and Cropping

Upon analysis, we discover that our network is not only learning the faces, but also 
learning the hairstyle and the background of the image. This is expected as our 
neural net model is trainging on all the information we feed to them, they learn
as they made mistakes and sometimes background color and hairstyle is helping with
the identification process!

Therefore, we resize the input to about 45% of it's original size and oversamples
minority classes.

Model Performance

- Training Loss: 
- Validation Loss: 1.35 (34% improvement)

Performance weighted by class:

- Accuracy: 68.05% (33% improvement)
- Precision: 71.33%
- Recall: 68.05%
- BCR: 69.69% (30% improvement)

Top 5/Bottom 5 classes for each category:

BCR: 

Top 3: 68[100.00%], 199[92.86%], 90[92.31%]

Bottom 5: 96[9.72%], 70[31.67%], 120[37.50%]

Precision: 

Top 3: 81[100.00%], 69[100.00%], 172[100.00%]

Bottom 3: 96[8.33%], 165[26.67%], 70[30.00%]

Recall: 

Top 3: 58[100.00%], 183[100.00%], 116[100.00%]

Bottom 3:  96[11.11%], 88[15.38%], 120[25.00%]

### 4.4 Transfer Learning

We followed PyTorch's tutorial on transfer learning, and picked ResNet50 as our
pretrained model.

Model Performance

- Training Loss: 0.0331
- Validation Loss: 0.2656 (34% improvement)

Performance weighted by class:

- Accuracy: 94.13% (84% improvement)
- Precision: 94.41%
- Recall: 94.13%
- BCR: 94.27%

Top 5/Bottom 5 classes for each category:

BCR: 

Top 5: 108[100.00%], 20[100.00%], 106[100.00%]

Bottom 5:  88[50.35%], 70[76.19%], 104[77.38%]

Precision: 

Top 5: 135[100.00%], 119[100.00%], 121[100.00%]

Bottom 5: 88[54.55%], 162[72.73%], 124[73.33%]

Recall: 

Top 5: 100[100.00%], 60[100.00%], 138[100.00%]

Bottom 5:  88[46.15%], 84[62.50%], 70[66.67%]



## 5. Conclusion

Starting from our vanilla model with only about 50% accuracy, we improves
our model steps by steps and finally reaches 94% accuracy.

First we implemented AlexNet from scratch and reaches about 60% accuracy.
It's better, but not enough. Then we take a deep look on what's stopping
our model from learning the faces.

By then we realize that having too much background information and 
imbalance class distribution are definitly not helping with the problem.
Therefore we kept center 45% of the original image, and oversample the
minority classes. By doing these we acheives 68% accuracy.

Finally, we tried out transfer learning, i.e. using models pretrained on
bigger datasets for our models. With the previous mentioned tricks and
pretrained ResNet(50) model, we finally reached a surprising 94% accuracy,
and therefore conclude this project. 

In this project, we explore differents methods to optimizing our classifiers
and actually see the difference they made.