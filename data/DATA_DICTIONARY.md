# STADIOcare Data Dictionary

Field-level definitions for every column in the extract, as requested in section 6. Types use the labels in your request. Missing values are empty fields throughout.

**Coverage period:** 2022-01-01 to 2026-12-31. **Datetime format:** `YYYY-MM-DD HH:MM:SS`. **Prediction point:** `prediction_timestamp` on table 2 (= discharge).

## 1. `1_patient_demographics.csv` — one row per patient

| Column | Type | Description |
|---|---|---|
| `patient_id` | String | Unique anonymised patient identifier; consistent across encounters. |
| `age` | Integer | Age in completed years at admission. |
| `gender` | Categorical | Female / Male / Unknown. |
| `chronic_conditions` | String | Semicolon-separated conditions diagnosed **before** discharge. Empty means none recorded, not unknown. |
| `province` | Categorical | Province of residence. One of the nine SA provinces. |
| `medical_aid_type` | Categorical | Funding type: Medical aid / Self-pay / Hospital plan / Government - provincial / Workplace - COID. |

## 2. `2_hospital_admissions.csv` — one row per encounter

| Column | Type | Description |
|---|---|---|
| `admission_id` | String | Unique identifier for the hospital encounter. |
| `patient_id` | String | Links the encounter to the patient in table 1. |
| `admission_date` | Datetime | Actual admission time, `YYYY-MM-DD HH:MM:SS`. |
| `discharge_date` | Datetime | Actual discharge time, same format. |
| `admission_type` | Categorical | Emergency / Elective / Surgery / Birth / Urgent / Transfer. |
| `admission_source` | Categorical | Where the patient came from — see valid values below. |
| `discharge_disposition` | Categorical | Destination or status at discharge, set **at** discharge. |
| `length_of_stay_days` | Numeric | Discharge minus admission, in days to 2dp. Derived, supplied for convenience. |
| `icu_admission_flag` | Binary | 1 if the patient entered ICU during the encounter, else 0. |
| `icu_days` | Numeric | ICU days to 2dp. Empty where `icu_admission_flag` = 0. |
| `prediction_timestamp` | Datetime | The defined prediction point for this encounter — equal to discharge. Every predictor must be knowable by this moment. |

## 3. `3_clinical_treatment.csv` — one row per encounter

| Column | Type | Description |
|---|---|---|
| `admission_id` | String | Links to table 2. |
| `patient_id` | String | Links to table 1. |
| `primary_diagnosis_code` | String | Code in `STADIOcare-DX v2026.1`; see table 6. |
| `primary_diagnosis_name` | String | Mapped description of the primary diagnosis. |
| `secondary_diagnosis_codes` | String | Semicolon-separated additional diagnosis codes. |
| `secondary_diagnosis_names` | String | Semicolon-separated mapped names, positionally aligned with the codes. |
| `number_of_diagnoses` | Integer | Total diagnoses for the encounter (primary + secondary). |
| `procedure_codes` | String | Semicolon-separated procedure codes performed before the prediction point. |
| `procedure_names` | String | Semicolon-separated mapped procedure names. |
| `number_of_procedures` | Integer | Count of procedures for the encounter. |
| `medication_names` | String | Semicolon-separated medications associated with the encounter. |
| `medication_category` | String | Semicolon-separated distinct categories of those medications. |
| `number_of_medications` | Integer | Count of medications for the encounter. |
| `lab_test_code` | String | Code of the recorded laboratory test; see table 6. |
| `lab_test_name` | String | Test name. |
| `lab_result_value` | Numeric | Result value. |
| `lab_result_unit` | String | Unit of measure for the result. |
| `lab_reference_range` | String | Normal reference range for the test. |
| `lab_result_datetime` | Datetime | When the result became available. |
| `clinical_severity_indicator` | Categorical | Severity level 1-5, 5 most severe. Empty where not assessed. |
| `complication_indicator` | Binary | 1 where a complication was recorded before the prediction point. |

## 4. `4_previous_utilisation.csv` — one row per encounter, history before it

| Column | Type | Description |
|---|---|---|
| `admission_id` | String | Links to table 2 — the **index** encounter this history precedes. |
| `patient_id` | String | Links to table 1. |
| `previous_admissions` | Integer | Completed admissions for this patient **before** the index admission, within the extract period. |
| `previous_readmissions` | Integer | Of those, how many were themselves 30-day readmissions. |
| `previous_outpatient_visits` | Integer | Outpatient / clinic visits in the 12 months before admission. |
| `previous_emergency_visits` | Integer | Emergency presentations in the 12 months before admission. |
| `previous_icu_admissions` | Integer | Prior encounters involving ICU, within the extract period. |
| `days_since_previous_admission` | Numeric | Days from the previous admission date to this one. Empty for a first admission. |
| `days_since_previous_discharge` | Numeric | Days from the previous discharge to this admission. Empty for a first admission. |
| `previous_admission_diagnoses` | String | Primary diagnoses of up to the last 3 prior encounters. |
| `previous_readmission_diagnoses` | String | Diagnoses recorded on the patient's prior readmissions. |
| `previous_procedures` | String | Procedure codes from up to the last 3 prior encounters. |
| `previous_length_of_stay` | Numeric | Length of stay of the immediately preceding encounter, in days. |

## 5. `5_readmission_outcome.csv` — one row per encounter, contains the target

| Column | Type | Description |
|---|---|---|
| `admission_id` | String | Links to table 2. |
| `patient_id` | String | Links to table 1. |
| `readmission_within_30_days` | Binary | **Target.** 1 where the patient was readmitted within 30 days of discharge. |
| `readmission_date` | Datetime | Admission datetime of the subsequent encounter. Empty where none. |
| `readmission_type` | Categorical | Emergency / Urgent / Planned / Transfer. Empty where none. |
| `readmission_diagnosis_name` | String | Primary diagnosis of the readmission. Empty where none. |
| `planned_readmission_flag` | Binary | 1 where `readmission_type` = Planned. Empty where there was no readmission. |
| `days_to_readmission` | Numeric | Days from discharge to readmission, 1-30. Empty where none. |

## 6. `6_reference_code_mapping.csv` — one row per code

| Column | Type | Description |
|---|---|---|
| `code` | String | The coded value as it appears in table 3. |
| `code_type` | Categorical | Diagnosis / Procedure / Laboratory test. |
| `name` | String | Mapped human-readable description. |
| `category` | String | Grouping for the code, for feature aggregation. |
| `coding_system` | String | Name and version of the coding system the code belongs to. |

## Coding systems

| System | Applies to | Version |
|---|---|---|
| `STADIOcare-DX v2026.1` | `primary_diagnosis_code`, `secondary_diagnosis_codes` | 2026.1 |
| `STADIOcare-PROC v2026.1` | `procedure_codes` | 2026.1 |
| `STADIOcare-LAB v2026.1` | `lab_test_code` | 2026.1 |

Every coded value appearing in table 3 has a row in table 6. The project should not
invent or assume coding values that are absent from that mapping.

_Synthetic data supplied for the CAP182 capstone project. Not real client data._
