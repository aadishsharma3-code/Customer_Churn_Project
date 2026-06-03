# 📊 Customer Churn Analysis & Prediction

> *Identifying at-risk customers through data analytics and machine learning to reduce revenue loss and improve retention strategies.*

---

## 📋 Table of Contents

1. [Project Title](#-customer-churn-analysis--prediction)
2. [Brief Summary](#-brief-summary)
3. [Overview](#-overview)
4. [Problem Statement](#-problem-statement)
5. [Dataset](#-dataset)
6. [Tools and Technologies](#️-tools-and-technologies)
7. [Methods](#-methods)
   - [Data Cleaning](#-data-cleaning)
   - [Exploratory Data Analysis (EDA)](#-exploratory-data-analysis-eda)
   - [Feature Engineering](#️-feature-engineering)
   - [Hypothesis Testing](#-hypothesis-testing)
   - [Machine Learning Model](#-machine-learning-model)
8. [Dashboard](#-dashboard)
9. [Images](#️-dashboard-images)
10. [Final Recommendations](#-final-recommendations)
11. [Author & Contact](#-author--contact)

---

## 📝 Brief Summary

This project delivers an end-to-end **customer churn analysis and prediction system** for a telecom company. By combining exploratory data analysis, statistical hypothesis testing, feature engineering, and machine learning, the project identifies the key drivers behind customer churn and predicts which customers are likely to leave. The findings are presented through an interactive Power BI dashboard, providing business stakeholders with actionable, data-driven retention strategies.

---

## 🔭 Overview

Customer churn is one of the most critical business challenges in the telecommunications industry, where acquiring a new customer costs significantly more than retaining an existing one. This project takes a **full-stack analytical approach** — from raw data cleaning to deploying a tuned classification model — to help the business understand *who* is churning, *why* they are churning, and *what* can be done to prevent it.

The project covers:
- In-depth **Exploratory Data Analysis** to surface churn patterns across demographics, service subscriptions, and customer behavior
- **Statistical hypothesis testing** to validate which features are truly significant drivers of churn
- **Feature engineering** to create meaningful new variables that enhance model performance
- **Machine learning modeling** with hyperparameter and threshold tuning to achieve business-optimal recall
- **SHAP analysis** to provide transparent, interpretable model explanations
- An **interactive Power BI dashboard** enabling stakeholders to slice and filter insights dynamically

---

## ❗ Problem Statement

A telecom company is experiencing a **churn rate of approximately 26.54%**, resulting in an estimated **annual revenue loss of ₹16.6 lakh**. The business lacks clarity on which customer segments are most at risk and what factors are driving their departure.

The core challenges addressed in this project:
- **High churn concentration** among new customers, month-to-month contract holders, and electronic check users
- **No predictive mechanism** to proactively flag at-risk customers before they leave
- **Limited insight** into which service and demographic factors most strongly correlate with churn
- **Business need** for a reliable, interpretable model that prioritizes recall — catching as many churners as possible — to enable timely intervention

---

## 📂 Dataset

- **Source:** Telecom Customer Churn Dataset (publicly available)
- **Dimensions:** 7,043 rows × 21 columns
- **Class Imbalance:** ~26% churn (Yes) vs ~74% non-churn (No) — moderately imbalanced dataset

### Column Descriptions

| Column | Description |
|---|---|
| `customerID` | Unique identifier for each customer |
| `gender` | Customer's gender (Male / Female) |
| `SeniorCitizen` | Whether the customer is a senior citizen (1 = Yes, 0 = No) |
| `Partner` | Whether the customer has a partner (Yes / No) |
| `Dependents` | Whether the customer has dependents (Yes / No) |
| `tenure` | Number of months the customer has been with the company |
| `PhoneService` | Whether the customer has phone service (Yes / No) |
| `MultipleLines` | Whether the customer has multiple phone lines |
| `InternetService` | Type of internet service (DSL / Fiber optic / No) |
| `OnlineSecurity` | Whether the customer has online security add-on |
| `OnlineBackup` | Whether the customer has online backup add-on |
| `DeviceProtection` | Whether the customer has device protection add-on |
| `TechSupport` | Whether the customer has tech support add-on |
| `StreamingTV` | Whether the customer streams TV |
| `StreamingMovies` | Whether the customer streams movies |
| `Contract` | Contract type (Month-to-month / One year / Two year) |
| `PaperlessBilling` | Whether the customer uses paperless billing |
| `PaymentMethod` | Payment method used by the customer |
| `MonthlyCharges` | Monthly amount charged to the customer |
| `TotalCharges` | Total amount charged over the customer's tenure |
| `Churn` | Target variable — whether the customer churned (Yes / No) |

---

## 🛠️ Tools and Technologies

| Category | Tools |
|---|---|
| **Programming Language** | Python |
| **Data Manipulation** | Pandas, NumPy |
| **Visualization** | Matplotlib, Seaborn |
| **Statistical Testing** | SciPy (Chi-square, t-test) |
| **Machine Learning** | Scikit-learn, XGBoost |
| **Model Interpretability** | SHAP |
| **Dashboard & BI** | Microsoft Power BI |
| **Development Environment** | Jupyter Notebook |

---

## 🔬 Methods

### 🧹 Data Cleaning

- **Service-related columns** (`OnlineSecurity`, `OnlineBackup`, `DeviceProtection`, `TechSupport`, `StreamingTV`, `StreamingMovies`) had missing values where customers had no internet service — these were filled with `'No Internet Service'` to reflect the actual business reality rather than treating them as unknown values
- **`MultipleLines`** missing values were imputed using the **mode** (most frequent value), as the missingness was small and random
- **`TotalCharges`** had missing values for customers with zero tenure (likely new customers with no billing history yet) — these were **computed logically** by multiplying `MonthlyCharges × tenure`, maintaining data integrity without introducing bias from statistical imputation

---

### 📊 Exploratory Data Analysis (EDA)

Key business insights uncovered during EDA:

- **Gender has no meaningful impact on churn** — churn rates are nearly identical across male and female customers, making it a non-driver variable
- **Family status is a significant churn driver** — single customers churn at a considerably higher rate compared to customers with partners or dependents, indicating that family-oriented customers have greater loyalty
- **Contract type is the strongest churn predictor:**
  - Month-to-month contract customers churn at **42.71%** — by far the highest of any contract type
  - Customers on one-year and two-year contracts show substantially lower churn rates
- **Payment method is a high-risk indicator:**
  - Electronic check users churn at **45.29%** — the highest of any payment method
  - Automated payment methods (bank transfer, credit card) are associated with significantly lower churn
- **New customers are the most vulnerable:**
  - Customers with tenure of 0–12 months (New Customers) churn at **47.44%**
  - Churn rate steadily decreases as customer tenure increases, confirming that early engagement is critical
- **Fiber optic internet users churn at a disproportionately high rate (41.9%)** compared to DSL users, suggesting potential dissatisfaction with service quality or pricing

---

### 🏗️ Feature Engineering

Two meaningful new features were created to enhance model performance and business interpretability:

**1. `Family_Status`** — Combines `Partner` and `Dependents` into a single categorical variable representing customer household type:
- **Single** — No partner, no dependents
- **Couple** — Has a partner but no dependents
- **Family** — Has dependents

**2. `Customer_Status`** — Segments customers based on their `tenure` to capture loyalty lifecycle stages:
- **New Customers** — Tenure of 0–12 months
- **Developing Customers** — Tenure of 13–24 months
- **Established Customers** — Tenure of 25–48 months
- **Loyal Customers** — Tenure of 49+ months

---

### 🧪 Hypothesis Testing

Chi-square tests and t-tests were performed to statistically validate which features have a significant association with churn:

**Statistically Significant Drivers (p < 0.05):**

| Feature | Cramér's V | Interpretation |
|---|---|---|
| **Contract** | 0.410 | Very strong association with churn |
| **PaymentMethod** | 0.303 | Strong association with churn |
| **InternetService** | 0.322 | Strong association with churn |
| **Customer_Status** | 0.349 | Strong association with churn |
| **Family_Status** | 0.182 | Moderate association with churn |
| **SeniorCitizen** | 0.150 | Moderate association with churn |

**Not Significant:**
- **Gender** (p = 0.487, Cramér's V = 0.008) — No meaningful association with churn; confirmed as a non-driver

**Tenure (t-test):**
- There is a **statistically significant difference** in mean tenure between customers who churn and those who do not — churned customers have considerably lower average tenure, reinforcing that early-tenure customers are at the highest risk

---

### 🤖 Machine Learning Model

**Preprocessing:**
- Categorical features encoded using **One-Hot Encoding**
- Numerical features scaled using **Standard Scaler**

**Models Evaluated:**
- Logistic Regression
- Support Vector Classifier (SVC)
- K-Nearest Neighbors (KNN)
- Naïve Bayes
- Decision Tree
- Random Forest
- XGBoost

**Hyperparameter Tuning:**
- Grid search / RandomizedSearchCV applied to **Decision Tree**, **Random Forest**, and **XGBoost** for optimal performance

**Threshold Tuning:**
- Precision-Recall threshold was tuned to achieve a **target recall of 80%** — prioritizing catching actual churners over minimizing false positives, which aligns with the business goal of proactive retention

**Final Model Performance:**
- **Recall: 80%** — 4 out of every 5 churners are correctly identified
- **ROC-AUC: 0.84** — Strong discriminative ability between churners and non-churners

**SHAP Analysis — Top Churn Drivers:**
- 🥇 **Contract type** — Month-to-month contracts are the single biggest predictor of churn
- 🥈 **Tenure** — Low tenure customers are significantly more likely to churn
- 🥉 **Internet Service** — Fiber optic service is associated with higher churn
- 4️⃣ **Payment Method** — Electronic check payments are a strong churn signal

---

## 📊 Dashboard

The project includes a **two-page interactive Power BI dashboard** designed for business stakeholders to explore churn patterns dynamically using slicers for gender, senior citizen status, partner, dependents, and payment method.

---

### Page 1 — Churn Overview

**KPI Cards at the top:**
- **Total Customers:** 7K — overall customer base size
- **Churned Customers:** 2K — total customers who left
- **Churn Rate:** 26.54% — proportion of the base that churned
- **Revenue Lost:** 16.6 lakhs — direct business impact of churn

**Visuals on this page:**

- **Churn Rate by Contract (Donut Chart):**
  Visualizes the distribution of churn across contract types. The majority of churn (42.71%) comes from month-to-month customers, followed by one-year (11.27%) and two-year (2.83%) — clearly showing that short-term contracts are the highest churn risk. This guides retention teams to focus upgrade incentives on month-to-month users.

- **Churn Rate by Internet Service (Donut Chart):**
  Breaks down churn by internet service type. Fiber optic customers account for 41.9% of churn, DSL for 19%, and customers with no internet service for 7.4%. The high fiber optic churn suggests potential dissatisfaction with pricing or service reliability.

- **Churn Rate by Payment Method (Horizontal Bar Chart):**
  Compares churn rates across payment methods. Electronic check (45.29%) leads significantly, followed by mailed check (19.11%), bank transfer (16.71%), and credit card (15.24%). Automated payment users are far less likely to churn, pointing to financial disengagement as a churn signal.

- **Churn Rate by Tenure (Line Chart):**
  Plots churn rate over customer tenure (0–80 months). The chart peaks near 50% for new customers and declines sharply as tenure increases, leveling off at lower rates for long-tenure customers. This visual confirms that the first 12 months are the most critical period for retention intervention.

---

### Page 2 — Deep Dive Analysis

**KPI Cards at the top:**
- **Avg Monthly Revenue:** $64.76 — average monthly charge per customer
- **Avg Tenure – Churned:** 17.98 months — confirms churned customers leave relatively early
- **Senior Citizen Churn Rate:** 41.68% — senior citizens churn at a significantly elevated rate

**Visuals on this page:**

- **Churn by Services (Matrix/Table):**
  Displays counts of churned (Yes) vs retained (No) customers across key services — TechSupport, StreamingTV, PhoneService, and MultipleLines. Customers without TechSupport (310 churned) and StreamingTV subscriptions are more likely to churn, suggesting these add-ons improve stickiness.

- **Churn Rate by Online Backup (Pie Chart):**
  Customers without online backup churn at 29.17% vs 21.53% for those who have it — indicating that value-added services like online backup reduce the likelihood of leaving.

- **Churn Rate by Gender (Bar Chart):**
  Female customers churn at 26.92% vs male at 26.16% — the marginal difference statistically confirms gender is not a significant churn driver, consistent with hypothesis testing results.

- **Churn Flow (Sankey/Decomposition Tree):**
  Traces churned customers (1,869 total) through a multi-level breakdown: Contract type → Internet Service → Payment Method → Tech Support. The dominant churn path is clearly: **Month-to-month → Fiber optic → Electronic check → No Tech Support**, with 1,655 of 1,869 churned customers being month-to-month. This flow diagram gives decision-makers an at-a-glance view of the highest-risk customer profile.

---

## 🖼️ Dashboard Images

### Page 1 — Churn Overview
![Dashboard Page 1](dashboard_images/1st%20page.png)

### Page 2 — Deep Dive Analysis
![Dashboard Page 2](dashboard_images/2nd%20Page.png)

---

## ✅ Final Recommendations

Based on the analysis, hypothesis testing, and SHAP-driven model insights, the following business strategies are recommended:

**1. Target Month-to-Month Contract Customers for Upgrades**
- With a churn rate of 42.71%, month-to-month customers represent the single highest-risk segment
- Offer **loyalty discounts, service bundles, or incentives** to encourage migration to one-year or two-year contracts
- Even a modest upgrade rate would significantly reduce overall churn

**2. Launch an Early-Tenure Retention Program**
- New customers (0–12 months) churn at 47.44% — nearly half leave before becoming established
- Implement **onboarding campaigns, welcome calls, and proactive check-ins** during the first 3–6 months
- Offer introductory loyalty rewards to reduce early-stage disengagement

**3. Investigate and Address Fiber Optic Dissatisfaction**
- Fiber optic customers represent the largest churn segment by internet service type
- Conduct **customer satisfaction surveys** to identify whether pricing, reliability, or support is the root cause
- Consider targeted **service quality improvements or competitive pricing reviews** for fiber optic users

**4. Promote Auto-Payment Enrollment**
- Electronic check users churn at 45.29%, nearly 3× the rate of credit card users (15.24%)
- Encourage migration to automated payment methods through **discounts or billing convenience campaigns**
- Auto-pay enrollment is strongly correlated with lower churn and higher customer stickiness

**5. Design Senior Citizen–Specific Retention Plans**
- Senior citizens churn at 41.68%, indicating a distinct vulnerability in this segment
- Offer **simplified plans, dedicated support lines, or loyalty pricing** tailored to senior customers
- Consider pairing senior citizen accounts with tech support add-ons to reduce friction

---

## 👤 Author & Contact

**Aadish Sharma**

| Platform | Details |
|---|---|
| 📧 **Email** | [aadishsharma3@gmail.com](mailto:aadishsharma3@gmail.com) |
| 💼 **LinkedIn** | [linkedin.com/in/aadish13](https://www.linkedin.com/in/aadish13) |
| 📞 **Phone** | +91 7973483795 |

---

*If you found this project helpful or insightful, feel free to connect on LinkedIn or drop a message. Feedback and collaboration are always welcome!*
