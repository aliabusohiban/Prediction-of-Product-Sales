# Prediction of Product Sales
**Author:** Ali Abu Sohiban

---

## Business Problem

A food retail company wants to understand what drives product sales across its outlets.
The goal is to build a machine learning model that can accurately predict `Item_Outlet_Sales`
based on product attributes and outlet characteristics — giving stakeholders a data-backed
tool for pricing, inventory, and outlet strategy decisions.

---

## Data

**Source:** `sales_predictions_2023.csv`  
**Rows:** 8,523  
**Columns:** 12 (9 features after dropping ID columns)

| Column | Type | Description |
|---|---|---|
| `Item_Identifier` | ID | Unique product ID (dropped) |
| `Item_Weight` | Numeric | Weight of the product |
| `Item_Fat_Content` | Ordinal | Low Fat or Regular |
| `Item_Visibility` | Numeric | % of display area allocated to product |
| `Item_Type` | Nominal | Product category (16 categories) |
| `Item_MRP` | Numeric | Maximum Retail Price |
| `Outlet_Identifier` | ID | Unique outlet ID (dropped) |
| `Outlet_Establishment_Year` | Numeric | Year the outlet was established |
| `Outlet_Size` | Ordinal | Small / Medium / High |
| `Outlet_Location_Type` | Ordinal | Tier 1 / Tier 2 / Tier 3 |
| `Outlet_Type` | Ordinal | Grocery Store / Supermarket Type 1–3 |
| `Item_Outlet_Sales` | **Target** | Sales of the product at the outlet |

---

## Methods

- Cleaned inconsistent category labels in `Item_Fat_Content` (3 aliases → 2 values)
- Imputed `Item_Weight` (17.2% missing) with **median**
- Imputed `Outlet_Size` (28.3% missing) with **most frequent** category
- Used **OrdinalEncoder** for features with natural order (`Outlet_Size`, `Outlet_Location_Type`, `Outlet_Type`, `Item_Fat_Content`)
- Used **OneHotEncoder** for `Item_Type` (no natural order, 16 categories)
- Applied **StandardScaler** to all numeric features
- All preprocessing fit on training data only to prevent data leakage
- Compared 3 models: Linear Regression, Random Forest (default), Random Forest (tuned via GridSearchCV)

---

## Results

### Model Comparison

| Model | Train R² | Test R² | Test MAE | Test RMSE |
|---|---|---|---|---|
| Linear Regression | 0.508 | 0.507 | $871.41 | $1,166.21 |
| Random Forest — Default | 0.938 | 0.547 | $775.87 | $1,118.02 |
| **Random Forest — Tuned** | **0.699** | **0.595** | **$736.45** | **$1,057.12** |

The **Tuned Random Forest** is the best model. It explains **59.5%** of the variance in
product sales on unseen data, with a mean absolute error of **$736** per item per outlet.

The default Random Forest had a training R² of 0.938 vs a test R² of 0.547 — a clear sign
of overfitting. Tuning reduced this gap significantly (0.699 train / 0.595 test).

Linear Regression is consistent between train and test (no overfitting) but has a lower
ceiling — it cannot capture the non-linear interactions between price and outlet type that
the tree-based model handles naturally.

---

## Linear Regression — Coefficients

<img width="790" height="590" alt="download" src="https://github.com/user-attachments/assets/429ebd7b-41ce-4379-8a62-8c6fab5d37de" />


### Interpretation

The top 3 most impactful features from the linear model:

**1. `Item_MRP` — coefficient: +983.68**
The single strongest predictor of sales. Every 1 standard deviation increase in item price
is associated with an increase of ~$984 in predicted sales. This makes direct business sense:
higher-priced items generate more revenue per unit sold.

**2. `Outlet_Type` — coefficient: +796.22**
Supermarket-type outlets (particularly Type 2 and Type 3) sell significantly more than
Grocery Stores. The model assigns a ~$796 premium to being in a higher-tier outlet type.
This is the second most important lever after price.

**3. `Outlet_Location_Type` — coefficient: −341.26**
Outlets in higher-tier locations (Tier 1 cities) are associated with *lower* predicted sales
in this dataset. This is counterintuitive but may reflect that Tier 3 locations have fewer
competing outlets, driving higher per-store volumes.

---

## Random Forest — Feature Importances

<img width="790" height="590" alt="download" src="https://github.com/user-attachments/assets/c56d82ce-8909-4479-90ac-14d98e272c13" />

### Interpretation

The top 5 most important features from the tuned Random Forest:

**1. `Item_MRP` — importance: 0.5201**
Dominates the model — 52% of all splits. The price of the item is by far the most
informative signal for predicting sales. This aligns perfectly with what the linear model
found. Price is the primary driver.

**2. `Outlet_Type` — importance: 0.3522**
The second dominant feature at 35%. Supermarkets vs. grocery stores have fundamentally
different sales volumes. Together, `Item_MRP` and `Outlet_Type` account for **87%** of
the model's predictive power.

**3. `Item_Visibility` — importance: 0.0445**
Shelf visibility is the third most used feature (4.4%). Its negative coefficient in the
linear model suggests a reverse-causality artifact — slow-selling items may be given more
shelf space to compensate — which the tree model partially captures through non-linear splits.

**4. `Outlet_Establishment_Year` — importance: 0.0376**
Older outlets tend to have more established customer bases and higher sales volumes. This
structural effect provides useful signal that the linear model largely ignored.

**5. `Item_Weight` — importance: 0.0196**
Heavier items tend to have higher prices and sell in lower unit volumes, but the weight
signal itself contributes ~2% of the tree's predictive information.

---

## Final Recommendations to Stakeholders

Based on the analysis, here are three actionable recommendations:

**1. Focus pricing strategy — it is the #1 driver of sales revenue.**
`Item_MRP` alone explains over 50% of sales variance. The company should use price
optimization tools to ensure high-margin items are priced to maximize revenue. Items in
the lower MRP range have significantly lower sales ceilings regardless of outlet placement.

**2. Prioritize inventory for Supermarket Type 2 and Type 3 outlets.**
`Outlet_Type` is the second largest driver. Supermarket-type outlets consistently
outperform Grocery Stores by a large margin. Stock allocation and promotional budgets
should be weighted toward these outlet types.

**3. Investigate high-visibility, low-sales items.**
`Item_Visibility` has a negative relationship with sales, which suggests some products
are receiving premium shelf space without a corresponding sales return. A shelf-space
audit — particularly for Dairy and "Others" categories (both negative in the linear model)
— could free up space for higher-performing categories like Seafood and Starchy Foods.

---

## Limitations

| Issue | Impact |
|---|---|
| `Outlet_Size` 28% missing | Imputing with most frequent may mask real size differences |
| `Item_Weight` 17% missing | Median imputation introduces small bias for weight-dependent predictions |
| No temporal data | Seasonality and promotional effects cannot be modelled |
| Test R² = 0.595 | ~40% of sales variance is unexplained — other factors (promotions, local competition, stock levels) are not in the dataset |

---

## For More Information

See the full analysis in [`Prediction_of_Product_Sales.ipynb`](Prediction_of_Product_Sales.ipynb).

**Author:** Ali Abu Sohiban  
**Contact:** [LinkedIn](https://linkedin.com) | [GitHub](https://github.com)
