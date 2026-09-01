# Car-Price-Prediction-Regression

<div align="center">

# 🚗 Car Price Prediction

**Predicting car prices in the US market — and finding out which features actually drive them**

<img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python">
<img src="https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white" alt="scikit-learn">
<img src="https://img.shields.io/badge/pandas-150458?style=for-the-badge&logo=pandas&logoColor=white" alt="pandas">
<img src="https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white" alt="Jupyter">

<br><br>

<img src="https://img.shields.io/badge/Best_Model-Random_Forest-2ea44f?style=flat-square" alt="Best model">
<img src="https://img.shields.io/badge/R²-0.9595-2ea44f?style=flat-square" alt="R2">
<img src="https://img.shields.io/badge/MAE-$1,248-2ea44f?style=flat-square" alt="MAE">
<img src="https://img.shields.io/badge/Models_Compared-5-blue?style=flat-square" alt="Models">

</div>

---

## 📌 Problem Statement

A Chinese automobile company wants to enter the US market. They need to know:

> **1.** Which variables are significant in predicting the price of a car?
>
> **2.** How well do those variables describe the price of a car?

**Dataset:** `CarPrice_Assignment.csv` — 205 rows × 26 columns of car specifications and prices.

---

## 🛠️ Steps Performed

<table>
<tr><th align="left" width="30%">Step</th><th align="left">What was done</th></tr>

<tr>
<td valign="top"><b>1. Loading &amp; Preprocessing</b></td>
<td>
Checked shape, data types and summary statistics<br>
Checked missing values and duplicates — <b>none found</b><br>
Removed <code>car_ID</code> (row identifier)<br>
Extracted brand from <code>CarName</code> and fixed typos (<code>maxda</code>→<code>mazda</code>, <code>toyouta</code>→<code>toyota</code>, <code>vw</code>→<code>volkswagen</code>)<br>
Analysed correlations, outliers (IQR rule) and skewness<br>
One-hot encoded categorical columns<br>
Split 80/20 and applied scaling
</td>
</tr>

<tr>
<td valign="top"><b>2. Model Implementation</b></td>
<td>Built 5 models — Linear Regression, Decision Tree, Random Forest, Gradient Boosting, SVR</td>
</tr>

<tr>
<td valign="top"><b>3. Model Evaluation</b></td>
<td>Compared all 5 using R², MSE and MAE on the test set</td>
</tr>

<tr>
<td valign="top"><b>4. Feature Importance</b></td>
<td>Extracted Random Forest importances and verified by retraining on top features only</td>
</tr>

<tr>
<td valign="top"><b>5. Hyperparameter Tuning</b></td>
<td><code>GridSearchCV</code> with 5-fold CV across 36 parameter combinations</td>
</tr>

</table>

---

## 📊 Results

### Model Comparison

<div align="center">

| | Model | R² | MSE | MAE |
|:---:|:---|---:|---:|---:|
| 🥇 | **Random Forest** | **0.9584** | **3,282,767** | **1,288.83** |
| 🥈 | Gradient Boosting | 0.9242 | 5,985,429 | 1,710.34 |
| 🥉 | Linear Regression | 0.9097 | 7,128,547 | 1,763.57 |
| | Decision Tree | 0.9069 | 7,349,134 | 1,782.59 |
| | Support Vector Regressor | 0.8520 | 11,682,466 | 2,147.51 |

</div>

> **🏆 Random Forest is the best model.** It has the highest R² and the lowest MSE and MAE, so it wins on every metric. Car price depends on non-linear relationships between features, and averaging many trees keeps the model stable on a small dataset.

<details>
<summary><b>Why did the other models do worse?</b></summary>

<br>

**Gradient Boosting (0.9242)** — close behind, but slightly overfits with only 164 training rows.

**Linear Regression (0.9097)** — assumes price is a straight-line sum of features, which it isn't. Also affected by multicollinearity between the size variables.

**Decision Tree (0.9069)** — a single tree memorises the training data. Random Forest fixes this by averaging 100 of them.

**SVR (0.8520)** — the RBF kernel struggles with mostly 0/1 dummy columns and badly under-predicts expensive luxury cars.

</details>

### Feature Importance

<div align="center">

| Rank | Variable | Importance | Effect |
|:---:|:---|---:|:---|
| 1 | `enginesize` | **0.544** | ⬆️ Bigger engine → higher price |
| 2 | `curbweight` | **0.298** | ⬆️ Heavier car → higher price |
| 3 | `highwaympg` | 0.045 | ⬇️ Better mileage → lower price |
| 4 | `horsepower` | 0.033 | ⬆️ More power → higher price |
| 5 | `carwidth` | 0.013 | ⬆️ Wider car → higher price |
| 6 | `carlength` | 0.008 | ⬆️ Longer car → higher price |
| 7 | `brand_bmw` | 0.007 | ⬆️ Premium brand → higher price |

</div>

> **`enginesize` and `curbweight` alone account for ~84% of the model's importance.**
>
> Variables with almost no effect: `doornumber`, `carheight`, `fuelsystem`, `symboling`.

### Hyperparameter Tuning

<div align="center">

| | R² | MSE | MAE |
|:---|---:|---:|---:|
| Before tuning | 0.9584 | 3,282,767 | 1,288.83 |
| **After tuning** | **0.9595** | **3,193,302** | **1,248.12** |
| **Change** | 🔼 +0.0011 | 🔽 −89,464 | 🔽 −$40.71 |

</div>

Best parameters: `n_estimators=200`, `max_depth=None`, `min_samples_split=2`, `min_samples_leaf=1`

Performance improved slightly. The gain is small because Random Forest already performs well with default settings — bagging controls overfitting on its own.

---

## ✅ Conclusion

<table>
<tr>
<td width="50%" valign="top">

**Which variables are significant?**

Engine size and curb weight are the main drivers, followed by highway mileage (negative effect), horsepower and car width. Brand matters mainly for premium makes such as BMW.

</td>
<td width="50%" valign="top">

**How well do they describe price?**

Very well. The tuned Random Forest explains **~96%** of the variation in price, with an average error of **$1,248** on cars averaging $13,277 — under 10% error.

</td>
</tr>
</table>

<details>
<summary><b>📝 Note on outliers — why they were kept</b></summary>

<br>

The IQR rule flagged about 15 cars as high-price outliers. All of them were BMW, Porsche, Jaguar and Buick models — genuine luxury cars, not data errors.

They were kept because:

- Their price is high **because** their specs are high, which is exactly the relationship being modelled
- With only 205 rows, dropping 7% of the data is expensive
- The company needs the model to cover the premium segment too — a model that never sees a $45,000 car cannot predict one
- Tree models split on rank order, not magnitude, so outliers barely affect them

</details>
