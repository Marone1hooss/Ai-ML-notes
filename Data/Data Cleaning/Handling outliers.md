# How do you Detect Outliers?

Outliers are not always bad, they can indicate measurement errors or valid observations but extreme values. In this example we will use a dataset containing average IQ found on [Kaggle](https://www.kaggle.com/datasets/rizal1015/iq-and-gender)[.](https://www.kaggle.com/datasets/rizal1015/iq-and-gender) To detect outliers we can do some rare :

## 1. Data Visualization:

Graphical methods like Scatter Plots, Box Plots, and histograms can help identify outliers.

```python
import pandas as pd  
import matplotlib.pyplot as plt  
import seaborn as sns  
import numpy as np  
from scipy.stats import zscore  
from scipy import stats

#import Dataset  
df = pd.read_csv('IQ_GPA_and_Gender.csv')

sns.boxplot(data=df, x="iq")  
plt.show()
```


![[1_pvspeDBaZVwoWvZurw3PSQ.webp]]

The boxplot divides into 3 parts namely Q1, Q2, and Q3 where data **smaller than Q1 or larger than Q3** can be categorized as outliers.

## 2. Interquartile Range (IQR) Method for all the data distribution 


The IQR is the range between the first quartile (25th percentile) and the third quartile (75th percentile) of the data. Any data point that falls below Q1–1.5 *IQR or above Q3 + 1.5*IQR can be classified as an outlier.

```python
columns = ['iq']  
outlier_percentage = {}  
  
for column in columns:  
    column_data = df[column]  
  
    Q1 = np.percentile(column_data, 25)  
    Q3 = np.percentile(column_data, 75)  
    IQR = Q3 - Q1  
  
    lower = Q1 - 1.5 * IQR  
    upper = Q3 + 1.5 * IQR  
  
    outliers = column_data[(column_data < lower) | (column_data > upper)]  
    outlier_percentage[column] = len(outliers) / len(column_data) * 100  
  
# Display the outlier percentage for each column  
for column, percentage in outlier_percentage.items():  
    print(f"Count of outliers in column '{column}': {len(outliers)}")  
    print(f"Percentage of outliers in column '{column}':{percentage:.2f}%")  
    print()  
    print(f"Lower : {lower}")  
    print(f"Upper : {upper}")  
    print()  
    print(f"Data Outlier': {np.array(outliers)}")
    ```
    
## 3. For Normal distributions: the Z-Score

The **Z-score** represents the number of standard deviations a data point is from the mean. A Z-score greater than 3 (or less than -3) is often considered an outlier.

```python
import numpy as np from scipy import stats 
# Calculate Z-scores 
z_scores = np.abs(stats.zscore(df)) outliers = (z_scores > 3).all(axis=1)
```

## 2. Methods for Handling Outliers

Once outliers are detected, there are several approaches to handling them, depending on the situation and the impact of the outliers on your analysis.

### 2.1. Removing Outliers

In some cases, it might be appropriate to simply remove outliers from the dataset, especially if they are errors or irrelevant.

```python
# Remove rows with outliers
df_clean = df[~outliers]
```



### 2.2. Capping (Winsorization)

Instead of removing outliers, you can cap them to a specified upper and lower bound. This approach keeps the data but limits extreme values.



```python
# Capping values based on IQR 
df_clean = df.copy() df_clean[df_clean < (Q1 - 1.5 * IQR)] = (Q1 - 1.5 * IQR) 
df_clean[df_clean > (Q3 + 1.5 * IQR)] = (Q3 + 1.5 * IQR)```


### 2.3. Transformation

Applying mathematical transformations (like logarithmic or square root transformations) can reduce the effect of outliers by compressing the scale of the data.


```python
# Log transformation 
df['column_name'] = np.log1p(df['column_name'])
```



### 2.4. Imputation

If outliers are valid values but still considered as anomalies, you may replace them with a calculated value like the mean, median, or mode of the column.


```python
# Replace outliers with the median 
df_clean = df.copy() df_clean[outliers]=df['column_name'].median()
```


### Models Sensitive to Outliers (Require Outlier Treatment)

These models assume a specific relationship between features and the target variable, and outliers can significantly skew their results:

#### **Regression-Based Models**:

1. Linear Regression
2. Logistic Regression
3. Support Vector Machine (SVM) with linear and non-linear kernels
4. Ridge Regression
5. Lasso Regression
6. Elastic Net Regression

#### **Distance-Based Models**:

1. k-Nearest Neighbors (KNN)
2. k-Means Clustering
3. Principal Component Analysis (PCA)

#### **Neural Networks**:

1. Artificial Neural Networks (ANNs)
2. Convolutional Neural Networks (CNNs)
3. Recurrent Neural Networks (RNNs)

#### **Probabilistic Models**:

1. Gaussian Naive Bayes
2. Any model assuming normality in features (e.g., some parametric statistical tests)

#### **Gradient-Based Models**:

1. Gradient Descent-based models (e.g., vanilla Neural Networks, deep learning models) may converge poorly with outliers.

---

### Models Robust to Outliers (Do Not Necessarily Require Outlier Treatment)

These models are either insensitive to extreme values or have mechanisms to handle them effectively:

#### **Tree-Based Models**:

1. Decision Trees
2. Random Forest
3. Gradient Boosted Machines (e.g., XGBoost, LightGBM, CatBoost)
4. Extra Trees (Extremely Randomized Trees)

#### **Ensemble Models**:

1. Bagging-based models
2. Boosting-based models

#### **Robust Regression Models**:

1. Robust Linear Regression (e.g., Huber Regression, RANSAC Regression)

#### **Anomaly Detection Models**:

1. Isolation Forest
2. One-Class SVM
3. Local Outlier Factor (LOF)

#### **Clustering Algorithms**:

1. DBSCAN (Density-Based Spatial Clustering of Applications with Noise)
2. Hierarchical Clustering (less sensitive than k-Means)

#### **Neural Networks** (with robust training):

1. Autoencoders (used for anomaly detection)
2. Models trained with robust loss functions (e.g., Huber Loss)

