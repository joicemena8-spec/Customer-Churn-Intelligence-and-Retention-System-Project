# Customer Churn Intelligence and Retention System

## Project Overview
An end-to-end data engineering and analytics solution designed to predict customer churn, identify risk factors, and extract actionable business insights using the IBM Telco Customer Churn dataset. Developed as part of the Ruwad Masr initiative graduation project.

## Repository Structure
- `data/`
  - `raw/`: Original dataset obtained from Kaggle.
  - `processed/`: Cleaned and pre-processed dataset (`cleaned_Customers.csv`).
- `notebooks/`: Contains the Jupyter Notebook covering data cleaning, preprocessing, SQLite database integration, and SQL analytical queries.
- `sql/`: Contains database schema definitions (`schema.sql`).

## Technical Stack & Tools
- **Python (Pandas, NumPy):** Data cleaning, missing values imputation, and data transformation.
- **SQLite & SQL:** Relational database setup, table structuring, and execution of business-driven analytical queries.
- **Matplotlib & Seaborn:** Exploratory data visualization and statistical distribution analysis.

## Key Features & Analysis
- Standardized data types and resolved structural formatting issues in complex customer attributes.
- Implemented relational queries to evaluate churn rates across different contract types, payment methods, and demographic segments.
- Extracted key behavioral insights isolating high-risk customers prone to churn.
