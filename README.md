# Sentinel AI: Intelligent Fraud Detection & Decision Support

### PEAK TRANSFORMERS | Sol Plaatje University Data Science Club Finance & Digital Innovation Hackathon 2026

An intelligent machine learning solution for detecting suspicious financial transactions and supporting risk-based fraud investigation.

## Overview

**Sentinel AI** is an intelligent fraud detection and decision-support concept developed by **PEAK TRANSFORMERS** during the Sol Plaatje University Data Science Club Finance & Digital Innovation Hackathon 2026.

The project addresses a major challenge faced by financial institutions: detecting fraudulent transactions while minimizing false alarms that can disrupt legitimate customers.

We developed a machine learning pipeline using **636,262 simulated financial transactions**, calibrated to reflect real African financial-services behavior. Only **849 transactions (0.13%) were fraudulent**, making class imbalance one of the central challenges of the project.

The project combines machine learning with a proposed operational decision-support platform that can:

- Detect potentially fraudulent transactions
- Generate fraud risk scores
- Prioritize suspicious transactions
- Support fraud analysts with interpretable risk factors
- Recommend appropriate responses based on risk
- Minimize unnecessary customer friction

## Problem Statement

Traditional rule-based fraud detection systems can struggle with increasingly sophisticated fraud patterns and may generate large numbers of false-positive alerts.

For a financial institution, this creates a difficult trade-off:

**Detect more fraud → potentially increase false alarms**

**Reduce false alarms → potentially allow more fraud to pass undetected**

Sentinel AI aims to address this trade-off using machine learning to identify behavioral patterns associated with fraudulent transactions while maintaining a practical balance between precision and recall.

## Project Objectives

The main objectives were to:

1. Explore patterns associated with fraudulent financial transactions.
2. Develop meaningful behavioral features from transaction data.
3. Address the severe class imbalance between fraudulent and legitimate transactions.
4. Develop and evaluate machine learning classification models.
5. Compare model performance using fraud-specific evaluation metrics.
6. Select a model that provides a practical balance between fraud detection and false-positive reduction.
7. Translate machine learning predictions into an operational fraud-management workflow.
8. Demonstrate how an intelligent fraud platform could support fraud analysts and customer protection.

## Dataset

The dataset contained:

Total transactions - **636,262**
Fraudulent transactions - **849**
Fraud rate - **0.13%**
Legitimate transactions - **99.87%**
Original variables - **11**
Target variable - **isFraud**

The dataset consists of simulated mobile-money transactions generated using a financial transaction simulation engine calibrated to reflect real-world African financial behavior.

It contains information relating to:

- Transaction amounts
- Transaction types
- Sender accounts
- Recipient accounts
- Sender balances
- Recipient balances
- Balance changes
- Fraud labels

The dataset was complete, with no missing values requiring imputation. Repeated customer identifiers were retained because they represented customers performing multiple transactions rather than erroneous duplicate records.

## Exploratory Data Analysis

The exploratory analysis focused on understanding:

- Fraud distribution
- Transaction types associated with fraud
- Transaction amounts
- Relationships between numerical variables
- Behavioural changes in account balances

### Key Findings

#### Transaction Type

Fraudulent activity occurred almost entirely within:

- `TRANSFER`
- `CASH_OUT`

Meanwhile, `PAYMENT`, `DEBIT`, and `CASH_IN` showed little to no fraudulent activity.

This suggests that transaction types involving rapid movement or withdrawal of funds may warrant additional monitoring.

#### Transaction Amount

Fraudulent transactions generally involved larger monetary values than legitimate transactions.

However, transaction amount alone was not sufficient to distinguish fraud from legitimate behaviour. Its predictive value increased when combined with transaction type and behavioural account features.

#### Behavioural Features

Engineered balance-related variables showed stronger relationships with fraudulent behaviour than several original balance variables.

This highlighted the importance of capturing **changes in customer behavior**, rather than relying solely on static account information.

## Feature Engineering

Several behavioural features were created to provide the models with more informative representations of transaction activity.

### Engineered Features

`orig_balance_diff` - Difference between the sender's balance before and after the transaction
`dest_balance_diff` - Difference between the recipient's balance before and after the transaction
`orig_emptied` - Indicates whether the sender's account was completely emptied
`dest_was_empty` - Indicates whether the recipient initially had a zero balance

The categorical `type` variable was transformed using **One-Hot Encoding**.

The customer identifier variables `nameOrig` and `nameDest` were removed because they identify customers rather than represent meaningful transaction behavior.

## Handling Class Imbalance

Fraud detection presents a particularly difficult machine learning problem because fraudulent transactions are extremely rare.

Only **0.13%** of the transactions in the dataset were fraudulent.

A model that simply predicted every transaction as legitimate could achieve very high accuracy while being completely ineffective at detecting fraud.

To address this, two approaches were considered:

### SMOTE

**Synthetic Minority Oversampling Technique (SMOTE)** was considered to generate synthetic examples of the minority fraud class and create a more balanced training dataset.

### Class Weighting

Class weighting gives greater importance to fraudulent transactions during model training without changing the original class distribution.

The approaches were evaluated using:

- Accuracy
- Precision
- Recall
- F1-score
- ROC-AUC

## Machine Learning Models

The project evaluated different classification approaches as part of the model-development process.

### Logistic Regression

Logistic Regression was used as a baseline because of its simplicity and interpretability.

### XGBoost

**Extreme Gradient Boosting (XGBoost)** was selected because it can capture complex, non-linear relationships within structured datasets and is well suited to challenging classification problems.

XGBoost ultimately produced the strongest overall performance and was selected as the final fraud detection model.

## Model Performance

### Final XGBoost Results

Accuracy - **99.94%**
Precision - **73.33%**
Recall - **90.59%**
F1-Score - **81.05%**
ROC-AUC - **0.9997**

### Logistic Regression vs XGBoost

Metric | Logistic Regression | XGBoost
Accuracy | 97.37% | **99.94%** |
Precision | 4.83% | **73.33%** |
Recall | **100.00%** | 90.59% |
F1-Score | 9.22% | **81.05%** |
ROC-AUC | — | **0.9997** |

Although Logistic Regression detected all fraudulent transactions, its extremely low precision meant that a large proportion of transactions flagged as fraud were actually legitimate.

XGBoost provided a much more practical balance between **detecting fraud and reducing unnecessary investigations**.

## Confusion Matrix

The final XGBoost model produced:

| Predicted Legitimate | Predicted Fraud |
| **Actual Legitimate** | 127,027 | 56 |
| **Actual Fraud** | 16 | 154 |

### What this means
**154 True Positives** - fraudulent transactions correctly identified.
**56 False Positives** - legitimate transactions incorrectly flagged.
**16 False Negatives** - fraudulent transactions that were missed.
**127,027 True Negatives** - legitimate transactions correctly identified.

The results demonstrate the practical importance of evaluating more than accuracy when working with highly imbalanced fraud data.

## Feature Importance

The XGBoost feature-importance analysis identified the following as particularly influential:

1. orig_balance_diff
2. orig_emptied
3. Transaction amount
4. Transaction type

This suggests that changes in account balances provide important signals for distinguishing fraudulent transactions.

The finding also demonstrates the value of feature engineering: meaningful behavioral indicators can expose patterns that may not be apparent from raw transaction variables.

# Sentinel AI

The machine learning model forms the analytical foundation of **Sentinel AI**, our proposed intelligent fraud operations platform.

Rather than simply returning: Fraud = 1
Sentinel AI translates the model's prediction into an operational decision-support process.

## Risk-Based Decision Framework

The prototype demonstrates four risk levels:

| Risk Score | Classification | Proposed Response |
| < 30% | Low Risk | Approve immediately |
| 30–70% | Medium Risk | Step-up authentication |
| 70–95% | High Risk | Hold and route to analyst |
| > 95% | Critical | Immediate freeze / investigation |

These thresholds form part of the proposed decision-support framework rather than claiming to represent an actual bank's production thresholds.

## Interactive Prototype

The project includes an interactive HTML prototype designed to demonstrate how Sentinel AI could be used by banking risk teams.

### Executive Briefing

Provides a high-level overview of:

- Capital protected
- Fraud interception rate
- False-positive events
- Escaped fraud
- Overall business impact

### Risk & Impact Matrix

Translates traditional machine-learning concepts such as:

- True Positives
- True Negatives
- False Positives
- False Negatives

into business-oriented outcomes such as capital protection, customer friction, and model improvement priorities.

### Customer Journey & Workflow

Demonstrates how transactions could move through the Sentinel AI decision pipeline based on their risk score.

### Analyst Decision Support

Provides a simulated fraud analyst workspace containing:

- Prioritised alerts
- Fraud probability
- Risk drivers
- Recommended actions
- Investigation notes

### Real-Time Operations

Demonstrates a simulated transaction stream and operational monitoring dashboard.

The prototype contains five main views: **Executive Briefing, Risk & Impact Matrix, Customer Journey & Workflow, Analyst Decision Support, and Real-Time Operations**.

## Business Impact

The project was designed around the idea that a successful fraud detection system should not only maximise fraud detection.

It should also consider:

- Customer experience
- False-positive investigation costs
- Analyst workload
- Financial exposure
- Operational efficiency
- Trust in digital banking

Based on the final model's confusion matrix, the prototype translates the results into a business scenario where 154 fraudulent transactions are intercepted while 56 legitimate transactions receive additional verification.

The prototype uses an illustrative average fraud value of **R25,000** to demonstrate potential financial impact.


## Technologies Used

### Programming & Data Science

- Python
- Pandas
- NumPy
- Scikit-learn
- XGBoost
- Imbalanced-learn / SMOTE
- Matplotlib
- Seaborn
- Jupyter Notebook

### Prototype

- HTML5
- CSS3
- JavaScript

### Machine Learning Techniques

- Logistic Regression
- XGBoost
- SMOTE
- Class Weighting
- One-Hot Encoding
- Feature Engineering
- Feature Importance Analysis
- Confusion Matrix Analysis

## Getting Started

To explore the machine learning model:
1. Open FraudDetectionModel.ipynb in Jupyter Notebook or Google Colab
2. Install the required libraries (pip install pandas numpy scikit-learn xgboost imbalanced-learn matplotlib seaborn jupyter)
3. Run the notebook cells sequentially

To view the Sentinel AI prototype:
1. Open fraud_code.html in a browser
2. No backend is required

### Synthetic Data

The dataset consists of simulated mobile-money transactions. Although it was calibrated to reflect real African financial behavior, performance on real banking transaction data would need to be evaluated before production deployment.

### Class Imbalance

Fraudulent transactions represented only 0.13% of the dataset, making fraud detection inherently difficult and requiring specialized imbalance-handling strategies.

## Team

### PEAK TRANSFORMERS

Developed during the **Sol Plaatje University Data Science Club Finance & Digital Innovation Hackathon 2026**.

The team was responsible for the data preprocessing, feature engineering, machine learning development, evaluation and interpretation of results. AI tools were used as supporting tools for code interpretation, report refinement and documentation rather than replacing the team's analytical decision-making.

## Project Deliverables

This repository contains the key project artefacts:

- **FraudDetectionModel.ipynb** — Machine learning workflow
- **fraud_code.html** — Interactive Sentinel AI prototype
- **README.md** — Project documentation
- **Project report** — Detailed methodology, evaluation and findings

## Conclusion

Sentinel AI demonstrates how machine learning can be combined with risk-based decision support to address financial fraud.

The final XGBoost model achieved **99.94% accuracy, 73.33% precision, 90.59% recall, 81.05% F1-score and 0.9997 ROC-AUC** on the evaluated dataset.

More importantly, the project demonstrates that fraud detection should not be viewed purely as a classification problem. The model's predictions need to be translated into meaningful operational decisions that balance **fraud prevention, customer experience and analyst efficiency**.

**PEAK TRANSFORMERS** therefore proposed Sentinel AI as a framework for moving from simple fraud classification toward an intelligent, risk-aware fraud operations platform.
