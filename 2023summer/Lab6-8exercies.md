# Lab 6 - 8 Exercise

Please complete the following exercises and submit your answers in `Orange format (.ows)` file. The file should contain all the widgets and connections between them. You can use the `Save As` option in the `File` menu to save the workflow.

## Q1

Import 'data_classification.csv' (x1, x2 are two features, y is the class label). Perform 10-fold cross-validation on this dataset using SVM with linear kernel. Output the accuracy of 10-fold cross-validation on this dataset.

## Q2

Perform 10-fold cross-validation on the dataset using SVM with 'rbf' kernel. Try different parameter settings for C = {0.1, 0.5, 1}. Output the accuracy of 10-fold cross-validation on the dataset with different values of C.

## Q3

Perform 10-fold cross-validation on the dataset using multi-layers perceptron. Try different parameter settings for hidden_layer_sizes = {(50,), (100,)}. Output the accuracy of 10-fold cross-validation on the dataset with different values of hidden_layer_sizes.

## Q4

Import 'diabetes_dataset.csv'. The output variable is the last column (i.e., column 'target'). Use 'bmi' as the only input variable and 'target' as the output variable. Perform linear regression to fit a straight to model the relationship between 'bmi' and 'target'. Visualize this two-dimensional data and the fitted straight line.
