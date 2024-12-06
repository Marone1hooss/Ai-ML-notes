
## 1. Labeling Categorical Data

Categorical data must be converted into numerical representations for use in most machine learning algorithms.  
### 1.1. Label Encoding - Assigns a unique integer to each category. - Example: `Male = 0`, `Female = 1`.  


```python
from sklearn.preprocessing import LabelEncoder  
le = LabelEncoder()
df['category_encoded'] = le.fit_transform(df['category'])
```

- **Description**: Assigns a unique integer to each category.
    - Example: `{'Red': 0, 'Green': 1, 'Blue': 2}`
- **Advantages**:
    - Simple and memory-efficient.
    - Preserves ordinal relationships if categories have a natural order.
- **Disadvantages**:
    - Introduces an artificial ordinal relationship for nominal data, which may mislead some algorithms.
- **When to Use**:
    - When categories are **ordinal** (e.g., `Low < Medium < High`).
    - With **tree-based models** (e.g., Decision Trees, Random Forest) that are not affected by arbitrary integer labels.

### 1.2. One-Hot Encoding

- Converts categories into binary columns.


```python
pd.get_dummies(df, columns=['category'])
```


- **Description**: Creates binary columns for each category, with a `1` indicating the presence of the category and `0` otherwise.
    - Example: `{'Red': [1,0,0], 'Green': [0,1,0], 'Blue': [0,0,1]}`
- **Advantages**:
    - No ordinal assumptions; treats all categories equally.
    - Works well with most machine learning algorithms.
- **Disadvantages**:
    - Can lead to a **high-dimensional dataset** if there are many categories.
    - Inefficient for models sensitive to feature space (e.g., linear regression).
- **When to Use**:
    - When categories are **nominal** (no order).
    - For algorithms that are sensitive to numerical relationships in input data (e.g., Neural Networks, Logistic Regression).
### 3. Target Encoding (Mean Encoding)


- **Description**: Replaces each category with the mean of the target variable for that category.
    - Example:
        - Target (`Sales`): `{'Red': 100, 'Green': 200, 'Blue': 300}`
        - Encoded: `{'Red': 100, 'Green': 200, 'Blue': 300}`
- **Advantages**:
    - Captures the relationship between the category and the target variable.
    - Reduces dimensionality compared to one-hot encoding.
- **Disadvantages**:
    - Can lead to **data leakage** if not done properly (e.g., without stratified or fold-based encoding in training).
    - Depends heavily on the target variable's distribution.
- **When to Use**:
    - When the number of categories is high, and one-hot encoding is impractical.
    - In **regression tasks** or when there’s a strong relationship between categories and the target variable.
    - Typically used with algorithms like **gradient boosting models** (e.g., XGBoost, LightGBM).

```python
import category_encoders as ce
# Applying Target Encoding
encoder = ce.TargetEncoder(cols=['Category']) df['Category_encoded']= encoder.fit_transform(df['Category'], df['Target']) print(df)
```

### 4. Frequency Encoding


- **Description**: Replaces categories with their frequency or count in the dataset.
    - Example: `{'Red': 50, 'Green': 30, 'Blue': 20}` (if these are the counts of each color in the dataset).
- **Advantages**:
    - Simple to compute and memory-efficient.
    - Preserves category importance if the frequency is meaningful.
- **Disadvantages**:
    - Ignores relationships with the target variable.
- **When to Use**:
    - When frequency of categories holds significance (e.g., popularity or occurrence).
    - For quick encoding in exploratory data analysis or feature selection.

    ```python
# Frequency Encoding 
frequency_map = df['Category'].value_counts().to_dict() df['Category_freq_encoded'] = df['Category'].map(frequency_map) print(df)
```


### 5. Binary Encoding

- **Description**: Converts categories to binary digits and represents them as binary vectors.
    - Example: `{'Red': 1 → [0,1], 'Green': 2 → [1,0], 'Blue': 3 → [1,1]}`
- **Advantages**:
    - Reduces dimensionality compared to one-hot encoding.
    - Retains some information about category relationships.
- **Disadvantages**:
    - May introduce unintended relationships between categories.
- **When to Use**:
    - When the number of categories is large, but you want to reduce dimensionality without losing too much information.
    
```python
# Binary Encoding 
encoder_binary = ce.BinaryEncoder(cols=['Category']) 
df_binary_encoded = encoder_binary.fit_transform(df)
print(df_binary_encoded)
```


## 2. Normalization and Standardization

- Normalization and standardization improve the performance of machine learning algorithms by ensuring similar scales for features.
- They prevent any one feature from dominating the others, leading to more accurate predictions.
- They also help to handle outliers and improve the computational efficiency of the algorithms.

### 2.1. Normalization

- Rescales values to a fixed range, usually [0,1].
- Recommended for distance-based models like KNN or Neural Networks.
- Normalization is particularly effective when the data distribution is unknown or non-Gaussian.
- Algorithms that do not make assumptions about the data distribution, like k-nearest neighbors and neural networks, can benefit from normalization.
  
- **Formula**: 
$$ X_{\text{norm}} = \frac{X - X_{\text{min}}}{X_{\text{max}} - X_{\text{min}}}
 $$


```python
from sklearn.preprocessing import MinMaxScaler  
scaler = MinMaxScaler() 
df[['feature']] = scaler.fit_transform(df[['feature']])
```

### 2.2. Standardization

- Centers data around the mean (0) and scales it to have a standard deviation of 1.
- Useful for algorithms sensitive to feature scaling like SVM or PCA.

$$X_{\text{std}} = \frac{X - \mu}{\sigma}$$
Where: 
- \(X\): Original value.
- Minimum and maximum values of the dataset.  $(X_{\text{min}}, X_{\text{max}})$
- $\mu$: Mean of the dataset. 
- $(\sigma$): Standard deviation of the dataset.



```python
from sklearn.preprocessing import StandardScaler  
scaler = StandardScaler() 
df[['feature']] = scaler.fit_transform(df[['feature']])
```


- Standardization works best when the data distribution is Gaussian or bell-shaped.
- Algorithms that assume a Gaussian distribution, like linear regression and logistic regression, can benefit from standardization

### Summary

1. **Normalization**:
    
    - Use when feature ranges differ significantly, and the model relies on distances (e.g., KNN, K-Means).
    - Prefer for non-Gaussian data.
2. **Standardization**:
    
    - Use when the algorithm assumes Gaussian-distributed data or is sensitive to data scaling (e.g., SVM, PCA, Logistic Regression).
    - Suitable for gradient-based optimization algorithms.
    

|**Aspect**|**Normalization**|**Standardization**|
|---|---|---|
|**Output Range**|Scaled to [0,1] or [-1,1]|Mean = 0, Standard Deviation = 1|
|**When to Use**|Distance-based models, Neural Networks|Models assuming Gaussian distributions|
|**Data Distribution**|Non-Gaussian|Gaussian|
|**Effect of Outliers**|More sensitive to outliers|Less sensitive to outliers|
|**Algorithm Suitability**|KNN, K-Means, DBSCAN, Neural Networks|SVM, Logistic Regression, PCA|

### Models that Require Normalization or Standardization (Sensitive to Distance):

1. K-Nearest Neighbors (KNN)
2. Support Vector Machines (SVM)
3. K-Means Clustering
4. Principal Component Analysis (PCA)
5. Linear Regression (with regularization)
6. Logistic Regression (with regularization)
7. Neural Networks

### Models Robust to Distance (Do Not Require Normalization or Standardization):

1. Decision Trees
2. Random Forests
3. Gradient Boosting Machines (GBM)
4. XGBoost
5. LightGBM
6. Naive Bayes
7. Ensemble Methods (like Bagging, Boosting)


