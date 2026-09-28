# Part C: Model 1 Performance – Logistic Regression

## 1. Introduction

This section presents the performance of the Logistic Regression model developed in Part B. The model was applied to the Diabetes 130-US Hospitals dataset to predict whether a patient would be readmitted within 30 days of discharge.

The positive class was defined as `1` for readmission within 30 days (`<30`), while `0` represented no readmission within 30 days (`NO` or `>30`).

## 2. Code and Instructions

The model performance calculations are contained in:

* Notebook: `src/logistic_regression_mode.ipynb`
* Trained model: `models/logistic_regression_model.joblib`
* Metrics output: `outputs/logistic_regression_metrics.csv`
* Predictions: `outputs/logistic_regression_predictions.csv`

To reproduce the results:

1. Set up the environment using `requirements.txt`.
2. Run the preprocessing and feature engineering notebooks.
3. Run all cells in `src/logistic_regression_model.ipynb`.
4. Review the evaluation metrics, classification report, confusion matrix, ROC curve and precision-recall curve.

## 3. Test Dataset

The model was evaluated on 20,153 test observations.

| Test Set Measure                       | Number |
| -------------------------------------- | -----: |
| Total test observations                | 20,153 |
| Actual readmissions within 30 days     |  2,151 |
| Actual non-readmissions within 30 days | 18,002 |

The positive class represented approximately 10.67% of the test observations, demonstrating an imbalance between the two classes.

## 4. Performance Metrics

| Metric    | Logistic Regression |
| --------- | ------------------: |
| Accuracy  |              0.6567 |
| Precision |              0.1661 |
| Recall    |              0.5514 |
| F1-score  |              0.2553 |
| ROC-AUC   |              0.6535 |
| PR-AUC    |              0.2011 |

## 5. Results and Interpretation

Logistic Regression achieved an accuracy of 65.67%, indicating that approximately 65.67% of the test observations were classified correctly at the default classification threshold of 0.5.

The recall was 55.14%, meaning that the model identified approximately 55.14% of the actual readmissions within 30 days in the test dataset.

The precision was 16.61%. This indicates that approximately 16.61% of patients classified as readmitted within 30 days were actual readmissions in the test data.

The F1-score was 0.2553, reflecting the balance between precision and recall. The ROC-AUC was 0.6535, indicating that the model demonstrated discriminatory ability above the level of random ranking.

The PR-AUC was 0.2011. This measure is particularly relevant because the positive class represents a relatively small proportion of the test observations.

## 6. Statistical and Visual Evaluation

The confusion matrix was used to examine true positives, true negatives, false positives and false negatives. 

The ROC curve assessed discrimination across classification thresholds, while the precision-recall curve examined the trade-off between identifying actual readmissions and generating false-positive predictions.

The class-based metrics were calculated using a threshold of 0.5. Different thresholds may produce different precision and recall values.

## 7. Limitations

The model predicts observed readmission outcomes and does not establish whether readmissions were avoidable or caused by particular patient or hospital characteristics.

The dataset represents historical US hospital encounters from 1999 to 2008. The results may not generalise directly to current South African healthcare settings.

The model should be considered a decision-support tool rather than a replacement for clinical judgement.
