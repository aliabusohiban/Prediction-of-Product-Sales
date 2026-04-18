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
`Item_MRP` is not randomly spread. The histogram shows a multi-modal pattern with four clear price bands, which suggests the products are grouped into distinct pricing segments rather than following one smooth continuous distribution. The mean and median are close, but that does not mean the feature is “normal”; the visible clusters show the data likely contains several underlying product classes. This matters because `Item_MRP` may behave more like a structured commercial pricing variable than a simple numeric field.

<img width="989" height="690" alt="EDA_Item_MRP" src="https://github.com/user-attachments/assets/2e74a8fd-f84a-47d3-9431-37c6da90d793" />


### 2) Correlation Heatmap
The correlation heatmap shows that most numeric features have little to no linear relationship with `Item_Outlet_Sales`. The only feature with a meaningful correlation is `Item_MRP` at 0.57, which indicates a moderate positive relationship with sales. `Item_Weight`, `Item_Visibility`, and `Outlet_Establishment_Year` are all near zero, so they do not appear to explain sales well on a linear basis. This tells us that the target is likely driven more by `Item_MRP` than by the other numeric variables, and that some important relationships may be nonlinear or hidden in categorical features.

<img width="659" height="590" alt="HeatMap" src="https://github.com/user-attachments/assets/6556c6fb-fb94-4592-a055-b4e1df53f46d" />

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
---

## 🤖 CRISP-DM Phase 4 — Modeling Results

### Model Comparison Summary

| Model | Train R² | Test R² | Train RMSE | Test RMSE | Overfit? |
|---|---|---|---|---|---|
| Linear Regression | 0.508 | 0.507 | 1,207.183 | 1,166.213 | ❌ No (underfit) |
| Random Forest (Default) | 0.938 | 0.548 | 429.783 | 1,116.357 | ⚠️ Yes (heavily) |
| Random Forest (Tuned) | 0.699 | 0.595 | 944.221 | 1,056.901 | ⚠️ Mild |

---

## ✅ CRISP-DM Phase 5 — Evaluation

### 4. Model Recommendation

**Recommended Model: Tuned Random Forest**

#### Why not Linear Regression?
Linear Regression scored nearly the same on training (R² = 0.508) and test data (R² = 0.507), which means it is **underfitting** — it is too simple to capture the patterns in the data. Roughly half of the variation in sales is left unexplained, making it unreliable for business decisions.

#### Why not the Default Random Forest?
The default Random Forest achieved a very high training R² of **0.938** but only **0.548** on test data — a gap of **0.39**. This is a classic sign of **severe overfitting**: the model memorized the training data instead of learning general patterns. It cannot be trusted on new data.

#### Why the Tuned Random Forest?
After GridSearchCV tuning, the Random Forest improved meaningfully:
- Test R² improved from **0.548 → 0.595** (+0.047)
- Test RMSE improved from **1,116.357 → 1,056.901** (saving ~$59 per prediction)
- The train/test gap shrank from **0.39 → 0.10**, meaning the overfitting was significantly reduced

It is the best balance between predictive power and generalizability across all three models.

---

### 📊 Interpreting Model Performance for Non-Technical Stakeholders

#### What does R² = 0.595 mean?
> Imagine 100 products in your stores with varying sales figures. Our model can explain **~60% of the reasons why some products sell more than others** — factors like the product's price range, the type of outlet, and its location. The remaining 40% is driven by things not captured in the data, such as seasonal promotions, local events, or customer preferences.
>
> In practical terms: **our model is a useful starting point for planning inventory and understanding which products and store types drive revenue**, but it should be used alongside business judgment, not as a replacement for it.

---

#### Metric for Stakeholders: RMSE = $1,056.90

**Why RMSE?**
RMSE (Root Mean Squared Error) was selected because it is expressed in the **same unit as sales (dollars)**, making it directly interpretable. It also penalizes large prediction errors more heavily, which matters in a retail setting where significantly under- or over-stocking a product is costly.

> On average, our model's sales predictions are off by approximately **$1,057 per product per outlet**. For context, the average item sale in this dataset is roughly $2,181. This means our model's typical error is about **48% of the average sale value** — acceptable for high-level planning and category decisions, but not precise enough for individual SKU-level inventory management without further refinement.

---

#### Overfitting/Underfitting Assessment

| | Train R² | Test R² | Gap | Assessment |
|---|---|---|---|---|
| Tuned Random Forest | 0.699 | 0.595 | 0.104 | **Mild overfit** |

The tuned model shows only a **0.10 gap** between training and test R², which is a significant improvement over the default Random Forest (gap of 0.39). A mild degree of overfitting is normal and acceptable — the model generalizes reasonably well to unseen data. Further improvement could be achieved by collecting more features, engineering new variables (e.g., product age, promotional flags), or experimenting with gradient boosting models such as XGBoost.
## Author
Ali Abu Sohiban
