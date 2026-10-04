# Stacked machine learning models for asthma risk prediction from genotype and local ancestry

Python code for the paper "Machine learning models incorporating genotype and ancestry improve severe asthma risk prediction", Scientific Reports 15, 40243 (2025). https://doi.org/10.1038/s41598-025-24080-x

## Overview

This repository contains an end-to-end genomics machine learning pipeline. It runs genotype quality control in PLINK, trains separate classifiers on SNP genotype data and on local ancestry data, and combines them with a stacked ensemble.

Each data type has two pipelines. P1 is a Lasso model. P2 combines feature selection (recursive feature elimination for SNPs, Elastic Net for local ancestry) with a random forest classifier. P3 averages P1 and P2, and the final model is a weighted ensemble of the SNP and ancestry predictions.

All models share the same stratified 10-fold cross-validation, so out-of-fold predictions line up across pipelines with no data leakage between training and test folds. Feature importance is measured with SHAP, and the models are evaluated on ROC-AUC, precision, recall, specificity and balanced accuracy.

## Methods used

Feature selection: recursive feature elimination (RFE), Lasso and Elastic Net regularization.
Models: random forest, Lasso regression, stacked ensemble learning.
Evaluation: stratified k-fold cross-validation, ROC-AUC, balanced accuracy, paired t-tests.
Interpretability: SHAP values for tree-based models.
Genomic data processing: PLINK quality control (MAF and HWE filtering), linkage disequilibrium (LD) pruning, local ancestry inference outputs.

## Files

```
preprocessing/plink_commands.txt               genotype QC and LD pruning (PLINK 1.9)
notebooks/01_snp_pipelines.ipynb               SNP models (P1_SNP, P2_SNP)
notebooks/02_local_ancestry_pipelines.ipynb    local ancestry models (P1_LA, P2_LA)
notebooks/03_stacking.ipynb                    stacked ensemble and statistical testing
```

## Running the code

1. Run the PLINK commands to filter and LD-prune the VCF.
2. Load three objects into the notebooks: all_data_only (samples by SNPs, indexed by sample ID), y (0/1 labels in the same row order) and ancs_data (local ancestry, positions by samples).
3. Run notebooks 01, 02 and 03 in order. Notebooks 01 and 02 save out-of-fold predictions to pred_probs/, which notebook 03 reads to build the ensemble. Per-fold metrics are written to results/.

## Requirements

Python 3.10, scikit-learn, pandas, NumPy, SciPy, SHAP, imbalanced-learn, Matplotlib and PLINK 1.9.

## Data

No data is included. The genotype and clinical data are under controlled access.
