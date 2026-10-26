# Data-Science-Capstone-Project-Wildfire-Prediction

Data Science Capstone (DATA 4380), University of Texas at Arlington, Fall 2026

Team: Daniel R., Humberto A., Thach T., Tanvir S.

## Overview

Texas has a lot of wildfires, and how big a fire gets depends on location, season, cause, and local conditions. In this project we use 41 years of Texas A&M Forest Service fire records to predict how many acres a fire will burn and to find which factors matter most. Later we plan to add weather, drought, and land cover data to improve the predictions.

This project is still in progress.

## Data

- **Source:** Texas A&M Forest Service (requested through their Open Records service)
- **Size:** 285,858 fires from 1985 to 2026, 17 columns
- **Target:** `acres` (acres burned)
- **Main features:** start date, county, latitude/longitude, agency, department, and fire cause

The raw data file is not included in this repo.

Data we plan to add:

- Wildfire risk estimates from Pyrologix
- Land cover data from the Texas Parks and Wildlife Ecological Mapping System
- Drought data from the U.S. Drought Monitor

## What we've done so far

**Exploratory data analysis**
- Checked missing values. `Contained` and `Controlled` dates are missing for most fires.
- Looked at outliers in `acres`. The data is very right-skewed: most fires are small, and a few are very large.
- Found invalid values, including one negative acreage and more than 13,000 fires listed as 0 acres.
- Summarized fires by county and by cause.

**Baseline models**
- Linear regression using date features (year, month, day of year, season), location, and categorical features like county and cause
- Preprocessing (imputation, scaling, one-hot encoding) inside a scikit-learn pipeline to avoid data leakage
- Evaluated with time-series cross-validation, so the model is always tested on fires that happened after the ones it trained on
- Compared predicting raw acres with predicting log(1 + acres)

## Baseline results

| Model | MAE (acres) | RMSE (acres) | R² |
|---|---|---|---|
| Linear regression, raw target | 254.39 | 2,865.52 | -0.544 |
| Linear regression, log target | 58.86 | 2,745.58 | 0.004 |

The log transform lowered MAE a lot, but R² is still close to zero. The large gap between MAE and RMSE comes from a small number of very large fires. A linear model can't capture these patterns well, which is why we're moving to nonlinear models next.

## Next steps

- Aggregate the fire records and combine them with the weather, drought, and land cover data
- Add a classification approach that predicts fire size category, alongside the regression model
- Try Random Forest, CatBoost, and LightGBM
- Evaluate with RMSE, PR-AUC, and Brier score, and use SHAP to explain the models
- Goal: reduce prediction error by at least 30% compared to the baseline
- Build an interactive map dashboard of the results

## Repo contents

| File | Description |
|---|---|
| `EDA_work.ipynb` | Main EDA: missing values, outliers, cause counts, county summaries, and a first baseline model |
| `Quick_EDA_Baseline_DER.ipynb` | Quick EDA and a baseline multiple linear regression |
| `Improved_Baseline_Modeling.ipynb` | Baseline models with time-series cross-validation, comparing raw and log-transformed targets |
