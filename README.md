# Life Expectancy Prediction and Generalization Analysis

## Project Overview

This project analyzes the determinants of life expectancy across countries using socioeconomic, demographic, and health-related variables. Beyond predictive accuracy, the main objective is to assess how well machine learning models generalize across time and across countries.

Because life expectancy data is structured by both country and year, particular attention is paid to evaluation design in order to avoid overly optimistic conclusions.

---

## Dataset Description

The dataset consists of country–year observations, where each row corresponds to a specific country in a given year. The target variable is life expectancy, and the feature set includes:

- Economic indicators (e.g., GDP per capita)
- Health indicators (e.g., infant mortality rate)
- Demographic variables
- Nutritional and dietary indicators (where available)

---

## Data Preprocessing

All models are trained using a unified preprocessing pipeline implemented with scikit-learn. This pipeline ensures consistency and prevents data leakage by applying all transformations within the training process only.

Preprocessing steps include:
- Handling missing values
- Scaling numerical variables
- One-hot encoding of categorical variables

---

## Modeling Approach

Multiple regression-based and tree-based models are evaluated. Each model is embedded within a preprocessing–model pipeline to ensure that preprocessing is applied identically across models and evaluation settings.

---

## Evaluation Strategy

To assess model robustness and generalization ability, three train–test splitting strategies are used. These represent increasing levels of difficulty.

### Random Split

In the random split setting, all country–year observations are randomly divided into training (80%) and testing (20%) sets.

This is the simplest evaluation scenario and serves as a baseline. Because observations from the same country can appear in both the training and test sets, performance estimates under this split tend to be optimistic. Results from this setting should therefore be interpreted as an upper bound on model performance.

---

### Time-Based Split

In the time-based split, the model is trained on observations from years up to and including 2012 and tested on observations from years after 2012.

This evaluation setting examines whether relationships learned from historical data generalize to future periods. It more closely resembles a real-world forecasting scenario and is more challenging than random splitting due to temporal changes in economic and health conditions.

---

### Country-Based Split

In the country-based split, training and testing sets are separated by country using group-based splitting. Entire countries are held out from training and appear only in the test set.

This is the most demanding evaluation scenario. The model must predict life expectancy for countries it has never seen before, making this split a strong test of true cross-country generalization.

---

## Model Training and Evaluation Procedure

For each train–test split and each model, the following procedure is applied:

1. Construct a preprocessing–model pipeline  
2. Train the model on the training set  
3. Evaluate predictive performance on the test set using error-based metrics  
4. Estimate feature importance on the test data  

This ensures fair comparison across models and evaluation settings.

---

## Feature Importance Analysis

Feature importance is estimated using permutation importance computed on the transformed feature space produced by the preprocessing pipeline.

Permutation importance measures the increase in prediction error when a feature’s values are randomly permuted, capturing the feature’s contribution to predictive performance. Computing importance on the transformed feature space ensures that importance scores correspond to the actual inputs used by the model, including features created through one-hot encoding.

Where available, native model-specific importance measures (such as coefficients or tree-based importances) are also recorded for comparison.

---

## Results Interpretation

Model performance consistently follows the pattern:

**Random split → Time-based split → Country-based split**

This monotonic decline in performance reflects increasing evaluation difficulty. The gap between random and country-based results highlights the importance of country-specific structures in determining life expectancy and underscores the challenges of generalizing predictions to unseen countries.

---

## Key Takeaways

- Random train–test splits provide optimistic performance estimates.
- Time-based splits reveal temporal instability in learned relationships.
- Country-based splits provide the strongest test of generalization.
- Proper evaluation design is critical when modeling global health outcomes.

---

## Reproducibility

All experiments are fully reproducible. Random seeds are fixed where applicable, and all preprocessing and modeling steps are implemented within scikit-learn pipelines to prevent data leakage.

---

## Author

Murad  
Data Science Project
