# Processed Dataset – Telco Customer Churn

## Purpose

This folder contains the cleaned/processed version of the raw Telco Customer Churn dataset used for the IT3091 Machine Learning Group 27 project.

The project uses **Churn Risk** as the primary business lens and **High-Risk Customer Profiling** as the secondary lens.

## Processed Dataset

**File:** `telco_churn_cleaned.csv`

The processed dataset contains the cleaning steps required before the machine-learning preprocessing pipeline:

1. `TotalCharges` was converted from text/object format to numeric.
2. The 11 missing `TotalCharges` values were treated as `0`.
   - All 11 affected customers had `tenure = 0`.
   - All 11 had `Churn = No`.
3. The `Churn` target remains available for modelling.
4. `customerID` is retained in the cleaned CSV for customer reference, but it is excluded from the machine-learning predictor matrix.

## Important: Model-Specific Preprocessing

The processed CSV is **not** the final one-hot-encoded or scaled model matrix.

Model-specific transformations are performed inside the scikit-learn pipelines:

- Numerical features → `StandardScaler`
- Categorical features → `OneHotEncoder(handle_unknown="ignore")`
- Numerical and categorical transformations → `ColumnTransformer`

Keeping these transformations inside the model pipelines helps prevent data leakage because the transformations are fitted as part of the training/validation workflow.

## Data Flow

```text
Raw Dataset
    ↓
Data Type Correction
    ↓
Missing-Value Investigation and Treatment
    ↓
Cleaned / Processed Dataset
    ↓
Train/Test Split
    ↓
Pipeline
    ├── StandardScaler (numerical features)
    └── OneHotEncoder (categorical features)
    ↓
Machine-Learning Models
```

## Relationship to the Project

The cleaned dataset supports the preprocessing and feature-preparation stage documented in:

- `documentation/Preprocessing_Feature_Log.xlsx`
- `notebooks/IT3091_Group27_Telco_Churn.ipynb`

The trained model pipelines are stored in:

```text
models/
├── logistic_regression.joblib
├── decision_tree.joblib
├── random_forest.joblib
└── svm.joblib
```

## Reproducibility

The raw dataset used for the final project is:

`WA_Fn-UseC_-Telco-Customer-Churn.csv`

The final notebook contains the complete preprocessing and modelling workflow. The project also maintains decision, EDA, preprocessing, and model-comparison logs to document the major analytical decisions.

## Note

Do not replace this cleaned dataset with a manually scaled or one-hot-encoded CSV. Encoding and scaling are intentionally handled inside the model pipelines to keep the workflow reproducible and leakage-safe.
