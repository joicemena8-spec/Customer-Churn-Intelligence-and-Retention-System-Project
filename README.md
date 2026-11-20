# Customer Churn Intelligence & Retention System

An end-to-end data analytics solution that studies customer behavior, identifies the factors behind churn, and extracts actionable business insights from the IBM Telco Customer Churn dataset. Developed as part of the Ruwad Masr initiative (DEPI) – Data Science track graduation project.

---

## 📌 Problem Statement

Telecom companies operate in a highly competitive market where customers can switch providers easily, and winning back or replacing a lost customer costs far more than keeping an existing one. In the IBM Telco dataset, **26.5% of customers (1,869 out of 7,043) have churned**, yet it is not clear who is leaving, why they leave, or at what point in their lifecycle. This project analyzes customer demographics, subscription details, services, payment methods, and charges to uncover the patterns behind churn, identify the high-risk customer segments, and turn the findings into clear, data-driven retention recommendations.

---

## 🎯 Project Objectives
1. Analyze customer data and identify the main churn patterns and drivers.
2. Answer business questions using SQL.
3. Identify high-risk customer segments and recommend retention actions.
4. Prepare clean, model-ready data (feature engineering, encoding, scaling).
5. Build an interactive dashboard to visualize insights.

---

## 📊 Dataset

**Telco Customer Churn – IBM Dataset**

- **Link:** [Kaggle – yeanzc/telco-customer-churn-ibm-dataset](https://www.kaggle.com/datasets/yeanzc/telco-customer-churn-ibm-dataset)
- **Rows:** 7,043  |  **Columns:** 33
- **Target variable:** `Churn Value` (0 = retained, 1 = churned)

### Column Descriptions

| Column | Meaning |
|---|---|
| CustomerID | Unique customer identifier |
| Count | Constant value (1) used for counting |
| Country, State, City, Zip Code | Customer location |
| Lat Long, Latitude, Longitude | Geographic coordinates |
| Gender | Customer gender |
| Senior Citizen | Whether the customer is 65 or older (Yes/No) |
| Partner | Whether the customer has a partner (Yes/No) |
| Dependents | Whether the customer has dependents (Yes/No) |
| Tenure Months | Number of months the customer has stayed with the company |
| Phone Service | Whether the customer has phone service (Yes/No) |
| Multiple Lines | Whether the customer has multiple phone lines |
| Internet Service | Internet type: DSL, Fiber optic, or No |
| Online Security | Whether the customer has online security add-on |
| Online Backup | Whether the customer has online backup add-on |
| Device Protection | Whether the customer has device protection |
| Tech Support | Whether the customer has tech support |
| Streaming TV | Whether the customer streams TV through the service |
| Streaming Movies | Whether the customer streams movies through the service |
| Contract | Contract type: Month-to-month, One year, Two year |
| Paperless Billing | Whether the customer uses paperless billing (Yes/No) |
| Payment Method | Electronic check, Mailed check, Bank transfer (automatic), Credit card (automatic) |
| Monthly Charges | Current monthly bill |
| Total Charges | Total amount charged over the customer's lifetime |
| Churn Label | Whether the customer churned (Yes/No) |
| Churn Value | Churn as a number (1 = churned, 0 = retained) |
| Churn Score | IBM-predicted churn risk score (0–100) |
| CLTV | Customer Lifetime Value (predicted) |
| Churn Reason | Reason the customer gave for leaving |

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

**Table: `telco_customers`**: one row per customer, 33 columns. Primary key: `CustomerID`.

| Group | Main columns (type) |
|---|---|
| Customer information | CustomerID (TEXT, PK), Gender, SeniorCitizen, Partner, Dependents (TEXT) |
| Location | Country, State, City (TEXT), ZipCode (INTEGER), LatLong (TEXT), Latitude, Longitude (REAL) |
| Subscription | TenureMonths (INTEGER), Contract, InternetService, PhoneService, MultipleLines, PaperlessBilling (TEXT) |
| Services | OnlineSecurity, OnlineBackup, DeviceProtection, TechSupport, StreamingTV, StreamingMovies (TEXT) |
| Payment | PaymentMethod (TEXT), MonthlyCharges, TotalCharges (REAL) |
| Churn | ChurnLabel (TEXT), ChurnValue (INTEGER), ChurnScore (INTEGER), CLTV (INTEGER), ChurnReason (TEXT) |
| Other | Count (INTEGER) |

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

📷 *Screenshot:* `assets/dashboard.png` *(add image)*

🔜 **Next:** add the remaining KPIs and charts above and the interactive filters, then move the dashboard into a standalone app (`dashboard/app.py`).

---

## 📌 Project Status

| Stage | Status |
|---|---|
| Data cleaning & validation | ✅ Done |
| SQL business analysis (10 queries) | ✅ Done |
| EDA: descriptive statistics, distributions, correlations, group-by, significance tests | ✅ Done |
| Insights (observation → meaning → suggested action) | ✅ Done |
| Feature engineering, encoding & scaling | ✅ Done |
| Plotly dashboard (first version) | ✅ Done (static) |
| Interactive dashboard with filters | 🔜 In progress |
| Machine learning models & evaluation | 🔜 Next phase |

---

## 🔍 Top Insights

1. **Contract type is the strongest churn driver.** Month-to-month customers churn at **42.7%** vs. **11.3%** (one-year) and **2.8%** (two-year), and make up **88.6%** of all churn. → Convert month-to-month customers to annual plans with a targeted offer.
2. **Churn is concentrated in one segment.** Month-to-month + Fiber optic customers churn at **54.6%** and account for **62.2%** of all churn. → Make this segment the primary retention target.
3. **Churn is an early-life problem.** Churn is **49.8%** in the first 9 months vs. **7.7%** after 55 months. → Focus retention efforts on months 1–12.
4. **Competition, not price, is the main reason customers leave.** **33.2%** of churned customers named a competitor; only **5.2%** cited high prices. → Compete on speed, data, and devices rather than blanket discounts.

More insights, each with the number, its meaning, and a suggested action, are under every "Key Insights" section in the notebook.

---

## 🛠 Data Preprocessing

| Step | What we did | Why |
|---|---|---|
| Cleaning | Fixed `Total Charges` type, handled missing values, checked duplicates and outliers | Data quality |
| Drop columns | ID, constant, location, Gender, Churn Label/Score/Reason | No signal, or leaks the answer |
| Feature engineering | `Num Services`, `Tenure Group`, `Has Family`, `Auto Pay` | Capture patterns found in the EDA |
| Encoding | Yes/No → 1/0; one-hot for Contract, Internet Service, Payment Method, Tenure Group | Models need numbers |
| Split | 80% train / 20% test, stratified | Keep the churn ratio equal |
| Scaling | `StandardScaler` on 5 numeric columns (fit on train only) | Helps Logistic Regression / SVM; avoids leakage |
| Imbalance | Kept as is; use `class_weight='balanced'` when training | ~26.5% churn |

---

## 🧰 Tools & Technologies

| Tool | Purpose |
|---|---|
| Python | Main programming language |
| Pandas & NumPy | Data cleaning and manipulation |
| Matplotlib & Seaborn | Exploratory data visualization |
| SciPy | Statistical tests (Chi-square, Mann-Whitney U) |
| SQL & SQLite | Database and business queries |
| Scikit-learn | Preprocessing (and modeling in the next phase) |
| Plotly (Dash planned) | Dashboard |
| Jupyter Notebook | Analysis |
| Git & GitHub | Version control |

---

## 📁 Repository Structure

```
├── data/
│   ├── raw/          # Original dataset from Kaggle
│   └── processed/    # Cleaned / preprocessed data
├── notebooks/        # Cleaning, EDA, SQL analysis, preprocessing, dashboard
├── sql/              # Database schema (schema.sql)
├── assets/           # Dashboard screenshot
└── README.md
```

## 🚀 How to Run

```bash
pip install pandas numpy matplotlib seaborn scipy scikit-learn plotly nbformat openpyxl kagglehub joblib
jupyter notebook
```
Open the notebook in `notebooks/` and use **Restart & Run All**. The dataset is downloaded automatically with `kagglehub`.
