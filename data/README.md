# STADIOcare — 30-Day Readmission: Data Extract

Six tables covering **1 January 2022 to 31 December 2026**, the full coverage period named in section 7 of your request. Data is at **patient level** (table 1) and **admission level** (tables 2-5), joined on `patient_id` and `admission_id`. Table 6 is the code mapping reference you asked for in section 6. `patient_id` is anonymised and carries no re-identification key.

Datetimes are `YYYY-MM-DD HH:MM:SS` throughout, as your revised request specifies. Coding is consistent across every hospital in the group.

> **This extract supersedes the earlier four-table version.** It is built against your revised request and adds province and funding type, admission source, length of stay, ICU exposure, secondary diagnoses, procedure and medication names, laboratory results, severity and complication indicators, the full previous-utilisation table, days to readmission, and the code mapping table. Datetimes now carry seconds; the superseded extract used `HH:MM`.

## Files

### `1_patient_demographics.csv`
One row per patient. Rows: 8,625

| Column | Type | Description |
|---|---|---|
| `patient_id` | String | Unique anonymised patient identifier; consistent across encounters. |
| `age` | Integer | Age in completed years at admission. |
| `gender` | Categorical | Female / Male / Unknown. |
| `chronic_conditions` | String | Semicolon-separated conditions diagnosed **before** discharge. Empty means none recorded, not unknown. |
| `province` | Categorical | Province of residence. One of the nine SA provinces. |
| `medical_aid_type` | Categorical | Funding type: Medical aid / Self-pay / Hospital plan / Government - provincial / Workplace - COID. |

### `2_hospital_admissions.csv`
One row per hospital encounter. Rows: 15,446

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

Valid `admission_source` values: Emergency department, GP referral, Specialist referral, Clinic referral, Transfer from another hospital, Direct admission, Unknown.
Valid `discharge_disposition` values: Home, Home with care, Rehabilitation, Transferred, Step-down facility, Against medical advice, Deceased.

### `3_clinical_treatment.csv`
One row per encounter — diagnoses, procedures, medications and laboratory results. Rows: 15,446

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

### `4_previous_utilisation.csv`
One row per encounter — the patient's history **before** that encounter. Rows: 15,446

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

### `5_readmission_outcome.csv`
One row per encounter. Contains the target. Rows: 15,446

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

### `6_reference_code_mapping.csv`
One row per code. The mapping table requested in section 6. Rows: 38

| Column | Type | Description |
|---|---|---|
| `code` | String | The coded value as it appears in table 3. |
| `code_type` | Categorical | Diagnosis / Procedure / Laboratory test. |
| `name` | String | Mapped human-readable description. |
| `category` | String | Grouping for the code, for feature aggregation. |
| `coding_system` | String | Name and version of the coding system the code belongs to. |

A field-by-field dictionary covering all six tables is in [`DATA_DICTIONARY.md`](DATA_DICTIONARY.md).

## Data quality notes

This is an extract from live operational systems and has **not** been cleaned. Expect:

- Missing values in optional / conditionally-populated fields
- Inconsistent casing and stray whitespace in some free-text and categorical fields
- A small number of duplicate records
- Occasional out-of-range or implausible values from data-capture errors
- Some date fields captured in inconsistent formats

**Target:** `readmission_within_30_days` in `5_readmission_outcome.csv` — 1 where the patient was readmitted within 30 days of discharge, 0 otherwise. The rate in this extract is **11.3%**.

**Readmission definition:** any inpatient admission beginning within 30 calendar days of the discharge of the index admission, at any hospital in the group. The clock starts at discharge, not at admission.

**Prediction point.** Section 7 of your request asks for this explicitly, so it is supplied as `prediction_timestamp` on table 2, equal to the discharge datetime. A predictor is only legitimate if its value is knowable at that moment. Two fields need care:

- `discharge_disposition` is set *at* discharge — valid for a discharge-time model, not for an admission-time one.
- `lab_result_datetime` is populated, so you can check per row whether the result had actually returned before the prediction point. Some had not.

**Planned readmissions are identifiable, as requested.** `planned_readmission_flag` is 1 where `readmission_type` = Planned — staged procedures, scheduled chemotherapy cycles, booked follow-up surgery. These are clinically appropriate returns, not failures of care. About **20%** of readmissions here are planned. You will probably want to exclude them or model them separately. The flag is empty where there was no readmission.

**Leakage warning.** `readmission_date`, `readmission_type`, `readmission_diagnosis_name` and `days_to_readmission` exist only for readmitted patients — they *are* the label. Everything in tables 1-4 is knowable at or before discharge, subject to the prediction-point note above.

**Table 4 is already point-in-time.** Every count in `4_previous_utilisation.csv` is computed from encounters strictly **before** the index admission, so it will not leak. `previous_admissions` is 0 and the `days_since_*` fields are empty on a patient's first encounter in the window — that is a genuine absence of history, not a missing value to impute. Note that history is left-censored: a patient admitted in January 2022 may have had admissions before the extract begins.

**Missing and unknown values.** Missing is an **empty field**, never 0 or "N/A". Specifically: `admission_date` / `discharge_date` are absent on 1-2% of encounters where the source did not record a time; `chronic_conditions` is empty for patients with no recorded chronic diagnosis, which differs from unknown; `procedure_codes` is empty where no procedure was performed; `icu_days` is empty where the patient never entered ICU; laboratory fields are empty where no test was recorded for the encounter.

**One thing to decide explicitly:** patients discharged as `Deceased` cannot be readmitted and are always 0. Leaving them in inflates the negative class; most published readmission models exclude them.

**`patient_id` repeats across encounters** for patients admitted more than once. That is intentional and lets you build patient-level history — it also means you must split train and test **by patient**, not by admission, or the same patient will appear on both sides.

_Synthetic data supplied for the CAP182 capstone project. Not real client data._
