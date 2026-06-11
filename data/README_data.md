# Data

Raw ADNI and AIBL data are not included in this repository due to data use agreements.

Users must request access from the official data portals:

- ADNI: Alzheimer's Disease Neuroimaging Initiative
- AIBL: Australian Imaging, Biomarkers and Lifestyle Study of Ageing

## Expected Raw Data Files

To reproduce the reported results, place the harmonized baseline clinical data files under `data/raw/` or modify the file paths in the notebooks.

Expected local file paths:

```text
data/raw/adni_clinical_dataframe.csv
data/raw/aibl_clinical_dataframe.csv
```

The ADNI file is used for the primary internal experiments and for training the reduced shared-variable model.  
The AIBL file is used only for external validation.

Each file should contain one baseline record per participant, including identifiers, diagnostic labels, and baseline clinical variables.

Example columns:

```text
RID, VISCODE, DIAGNOSIS, DIAGNOSIS2, AGE, PTGENDER, PTEDUCAT, APOE4COUNT, MMSCORE, ADNI_MEM, ADNI_EF, FAQFORM, FAQSHOP, ...
```

## Synthetic Sample Data
The `data/sample/` directory contains synthetic CSV files that illustrate the expected raw input format:

```text
data/sample/ADNI_clinical_sample_data.csv
data/sample/AIBL_clinical_sample_data.csv
```

These files do not contain real participant-level records from ADNI or AIBL. ALL values are artifically generated for format inspection only.
The synthetic sample files are not intended for model training, performance evaluation, end-to-end execution, or reproduction of the reported results.

## ADNI Sample File
`ADNI_clinical_sample_data.csv` illustrates the baseline clinical dataframe format used for the primary ADNI experiments.

The same ADNI dataframe is also used for the reduced shared-variable model. The following four common variables are selected within `notebooks/03_external_validation.ipynb`:

- AGE
- APOE4COUNT
- MMSCORE
- CDGLOBAL

A separate four-variable ADNI dataframe is therefore not required.

## AIBL Sample File
`AIBL_clinical_sample_data.csv` illustrates the external validation input format.
The AIBL dataframe contains diagnostic labels and the shared clinical variables required for external validation.

## Diagnostic Labels

The notebooks assume the following diagnostic label columns:

- `DIAGNOSIS`: used for AD vs. CN, AD vs. MCI, and MCI vs. CN tasks
- `DIAGNOSIS2`: used for pMCI vs. sMCI and stratified splitting

Expected values in `DIAGNOSIS`:

- CN
- MCI
- AD

Expected values in `DIAGNOSIS2`:

- CN
- sMCI
- pMCI
- AD

## Processed Files

After running `notebooks/01_preprocessing.ipynb` with an appropriately prepared full ADNI dataframe, the following task-specific files are generated locally:

```text
data/processed/AD_vs_CN_train.csv
data/processed/AD_vs_CN_test.csv
data/processed/AD_vs_MCI_train.csv
data/processed/AD_vs_MCI_test.csv
data/processed/MCI_vs_CN_train.csv
data/processed/MCI_vs_CN_test.csv
data/processed/pMCI_vs_sMCI_train.csv
data/processed/pMCI_vs_sMCI_test.csv
```

The preprocessing notebook also generates:

```text
data/splits/task_split_summary.csv
data/splits/AD_vs_CN_features.txt
data/splits/AD_vs_MCI_features.txt
data/splits/MCI_vs_CN_features.txt
data/splits/pMCI_vs_sMCI_features.txt
```

## Feature Usage

The primary ADNI experiments use baseline clinical variables only.

The reduced shared-variable model uses the following clinical variables available in both ADNI and AIBL:

- AGE
- APOE4COUNT
- MMSCORE
- CDGLOBAL

The reduced model is trained using ADNI data only and evaluated on the AIBL external cohort.

## Version Control Note

Raw data and processed data files should not be committed to the public repository.

The following directories are excluded from version control:

```text
data/raw/
data/processed/
data/splits/
```

Generated data files are derived from restricted ADNI and AIBL data and should remain local.

The synthetic files under `data/sample/` are included only to document the expected input structure.
