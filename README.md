# Life Expectancy Prediction and Generalization Analysis

## Project Overview

This project investigates the determinants of **life expectancy across countries and over time** using a global country–year dataset that combines economic, demographic, health, infrastructure, and nutrition indicators.

Beyond predictive accuracy, the **central goal** of the project is to evaluate **how well machine learning models generalize** under increasingly realistic conditions. Because life expectancy data is structured as a panel (country × year), careful evaluation design is essential to avoid overly optimistic conclusions.

---

## Key Objectives

- Explain life expectancy using interpretable, mechanism-driven predictors  
- Compare linear and nonlinear models on structured global data  
- Quantify how performance changes under different generalization settings  
- Demonstrate why random train–test splits can be misleading in panel data  

---

## Dataset

- **Unit of observation:** Country–Year–Gender  
- **Time span:** 1990–2019  
- **Target variable:** Life Expectancy  
- **Final sample size:** 18,630 rows (after filtering non-country aggregates)

### Important data scope decision

Non-country aggregate entities (e.g. *“Africa”*, *“High-income countries”*) are removed prior to modeling. These aggregates exhibit extreme missingness and do not represent real countries, which would otherwise distort both descriptive analysis and model training.

---

## Features

Features are selected based on **theoretical relevance** and **data coverage**, not purely on availability.

### Main feature groups

- **Economic development**
  - log(GDP per capita)

- **Mortality burden**
  - Infant mortality rate  
  - Under-5 mortality rate  
  - Neonatal mortality rate  
  - Maternal mortality ratio (used cautiously)

- **Demographic structure**
  - Population age shares (0–14, 15–64, 65+)  
  - Dependency ratio (engineered)

- **Infrastructure & public health**
  - Basic drinking water access  
  - Basic sanitation services  
  - Clean fuel and technology

- **Nutrition**
  - Calories from animal protein  
  - Calories from plant protein  
  - Fat and carbohydrate calories  
  - Animal-to-plant protein ratio (engineered)

Sparse variables with very high missingness (e.g. medical personnel counts, tobacco/alcohol indicators) are excluded to avoid unstable estimation.

---

## Preprocessing

All models use a **single scikit-learn pipeline** to prevent data leakage:

- Numeric features: median imputation + standardization  
- Categorical features: most-frequent imputation + one-hot encoding  
- All transformations are learned **only on training data**

---

## Models

The following models are evaluated to balance interpretability and flexibility:

- Linear Regression  
- Ridge Regression  
- Lasso Regression  
- Random Forest  
- HistGradientBoostingRegressor  

---

## Evaluation Design: Three Train–Test Splits

Model performance is evaluated under **three increasingly difficult generalization settings**.

### 1. Random Split (Baseline)

- Random 80/20 split of country–year observations  
- Same countries appear in both train and test sets  

This setting yields **optimistic performance** and serves as an **upper bound**.

---

### 2. Time-Based Split

- Train: years ≤ 2012  
- Test: years > 2012  

Tests whether relationships learned from historical data generalize to **future periods**.

---

### 3. Country-Based Split (Most Important)

- Entire countries are held out using group-based splitting  
- Test set contains **countries never seen during training**

This is the **most realistic and demanding scenario**, requiring true cross-country generalization.

The country-based split prevents the model from exploiting stable country-specific patterns and exposes the limits of generalization.

---

## Results Summary

Performance follows a consistent and interpretable pattern:

**Random split → Time split → Country split**

| Split   | Best Model           | MAE (years) | RMSE | R²   |
|--------|----------------------|-------------|------|------|
| Random | Random Forest        | ~0.41       | ~0.62| ~0.996 |
| Time   | HistGradientBoosting | ~1.16       | ~1.68| ~0.956 |
| Country| HistGradientBoosting | ~1.43       | ~2.03| ~0.941 |

---

## Why Country-Split Performance Drops

Country identity implicitly encodes many latent factors not fully captured by observed features, including:

- Institutional quality and governance  
- Geography and climate  
- Historical shocks and conflict  
- Measurement and reporting differences  

Removing access to country identity forces the model to rely only on **generalizable structural relationships**, which explains the observed performance drop.

---

## Multivariate Structure (PCA)

Principal Component Analysis shows that:

- A small number of components capture most variance  
- The first component aligns with **demographic pressure and mortality burden**  
- Life expectancy varies primarily along this latent dimension  
- Countries cluster into interpretable development groups  

This confirms that life expectancy is shaped by **bundles of conditions**, not isolated variables.

---

## Key Takeaways

- Evaluation design matters as much as model choice  
- Random splits can be misleading in panel datasets  
- Country-based splits provide the strongest generalization test  
- Nonlinear models generalize better across countries  

---

## Reproducibility

- Random seeds are fixed  
- All preprocessing is pipeline-based  
- Results are fully reproducible  

---

## Author

**Murad Ibrahimov**  
Data Science Project  
Kaunas University of Technology
