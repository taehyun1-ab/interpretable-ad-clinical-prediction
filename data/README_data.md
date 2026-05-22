# Data

Raw ADNI and AIBL data are not included in this repository due to data use agreements.

Users must request access from the official data portals:

- ADNI: Alzheimer's Disease Neuroimaging Initiative
- AIBL: Australian Imaging, Biomarkers and Lifestyle Study of Ageing

## Expected Raw Data Files

Place the harmonized baseline clinical data files under `data/raw/` or modify the file paths in the notebooks.

Expected local file paths:

```text
data/raw/adni_clinical_dataframe.csv
data/raw/aibl_clinical_dataframe.csv
```

The ADNI file is used for the primary internal experiments.  
The AIBL file is used only for external validation.

These files should contain participant identifiers, diagnostic labels, and baseline clinical variables.

Example columns:

```text
RID, VISCODE, DIAGNOSIS, DIAGNOSIS2, AGE, PTGENDER, PTEDUCAT, APOE4COUNT, MMSCORE, ADNI_MEM, ADNI_EF, FAQFORM, FAQSHOP, ...
```

## Diagnostic Labels

The notebooks assume the following diagnostic label columns:

- `DIAGNOSIS`: used for AD vs. CN, AD vs. MCI, and MCI vs. CN tasks
- `DIAGNOSIS2`: used for pMCI vs. sMCI and stratified splitting

The expected diagnostic groups are:

- CN
- MCI
- AD
- sMCI
- pMCI

## Processed Files

After running `notebooks/01_preprocessing.ipynb`, the following task-specific files are generated locally:

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

The AIBL external validation uses shared clinical variables available in both ADNI and AIBL:

- AGE
- APOE4COUNT
- MMSCORE
- CDGLOBAL

## Version Control Note

Raw data and processed data files should not be committed to the public repository.

The following directories are excluded from version control:

```text
data/raw/
data/processed/
data/splits/
```

Generated data files are derived from restricted ADNI/AIBL data and should remain local.
