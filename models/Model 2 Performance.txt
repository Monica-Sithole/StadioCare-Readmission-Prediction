# Part C: Model 2 Performance – Random Forest

## 1. Introduction

This section presents the performance of the Random Forest classifier developed in Part B. The model was applied to the Diabetes 130-US Hospitals dataset to predict whether a patient would be readmitted within 30 days of discharge.

The positive class was defined as `1` for readmission within 30 days (`<30`), while `0` represented no readmission within 30 days (`NO` or `>30`).

## 2. Code and Instructions

The model performance calculations are contained in:

* Notebook: `src/random_forest_model.ipynb`
* Trained model: `models/random_forest_model.joblib`
* Metrics output: `outputs/random_forest_metrics.csv`
* Predictions: `outputs/random_forest_predictions.csv`
* Feature importance: `outputs/random_forest_feature_importance.csv`

To reproduce the results:

1. Set up the environment using `requirements.txt`.
2. Run the preprocessing and feature engineering notebooks.
3. Run all cells in `src/random_forest_model.ipynb`.
4. Review the metrics, classification report, confusion matrix, ROC curve, precision-recall curve and feature importance results.

## 3. Test Dataset

The model was evaluated on the same 20,153 test observations as Logistic Regression.

| Test Set Measure                       | Number |
| -------------------------------------- | -----: |
| Total test observations                | 20,153 |
| Actual readmissions within 30 days     |  2,151 |
| Actual non-readmissions within 30 days | 18,002 |

Using the same test observations allows the performance of the two models to be compared consistently.

## 4. Performance Metrics

| Metric    | Random Forest |
| --------- | ------------: |
| Accuracy  |        0.6444 |
| Precision |        0.1668 |
| Recall    |        0.5834 |
| F1-score  |        0.2594 |
| ROC-AUC   |        0.6711 |
| PR-AUC    |        0.2046 |

## 5. Results and Interpretation

Random Forest achieved an accuracy of 64.44%, indicating that approximately 64.44% of the test observations were classified correctly at the default classification threshold of 0.5.

The recall was 58.34%, meaning that the model identified approximately 58.34% of actual readmissions within 30 days in the test dataset.

The precision was 16.68%, indicating that approximately 16.68% of patients classified as readmitted within 30 days were actual readmissions in the test data.

The F1-score was 0.2594, reflecting the balance between precision and recall.

The ROC-AUC was 0.6711, while the PR-AUC was 0.2046. These results describe the model's ability to distinguish between the two classes and its precision-recall performance on the held-out test observations.

## 6. Statistical and Visual Evaluation

The confusion matrix was used to examine true positives, true negatives, false positives and false negatives.

The ROC curve assessed discrimination across classification thresholds, while the precision-recall curve examined the trade-off between recall and precision.

The model's class-based metrics were calculated using a threshold of 0.5.

A feature importance analysis was also conducted to identify features contributing to the Random Forest's predictions. Feature importance represents predictive contribution within the fitted model and does not establish causation.

## 7. Limitations

The model predicts observed readmission outcomes and does not establish whether readmissions were avoidable or caused by particular patient or hospital characteristics.

The dataset represents historical US hospital encounters from 1999 to 2008. Its results may not generalise directly to current South African healthcare settings.

Further validation using relevant local healthcare data would be required before operational use.
