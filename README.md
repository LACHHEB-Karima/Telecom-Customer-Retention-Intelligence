# 📡 Telecom Customer Retention Intelligence

### From Customer Churn Analysis to Proactive Retention

An end-to-end **Customer Retention Intelligence** project combining **SQL, Python, Machine Learning, and Power BI** to understand customer churn, identify its main drivers, predict customers at risk of leaving, and support data-driven retention strategies.

---

## 📌 Business Context

Customer churn is one of the most important challenges faced by telecommunications companies.

When customers leave, the company loses not only recurring revenue but also the long-term value associated with those customers. Understanding **who is leaving, why they are leaving, and which customers are most likely to leave next** is therefore essential for building effective retention strategies.

The objective of this project was to transform raw customer data into a practical analytical solution that can help a telecom company move from:

> **Reactive churn analysis → Proactive customer retention**

Instead of only looking at customers who have already churned, the project combines historical analysis with predictive modeling to identify customers who may be at risk.

---

# 🎯 Business Problem

The telecom company needs to answer several key business questions:

* How many customers are staying, joining, or churning?
* What are the main characteristics of churned customers?
* Which customer segments have higher churn risk?
* How do contract types influence customer retention?
* Which customer characteristics and services are associated with churn?
* Which customers are likely to churn in the future?
* How can the company prioritize retention efforts?

The challenge is therefore not simply to calculate the churn rate, but to turn customer data into **actionable retention intelligence**.

---

# 💡 Project Solution

To address the problem, I developed an end-to-end analytics workflow:

```text
Raw Customer Data
       │
       ▼
   SQL Server
       │
       ├── Data Cleaning
       ├── Data Exploration
       └── BI Views
       │
       ▼
   Clean Customer Data
       │
       ├───────────────┐
       ▼               ▼
   Power BI         Python
   Analytics        Machine Learning
       │               │
       │               ▼
       │          Churn Prediction
       │               │
       └───────┬───────┘
               ▼
      Retention Intelligence
               │
               ▼
     Business Recommendations
```

The project therefore combines two complementary perspectives:

### Descriptive Analytics

**What happened?**

Power BI and SQL were used to understand customer behavior and identify historical churn patterns.

### Predictive Analytics

**What is likely to happen?**

A Random Forest model was developed in Python to predict whether customers are likely to churn.

### Business Intelligence

**What should the business do?**

The results are presented through Power BI to allow stakeholders to explore customer behavior, churn patterns, and retention opportunities.

---

# 🗄️ Data Preparation with SQL

SQL Server was used as the main data preparation and analytical layer.

The data workflow included:

* Loading the raw customer dataset into a staging table.
* Exploring customer characteristics.
* Checking missing values.
* Cleaning and standardizing categorical fields.
* Creating a production-level churn table.
* Creating dedicated SQL views for Power BI.

The cleaning process replaces missing values in fields such as:

* Value Deal
* Multiple Lines
* Internet Type
* Online Security
* Online Backup
* Device Protection
* Premium Support
* Streaming services
* Churn Category
* Churn Reason

For example, missing service-related values are standardized to `"No"` or `"None"` depending on the business meaning.

The cleaned dataset is stored in:

```text
prod_Churn
```

Two dedicated views were then created for Power BI:

```text
vw_ChurnData
vw_JoinData
```

The first contains customers with a final status of **Churned** or **Stayed**, while the second isolates **Joined** customers.

---

# 🔎 Exploratory Data Analysis

SQL was also used to investigate the composition of the customer base.

Some important findings include:

### Customer demographics

Female customers represent approximately **63%** of the customer base.

### Contract distribution

The customer base is distributed across:

* **51% Month-to-Month contracts**
* **22% One-Year contracts**
* **27% Two-Year contracts**

This makes contract type an important dimension when analyzing customer retention.

### Customer status

The analysis distinguishes between:

* Stayed
* Churned
* Joined

This separation is important because newly joined customers should not be treated in the same way as established customers when measuring churn.

### Geographic distribution

The analysis also explores customer distribution by state, with Washington representing one of the largest customer concentrations in the dataset.

---

# 🤖 Machine Learning — Churn Prediction

After the SQL preparation stage, the cleaned data was imported into Python through SQL Server views.

The machine learning workflow included:

1. Selecting the prediction dataset.
2. Removing identifiers and post-churn explanatory fields.
3. Encoding categorical variables.
4. Defining the target variable.
5. Splitting the data into training and testing sets.
6. Training a Random Forest classifier.
7. Evaluating the model.
8. Analyzing feature importance.
9. Applying the model to recently joined customers.
10. Exporting the predictions for further analysis.

The prediction target was encoded as:

```text
Stayed  → 0
Churned → 1
```

Customer ID, Churn Category, and Churn Reason were excluded from the predictive features to avoid using information that would not be appropriate as direct predictors.

The dataset was divided using an **80/20 train-test split** with a fixed random state of 42.

---

# 📊 Model Performance

The Random Forest model achieved:

| Metric    | Stayed | Churned |
| --------- | -----: | ------: |
| Precision |   0.85 |    0.80 |
| Recall    |   0.94 |    0.61 |
| F1-score  |   0.89 |    0.69 |

Overall:

| Metric            |   Result |
| ----------------- | -------: |
| Accuracy          |  **84%** |
| Macro F1-score    | **0.79** |
| Weighted F1-score | **0.83** |

The confusion matrix was:

```text
                Predicted
                Stayed  Churned

Actual Stayed     796      54
Actual Churned    138     214
```

The model therefore performs well at identifying customers who stay, while churn detection remains the more challenging part of the classification problem.

This distinction is important from a business perspective: **accuracy alone is not sufficient for a churn model**, because missing an actual churner can be more costly than incorrectly flagging a customer as high risk.

---

# 🔮 Predicting Customers at Risk

The trained Random Forest model was subsequently applied to the **411 recently joined customers** available in the `vw_JoinData` view.

The model generated:

```text
Predicted Churn = 316 customers
Predicted Stay  = 95 customers
```

These predictions were added to the original customer records and exported as a prediction dataset for further analysis and visualization.

This creates the foundation for a proactive retention approach:

> Identify potentially vulnerable customers early and prioritize them for retention actions.

---

# 📊 Power BI Dashboard

Power BI was used as the business intelligence layer of the project.

The dashboard transforms the SQL analysis and machine-learning outputs into an interactive environment for business stakeholders.

The dashboard allows users to investigate customer retention through dimensions such as:

* Customer status
* Churn
* Contract type
* Customer demographics
* Tenure
* Services
* Payment methods
* Revenue
* Customer segments
* Predicted churn

The goal is to allow decision-makers to move from high-level KPIs to detailed customer analysis.

---

# 🧭 Dashboard Navigation

The Power BI report is organized into multiple analytical sections using a **page navigator**, allowing users to move between the different views of the analysis.

The dashboard is designed around three main questions:

### 1. What is happening?

Understand the current customer base and overall churn situation.

### 2. Why is it happening?

Identify the characteristics, services, contracts, and behaviors associated with churn.

### 3. Who should we act on?

Use the machine-learning predictions to identify customers who may require proactive retention attention.

---

# 📈 Key Business Insights

The analysis highlights several important areas for telecom retention management.

### 1. Contract type matters

Month-to-month customers represent the largest contract segment, accounting for approximately **51%** of customers.

This segment should receive particular attention because shorter contractual commitment can create greater opportunities for customers to leave.

### 2. Early identification is important

The predictive component extends the analysis beyond historical churn.

Rather than waiting for a customer to churn, the model provides a mechanism for identifying potentially vulnerable customers among newly joined customers.

### 3. Churn prediction requires more than accuracy

The model achieved **84% overall accuracy**, but churn recall was **61%**.

This demonstrates an important business consideration: a retention model should be evaluated according to its ability to detect actual churners, not simply its overall accuracy.

### 4. Customer data can support targeted retention

The dataset contains information covering:

* Customer demographics
* Tenure
* Contract
* Internet services
* Additional services
* Payment method
* Monthly charges
* Total charges
* Revenue
* Churn category
* Churn reason

This provides a strong foundation for moving from generic retention campaigns toward more targeted interventions.

---

# 💼 Business Recommendations

Based on the analysis, a telecom company could implement the following retention strategy:

### 🎯 1. Prioritize high-risk customers

Use churn predictions to create a retention priority list rather than contacting customers randomly.

### 📅 2. Focus on vulnerable customer segments

Customers with characteristics associated with higher churn should receive additional attention, particularly customers with less commitment to the company.

### 🤝 3. Improve early customer engagement

New customers should be monitored closely during their initial relationship with the company.

Early intervention can help address dissatisfaction before it becomes a churn decision.

### 💰 4. Consider customer value

Retention decisions should not rely solely on churn probability.

A high-risk customer generating significant revenue may deserve a different retention strategy from a low-value customer.

### 🔄 5. Continuously monitor churn

The Power BI dashboard can be used as a recurring monitoring tool to track changes in customer behavior and retention performance.

---

# 🛠️ Technology Stack

| Technology               | Purpose                                     |
| ------------------------ | ------------------------------------------- |
| **SQL Server**           | Data storage, cleaning and exploration      |
| **SQL**                  | Data transformation and analytical queries  |
| **Python**               | Machine learning and predictive analysis    |
| **Pandas**               | Data manipulation                           |
| **NumPy**                | Numerical processing                        |
| **Scikit-learn**         | Machine learning                            |
| **Random Forest**        | Churn classification                        |
| **Matplotlib / Seaborn** | Model and data visualization                |
| **Joblib**               | Model persistence                           |
| **Power BI**             | Interactive business intelligence dashboard |
| **DAX**                  | Measures and analytical calculations        |
| **Jupyter Notebook**     | Machine-learning workflow                   |

---

# 📂 Repository Structure

```text
Telecom-Customer-Retention-Intelligence/
│
├── Churn Analysis.pbix
│   └── Power BI dashboard
│
├── Churn_prediction.ipynb
│   └── Machine learning workflow
│
├── Customer_Data_English.csv
│   └── Customer dataset
│
├── data_cleaning.sql
│   └── SQL data preparation
│
├── data_exploration.sql
│   └── SQL exploratory analysis
│
├── bi_views.sql
│   └── SQL views used by Power BI
│
└── LICENSE
```

The repository currently contains the Power BI report, Python notebook, CSV dataset, and three SQL scripts supporting cleaning, exploration, and BI views.

---

# 🔄 End-to-End Workflow

```text
                 RAW CUSTOMER DATA
                         │
                         ▼
                   SQL SERVER
                         │
             ┌───────────┴───────────┐
             ▼                       ▼
       Data Cleaning           Data Exploration
             │                       │
             └───────────┬───────────┘
                         ▼
                  PROD_CHURN
                         │
                  ┌──────┴──────┐
                  ▼             ▼
            Power BI         Python
            Analytics       ML Model
                  │             │
                  │       Random Forest
                  │             │
                  │       Churn Prediction
                  │             │
                  └──────┬──────┘
                         ▼
              RETENTION INTELLIGENCE
                         │
                         ▼
              ACTIONABLE INSIGHTS
```

---

# 🎯 Project Outcome

The project demonstrates how multiple data technologies can be combined to create a complete customer retention solution.

Instead of treating data analysis, visualization, and machine learning as separate tasks, the project connects them into one workflow:

**SQL** prepares and structures the data.

**Python** adds predictive capabilities.

**Machine Learning** identifies customers who may be at risk.

**Power BI** transforms the results into an interactive business intelligence solution.

The final result is a **Customer Retention Intelligence framework** designed to help telecom businesses understand churn, identify risk, and make more informed retention decisions.

---

# 🚀 Future Improvements

The current solution provides a strong foundation, but several improvements could make the retention system more advanced:

* Improve churn recall through model optimization.
* Compare Random Forest with Logistic Regression, XGBoost, and other classifiers.
* Apply hyperparameter tuning.
* Address class imbalance explicitly.
* Add probability-based churn risk scores.
* Create Low / Medium / High risk customer segments.
* Add customer lifetime value to retention prioritization.
* Automate model retraining.
* Connect Power BI directly to regularly updated prediction results.
* Develop automated retention recommendations based on churn drivers.

---

# 📌 Conclusion

**Telecom Customer Retention Intelligence** demonstrates an end-to-end approach to solving a real business problem with data.

The project goes beyond simply answering **"How many customers churned?"**

It attempts to answer the more valuable questions:

> **Why are customers leaving?**

> **Which customers are most at risk?**

> **How can the company identify them early?**

> **How can data support better retention decisions?**

By combining **SQL, Python, Machine Learning, and Power BI**, the project transforms raw customer information into a practical analytical and predictive framework for proactive customer retention.

---

## 👩‍💻 Author

**Karima LACHHEB**


