# Handling Missing Values in Tabular Data

This document provides an overview of how to handle missing values in tabular data using pandas in Python. It includes methods for detecting missing values, calculating the percentage of missing values, and strategies for handling missing data.

## 1. Detecting Missing Values

To detect missing values in a pandas DataFrame, you can use the following functions:

- **`df.info()`**: This function provides a quick summary of the DataFrame, including the count of non-null values in each column.
- **`df.isnull()`**: This function returns a DataFrame of the same shape, with `True` for missing values and `False` for non-missing values.
- **`df.isnull().sum()`**: This gives the total number of missing values in each column.

### Example:

```python
import pandas as pd

# Sample DataFrame
data = {'A': [1, 2, None, 4], 'B': [None, 2, 3, 4], 'C': [1, None, 3, 4]}
df = pd.DataFrame(data)

# Detect missing values
print(df.info())
print(df.isnull().sum())

```

## 2. Calculating the Percentage of Missing Values

To calculate the percentage of missing values in each column, you can use:

```python
missing_percentage = df.isnull().mean() * 100 print(missing_percentage)
```

## 3. Handling Missing Values

### 3.1. Imputing with Mean (for Missing at Random)

For columns with missing values that are missing at random, you can impute the missing values using the mean:

python

Copy code

```python
# Impute missing values with the mean df['A'].fillna(df['A'].mean(), inplace=True) 
df['B'].fillna(df['B'].mean(), inplace=True)  print(df)
```


### 3.2. Imputing with Mode (for Categorical Data)

For categorical data, the mode (the most frequent value) is a good choice for imputation:

python

Copy code

```python
# Impute missing values with the mode 
df['C'].fillna(df['C'].mode()[0], inplace=True)  print(df)
```

### 3.3. Handling Missing Not at Random (MNAR)

When data is missing not at random, the missingness depends on the observed data. In such cases, you might need more advanced techniques like:

- **Prediction models** (e.g., regression imputation or k-Nearest Neighbors)
- **Multiple Imputation**: Using multiple imputation methods (like `IterativeImputer` from sklearn) to estimate missing values.

#### Example of using `IterativeImputer`:

```python

from sklearn.experimental import enable_iterative_imputer
from sklearn.impute import IterativeImputer  
# Initialize the IterativeImputer 
imputer = IterativeImputer()  
# Fit and transform the data 
df_imputed = pd.DataFrame(imputer.fit_transform(df), columns=df.columns) print(df_imputed)
```
### What does the Iterative Imputer do?

The **Iterative Imputer** is a method of imputing missing values based on a model that predicts missing values using the other features in the dataset. It works iteratively by:

1. **Fitting a model** for each feature with missing values using the other features as predictors.
2. **Predicting the missing values** based on this model.
3. **Replacing the missing values** with the predicted values.
4. **Repeating the process** for a set number of iterations or until convergence (i.e., when the imputations stabilize)


 ### **K-Nearest Neighbors (KNN) Imputation**:
 

- **KNN imputation** fills in missing values based on the values of the nearest neighbors in the feature space. It works by finding the 'k' closest observations (neighbors) to the data point with missing values and taking a weighted average of their values.
- KNN is useful for imputing missing values when the relationships between data points are not linear and are based on similarity.

**Example using KNN imputer**:



```python
from sklearn.impute import KNNImputer 
# Initialize the KNNImputer 
knn_imputer = KNNImputer(n_neighbors=3)  
# Apply imputation 
df_imputed = pd.DataFrame(knn_imputer.fit_transform(df), columns=df.columns) print(df_imputed)
```

