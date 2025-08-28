# Financial Fraud Detection

This project focuses on building a machine learning model to detect fraudulent transactions in a large dataset of financial transactions.

## Project Overview

Financial fraud is a significant problem for individuals and institutions. This project aims to identify patterns indicative of fraudulent activity and build a predictive model to flag suspicious transactions in real-time. The dataset contains information about various transactions, including the amount, type, origin and destination accounts, and their balances before and after the transaction.

## Data

The dataset used is `Fraud.csv`, which contains over 6 million transaction records. Key features include:

*   `step`: Represents a unit of time (e.g., hour or day).
*   `type`: Type of transaction (e.g., CASH_OUT, PAYMENT, TRANSFER).
*   `amount`: The transaction amount.
*   `nameOrig`, `oldbalanceOrg`, `newbalanceOrig`: Origin account details.
*   `nameDest`, `oldbalanceDest`, `newbalanceDest`: Destination account details.
*   `isFraud`: The target variable (1 for fraudulent, 0 otherwise).
*   `isFlaggedFraud`: A rule-based flag for suspicious transactions.

## Exploratory Data Analysis (EDA)

The EDA revealed several key insights:

*   **Severe Class Imbalance:** Fraudulent transactions are very rare (~0.13%).
*   **`isFlaggedFraud` Ineffectiveness:** This feature flags only a tiny fraction of actual frauds.
*   **Transaction Type Importance:** Fraud is heavily concentrated in `CASH_OUT` and `TRANSFER` types.
*   **Amount and Fraud:** Higher transaction amounts are more likely to be fraudulent.
*   **Temporal Patterns:** Fraudulent transactions are more frequent during early morning hours (2-7 AM), especially on weekends, despite lower overall transaction volume during these times.
*   **Balance Inconsistencies:** Discrepancies in account balances before and after transactions (`origin_error`, `dest_error`) are strong indicators of fraud.

## Feature Engineering

New features were created to capture the patterns observed during EDA:

*   Time-based features (`hour`, `day`, `is_weekend`, `is_working_hours`, cyclical hour/day features).
*   Balance error features (`origin_error`, `dest_error`).
*   Binary flags for zero balances (`origin_was_zero`, etc.).
*   Log-transformed amount (`log_amount`) and binned amount (`amount_bin`).
*   Flag for merchant destinations (`dest_is_merchant`).

## Data Preprocessing

A preprocessing pipeline was built using `ColumnTransformer` to handle different feature types:

*   One-Hot Encoding for categorical features (`type`).
*   Log transformation, imputation, and Robust Scaling for skewed numeric features (`amount`, balances).
*   Absolute value transformation, imputation, and Robust Scaling for error features.
*   Robust Scaling for other numeric features (`step`).
*   Passthrough for binary and cyclical features.
*   Handling of missing values in balance and error columns (imputed with 0).
*   Dropping of redundant or highly correlated features (`nameOrig`, `nameDest`, `isFlaggedFraud`, `newbalanceOrig`, `newbalanceDest`, `day_of_month`, `amount`, `hour`, `day`).

The dataset was split into training, validation, and test sets with stratification to maintain the class distribution.

## Modeling

Several machine learning models were evaluated:

*   **Logistic Regression:** Used as a baseline. Showed high recall but very low precision due to the imbalance.
*   **LightGBM:** A gradient boosting model. Performed well but also struggled with false positives at the default threshold.
*   **XGBoost:** Another gradient boosting model. Achieved very high performance on both training and validation sets.
*   **Random Forest:** An ensemble tree-based model. Demonstrated robust performance and good balance between precision and recall.

## Results and Conclusion

Based on the evaluation metrics (Precision, Recall, F1-score, ROC-AUC) and threshold tuning, the **Random Forest Classifier** was selected as the final model. It achieved excellent performance on the unseen test set, effectively identifying fraudulent transactions while minimizing false positives.

Key findings that informed the model:

*   Fraud is highly associated with `CASH_OUT` and `TRANSFER` transactions.
*   Higher transaction amounts and balance inconsistencies are strong predictors.
*   Early morning hours and weekends are high-risk periods.

## Prevention Strategies

Based on the model's insights, potential prevention strategies include:

*   Real-time monitoring and flagging based on the identified risk factors.
*   Enhanced authentication for high-risk transactions.
*   Implementing rules based on the model's findings.
*   Anomaly detection on balance changes.
*   Considering limits or delays on high-risk transactions during off-hours.
*   User education.
*   Continuous model retraining and adaptation.

## How to Evaluate Prevention Actions

To determine the effectiveness of prevention strategies, key metrics to monitor include:

*   Overall fraud rate.
*   False positive rate.
*   Analysis of new fraud patterns.
*   Model performance on new data.
*   A/B testing of new rules.
*   Cost-benefit analysis.
*   Customer feedback.
*   Manual review feedback.
