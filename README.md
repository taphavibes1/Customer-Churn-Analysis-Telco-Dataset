# Customer-Churn-Analysis-Telco-Dataset

This notebook explores the **Telco Customer Churn dataset** using Python.  
The main goal is to understand which factors drive churn and provide data-driven recommendations to reduce it.

**Key Questions:**
1. What is the overall churn rate?
2. Which customer segments churn the most?
3. How do contract type, tenure, and charges affect churn?
4. What actionable strategies can reduce churn?

**Dataset:** [Kaggle – Telco Customer Churn](https://www.kaggle.com/datasets/blastchar/telco-customer-churn)

---

# Data manipulation
import pandas as pd
import numpy as np

# Visualization
import matplotlib.pyplot as plt
import seaborn as sns

# Display options
pd.set_option('display.max_columns', None)

# Load dataset
df = pd.read_csv("WA_Fn-UseC_-Telco-Customer-Churn.csv")
df.head()

---

## Data Cleaning

Steps:
- Check for missing values
- Convert `TotalCharges` from string to numeric
- Handle blanks and duplicates
- Create helper columns (Churn flag, tenure buckets, etc.)

# Check nulls
df.isnull().sum()

# Convert TotalCharges to numeric
df['TotalCharges'] = pd.to_numeric(df['TotalCharges'], errors='coerce')
df['TotalCharges'] = df['TotalCharges'].fillna(0)

# Create churn flag
df['Churn_Flag'] = df['Churn'].apply(lambda x: 1 if x=="Yes" else 0)

# Tenure buckets
def tenure_bucket(x):
    if x <= 3: return "0-3"
    elif x <= 12: return "4-12"
    elif x <= 24: return "13-24"
    else: return "25+"
df['TenureBucket'] = df['tenure'].apply(tenure_bucket)

df.head()

---

## Exploratory Data Analysis
We explore churn patterns across contract types, tenure, payment methods, and charges.

churn_rate = df['Churn_Flag'].mean() * 100
print(f"Overall Churn Rate: {churn_rate:.2f}%")

plt.figure(figsize=(6,4))
sns.barplot(data=df, x='Contract', y='Churn_Flag')
plt.title("Churn Rate by Contract Type")
plt.ylabel("Churn Rate")
plt.show()

plt.figure(figsize=(6,4))
sns.barplot(data=df, x='TenureBucket', y='Churn_Flag')
plt.title("Churn Rate by Tenure Bucket")
plt.ylabel("Churn Rate")
plt.show()

plt.figure(figsize=(7,4))
sns.barplot(data=df, x='PaymentMethod', y='Churn_Flag')
plt.xticks(rotation=30)
plt.title("Churn Rate by Payment Method")
plt.show()

plt.figure(figsize=(6,4))
sns.boxplot(data=df, x='Churn', y='MonthlyCharges')
plt.title("Monthly Charges vs Churn")
plt.show()

---

## Key Insights
- Overall churn rate is around **20%**.  
- Customers on **month-to-month contracts** churn far more often than yearly customers.  
- Customers with **short tenure (0–3 months)** churn the most.  
- **Electronic check** payment users are high churners.  
- Customers with **higher MonthlyCharges but no added services** are at higher churn risk.  




