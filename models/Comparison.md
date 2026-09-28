# Part C: Comparison of Logistic Regression and Random Forest

## 1. Introduction

This section compares the performance of Logistic Regression (Model 1) and Random Forest (Model 2) in predicting hospital readmission within 30 days of discharge.

Both models were evaluated using the same held-out test dataset from the Diabetes 130-US Hospitals dataset. This ensures that the reported performance differences are based on the same test observations.

The positive class was defined as readmission within 30 days (`<30`).

## 2. Code and Instructions

The comparison calculations are contained in:

* Notebook: `src/ModelComparison.ipynb`
* Metric comparison: `outputs/model_comparison_metrics.csv`
* Bootstrap confidence intervals: `outputs/bootstrap_confidence_intervals.csv`
* Paired ROC-AUC comparison: `outputs/paired_auc_comparison.csv`

To reproduce the results:

1. Set up the environment using `requirements.txt`.
2. Run the preprocessing and feature engineering notebooks.
3. Run `src/Logistic Regression.ipynb` and `src/Random Forest.ipynb`.
4. Run all cells in `src/ModelComparison.ipynb`.
5. Review the comparison table, confusion matrices, ROC curves, precision-recall curves and bootstrap results.

## 3. Test Dataset

Both models were evaluated on 20,153 test observations.

| Test Set Measure                       | Number |
| -------------------------------------- | -----: |
| Total test observations                | 20,153 |
| Actual readmissions within 30 days     |  2,151 |
| Actual non-readmissions within 30 days | 18,002 |
| Positive class proportion              | 10.67% |

The same test labels and observations were used for both models, allowing a consistent comparison of their predictions and probabilities.

## 4. Performance Comparison

The following table presents the results obtained from the test dataset.

| Metric    | Logistic Regression | Random Forest | Difference (RF - LR) |
| --------- | ------------------: | ------------: | -------------------: |
| Accuracy  |              0.6567 |        0.6444 |              -0.0123 |
| Precision |              0.1661 |        0.1668 |              +0.0007 |
| Recall    |              0.5514 |        0.5834 |              +0.0321 |
| F1-score  |              0.2553 |        0.2594 |              +0.0041 |
| ROC-AUC   |              0.6535 |        0.6711 |              +0.0176 |
| PR-AUC    |              0.2011 |        0.2046 |              +0.0035 |

The difference is calculated as the Random Forest score minus the Logistic Regression score. Positive values indicate a higher score for Random Forest on that metric, while negative values indicate a higher score for Logistic Regression.

## 5. Comparison of Model Performance

### 5.1 Accuracy

Logistic Regression achieved an accuracy of 65.67%, compared with 64.44% for Random Forest. The difference was 1.23 percentage points in favour of Logistic Regression.

Accuracy measures the proportion of all test observations classified correctly. However, accuracy should be interpreted alongside the other metrics because only 10.67% of the test observations belonged to the positive class.

### 5.2 Precision and Recall

Logistic Regression achieved a precision of 16.61%, while Random Forest achieved 16.68%. The difference was 0.07 percentage points.

Random Forest achieved a recall of 58.34%, compared with 55.14% for Logistic Regression, a difference of 3.21 percentage points.

This means that, at the default classification threshold of 0.5, Random Forest identified a larger proportion of actual readmissions in the test dataset. Its precision was slightly higher, although both models had relatively low precision.

Low precision indicates that many patients flagged as readmitted were not actually readmitted within 30 days in the test data. In a practical healthcare setting, this could result in additional assessments or follow-up activity for patients who would not experience the observed outcome.

### 5.3 F1-score

The F1-score for Logistic Regression was 0.2553, compared with 0.2594 for Random Forest.

The difference was 0.0041. This indicates a small difference in the balance between precision and recall at the selected classification threshold.

### 5.4 ROC-AUC

Logistic Regression achieved a ROC-AUC of 0.6535, while Random Forest achieved 0.6711.

The observed difference was 0.0176 in favour of Random Forest. ROC-AUC measures how well a model ranks positive observations above negative observations across possible classification thresholds.

Both models demonstrated discriminatory ability above random ranking, although their ROC-AUC values indicate that distinguishing between the two classes remains challenging.

### 5.5 PR-AUC

The PR-AUC was 0.2011 for Logistic Regression and 0.2046 for Random Forest.

The difference was 0.0035. Both models had PR-AUC values above the positive-class proportion of approximately 0.1067, which provides the baseline precision for a no-skill classifier on this test set.

The results indicate that the precision-recall performance of the models was relatively similar, with Random Forest achieving a slightly higher PR-AUC.

## 6. Statistical Analysis

### 6.1 Bootstrap Confidence Intervals

A bootstrap procedure using 1,000 resamples was applied to estimate 95% confidence intervals for ROC-AUC and PR-AUC.

| Model               | Metric  | Estimate | 95% CI Lower | 95% CI Upper |
| ------------------- | ------- | -------: | -----------: | -----------: |
| Logistic Regression | ROC-AUC |   0.6535 |       0.6415 |       0.6668 |
| Logistic Regression | PR-AUC  |   0.2011 |       0.1875 |       0.2156 |
| Random Forest       | ROC-AUC |   0.6711 |       0.6595 |       0.6829 |
| Random Forest       | PR-AUC  |   0.2046 |       0.1904 |       0.2193 |

All 1,000 bootstrap samples were valid for each metric.

The confidence intervals describe the uncertainty around the estimated performance metrics under the bootstrap resampling procedure.

The ROC-AUC confidence intervals overlap slightly, while the PR-AUC confidence intervals overlap. Overlap between separate confidence intervals is not itself a formal test of whether the difference between models is statistically significant.

### 6.2 Paired Bootstrap Comparison of ROC-AUC

A paired bootstrap procedure was used to estimate the difference in ROC-AUC between Random Forest and Logistic Regression. Both models were evaluated on the same resampled test observations.

| Statistical Measure                 | Result |
| ----------------------------------- | -----: |
| ROC-AUC difference (RF - LR)        | 0.0176 |
| 95% confidence interval lower bound | 0.0081 |
| 95% confidence interval upper bound | 0.0261 |
| Valid bootstrap samples             |  1,000 |

The estimated ROC-AUC difference was 0.0176, with a 95% bootstrap confidence interval from 0.0081 to 0.0261.

The interval is entirely above zero, indicating that the paired bootstrap analysis provides evidence of a positive ROC-AUC difference for Random Forest relative to Logistic Regression on this test sample.

This result applies to ROC-AUC. It does not establish that Random Forest is statistically superior on every performance metric or in other healthcare populations.

The bootstrap procedure resampled test observations rather than patients as groups. Since patients can have multiple encounters, the confidence intervals should be interpreted as approximate test-set uncertainty and may not fully account for within-patient dependence.

## 7. Overall Discussion

The results show differences in the predictive performance of Logistic Regression and Random Forest.

Logistic Regression achieved higher accuracy, while Random Forest achieved higher precision, recall, F1-score, ROC-AUC and PR-AUC. The differences in precision, F1-score and PR-AUC were small, whereas the recall and ROC-AUC differences were more noticeable.

From a readmission identification perspective, recall is relevant because it measures the proportion of actual readmissions identified by a model. Random Forest identified a larger proportion of actual readmissions at the default threshold of 0.5.

However, both models had low precision. This means that using their predictions to trigger follow-up would also flag many patients who were not readmitted within 30 days. In practice, the choice of classification threshold would need to consider available healthcare resources, the consequences of missed readmissions and the workload created by false-positive predictions.

The ROC-AUC comparison provides evidence of a positive difference for Random Forest on the held-out test data. Nevertheless, the models results should be interpreted in the context of the dataset, class imbalance and the evaluation method.

## 8. Conclusion

Both Logistic Regression and Random Forest were able to identify patterns associated with readmission within 30 days in the Diabetes 130-US Hospitals dataset.

Logistic Regression achieved higher accuracy, while Random Forest achieved higher recall, F1-score, ROC-AUC and PR-AUC. The paired bootstrap analysis indicated a positive ROC-AUC difference for Random Forest on the test sample.

The findings are limited to historical US hospital encounters from 1999 to 2008. Further validation using relevant current South African healthcare data would be required before operational use.

The models predict observed readmission outcomes and do not establish whether readmissions were avoidable or caused by particular patient or hospital characteristics. Their predictions should support further assessment and clinical judgement rather than replace it.
