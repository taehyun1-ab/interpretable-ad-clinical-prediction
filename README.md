# interpretable-ad-clinical-prediction
Code for interpretable clinical data mining for Alzheimer's disease staging and MCI progression prediction.

## Overview
This repository contains the source code and experimental configuration files for the study:

**Interpretable Clinical Data Mining for Alzheimer's Disease Staging and MCI Progression Prediction**
The proposed framework uses baseline clinical variables from ADNI to perform Alzheimer's disease staging and MCI progression-risk prediction. The model combines feature-wise tokenization, a token-wise supervised autoencoder bottleneck, Transformer-based feature interaction modeling, and interpretable feature-level analyses.

## Prediction Tasks
The framework was evaluated on four binary classification tasks:
1. AD vs. CN
2. AD vs. MCI
3. MCI vs. CN
4. pMCI vs. sMCI

The positive class was defined as:
- AD for AD vs. CN
- AD for AD vs. MCI
- MCI for MCI vs. CN
- pMCI for pMCI vs. sMCI

## Repository Structure
```text
.
├── README.md
├── requirements.txt
├── configs/
│   ├── config_ad_vs_cn.yaml
│   ├── config_ad_vs_mci.yaml
│   ├── config_mci_vs_cn.yaml
│   └── config_pmci_vs_smci.yaml
├── data/
│   └── README_data.md
├── src/
│   ├── preprocessing.py
│   ├── dataset.py
│   ├── model.py
│   ├── train.py
│   ├── evaluate.py
│   └── interpret.py
├── scripts/
│   ├── run_internal_evaluation.sh
│   ├── run_external_validation.sh
│   ├── run_baseline_comparison.sh
│   └── run_shap_analysis.sh
└── results/
    └── README_results.md
```

## Data Availability
The raw clinical data used in this study were obtained from:
- Alzheimer's Disease Neuroimaging Initiative (ADNI)
- Australian Imaging, Biomarkers and Lifestyle Study of Ageing (AIBL)
Due to data use agreements, raw ADNI and AIBL data are not distributed with this repository. Users must request access from the official ADNI and AIBL data portals.

This repository provides:
- preprocessing scripts
- task definition procedures
- model training scripts
- evaluation scripts
- interpretation scripts
- experimental configuration files

 ## Expected Input Format
 After preprocessing, each task-specific input file should be formatted as a CSV file.

 Example columns
 ```text
RID, VISCODE, label, AGE, PTGENDER, PTEDUCAT, APOE4COUNT, MMSCORE, ADNI_MEM, ADNI_EF, FAQFORM, FAQSHOP, ...
```

Required columns:
- `RID`: participant identifier
- `VISCODE`: visit code
- `label`: binary task label
- clinical feature columns: baseline clinical variables used for model training
For the primary ADNI experiments, the complete baseline clinical feature set was used.
For AIBL external validation, the shared-variable model used:
- APOE ε4 count
- MMSE score
- age
- CDR global score

## Environment Setup
The experiments were conducted using Python 3.10.
Install the required packages with:

```bash
pip install -r requirements.txt
```

Main dependencies include PyTorch, scikit-learn, XGBoost, SHAP, pandas, and NumPy.

## Preprocessing
To preprocess the ADNI baseline clinical data, run:
```bash
python src/preprocessing.py \
  --input data/raw/adni_clinical.csv \
  --output data/processed/adni_baseline_features.csv
```

The preprocessing pipeline includes:
- baseline visit filtering
- task-specific label construction
- categorical encoding
- missing value handling
- z-score normalization fitted only on the training set
- task-specific train/test split generation

## Model Training
Example: train the proposed model for the pMCI vs. sMCI task.
```bash
python scr/train.py \
  --config configs/config_pmci_vs_smci.yaml
```


- 
