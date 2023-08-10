# Lab 7: Data Classification Using Perceptron and Artificial Neural Network (ANN) with Orange
---

# Introduction

In this lab, we will use Orange to classify data using perceptron and artificial neural network. Let's start with look at the following cheat sheet by sci-kit learn.

![](https://scikit-learn.org/stable/_static/ml_map.png)

## Objectives

- Understand the concept of perceptron
- Learn how to use Orange to classify data using perceptron.
- Understand the concept of artificial neural network with Multilayer Perceptron (MLP)
- Learn how to use Orange to classify data using MLP.

# Perceptron

**Perceptron** is a classification algorithm which shares the same underlying implementation with **Stochastic Gradient Descent (SGD)** Classifier. In fact, SGDClassifier is a more general algorithm than perceptron. Perceptron is only capable of performing binary classification, while SGDClassifier can perform both binary and multi-class classification. Perceptron is also limited to finding linear decision boundaries, while SGDClassifier can find non-linear decision boundaries as well. But Perceptron is more suitable for educational purposes due to its simplicity, while SGDClassifier is more suitable for practical purposes.

Actually, Perceptron is equivalent to SGDClassifier with the following parameters:

- loss="perceptron"
- learning_rate="constant"
- eta0=1 (the initial learning rate)
- penalty=None (no regularization)

## Loading dataset

In this lab, we will use the **Iris** dataset. The Iris dataset is a classic and very easy multi-class classification dataset. It contains 3 classes of 50 instances each, where each class refers to a type of iris plant. The dataset has 4 features: sepal length, sepal width, petal length, and petal width.

Please drag in a `File` widget and load the `iris.tab` file.

![](images/iris-file.gif)

## Splitting dataset into training and testing sets

Let's split the dataset into training and testing sets. Please drag in a `Data Sampler` widget and double-click on it to open the configuration window. Please set the following parameters:

- Fixed Proportion of data: 80%

After that, please click on the `Sample Data` button to apply the changes.


## Stochastic Gradient Descent (SGD) Classifier

Please drag in a `Stochastic Gradient Descent` widget and double click on it to open the configuration window. Please set the following parameters:

- Classifier: Perceptron
- Regression: Squared Loss
- Learning Rate: Constant
  - Initial Learning Rate: 1.0
- Number of Iterations: 400
- Regularization: None