# 1. Chi2 Test

#### Purpose:

Tests whether two categorical variables are independent or associated.

#### Hypotheses:

- **Null Hypothesis (H₀)**: The two variables are independent (no association).
- **Alternative Hypothesis (H₁)**: The two variables are associated.

#### Assumptions:

1. Observations are independent (e.g., no repeated measurements).
2. The sample size is large enough. Typically, at least 80% of expected frequencies should be ≥ 5.

#### Test Statistic:

The test statistic is:

$$
\chi^2 = \sum \frac{(O_{ij} - E_{ij})^2}{E_{ij}}
$$

Where:

- $O_{ij}$ : Observed frequency in the $i$-th row and $j$-th column.
- $E_{ij}$ : Expected frequency under \(H_0\), calculated as 

$$
E_{ij} = \frac{(\text{Row Total} \times \text{Column Total})}{\text{Grand Total}}
$$

#### When to Use:

- When you have two categorical variables (e.g., gender vs. preference).
- When the dataset is large enough to meet the assumptions.

Hypothèses :

- H0 (Hypothèse nulle) : Il n'y a pas de relation significative entre le genre et l'orientation académique
- H1 (Hypothèse alternative) : Il existe une relation significative entre le genre et l'orientation académique


#### A practical example :

|              | Homme | Femme | Total |
| ------------ | ----- | ----- | ----- |
| **Droit**    | 20    | 30    | 50    |
| **Médecine** | 40    | 10    | 50    |
| **Total**    | 60    | 40    | 100   |


Étapes de calcul :

1. Calcul des effectifs théoriques
    - Droit, Hommes : (50 × 60) / 100 = 30
    - Droit, Femmes : (50 × 40) / 100 = 20
    - Médecine, Hommes : (50 × 60) / 100 = 30
    - Médecine, Femmes : (50 × 40) / 100 = 20
2. Calcul du Chi-deux (χ²) Formule : χ² = Σ [(Observé - Théorique)² / Théorique] Calculs :
    - Droit, Hommes : (20 - 30)² / 30 = 3,333
    - Droit, Femmes : (30 - 20)² / 20 = 5,000
    - Médecine, Hommes : (40 - 30)² / 30 = 3,333
    - Médecine, Femmes : (10 - 20)² / 20 = 5,000 
    - total = 3,333 + 5,000 + 3,333 + 5,000 = 16,666
1. Degré de liberté (dl) dl = (nombre de lignes - 1) × (nombre de colonnes - 1) dl = (2 - 1) × (2 - 1) = 1
2. Seuil de signification Généralement choisi à 0,05 (5%)
3. Valeur critique Pour dl = 1 et α = 0,05, la valeur critique est 3,841
4. Décision statistique Comme 16,666 > 3,841, on rejette H0 au seuil de 5%

Conclusion : Il existe une relation statistiquement significative entre le genre et l'orientation académique dans cet échantillon.

```python
from scipy.stats import chi2_contingency

# Create a contingency table
contingency_table = pd.crosstab(df['column1'], df['column2'])

# Perform the Chi-Square Test
chi2, p, dof, expected = chi2_contingency(contingency_table)

# Output results
print(f"Chi-Square Statistic: {chi2}")
print(f"P-value: {p}")
print(f"Degrees of Freedom: {dof}")
print("Expected Frequencies:")
print(expected)

```

# 2. Pearson's Correlation Test

#### Purpose:

Measures the strength and direction of a **linear relationship** between two continuous variables.

#### Hypotheses:

- **Null Hypothesis (H₀)**: There is no linear correlation between the two variables ($\rho = 0$).
- **Alternative Hypothesis (H₁)**: There is a linear correlation between the two variables ($\rho \neq 0$).
#### Assumptions:

1. Both variables are continuous and approximately normally distributed.
2. The relationship between the variables is linear.
3. Homoscedasticity (constant variance of errors).

#### Test Statistic:

The Pearson correlation coefficient (r):

$$
r = \frac{\sum (x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum (x_i - \bar{x})^2 \sum (y_i - \bar{y})^2}}
$$

Where:
- $x_i$ and $y_i$ are the individual data points.
- $\bar{x}$ and $\bar{y}$ are the means of the respective variables.

**P-Value**:
The p-value is determined by testing $r$ against a t-distribution:

$$
t = \frac{r \sqrt{n - 2}}{\sqrt{1 - r^2}}
$$
Where $n$ is the number of data points and the p-value is derived from this $t$-value.

#### When to Use:

- To test for a linear relationship between two continuous variables.
- When assumptions of normality and linearity are satisfied.

```python
from scipy.stats import pearsonr

# Pearson's correlation test
r, p_value = pearsonr(df['column1'], df['column2'])

print(f"Pearson Correlation Coefficient: {r}")
print(f"P-value: {p_value}")

```


# 3. Spearman's Rank Correlation Test

#### Purpose:

Measures the strength and direction of a **monotonic relationship** (increasing or decreasing, not necessarily linear) between two variables, which can be continuous or ordinal.

#### Hypotheses:

- **Null Hypothesis (H₀)**: There is no monotonic correlation between the two variables ($\rho = 0$).
- **Alternative Hypothesis (H₁)**: There is a monotonic correlation between the two variables ($\rho \neq 0$).
#### Assumptions:

1. Variables are at least ordinal.
2. No assumptions about the distribution of the variables.

#### Test Statistic:

The Spearman correlation coefficient ($r_s$):

$$
r_s = 1 - \frac{6 \sum d_i^2}{n(n^2 - 1)}
$$
Where:
- $d_i$ is the difference between the ranks of the $i$-th observation in the two variables.
- $n$ is the sample size.
#### P-Value:

The p-value is computed based on the test statistic and approximated using a t-distribution for larger sample sizes.

```python
from scipy.stats import spearmanr

# Spearman's correlation test
r_s, p_value = spearmanr(df['column1'], df['column2'])

print(f"Spearman Correlation Coefficient: {r_s}")
print(f"P-value: {p_value}")

```

#### When to Use:

- When the relationship between variables is monotonic but not linear.
- For ordinal data or when normality assumptions are violated.
- **Spearman's Test** can handle ordinal data or non-linear monotonic relationships.