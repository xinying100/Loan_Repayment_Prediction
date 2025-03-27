# Loan_Repayment_Prediction

## Prerequisities 
* Python Environment: **Python 3.9.X**
* Install all required dependencies through: ``` !pip install -r "requirements.txt" ```

## Introduction
* This project aims to develop a predictive model to determine the risk of loan repayment.
* The approach involves extensive data exploration, preprocessing, visualization, and model training using multiple machine learning models. The models included in this study are LightGBM, Histogram-based Gradient Boosting, and XGBoost.
* Finally, a pipeline is built to process new incoming data and predict the risk of loan repayment. This ensure the easier integration into a API for real-time prediction.

## Implementation Details

### 1. Dataset Exploration
**Objective**: Build a supervised learning model for binary classification.

| Criteria | Description |
|----------|------------|
| **Type of Problem** | Supervised Learning (Label is given) |
| **Classification or Regression** | Binary Classification |
| **Target Variable** | Synthesized Label: (0: Client does not pay, 1: Client pays) |

### 2. Data Preprocessing
**Objective**: Merge tables, create labels, and split data.

#### **a. Handling Missing LoanIDs**
- Some entries in 'loan.csv' have missing `loanId`. These are retained, as missing identifiers occur in real-world applications, and the associated data may still be useful.

#### **b. Pivoting Installment Data**
- Each `loanId` has multiple payments indexed by `installmentIndex`.
- Pivot table used to structure the dataset properly.
- Maximum `installmentIndex` is 105, covering both regular and collection plans.

#### **c. Merging Clarity Underwriting Variables with Loan Data**
- **Key Join**: `clarityFraudId`
- **Issue**: 18,004 unmatched rows from `clarity.csv`, indicating possible incomplete data.

#### **d. Creating Synthesized Labels**
- Based on credit score classification:
  - `clearfraudscore >= 800` → **Label as 1 (Client pays the loan)**
  - `clearfraudscore <= 570` → **Label as 0 (Client does not pay the loan)**
  - Other cases → **Excluded from training**

#### **e. Data Cleaning**
- Focus on key columns from 'loan.csv'.
- Removed unnecessary identifiers (`loanId`, `anon_ssn`, `clarityFraudId`).
- Filled `fpStatus` column with 'Unknown'.
- Selected missing values left intentionally to preserve meaningful signals.
- Converted date columns to datetime format and extracted features like year and month.

#### **f. Splitting Data into Training & Testing Sets**
- **70% Training, 30% Testing**
- **Backup**: Training set saved as `new_data/train_df.csv`

### 3. Data Visualization
**Objective**: Use various plots to analyze data distribution and patterns.

#### **a. Unique Values Summary**
- Missing values filled with mode for numerical data and 'Unknown' for categorical data.
- Summary table created to provide quick insights.

#### **b. Boxplots for Feature Distribution**
- Displays outliers and feature spread.
- Excludes boolean and categorical columns.

#### **c. Bar Charts & Histogram + KDE**
- **Categorical Data**: Bar Charts with missing values labeled `-1`.
- **Numerical Data**: Histogram + Kernel Density Estimation (KDE) to observe distributions.

#### **d. Heatmaps & Boxplots for Payments**
- Heatmaps to show trends in categorical variables.
- Boxplots to detect outliers in numerical variables.

### 4. Model Training and Testing
**Objective**: Train and evaluate models using multiple algorithms.

#### **a. Train-Test Splitting**
- **Target Variable**: Synthesized label created in data preprocessing.
- Evaluates whether a client repays a loan based on historical records.

#### **b. Model Training**
- Models used:
  - **XGBClassifier**
  - **LGBMClassifier**
  - **HistGradientBoostingClassifier**
- Null values handled effectively.
- Models saved as `new_data/{model_name}_classifier_model.pkl`.

#### **c. Model Testing**
- Evaluated using standard classification metrics.
- **Best Performing Model**: **LGBMClassifier** (Highest F1-score).

#### **d. Feature Impact Analysis (SHAP)**
- **Waterfall Plot**: Shows individual feature contributions to predictions.
- **Summary Plot**: Highlights feature importance across all data points.

### 5. Prediction Pipeline for Real-World Use
User can replace the following files and run the code in this section to make predictions on new data:

```bash
📂 new_data/
├── 📄 clarity_underwriting_variables.csv   # Replace this file for predictions
├── 📄 loan.csv                             # Replace this file for predictions
└── 📄 payment.csv                          # Replace this file for predictions
```


### 6. Limitations & Future Work
| Limitation | Future Work |
|------------|------------|
| **Parameter Tuning**: Limited tuning may affect performance. | Use advanced tuning methods (e.g., Grid Search, Bayesian Optimization, or AutoML). |
| **Usable Data Size**: Only ~15,000 of 577,682 records are usable. | Improve data labeling strategies, explore semi-supervised learning, and leverage synthetic data augmentation. |
| **Pipeline Design**: Scalability and flexibility need improvement. | Enhance for large datasets, improve modularity, integrate feature selection techniques, and expand SHAP-based interpretability. |
| **Real-World Deployment**: The model is not currently deployed in a production environment. | Develop an API for real-time inference and integrate with financial systems. |


