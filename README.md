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
