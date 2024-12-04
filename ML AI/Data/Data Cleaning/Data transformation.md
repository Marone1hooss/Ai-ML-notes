
## 1. Labeling Categorical Data

Categorical data must be converted into numerical representations for use in most machine learning algorithms.  
### 1.1. Label Encoding - Assigns a unique integer to each category. - Example: `Male = 0`, `Female = 1`.  


```python
from sklearn.preprocessing import LabelEncoder  
le = LabelEncoder()
df['category_encoded'] = le.fit_transform(df['category'])
```


### 1.2. One-Hot Encoding

- Converts categories into binary columns.
- Example: `Red, Green, Blue` → `Red: [1,0,0], Green: [0,1,0], Blue: [0,0,1]`.

```python
pd.get_dummies(df, columns=['category'])
```
