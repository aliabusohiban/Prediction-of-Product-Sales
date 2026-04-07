# Prediction of Product Sales

## Project Overview
This project explores product sales data to understand the factors that influence sales and to build a regression model that predicts `Item_Outlet_Sales`.

The analysis focuses on data cleaning, exploratory data analysis (EDA), feature selection, and model evaluation. For the EDA section, I kept the two most useful visuals: the distribution of `Item_MRP` and the correlation heatmap.

## Business Problem
The goal is to estimate product sales from store and product attributes so the business can better understand pricing effects, product performance, and the strongest drivers of sales.

## Dataset
The dataset contains product and outlet attributes such as:

- `Item_Identifier`
- `Item_Weight`
- `Item_Fat_Content`
- `Item_Visibility`
- `Item_Type`
- `Item_MRP`
- `Outlet_Identifier`
- `Outlet_Establishment_Year`
- `Outlet_Size`
- `Outlet_Location_Type`
- `Outlet_Type`
- `Item_Outlet_Sales` (target)

## Tools Used
- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-Learn

## Data Preparation
The notebook includes:
- checking data types
- handling missing values
- fixing inconsistent categories
- removing duplicates
- selecting useful features for modeling
- encoding categorical variables

## Exploratory Data Analysis

### 1) Item MRP Distribution
`Item_MRP` is one of the most important numeric variables in the dataset. Its distribution helps show how product price points are spread across the data.

![Item MRP Distribution](images/item_mrp_distribution.png)

### 2) Correlation Heatmap
The heatmap shows the relationship between numerical variables and the target. This helps identify the strongest predictors and whether any variables move together.

![Correlation Heatmap](images/correlation_heatmap.png)

## Modeling
A regression model was trained to predict `Item_Outlet_Sales` using the processed feature set.

The workflow included:
- train/test split
- preprocessing
- model fitting
- prediction on train and test data
- evaluation using error metrics

## Evaluation
The model was assessed using standard regression metrics such as:
- MAE
- MSE
- RMSE
- R-squared

## Key Findings
- `Item_MRP` shows a strong relationship with sales and is one of the most useful features for prediction.
- The correlation heatmap is helpful for spotting relationships between numeric features and the target.
- A clear preprocessing pipeline improves model consistency and makes the analysis easier to reproduce.

## Conclusion
This project shows how product and outlet features can be used to predict sales with a regression model. The EDA suggests that price-related variables are especially important, and the final model provides a structured way to estimate sales from the available features.

## Repository Files
- `Prediction_of_Product_Sales.ipynb` — full analysis notebook
- `README.md` — project summary

## Author
Ali Abu Sohiban
