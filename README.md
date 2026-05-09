# Product Sales Prediction

**Author:** Ali Abu Sohiban

## Overview

This project analyzes retail product data to identify the factors that drive outlet sales and builds a regression model to predict `Item_Outlet_Sales`. The dataset contains 8,523 records across 12 product and store attributes, covering item characteristics, pricing, outlet type, and location.

The full analysis follows the CRISP-DM framework: data cleaning, exploratory analysis, feature engineering, model training, and evaluation.

---

## Data Insights

### 1. Item MRP Shows Four Distinct Price Bands

Rather than a smooth continuous distribution, `Item_MRP` clusters into four visible pricing tiers. This suggests products are grouped by commercial pricing strategy rather than priced independently. It matters for modeling because MRP behaves more like a structured categorical-ish variable than a plain numeric one.

![Item MRP Distribution](images/EDA_Item_MRP.png)

### 2. MRP Is the Only Numeric Feature That Correlates with Sales

The correlation heatmap shows that `Item_MRP` has a moderate positive correlation with `Item_Outlet_Sales` (r = 0.57). All other numeric features — item weight, visibility, and establishment year — are near zero. This means the remaining predictive signal is likely embedded in categorical features like outlet type and location tier, not in the numeric columns.

![Correlation Heatmap](images/HeatMap.png)

---

## Models and Results

Three regression models were built and evaluated using an 80/20 train/test split. Preprocessing included median imputation for numeric columns, ordinal encoding for structured categoricals, and one-hot encoding for nominal features — all handled inside a scikit-learn pipeline to prevent data leakage.

| Model | Train R² | Test R² | Test RMSE |
|---|---|---|---|
| Linear Regression | 0.508 | 0.507 | $1,166 |
| Random Forest (default) | 0.938 | 0.548 | $1,116 |
| Random Forest (tuned) | 0.699 | 0.595 | $1,057 |

**Recommended model: Tuned Random Forest**

Linear Regression underfits — it explains only ~50% of variance and leaves too much on the table. The default Random Forest overfits badly (train/test R² gap of 0.39). After GridSearchCV tuning, the Random Forest closes that gap to 0.10, improves test R² to 0.595, and reduces RMSE by ~$59 per prediction.

**What the metrics mean in plain terms:** the model explains roughly 60% of the variation in product sales across outlets. On average, predictions are off by about $1,057 — approximately 48% of the average sale value of ~$2,181. This makes the model useful for category-level planning and store performance analysis, but not precise enough for SKU-level inventory decisions without further refinement.

---

## Repository Contents

- `Prediction_of_Product_Sales.ipynb` — full analysis notebook with code, visualizations, and commentary
- `images/` — visualization assets used in this README
