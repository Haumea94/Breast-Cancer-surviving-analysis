Breast Cancer Clinical Data Analysis (METABRIC)

Exploratory analysis and survival modeling on the METABRIC clinical dataset, focused on breast cancer, which affects mostly women but also around 1% of men worldwide.

The goal here is simple: look at the clinical profile of patients in the dataset, then go a bit further with survival analysis to see which factors actually relate to outcomes.

What's in the dataset

The data comes from brca_metabric_clinical_data.xlsx, the METABRIC clinical cohort. Each row is a patient/sample with variables such as age at diagnosis, tumor stage and size, cancer type and subtype, ER/PR/HER2 status, treatments received (chemotherapy, hormone therapy, radiotherapy, surgery type), and outcome (vital status, overall survival, relapse-free status).

Rows without a recorded vital status are dropped before analysis.

What the notebook does

1. Exploration


Correlation heatmap of clinical numeric variables.
Distribution of age at diagnosis.
Age compared across surgery types.
Breakdown of detailed cancer types.
Tumor size by tumor stage.
Vital status breakdown.


2. Cleaning


Column names translated and standardized into snake_case.
Inconsistent category labels merged (e.g. different spellings for "Alive", "Mastectomy", "Yes/No").
Vital status simplified: only death from the disease counts as the event, everything else (alive, died of other causes) is treated as the "alive" group for survival purposes.
Categorical variables encoded as binary or one-hot (PAM50 subtype, ER/PR/HER2 status, treatments, surgery type).
Tumor stage and other numeric fields converted properly, with missing/invalid entries set to NaN.


3. Survival analysis


Kaplan-Meier curve for relapse-free survival on the whole cohort.
Kaplan-Meier curves comparing mastectomy vs breast-conserving surgery, with a log-rank test to check if the difference is statistically significant.
Cox proportional hazards model on overall survival, including age, chemotherapy, hormone therapy, radiotherapy, surgery type and mutation count as covariates.


Main findings


Most diagnoses happen between 40 and 60 years old.
Younger patients are more likely to have breast-conserving surgery rather than mastectomy.
Invasive ductal carcinoma is by far the most common cancer type in this cohort.
Relapse-free survival decreases steadily over time, with risk increasing somewhat after 100 months.
Tumor size tends to increase with tumor stage, as expected.
Mastectomy and breast-conserving surgery show significantly different survival curves (log-rank p < 0.0000002).
In the Cox model, age at diagnosis, chemotherapy, and mastectomy are all significantly associated with lower overall survival. This doesn't mean chemotherapy or mastectomy cause worse outcomes — it more likely reflects the fact that these treatments are given to patients with more advanced or aggressive disease in the first place.
Hormone therapy, radiotherapy, and mutation count show no significant association with overall survival in this model.


Requirements

pandas
numpy
matplotlib
seaborn
lifelines
openpyxl

Usage

Place brca_metabric_clinical_data.xlsx in the same folder as the notebook, then run the cells in order. Two figures are saved automatically: breast_cancer_clinical_analysis.png and survival_breast_surgery.png.

Notes

This is an exploratory clinical analysis, not a causal study. The associations found with the Cox model are confounded by disease severity (sicker patients get more aggressive treatment), so they shouldn't be read as "this treatment is harmful."
