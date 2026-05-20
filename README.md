# interpretable-ad-clinical-prediction

Code for interpretable clinical data mining for Alzheimer's disease staging and MCI progression prediction.

## Overview

This repository contains the source code and experimental configuration files for the study:

**Interpretable Clinical Data Mining for Alzheimer's Disease Staging and MCI Progression Prediction**

The proposed framework uses baseline clinical variables from ADNI to perform Alzheimer's disease staging and MCI progression-risk prediction. The model combines:

- feature-wise tokenization
- token-wise supervised autoencoder bottleneck
- Transformer-based feature interaction modeling
- interpretable feature-level analyses

## Prediction Tasks

The framework was evaluated on four binary classification tasks:

- AD vs. CN
- AD vs. MCI
- MCI vs. CN
- pMCI vs. sMCI

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
│   ├── config_aibl_shared.yaml
│   ├── config_mci_vs_cn.yaml
│   └── config_pmci_vs_smci.yaml
├── data/
│   └── README_data.md
├── results/
│   └── README_results.md
└── src/
    ├── dataset.py
    ├── evaluate.py
    ├── evaluate_external.py
    ├── interpret.py
    ├── model.py
    ├── preprocessing.py
    └── train.py
```

## Data Availability

The raw clinical data used in this study were obtained from:

- Alzheimer's Disease Neuroimaging Initiative (ADNI)
- Australian Imaging, Biomarkers and Lifestyle Study of Ageing (AIBL)

Due to data use agreements, raw ADNI and AIBL data are not distributed with this repository. Users must request access from the official ADNI and AIBL data portals.

This repository provides:

- preprocessing scripts
- task definition procedures
- model training code
- evaluation code
- interpretation analysis code
- experimental configuration files

## Input Format

After preprocessing, each task-specific input file should be formatted as a CSV file.

Example columns:

```text
RID, VISCODE, label, AGE, PTGENDER, PTEDUCAT, APOE4COUNT, MMSCORE, ADNI_MEM, ADNI_EF, FAQFORM, FAQSHOP, ...
```

Required columns:

- `RID`: participant identifier
- `VISCODE`: visit code
- `label`: binary task label
- clinical feature columns used for model training

For AIBL external validation, the shared-variable model used:

- APOE ε4 count
- MMSE score
- age
- CDR global score

## Compatibility

The experiments were conducted using:

- Python 3.10
- PyTorch
- scikit-learn
- XGBoost
- SHAP
- pandas
- NumPy

Install dependencies with:

```bash
pip install -r requirements.txt
```

## How to Preprocess Data

To preprocess the ADNI baseline clinical data:

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

## How to Train the Model

Example: train the model for the pMCI vs. sMCI task.

```bash
python src/train.py \
  --config configs/config_pmci_vs_smci.yaml
```

Hyperparameter tuning was performed only on the pMCI vs. sMCI task using stratified five-fold cross-validation and Bayesian optimization. The selected configuration was reused for the remaining binary classification tasks.

## How to Evaluate the Model

To evaluate a trained model on the held-out ADNI test set:

```bash
python src/evaluate.py \
  --config configs/config_pmci_vs_smci.yaml \
  --checkpoint results/checkpoints/pmci_vs_smci_best.pt
```

Reported metrics include:

- AUC
- accuracy
- precision
- sensitivity
- specificity
- F1-score

## External Validation

External validation was performed on AIBL using shared clinical variables available in both ADNI and AIBL.

```bash
python src/evaluate_external.py \
  --config configs/config_aibl_shared.yaml \
  --checkpoint results/checkpoints/shared_variable_model.pt
```

AIBL samples were not used for:

- training
- validation
- hyperparameter tuning
- early stopping
- model selection

## Model Interpretation

Interpretability analyses include:

- feature ablation
- permutation analysis
- SHAP global explanation
- SHAP local explanation
- attention heatmap visualization

Example: run SHAP analysis for pMCI vs. sMCI.

```bash
python src/interpret.py \
  --config configs/config_pmci_vs_smci.yaml \
  --checkpoint results/checkpoints/pmci_vs_smci_best.pt \
  --method shap
```

## Reproducibility

All experiments used fixed random seeds.

The held-out test set was not used for:

- hyperparameter optimization
- early stopping
- model selection

Task-specific train, validation, and test sample counts are provided in the Supplementary Material.


## License

This repository is released under the MIT License.

This license applies only to the source code and does not apply to the ADNI or AIBL datasets.

## Contact

Taehyeon Yun  
Department of Data Engineering, Pukyong National University  
Email: ytae1014@pukyong.ac.kr
