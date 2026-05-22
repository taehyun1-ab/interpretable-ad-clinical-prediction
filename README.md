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
│   ├── 02_model_training_and_internal_validation.ipynb
│   ├── 03_external_validation.ipynb
│   ├── 04_interpretability.ipynb
│   └── 05_supplementary_analysis.ipynb
└── results/
    └── README_results.md
```

## Data Availability

The raw clinical data used in this study were obtained from:

- Alzheimer's Disease Neuroimaging Initiative (ADNI)
- Australian Imaging, Biomarkers and Lifestyle Study of Ageing (AIBL)

Due to data use agreements, raw ADNI and AIBL data are not distributed with this repository. Users must request access from the official ADNI and AIBL data portals.

This repository provides the analysis code, preprocessing workflow, model training and validation procedures, interpretation analysis, and supplementary analysis notebooks.

## Input Data Format

The expected input is a harmonized baseline clinical dataframe containing participant identifiers, diagnostic labels, and baseline clinical variables.

Example columns:

```text
RID, VISCODE, DIAGNOSIS, DIAGNOSIS2, AGE, PTGENDER, PTEDUCAT, APOE4COUNT, MMSCORE, ADNI_MEM, ADNI_EF, FAQFORM, FAQSHOP, ...
```

For the primary ADNI experiments, baseline clinical variables were used. The full variable list is provided in Supplementary Table S1.

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

### 2. Model training and internal validation

```text
notebooks/02_model_training_and_internal_validation.ipynb
```

This notebook trains the proposed token-wise supervised autoencoder with Transformer-based feature interaction modeling and evaluates the selected model on the held-out ADNI test set.

Main steps:

- load processed task-specific train/test files
- define model hyperparameters
- define Dataset and DataLoader
- define feature-wise tokenizer
- define token-wise autoencoder bottleneck
- define Transformer encoder and classification head
- split the training data into inner-training and validation subsets
- apply z-score standardization fitted only on the inner-training subset
- train the model using classification and reconstruction losses
- apply early stopping based on validation AUC
- evaluate the selected model on the held-out ADNI test set
- save internal validation results and ROC curves

The selected hyperparameter configuration was obtained from the pMCI vs. sMCI task and reused for the remaining binary classification tasks.

Expected outputs:

```text
results/internal_test_results.csv
results/training_history.csv
results/figures/internal_roc_curves.png
results/checkpoints/AD_vs_CN_best.pt
results/checkpoints/AD_vs_MCI_best.pt
results/checkpoints/MCI_vs_CN_best.pt
results/checkpoints/pMCI_vs_sMCI_best.pt
```

### 3. External validation

```text
notebooks/03_external_validation.ipynb
```

This notebook performs external validation on AIBL using shared clinical variables available in both ADNI and AIBL.

The shared-variable model is trained using ADNI only and evaluated on the AIBL external cohort.

AIBL samples were not used for:

- training
- validation
- hyperparameter tuning
- early stopping
- model selection

Expected outputs:

```text
results/external_aibl_results.csv
results/shared_variable_validation_summary.csv
```

### 4. Interpretability analysis

```text
notebooks/04_interpretability.ipynb
```

This notebook performs SHAP-based interpretation analysis for the pMCI vs. sMCI task.

Main analyses:

- SHAP global explanation
- SHAP local/waterfall explanation for a correctly classified sample
- SHAP local/waterfall explanation for a misclassified sample

Expected outputs:

```text
results/shap_global_importance_pMCI_vs_sMCI.csv
results/shap_local_examples_pMCI_vs_sMCI.csv
results/figures/shap_summary_pMCI_vs_sMCI.png
results/figures/shap_waterfall_correct_pMCI_vs_sMCI.png
results/figures/shap_waterfall_incorrect_pMCI_vs_sMCI.png
```

### 5. Supplementary analysis

```text
notebooks/05_supplementary_analysis.ipynb
```

This notebook generates additional analyses used in the Supplementary Material.

Main analyses:

- threshold sensitivity analysis
- random seed robustness analysis
- feature ablation analysis
- permutation analysis
- t-SNE latent-space visualization
- UMAP latent-space visualization
- attention heatmap visualization, if attention weights are available

Expected outputs:

```text
results/supplementary/supplementary_table_s3_threshold_sensitivity.csv
results/supplementary/supplementary_table_s4_seed_robustness.csv
results/supplementary/supplementary_table_s6_feature_ablation.csv
results/supplementary/supplementary_table_s7_permutation.csv
results/supplementary/figures/tsne_AD_vs_CN.png
results/supplementary/figures/tsne_AD_vs_MCI.png
results/supplementary/figures/tsne_MCI_vs_CN.png
results/supplementary/figures/tsne_pMCI_vs_sMCI.png
results/supplementary/figures/umap_AD_vs_CN.png
results/supplementary/figures/umap_AD_vs_MCI.png
results/supplementary/figures/umap_MCI_vs_CN.png
results/supplementary/figures/umap_pMCI_vs_sMCI.png
```

## Reproducibility

All experiments used fixed random seeds.

The held-out test set was not used for:

- hyperparameter optimization
- early stopping
- model selection

Categorical mappings were derived only from the training subset. Continuous variables were standardized using training-derived statistics and then applied to validation and test sets.

AIBL samples were used only for external validation and were not used during training, validation, hyperparameter tuning, early stopping, or model selection.

Task-specific train, validation, and test sample counts are reported in the Supplementary Material.

## Notes on Data and Results

Raw ADNI and AIBL data are excluded from this repository. Generated processed files, model checkpoints, and result files are also excluded from version control to avoid sharing restricted data-derived files.

The following folders are expected to be generated locally after running the notebooks:

```text
data/processed/
data/splits/
results/checkpoints/
results/figures/
results/supplementary/
```

## License

This repository is released under the MIT License. See the `LICENSE` file for details.

This license applies only to the source code and does not apply to the ADNI or AIBL datasets.

## Contact

Taehyeon Yun  
Department of Data Engineering, Pukyong National University  
Email: ytae1014@pukyong.ac.kr
