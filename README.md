# Unemployment and Crime in the U.S.: Exploring State-Level Predictive Patterns

## Overview
This project evaluates how unemployment and related socioeconomic indicators predict **violent crime rates** across U.S. communities. We compare classical regression and machine-learning models—balancing **predictive accuracy** and **interpretability**—and assess performance with out-of-sample error. The data used is **Communities and Crime** dataset (UCI ML Repository), which aggregates 1990 Census, law-enforcement, and FBI UCR indicators at the community level. The outcome is **ViolentCrimesPerPop** (violent crimes per 100k).


## Key findings (brief)
- **Polynomial (log) OLS** and **LASSO (log-polynomial features)** achieve the lowest test MSE, outperforming baseline OLS and Random Forest.  
- **Random Forest** gives useful variable importance but higher test error than regularized regressions in this setting.  
- Socioeconomic structure (e.g., **two-parent households**, poverty) and employment indicators are strong predictors; relationships are **nonlinear** and sometimes **threshold-like**.

## Data
- **Source:** UCI ML Repository, *Communities and Crime* dataset (1,994 communities; 128 variables from 1990 Census, law enforcement, and FBI UCR).  
- **Outcome:** `ViolentCrimesPerPop` (violent crimes per 100,000 residents).  
- **Predictors (subset used):** `PctUnemployed`, `PctEmploy`, `PctLess9thGrade`, `PctBSorMore`, `perCapInc`, `PctFam2Par`, `PctPopUnderPov`, plus **state** to capture regional effects.  
- Notes from EDA: outcome is skewed; regional differences and nonlinear effects are evident. 

## Methods
- **Train/test split:** 70% / 30% random split; models evaluated on test MSE and variance explained.  
- **Pre-model checks:** PCA to inspect redundancy; VIF screening; Box-Cox recommending **log transform** of the response.  
- **Models compared:**
  - Baseline **OLS** (with stepwise/BIC variant)
  - **Polynomial OLS** (quadratic/cubic terms) and **log-polynomial OLS**
  - **Step-function OLS** (binned features for threshold effects)
  - **LASSO**: base, log-polynomial features, and binned features (10-fold CV for λ)
  - **Random Forest**: tuned `mtry` and `ntree`, OOB/CV selection
- **Diagnostics:** residual plots, heteroscedasticity checks, comparison via test MSE/AIC; feature importance for RF. :contentReference

## Results (selected)
- **Best predictive performance:** log-polynomial OLS and **LASSO** with log-polynomial features (lowest test MSE ≈ 0.010, highest adj. R² among candidates).  
- **RF**: captures interactions; test error higher than the best regression models; highlights importance of `PctFam2Par`, poverty, unemployment, and state effects.  
- **Interpretation:** unemployment and employment effects are **nonlinear** (diminishing returns/plateaus); threshold patterns appear when binned. :contentReference

## Repository contents
- `Unemployment_crime_report.pdf` — full write-up with figures, tables, and appendices.  
- `Unemploy_crime_proj.Rmd` - Data prep, EDA, and modeling notebooks/scripts (OLS, polynomial, LASSO, RF).
- `data/`  
  - `communities.data` — Comma-separated, **no header**. Each row = a community. Missing values are `"?"`. Columns are in the exact order defined in `communities.names`.
  - `communities.names` — Attribute documentation and **column order**. Lists variable names, types, and notes on identifier fields.

- `README.md`
