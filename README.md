# StadioCare 30-day hospital readmission prediction
## Motivation
STADIOcare Group operates a large private healthcare network in South Africa, serving approximately 3.1 million patients each year across 44 acute and day hospitals. With increasing pressure from older and sicker patients, rising operating costs, and a decline in operating margin from 15.9% to 13.8% over the past two years, the organisation needs to find ways of improving patient outcomes while using its existing resources more effectively.
One of STADIOcare’s key priorities for 2030 is to keep patients out of hospital when hospital care is not necessary and to support patients who are likely to deteriorate or return to hospital within a month. This creates an opportunity to use data to identify patients who may be at higher risk of a 30-day readmission after discharge. Early identification could allow healthcare teams to provide appropriate follow-up, primary care or virtual support to patients who may need additional assistance.
A data science solution could therefore support both patient care and operational efficiency. By using information from previous hospital encounters and patient characteristics to predict readmission risk, STADIOcare could better target its available resources rather than applying the same level of follow-up to every patient. This project is also aligned with the organisation’s broader goal of becoming more data-led and making better use of the information generated across its fragmented healthcare systems.
## Problem Statement
STADIOcare Group needs a reliable way to identify patients who are at increased risk of returning to hospital within 30 days of discharge, some patients return to hospital within 30 days of discharge, creating additional pressure on hospital capacity, healthcare resources and operating costs.Currently, it may be difficult to determine which patients require additional support after leaving hospital, particularly across a large and diverse healthcare network.
The problem this project will address is whether patient and hospital encounter characteristics can be used to predict the likelihood of a 30-day readmission. A data science model could help STADIOcare identify higher risk patients earlier, allowing healthcare teams to prioritise appropriate follow-up and support. This could contribute to better patient outcomes while supporting the organisation’s goal of reducing unnecessary hospital use and making more effective use of its healthcare resources.
## Project Structure
The repository is organised according to the main stages of the data science project.
data:contains raw and processed datasets.
Preprocessing:contains data quality checks and preprocessing activities.
feature-extraction: contains the creation and preparation of features for modelling.
Modelling:contains the development of models for predicting 30-day readmission risk.
evaluation: contains model evaluation and comparison results.
visualisation:contains scripts and notebooks used to create project visualisations.
utils:contains statistical helper functions and reusable supporting scripts.
Experiments:contains information about experimental setup and experimental results.
## RAAIDD Log
| Type | RAAIDD Entry |
|---|---|
| **Risk** | The public dataset may not contain all the patient and hospital information required to accurately represent STADIOcare's readmission problem. |
| **Risk** | Missing values, inconsistent coding, or repeated patient encounters may affect the quality and reliability of the prediction model. |
| **Action** | Inspect and document the quality, completeness, and structure of the dataset before beginning model development. |
| **Action** | Compare different classification models and evaluation metrics to determine which approach is most appropriate for identifying patients at increased risk of 30-day readmission. |
| **Assumption** | Patient and hospital encounter characteristics available in the dataset are sufficiently related to the likelihood of a 30-day readmission to support predictive modelling. |
| **Assumption** | A patient who is recorded as being readmitted within 30 days represents a meaningful outcome for evaluating the proposed prediction approach. |
| **Issue** | 1.The available public dataset is a proxy for STADIOcare's actual patient data, meaning that the findings may not fully represent STADIOcare's patient population or operating environment. 2.The data might not correctly present the stadioCare |
| **Decision** | The project will focus on predicting 30-day readmission risk rather than general hospital performance or other opportunities identified from the STADIOcare briefing pack. |
| **Dependency** | Data quality assessment must be completed before preprocessing and feature engineering can be finalised. |
| **Dependency** | Preprocessing and feature engineering must be completed before the readmission prediction models can be trained and evaluated. |

## Model Performance and Comparison
##  Model Performance and Comparison

Part C evaluates the Logistic Regression and Random Forest models using the Diabetes 130-US Hospitals dataset. Both models were evaluated on the same held-out test dataset of 20,153 observations.

### Model 1 Performance – Logistic Regression
[Model 1 Performance – Logistic Regression](Model1Performance.MD)for the Logistic Regression evaluation results, including accuracy, precision, recall, F1-score, ROC-AUC, PR-AUC and instructions for reproducing the calculations using `src/LogisticRegression.ipynb`.

### Model 2 Performance – Random Forest

See [Model 1 Performance – Logistic Regression](model/Model1Performance.MD) for the Random Forest evaluation results, hyperparameter configuration, feature importance and instructions for reproducing the calculations using `src/RandomForest.ipynb`.

### Comparison of Model 1 and Model 2

See [Model Comparison](model/Comparison.MD) for the side-by-side comparison of both models, confusion matrices, ROC and precision-recall curves, bootstrap confidence intervals and paired ROC-AUC analysis.

The comparison calculations can be reproduced using `src/ModelComparison.ipynb` after running both model notebooks.

