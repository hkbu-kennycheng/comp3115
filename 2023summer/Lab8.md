# Lab 8: Support Vector Machine and KNN with Orange

---

# Introduction

In this lab, we will use Orange to classify data using Support Vector Machine (SVM) and K-Nearest Neighbors (KNN).

## Objectives

- Understand the concept of Support Vector Machine (SVM)
- Learn how to use Orange to classify data using SVM.
- Understand the concept of K-Nearest Neighbors (KNN)
- Learn how to use Orange to classify data using KNN.
- Understand the concept of cross-validation

## Prerequisites

- You have read the lecture notes on SVM and KNN.
- You have read the lecture notes on cross-validation.

## Dataset

In this lab, we will use the Iris dataset. The Iris dataset contains 150 instances of three classes: Iris-setosa, Iris-versicolor, and Iris-virginica. Each instance has four attributes: sepal length, sepal width, petal length, and petal width. The dataset could be loaded with the `File` widget in Orange. The dataset is located at `iris.tab`. Please drag the `File` widget to the canvas and connect it to the `Data Table` widget. The `Data Table` widget will display the data.

# Support Vector Machine (SVM)

Support Vector Machine (SVM) is a supervised machine learning algorithm that can be used for both classification and regression. The main idea of SVM is to find a hyperplane that separates the data into two classes. The hyperplane is chosen so that the distance between the hyperplane and the nearest data point from each class is maximized. The data points that are closest to the hyperplane are called support vectors.

## Linear SVM classifier

Linear SVM is used when the data is linearly separable. In this case, the hyperplane is a line that separates the data into two classes.
