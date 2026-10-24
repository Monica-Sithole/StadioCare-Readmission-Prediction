# SS3 Part B — Random Forest 30-Day Readmission

## Project overview
This project applies the Random Forest classification method selected in SS2 to the supplied STADIOcare synthetic readmission extract. The prediction target is `readmission_within_30_days` and the prediction point is discharge.

## Files required 

- `1_patient_demographics (1).csv`
- `2_hospital_admissions (1).csv`
- `3_clinical_treatment (1).csv`
- `4_previous_utilisation.csv`
- `5_readmission_outcome.csv`
- `6_reference_code_mapping.csv`

The notebook currently uses the uploaded filenames above.

## To run the notebook
1. Install Python 3.9+ and Jupyter Notebook/JupyterLab.
2. Install the dependencies listed in `requirements.txt`.
3. Start Jupyter in the project directory.
4. Open `SS3_PartB.ipynb`.
5. Run all cells from top to bottom.

## Method
Random Forest was retained as the final method selected in SS2. The notebook includes dataset loading, data cleaning and feature engineering, patient-grouped train/validation/test splits, preprocessing within a scikit-learn pipeline, model fitting, validation, final test evaluation and visualisations.

## Important methodological decisions
- The outcome table contributes only `readmission_within_30_days`. Readmission date/type/diagnosis and days to readmission are excluded from predictors to prevent target leakage.
- Deceased encounters are excluded.
- Planned readmissions remain in the target as supplied. The resulting model predicts the provided binary target, not only unplanned readmission.
- Laboratory values recorded after the discharge prediction timestamp are excluded.
- Splitting is grouped by `patient_id` to avoid placing the same patient in multiple subsets.
- Missing values are imputed inside the pipeline. Categorical variables are one-hot encoded.
- The source documentation states that the dataset is synthetic and is not real patient/client data.




