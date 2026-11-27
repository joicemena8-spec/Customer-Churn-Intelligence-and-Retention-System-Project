# Customer Churn Intelligence & Retention System

An end-to-end data science solution that studies customer behavior, identifies the factors behind churn, predicts which customers are likely to leave, and turns the findings into actionable retention recommendations. Built on the IBM Telco Customer Churn dataset as part of the Ruwad Masr initiative (DEPI) – Data Science track graduation project.

---

## 📌 Problem Statement

Telecom companies operate in a highly competitive market where customers can switch providers easily, and winning back or replacing a lost customer costs far more than keeping an existing one. In the IBM Telco dataset, **26.5% of customers (1,869 out of 7,043) have churned**, yet it is not clear who is leaving, why they leave, or at what point in their lifecycle. This project analyzes customer demographics, subscription details, services, payment methods, and charges to uncover the patterns behind churn, identify the high-risk customer segments, build models that score each customer's churn risk, and turn the findings into clear, data-driven retention recommendations.

---

## 🎯 Project Objectives

1. Analyze customer data and identify the main churn patterns and drivers.
2. Answer business questions using SQL.
3. Identify high-risk customer segments and recommend retention actions.
4. Prepare clean, model-ready data (feature engineering, encoding, scaling).
5. Build and compare machine learning models that predict churn.
6. Build an interactive dashboard to visualize insights.

---

## 📊 Dataset

**Telco Customer Churn – IBM Dataset**

- **Link:** [Kaggle – yeanzc/telco-customer-churn-ibm-dataset](https://www.kaggle.com/datasets/yeanzc/telco-customer-churn-ibm-dataset)
- **Rows:** 7,043 | **Columns:** 33
- **Target variable:** `Churn Value` (0 = retained, 1 = churned)

The full column list and meanings are in [`data_dictionary.md`](data_dictionary.md).

---

## 💡 Business Questions

1. What is the overall customer churn rate?
2. Which contract type has the highest churn rate?
3. How do monthly charges differ between churned and retained customers?
4. What are the most common reasons for churn?
5. Which internet service type has the highest churn rate?
6. How do payment methods relate to churn?
7. How does customer tenure relate to churn?
8. Do additional services reduce churn?

*Answers are in the notebook under **SQL Analysis & Business Questions** and **Exploratory Data Analysis**.*

---

## 🗄 Database Structure

SQLite database: `telco_churn.db` (schema in `sql/schema.sql`)

**Table: `telco_customers`** : one row per customer, 33 columns. Primary key: `CustomerID`.

| Group                | Main columns (type)                                                                                     |
| -------------------- | ------------------------------------------------------------------------------------------------------- |
| Customer information | CustomerID (TEXT, PK), Gender, SeniorCitizen, Partner, Dependents (TEXT)                                |
| Location             | Country, State, City (TEXT), ZipCode (INTEGER), LatLong (TEXT), Latitude, Longitude (REAL)              |
| Subscription         | TenureMonths (INTEGER), Contract, InternetService, PhoneService, MultipleLines, PaperlessBilling (TEXT) |
| Services             | OnlineSecurity, OnlineBackup, DeviceProtection, TechSupport, StreamingTV, StreamingMovies (TEXT)        |
| Payment              | PaymentMethod (TEXT), MonthlyCharges, TotalCharges (REAL)                                               |
| Churn                | ChurnLabel (TEXT), ChurnValue (INTEGER), ChurnScore (INTEGER), CLTV (INTEGER), ChurnReason (TEXT)       |
| Other                | Count (INTEGER)                                                                                         |

---

## 🔍 Top Insights

1. **Contract type is the strongest churn driver.** Month-to-month customers churn at **42.7%** vs. **11.3%** (one-year) and **2.8%** (two-year), and make up **88.6%** of all churn. → Convert month-to-month customers to annual plans with a targeted offer.
2. **Churn is concentrated in one segment.** Month-to-month + Fiber optic customers churn at **54.6%** and account for **62.2%** of all churn. → Make this segment the primary retention target.
3. **Churn is an early-life problem.** Churn is **49.8%** in the first 9 months vs. **7.7%** after 55 months. → Focus retention efforts on months 1–12.
4. **Competition, not price, is the main reason customers leave.** **33.2%** of churned customers named a competitor; only **5.2%** cited high prices. → Compete on speed, data, and devices rather than blanket discounts.

More insights, each with the number, its meaning, and a suggested action, are under every "Key Insights" section in the notebook.

---

## 🛠 Data Preprocessing

| Step                | What we did                                                                         | Why                                             |
| ------------------- | ----------------------------------------------------------------------------------- | ----------------------------------------------- |
| Cleaning            | Fixed `Total Charges` type, handled missing values, checked duplicates and outliers | Data quality                                    |
| Drop columns        | ID, constant, location, Gender, Churn Label/Score/Reason                            | No signal, or leaks the answer                  |
| Feature engineering | `Num Services`, `Tenure Group`, `Has Family`, `Auto Pay`                            | Capture patterns found in the EDA               |
| Encoding            | Yes/No → 1/0; one-hot for Contract, Internet Service, Payment Method, Tenure Group  | Models need numbers                             |
| Split               | 80% train / 20% test, stratified                                                    | Keep the churn ratio equal                      |
| Scaling             | `StandardScaler` on 5 numeric columns (fit on train only)                           | Helps Logistic Regression / SVM; avoids leakage |
| Imbalance           | Kept as is; `class_weight='balanced'` is tried during tuning                        | ~26.5% churn                                    |

---

## 🤖 Machine Learning

**Task:** supervised binary classification (`Churn Value`: 1 = churned, 0 = stayed).

**Main metric:** ROC-AUC, because it measures how well a model ranks real churners above non-churners, and only 26.5% of customers churn. Accuracy, precision, recall and F1 are reported as supporting metrics.

**Baseline:** always predicting "stays" gives **0.735 accuracy**, so any useful model has to beat that.

**Models built:** K-Nearest Neighbors, Support Vector Machine, Decision Tree (pre-pruned and post-pruned), Random Forest, and Logistic Regression. Hyperparameters were tuned with cross-validation on the training set only, and every model was then scored once on the same held-out test set (1,409 customers).

### Model comparison (test set, 0.5 cut-off)

| Model                       | Accuracy | ROC-AUC | Precision | Recall |    F1 |
| --------------------------- | -------: | ------: | --------: | -----: | ----: |
| Random Forest               |    0.807 |   0.852 |     0.669 |  0.540 | 0.598 |
| Logistic Regression         |    0.801 |   0.850 |     0.647 |  0.548 | 0.593 |
| Decision Tree (post-pruned) |    0.797 |   0.829 |     0.615 |  0.628 | 0.622 |
| KNN                         |    0.792 |   0.835 |     0.612 |  0.594 | 0.602 |
| Decision Tree (pre-pruned)  |    0.786 |   0.830 |     0.599 |  0.583 | 0.591 |
| SVM                         |    0.744 |   0.850 |     0.512 |  0.786 | 0.620 |

### Choosing the cut-off for a retention campaign

A 0.5 cut-off misses almost half of the real churners (recall about 0.55). For retention, catching churners matters more than overall accuracy, so the Logistic Regression model also has a tuned cut-off chosen **on the training data only** (out-of-fold F1, never on the test set):

| Logistic Regression | Cut-off | Accuracy | ROC-AUC | Precision | Recall |
| ------------------- | ------: | -------: | ------: | --------: | -----: |
| Default             |     0.5 |    0.801 |   0.850 |     0.647 |  0.548 |
| Tuned               |   0.338 |    0.775 |   0.850 |     0.558 |  0.738 |

Lowering the cut-off trades about 2.6 points of accuracy for roughly 19 more points of recall: the team contacts more customers who would have stayed anyway, but catches far more of those who would have left. Which setting is right depends on the cost of a retention offer versus the value of a saved customer.

### How to read the results

- Every model beats the 0.735 baseline, and the best ones add roughly 7–8 correct predictions per 100 customers.
- The top models (Random Forest, Logistic Regression, SVM) are within about 0.002 ROC-AUC of each other. With 1,409 test customers, accuracy has roughly ±1 point of uncertainty, so differences this small should not be over-interpreted.
- Accuracy around 80% is realistic for this dataset. The remaining errors come from churn that these features cannot explain, not from a missing trick.
- `Total Charges` is dropped for Logistic Regression because it is almost the same information as `Tenure × Monthly Charges`.
- The saved model, `models/best_model.pkl`, is the Random Forest (selected by test accuracy; 29 features; 0.5 cut-off). It overfits more than Logistic Regression (train accuracy 0.879 vs. test 0.807).

---

## 📈 Dashboard

### Dashboard plan

**KPIs:** Total Customers · Churned Customers · Churn Rate · Retention Rate · Average Monthly Charges · Average Tenure

**Charts:** Churned vs. Retained · Churn by Contract Type · Churn by Internet Service · Churn by Payment Method · Churn by Tenure · Most Common Churn Reasons · Monthly Charges Comparison

**Interactive filters:** Contract Type · Internet Service · Payment Method · Customer Characteristics (Senior Citizen, Partner, Dependents)

### Current status

A first Plotly version is built inside the notebook. It includes:

- **KPI:** Churn Rate
- Churn by Segment (Senior vs. Non-senior)
- Churn by Contract Type
- Churn Trend by Tenure
- Top 10 High-Risk Customers

📷 *Screenshot:* `dashboard/` *(add image)*

🔜 **Next:** add the remaining KPIs and charts above and the interactive filters, then move the dashboard into a standalone app (`dashboard/app.py`) that reads churn probabilities from the trained model.

---

## 📌 Project Status

| Stage                                                                                  | Status          |
| -------------------------------------------------------------------------------------- | --------------- |
| Data cleaning & validation                                                             | ✅ Done          |
| SQL business analysis (10 queries)                                                     | ✅ Done          |
| EDA: descriptive statistics, distributions, correlations, group-by, significance tests | ✅ Done          |
| Insights (observation → meaning → suggested action)                                    | ✅ Done          |
| Feature engineering, encoding & scaling                                                | ✅ Done          |
| Machine learning models & evaluation (6 models compared)                               | ✅ Done          |
| Plotly dashboard (first version)                                                       | ✅ Done (static) |
| Interactive dashboard with filters                                                     | 🔜 In progress   |

### Known limitations and next steps

- Results come from a single train/test split; cross-validated scores with confidence intervals would make the model comparison more rigorous.
- The saved model uses a 0.5 cut-off. A cut-off chosen from the real cost of a retention offer, and a revenue-at-risk estimate (churn probability × CLTV), are natural next steps.
- Wire the model's churn scores into the interactive dashboard.

---

## 🧰 Tools & Technologies

| Tool                  | Purpose                                              |
| --------------------- | ---------------------------------------------------- |
| Python                | Main programming language                            |
| Pandas & NumPy        | Data cleaning and manipulation                       |
| Matplotlib & Seaborn  | Exploratory data visualization                       |
| SciPy                 | Statistical tests (Chi-square, Mann-Whitney U)       |
| SQL & SQLite          | Database and business queries                        |
| Scikit-learn          | Preprocessing, modeling, tuning and evaluation       |
| Joblib                | Saving the trained model                             |
| Plotly (Dash planned) | Dashboard                                            |
| Jupyter Notebook      | Analysis                                             |
| Git & GitHub          | Version control                                      |

---

## 📁 Repository Structure

```
├── dashboard/        # Dashboard (Plotly now, standalone app planned)
├── data/
│   ├── raw/          # Original dataset from Kaggle
│   └── processed/    # Cleaned / preprocessed data
├── docs/             # Project documentation
├── models/           # Trained model (best_model.pkl)
├── notebooks/        # Cleaning, EDA, SQL analysis, preprocessing, modeling
├── sql/              # Database schema (schema.sql)
├── data_dictionary.md
├── requirements.txt
└── README.md
```

## 🚀 How to Run

```
pip install -r requirements.txt
jupyter notebook
```

Open the notebook in `notebooks/` and use **Restart & Run All**. The dataset is downloaded automatically with `kagglehub`. The final cells save the trained model to `models/best_model.pkl`.

To score new customers:

```python
import joblib

bundle = joblib.load("models/best_model.pkl")
model = bundle["model"]            # trained Random Forest
features = bundle["features"]      # column order the model expects
threshold = bundle["threshold"]    # 0.5
print(bundle["metrics"])           # test-set scores

# new_customers must be preprocessed exactly like the training data
# (same encoding and scaling as in the notebook), then:
# X = new_customers[features]
# churn_risk = model.predict_proba(X)[:, 1]
# will_churn = churn_risk >= threshold
```
