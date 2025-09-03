# Fraud Detection Analysis

This repository contains a Jupyter notebook that performs an exploratory data analysis (EDA), feature engineering, and builds machine learning models to detect fraudulent transactions in a financial dataset.

## Dataset

The dataset used in this analysis is named `Fraud.csv` and contains transaction data with the following columns:

*   `step`: Represents a unit of time where 1 step equals 1 hour.
*   `type`: Type of transaction (e.g., CASH_OUT, PAYMENT, TRANSFER).
*   `amount`: The amount of the transaction.
*   `nameOrig`: Customer who originated the transaction.
*   `oldbalanceOrg`: Original balance of the originator account before the transaction.
*   `newbalanceOrig`: New balance of the originator account after the transaction.
*   `nameDest`: Customer who is the recipient of the transaction.
*   `oldbalanceDest`: Original balance of the recipient account before the transaction.
*   `newbalanceDest`: New balance of the recipient account after the transaction.
*   `isFraud`: This is the target variable, indicating whether the transaction is fraudulent (1) or not (0).
*   `isFlaggedFraud`: Indicates transactions flagged as fraudulent by the system (1) or not (0).

## Notebook Contents

The notebook covers the following steps:

1.  **Data Loading and Initial Inspection:** Loading the dataset and performing initial checks for missing values and duplicates.
2.  **Exploratory Data Analysis (EDA):** Analyzing the distribution of transaction types, amounts, and temporal patterns. Investigating the relationship between these features and fraudulent transactions.
3.  **Feature Engineering:** Creating new features to capture more informative patterns, such as time-based features (hour, day of week, weekend, working hours) and balance error features.
4.  **Data Preprocessing:** Handling categorical features, scaling numerical features, and splitting the data into training, validation, and test sets.
5.  **Model Training and Evaluation:** Training and evaluating several classification models (Logistic Regression, LightGBM, XGBoost, and Random Forest) to predict fraudulent transactions.
6.  **Model Performance Summary:** Comparing the performance of the different models using metrics like precision, recall, F1-score, and ROC-AUC.
7.  **Key Factors and Prevention Strategies:** Identifying the most important factors that predict fraudulent transactions and suggesting potential prevention strategies.

## Model Performance

The analysis shows that ensemble models like XGBoost and Random Forest perform significantly better than Logistic Regression in detecting fraud in this imbalanced dataset. The Random Forest model achieved the highest performance metrics and was selected as the final model.

## Key Findings

*   Fraudulent transactions are heavily concentrated in **CASH_OUT** and **TRANSFER** transaction types.
*   **Higher transaction amounts** are significantly more likely to be associated with fraud.
*   **Inconsistent balance changes** at the destination account are strong indicators of fraud.
*   Fraudulent activities are more frequent during **early morning hours**, especially on **weekends**.

## Prevention Strategies

Based on the findings, the following prevention strategies are suggested:

*   Real-time monitoring and blocking of high-risk transactions.
*   Enhanced authentication for suspicious transactions.
*   Integrating model insights into rule-based systems.
*   Implementing checks for balance discrepancies.
*   Limiting transactions during high-risk hours.
*   Educating users about fraud.
*   Continuously updating the fraud detection model.
