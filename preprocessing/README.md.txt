
## Data Preprocessing

### 1. Overview

This stage prepares the Diabetes 130-US Hospitals for Years 1999–2008 dataset for the development of two machine learning models to predict hospital readmission within 30 days of discharge.

The original dataset contains 101,766 hospital encounters and 50 columns. The target variable is `readmitted`, which contains three categories: `<30`, `>30`, and `NO`.

The preprocessing workflow converts the original target into a binary classification variable, maps hospital ID codes to descriptive categories, cleans the dataset, removes identifiers from the model predictors, and creates training and testing datasets.

### 2. Files Used

The preprocessing stage uses the following files:

| File | Purpose |
|---|---|
| `data/diabetic_data.csv` | Main dataset containing hospital encounter records. |
| `data/IDS_mapping.csv` | Provides descriptions for admission type, discharge disposition, and admission source ID codes. |
| `src/preprocessing.py` | Python script that performs data loading, mapping, cleaning, target creation, and splitting. |

### 3. ID Mapping

The original dataset contains three hospital-related ID columns that represent categories rather than continuous numerical measurements.

The mapping file `IDS_mapping.csv` is used to replace these numeric codes with their corresponding descriptions.

| Original column | Mapped column | Description |
|---|---|---|
| `admission_type_id` | `admission_type` | Describes the type of hospital admission. |
| `discharge_disposition_id` | `discharge_disposition` | Describes the patient's discharge destination or disposition. |
| `admission_source_id` | `admission_source` | Describes the source from which the patient was admitted. |

The mapping is performed using the descriptions provided in the supplied mapping file rather than manually assigning meanings to numeric codes.

The original ID columns are removed after mapping. The descriptive columns are retained as categorical features for use in feature engineering and modelling.

This prevents the models from treating the numeric IDs as continuous numerical measurements or assuming that a higher ID represents a greater quantity.

The script also checks for unmapped ID values and raises an error if any non-missing ID cannot be matched to a description in the mapping file.

## 4. Preprocessing Techniques

### 4.1 Missing-value handling

The original dataset contains missing values represented by question marks, particularly in fields such as `weight`, `payer_code`, and `medical_specialty`.

These placeholders are converted to missing values.

Numerical and categorical imputation will be performed in the feature engineering and modelling pipelines. Imputation values will be learned from the training data only to prevent information leakage from the test set.

### 4.2 Target transformation

The original `readmitted` column is converted into a binary classification target called `target`.

The target is defined as follows:

| Original value | Target value | Meaning |
|---|---|---|
| `<30` | 1 | Readmitted within 30 days. |
| `>30` | 0 | Readmitted after 30 days, not within the target window. |
| `NO` | 0 | No recorded readmission. |

The original `readmitted` column is removed after creating the binary target.

This formulation supports the research question by treating readmission within 30 days as the positive class.

### 4.3 Identifier and constant column removal

The `encounter_id` column is removed because it is an encounter identifier and does not represent a meaningful patient characteristic.

The `patient_nbr` column is retained temporarily for grouping encounters during the train-test split but is excluded from the model input features.

The `examide` and `citoglipton` columns are removed because they do not provide useful variation for model training.

### 4.4 Training and testing split

An 80/20 patient-grouped split is implemented using `GroupShuffleSplit` with a random state of 42.

The split is grouped using `patient_nbr` to prevent encounters belonging to the same patient from appearing in both the training and testing datasets.

This reduces the risk of data leakage caused by repeated encounters for the same patient.

The split is performed before fitting imputers, encoders, scalers, or models.

## 5. Python Script and How to Run

The preprocessing is implemented in:

`src/preprocessing.py`

The script performs the following operations:

1. Loads the main hospital encounter dataset.
2. Loads the ID descriptions from `IDS_mapping.csv`.
3. Maps the three hospital ID columns to descriptive categorical columns.
4. Converts missing-value placeholders and creates the binary target.
5. Removes identifiers and constant columns.
6. Creates patient-grouped training and testing datasets.
7. Saves the resulting datasets in the `outputs` directory.

From the root of the repository, run:

```bash
python src/preprocessing.py
```

The script requires the following files to be present:

```text
data/diabetic_data.csv
data/IDS_mapping.csv
```

## 6. Expected Output

The script creates the following files in the `outputs` directory:

- `X_train.csv` - Training predictors.
- `X_test.csv` - Testing predictors.
- `y_train.csv` - Training target labels.
- `y_test.csv` - Testing target labels.

The script also displays the dataset dimensions, target distribution, number of training and testing records, and the number of patients appearing in both sets.

The expected patient overlap is zero.

The exact number of records in each split may differ from a simple 80/20 row-level split because the split is grouped by patient.

## 7. Limitations

The dataset consists of historical records from US hospitals between 1999 and 2008. for diabetic patients  Model performance may not generalise directly to current South African hospitals.

The target represents recorded readmission within 30 days and does not necessarily indicate avoidable readmission, poor quality of care, or clinical necessity.

The discharge disposition variable is only available at discharge. Its use is appropriate when the model is intended to estimate readmission risk at or near discharge, but it should not be used for predictions intended to be made earlier during the hospital stay.