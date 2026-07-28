# Dummy Variables & Interaction Terms in Regression Analysis

A regression analysis project examining how seasonality affects revenue for a manufacturing company, using dummy variables and interaction terms in an OLS model. Built for AFM244 (Analytics for Accounting and Finance) at the University of Waterloo.

## Overview

This project uses monthly financial and weather data (2011–2014) to model the relationship between production volume and revenue, while accounting for seasonal effects. Rather than assuming a single constant relationship between production and revenue year-round, the model tests whether **winter months** shift both the baseline revenue level and the sensitivity of revenue to production.

## Dataset

The data (`AICPA_regressionAnalysisData.csv`) contains monthly observations with the following fields:

| Column | Description |
|---|---|
| `type` | Split label — `dt4training` (2011–2013) or `dt4testing` (2014) |
| `date` | Month-end date |
| `revenue` | Monthly revenue |
| `production` | Monthly production volume |
| `coolDD` | Cooling degree days |
| `heatDD` | Heating degree days |

## Methodology

1. **Data preparation** — Converted `date` to datetime and inspected the raw dataset.
2. **Dummy variable creation** — Built a `winter_DV` indicator equal to 1 for December, January, and February, and 0 otherwise, using `date.dt.month.isin([12, 1, 2])`.
3. **Interaction term** — Created `winter_interaction = production × winter_DV` to test whether the production–revenue relationship itself changes in winter (not just the intercept).
4. **Train/test split** — Partitioned the data by the existing `type` column into a training set (2011–2013) and a holdout test set (2014).
5. **Model estimation** — Fit an OLS regression using `statsmodels`:

   ```
   revenue = β0 + β1(production) + β2(winter_DV) + β3(winter_interaction) + ε
   ```

## Results

| Coefficient | Estimate |
|---|---|
| Intercept (`const`) | 5,629,257.08 |
| `production` | 13.51 |
| `winter_DV` | -201,742.73 |
| `winter_interaction` | 14.16 |

**Interpretation:**
- Outside of winter, each additional unit of production is associated with roughly **$13.51** in additional revenue.
- Winter months carry a large negative intercept shift (**-$201,743**) on their own, but this is offset by a steeper production slope in winter — each unit of production adds an *extra* **$14.16** on top of the base effect during winter months.
- Combined, this suggests that in winter, revenue is more sensitive to production volume, even though the flat/baseline component of revenue is lower.

## Tools & Libraries

- Python
- `pandas`, `numpy` — data manipulation
- `statsmodels` — OLS regression
- `matplotlib` — visualization

## Files

- `AFM244_S26_Thursday_Dummy_Variables.ipynb` — full notebook with code, output, and walkthrough

## Key Takeaways

This project demonstrates core regression techniques for handling categorical/seasonal effects:
- Encoding categorical time periods as dummy variables
- Using interaction terms to let slopes vary across groups
- Structuring a proper train/test split for out-of-sample evaluation
