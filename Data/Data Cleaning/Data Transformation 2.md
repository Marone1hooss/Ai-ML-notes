
 As we know that **Normal Distribution** is a very important distribution in **Statistics**, which is key to many statisticians for solving problems in statistics. Usually, the data distribution in Nature follows a Normal distribution (**examples like – age, income, height, weight, etc.,** ). But the features in the real-life data are not normally distributed, however it is the best approximation when we are not aware of the underlying distribution pattern.
 
## Function Transformations

### SQUARE TRANSFORMATION
#### **Intuition:**

- Squaring emphasizes larger values more than smaller ones, making differences between large and small values more pronounced.
- It’s useful for correcting **left-skewed distributions** by stretching values in the lower tail.

#### **Mathematical Formula:**
$$y = x^2$$

#### **When to Use:**

- Correcting left-skewed data.
- When large values need to have a stronger influence in the analysis.

#### **Python Code:**

```python
df['transformed_column'] = df['column'].apply(lambda x: x**2)
```


### Log Transformation 
#### **Intuition:**

- Logarithmic transformation compresses the range of data by reducing the impact of extreme values.
- It’s particularly useful for **right-skewed distributions**, where large values dominate.

#### **Mathematical Formula:**

$$y=log⁡(x)y = \log(x)y=log(x)$$

#### **When to Use:**

- Correcting right-skewed data.
- Works only with positive values (x>0).

#### **Python Code:**

```python
import numpy as np
df['transformed_column'] = df['column'].apply(lambda x: np.log(x))
```

## Power transformation

### BOX-COX TRANSFORMATION
#### **Intuition:**

- The Box-Cox transformation makes data closer to normal by applying a flexible power transformation controlled by a parameter λ\lambdaλ.
- It’s effective for stabilizing variance and normalizing **positive data**.

#### **Mathematical Formula:**

$$y(\lambda) = \begin{cases} 
\frac{(x^\lambda - 1)}{\lambda}, & \lambda \neq 0 \\ 
\log(x), & \lambda = 0 
\end{cases}$$


#### **When to Use:**

- To normalize and stabilize variance in data.
- Only works for strictly positive values (x>0x > 0x>0).

#### **Python Code:**

```python
from scipy.stats import boxcox  
# Apply Box-Cox transformation
df['transformed_column'], lambda_value = boxcox(df['column']) print(f"Optimal lambda: {lambda_value}")
```


### YEO-JOHNSON TRANSFORMATION

#### **Intuition:**

- The Yeo-Johnson transformation generalizes the Box-Cox transformation to handle **positive, zero, and negative values**.
- It adapts the transformation to the data’s distribution using a parameter λ\lambdaλ.

#### **Mathematical Formula:**


$$
y(\lambda) = 
\begin{cases} 
\frac{(x+1)^\lambda - 1}{\lambda}, & \text{if } \lambda \neq 0 \text{ and } x \geq 0, \\ 
\log(x + 1), & \text{if } \lambda = 0 \text{ and } x \geq 0, \\ 
\frac{-((-x+1)^{2-\lambda} - 1)}{2-\lambda}, & \text{if } \lambda \neq 2 \text{ and } x < 0, \\ 
-\log(-x+1), & \text{if } \lambda = 2 \text{ and } x < 0.
\end{cases}
$$

​

#### **When to Use:**

- To normalize and stabilize variance in data that includes zero or negative values.

#### **Python Code:**

```python
from sklearn.preprocessing import PowerTransformer
# Apply Yeo-Johnson transformation 
pt = PowerTransformer(method='yeo-johnson') 
df['transformed_column'] = pt.fit_transform(df[['column']])
```


 
 ###  Models that suppose normal distribution of the variables

- Linear Regression
    
- Analysis of Variance (ANOVA)
    
- t-Tests
    
- Mixed-Effects Models
    
- General Linear Models (GLM) with Gaussian Distribution
    
- Discriminant Analysis (e.g., Linear Discriminant Analysis, Quadratic Discriminant Analysis)
    
- Factor Analysis
    
- Principal Component Analysis (PCA)
