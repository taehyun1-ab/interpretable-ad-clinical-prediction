# interpretable-ad-clinical-prediction

Code for interpretable clinical data mining for Alzheimer's disease staging and MCI progression prediction.

## Overview

This repository contains the experimental code for the study:

**Interpretable Clinical Data Mining for Alzheimer's Disease Staging and MCI Progression Prediction**

The proposed framework uses baseline clinical variables from ADNI to perform Alzheimer's disease staging and MCI progression-risk prediction. The model combines:

- feature-wise tokenization
- token-wise supervised autoencoder bottleneck
- Transformer-based feature interaction modeling
- feature-level interpretation

## Prediction Tasks

The model was evaluated on four binary classification tasks:

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
├── LICENSE
├── .gitignore
├── data/
│   └── README_data.md
├── notebooks/
│   ├── 01_preprocessing.ipynb
│   ├── 02_model_training.ipynb
│   ├── 03_internal_validation.ipynb
│   ├── 04_external_validation.ipynb
│   ├── 05_interpretability.ipynb
│   └── 06_supplementary_analysis.ipynb
└── results/
    └── README_results.md
```

## Data Availability

The raw clinical data used in this study were obtained from:

- Alzheimer's Disease Neuroimaging Initiative (ADNI)
- Australian Imaging, Biomarkers and Lifestyle Study of Ageing (AIBL)

Due to data use agreements, raw ADNI and AIBL data are not distributed with this repository. Users must request access from the official ADNI and AIBL data portals.

This repository provides the analysis code, preprocessing workflow, model training procedure, validation scripts, and interpretation notebooks.

## Input Data Format

The expected input is a preprocessed or harmonized clinical dataframe containing participant identifiers, diagnostic labels, and baseline clinical variables.

Example columns:

```text
RID, VISCODE, DIAGNOSIS, DIAGNOSIS2, AGE, PTGENDER, PTEDUCAT, APOE4COUNT, MMSCORE, ADNI_MEM, ADNI_EF, FAQFORM, FAQSHOP, ...
```

For the primary ADNI experiments, baseline clinical variables were used.

For AIBL external validation, a reduced shared-variable setting was used because not all ADNI variables were consistently available in AIBL. The shared-variable model used:

- APOE ε4 count
- MMSE score
- age
- CDR global score

## Notebook Workflow

Open and run the notebooks in order.

### 1. Data preprocessing

```text
notebooks/01_preprocessing.ipynb
```

This notebook prepares task-specific train/test files from the baseline ADNI clinical dataframe.

Main steps:

- load the baseline clinical dataframe
- define binary classification tasks
- create a shared stratified train/test split
- construct task-specific labels
- encode categorical variables using training-derived mappings
- convert numeric variables and handle missing values
- save processed train/test files for each task
- save task-wise sample count summary

Expected outputs:

```text
data/processed/AD_vs_CN_train.csv
data/processed/AD_vs_CN_test.csv
data/processed/AD_vs_MCI_train.csv
data/processed/AD_vs_MCI_test.csv
data/processed/MCI_vs_CN_train.csv
data/processed/MCI_vs_CN_test.csv
data/processed/pMCI_vs_sMCI_train.csv
data/processed/pMCI_vs_sMCI_test.csv
data/splits/task_split_summary.csv
```

### 2. Model training

```text
notebooks/02_model_training.ipynb
```

This notebook trains the proposed token-wise supervised autoencoder with Transformer-based feature interaction modeling.

Main steps:

- load processed task-specific train/test files
- define model hyperparameters
- define Dataset and DataLoader
- define feature-wise tokenizer
- define token-wise autoencoder bottleneck
- define Transformer encoder and classification head
- train the model using an internal validation split
- apply early stopping based on validation AUC
- save trained model checkpoints

The selected hyperparameter configuration was obtained from the pMCI vs. sMCI task and reused for the remaining binary classification tasks.

### 3. Internal validation

```text
notebooks/03_internal_validation.ipynb
```

This notebook evaluates the trained models on the held-out ADNI test set.

Reported metrics include:

- AUC
- accuracy
- precision
- sensitivity
- specificity
- F1-score

Expected outputs:

```text
results/internal_test_results.csv
results/internal_roc_curves.png
```

### 4. External validation

```text
notebooks/04_external_validation.ipynb
```

This notebook performs external validation on AIBL using shared clinical variables available in both ADNI and AIBL.

AIBL samples were not used for:

- training
- validation
- hyperparameter tuning
- early stopping
- model selection

Expected output:

```text
results/external_aibl_results.csv
```

### 5. Interpretability analysis

```text
notebooks/05_interpretability.ipynb
```

This notebook performs feature-level interpretation analyses.

Main analyses:

- feature ablation
- permutation analysis
- SHAP global explanation
- SHAP local explanation
- attention heatmap visualization

Expected outputs:

```text
results/feature_ablation_results.csv
results/permutation_results.csv
results/shap_summary_pmci_vs_smci.png
results/shap_local_examples.png
results/attention_heatmaps.png
```

### 6. Supplementary analysis

```text
notebooks/06_supplementary_analysis.ipynb
```

This notebook generates additional analyses used in the Supplementary Material.

Main analyses:

- threshold sensitivity analysis
- random seed robustness analysis
- t-SNE latent-space visualization
- UMAP latent-space visualization
- extended comparison tables

Expected outputs:

```text
results/threshold_sensitivity.csv
results/random_seed_robustness.csv
results/tsne_latent_space.png
results/umap_latent_space.png
```

## Reproducibility

All experiments used fixed random seeds.

The held-out test set was not used for:

- hyperparameter optimization
- early stopping
- model selection

Categorical mappings were derived only from the training subset. Continuous variables were standardized using training-derived statistics and then applied to validation and test sets.

Task-specific train, validation, and test sample counts are reported in the Supplementary Material.

## License

This repository is released under the MIT License. See the `LICENSE` file for details.

This license applies only to the source code and does not apply to the ADNI or AIBL datasets.

## Contact

Taehyeon Yun  
Department of Data Engineering, Pukyong National University  
Email: ytae1014@pukyong.ac.kr
