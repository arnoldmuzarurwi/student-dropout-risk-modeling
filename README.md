# Student Dropout Risk Modeling (NYS Education Data)

Count-based regression modeling of New York State student dropout data using Poisson and Negative Binomial models with feature selection and cross-validation.

Arnold Muzarurwi  
MS Data Analytics | Applied Machine Learning & Statistical Modeling  

---

## Project Overview

This project applies a full end-to-end data science workflow to analyze and predict student dropout counts across New York State school districts using official NYSED graduation outcomes data (2018–2019).

The objective is to:

- Identify school- and subgroup-level factors associated with higher dropout counts  
- Evaluate appropriate statistical models for count data  
- Compare Poisson, Negative Binomial, and OLS approaches  
- Select the best-performing and most interpretable model  

Target variable:

`dropout_cnt` — number of students who discontinued enrollment

---

## Dataset

**Source:** New York State Education Department (NYSED)  
**Academic Year:** 2018–2019  

- Raw observations: 73,152  
- Cleaned dataset: 39,674 records  
- Final feature space: 140 engineered variables  

Key predictors include:

- `enroll_cnt`
- `nrc_code`
- `subgroup_code`
- `county_code`
- GED and credential counts

---

## Project Workflow

### 1. Exploratory Data Analysis

- Identified heavy right-skew in count variables  
- Strong correlation between `dropout_cnt` and enrollment size (ρ ≈ 0.66)  
- Kruskal-Wallis tests confirmed significant subgroup and geographic differences (p < 0.001)  
- Highest dropout rates observed among:
  - Migrant students (~39.6%)
  - English Language Learners (~35.2%)
  - NYC districts (13.2% vs 7.5% elsewhere)

---

### 2. Data Preparation & Feature Engineering

- Converted percentage strings to numeric
- Reconstructed suppressed values
- Removed multicollinear variables (VIF reduction)
- Applied Yeo–Johnson transformations
- Final dataset: no missing or infinite values

---

### 3. Feature Selection

Methods used:

- Variance Inflation Factor (VIF)
- RFECV
- LassoCV
- Random Forest permutation importance

Consistent top predictors:

- `enroll_cnt`
- County indicators
- Subgroup indicators

---

### 4. Model Comparison

| Model | RMSE | MAE | Notes |
|-------|------|-----|-------|
| Poisson (cats only) | 20.73 | 6.68 | Baseline |
| **Poisson (full model)** | **14.97** | **5.52** | Best count-scale performance |
| Negative Binomial | 25+ | 7+ | Overdispersion present |
| OLS (log-rate) | 0.67 | 0.52 | Log-scale prediction |
| OLS (raw counts) | 19.52 | 9.08 | R² = 0.82 |

**Preferred Model:** Poisson with exposure offset  

Cross-validation (5-fold):  
- CV RMSE ≈ 14.78  
- CV MAE ≈ 5.59  

---

## Key Insights

- School size is the dominant predictor (scale effect).
- Migrant and ELL students face over 3× higher dropout rates.
- Geographic effects are significant.
- Proper count modeling outperforms naive OLS approaches.

---

## Tech Stack

- Python 3.10+
- pandas
- numpy
- scikit-learn
- statsmodels
- matplotlib
- seaborn
- scipy

Install dependencies:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn statsmodels scipy
```

---

## How to Run

1. Clone the repository  
2. Open `A_Muzarurwi_StudentDropout.ipynb`  
3. Restart runtime and run all cells  
4. Cleaned dataset will be generated during execution  

---

## What This Demonstrates

- Proper statistical modeling of count data  
- Handling overdispersion  
- Multi-method feature selection  
- Cross-validated model comparison  
- Interpretability via Incident Rate Ratios (IRRs)
