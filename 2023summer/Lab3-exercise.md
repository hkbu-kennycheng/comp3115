# Lab 3 Exercise

Please complete the following exercises and submit your answers in `Orange format (.ows)` file. The file should contain all the widgets and connections between them. You can use the `Save As` option in the `File` menu to save the workflow.

## Exercise 1

Load the `loan.csv` dataset into Orange and show the general information of the dataset.

## Exercise 2

Remove uninformative column 'Loan_ID' from the dataset.

## Exercise 3

Fill the missing values in columns `Gender`, `Married`, `Dependents` and `Self_Employed` by mode

## Exercise 4

Fill the missing values in column `Loan_Amount_Term` by median

## Exercise 5

Fill the missing values in column `Credit_History` by mean

## Exercise 6

Fill the missing values in column `LoanAmount` by the mean amount of each {`Gender`, `Married` and `Self_Employed`} group.

## Exercise 7

Mapping the Ordinal Feature `Dependents` to numeric by:

- `0` -> `0`
- `1` -> `1`
- `2` -> `2`
- `3+` -> `3`

## Exercise 8

Mapping nominal features `Gender`, `Married`, `Education` and `Self_Employed` to numeric features.

- `Gender`
  - `Male` -> `0`
  - `Female` -> `1`
- `Married`
  - `Yes` -> `0`
  - `No` -> `1`
- `Education`
  - `Graduate` -> `0`
  - `Not Graduate ` -> `1`
- `Self_Employed`
  - `Yes` -> `0`
  - `No` -> `1`

## Exercise 9

One-hot encoding for `Property_Area` column and drop original `Property_Area` column.

## Exercise 10

Apply Z-score normalization to columns `ApplicantIncome` and `CoapplicantIncome`