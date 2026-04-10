# Beyond Algorithmic Novelty: Deployment-Centred Diabetes Detection and Disease Progression Prevention Using Population-Representative Data

![TRIPOD+AI Compliant](https://img.shields.io/badge/TRIPOD%2BAI-Compliant-blue)
![Paper](https://img.shields.io/badge/Paper-[Conference%20Link]-lightgrey)
![Licence](https://img.shields.io/badge/Licence-All%20Rights%20Reserved-red)

## Overview

This is a research companion repository accompanying the paper by Mary Murphy, Dr. Joseph Rafferty, Prof. Jonathan Wallace, and Dr. Roy Harper (Ulster University / South Eastern Health and Social Care Trust, Northern Ireland). It contains supplementary tables covering study demographics, model hyperparameters, performance results, and a feature dictionary, but does not include the pipeline code. The central contribution is the argument that deployment context, not algorithmic novelty, should drive model selection for diabetes screening — evaluated across three realistic settings: mobile screening, GP surgery, and specialist clinic. The methodology follows TRIPOD+AI guidelines throughout.

## Abstract

Diabetes prediction research faces methodological challenges. Many published models lack adequate validation and overlook gestational diabetes mellitus (GDM) history despite its 7.4-fold risk elevation for Type 2 diabetes. Here, cross-sectional detection is examined across three realistic contexts: mobile screening, GP surgeries, and specialist clinics using population-representative NHANES data (2013–2016, n=8,259). Survey-weighted, cost-sensitive methods maintained representativeness, with neglect of design effects shown to almost halve statistical power. Results challenge expectations: traditional machine learning (ML) consistently outperformed deep learning (DL), with gradient boosting identifying female-specific risk patterns missed by linear models and neural networks. AUC values were reasonably high (83.2% mobile, 86.2% GP, 86.6% specialist), yet class imbalance exposed modest precision rates where mobile screening produced 64.8% false positives versus 59.1% in GP surgeries and 52.3% in specialist clinics. GDM features were not retained in optimal models, with metabolic measurements capturing risk information more efficiently. Advanced laboratory panels offered marginal gains over standard tests, suggesting their complexity may be hard to justify outside specialist contexts. The findings support a tiered deployment strategy: matching model and assessment depth to setting rather than chasing algorithmic novelty. Differential risk weighting between females and males suggests unintentional bias, evidence of gender disparities, alongside varying performance across ethnic groups, which warrants further investigation into stratified models.

## Repository Contents

This repository contains four supplementary tables in support of the paper:

- **Demographics table** — survey-weighted participant characteristics stratified by diabetes status, drawn from the analytic sample (n=8,259)
- **Hyperparameter table** — model configurations and imbalance handling for all 14 models evaluated across the three deployment contexts
- **Performance results table** — full model performance across all three deployment contexts, including AUC, sensitivity, specificity, F1, precision, balanced accuracy, and Brier score
- **Feature dictionary** — definitions and descriptions of all features used across the three deployment contexts (mobile screening, GP surgery, specialist clinic), including NHANES variable codes where applicable

The pipeline code is not included in this repository. Enquiries regarding the code or potential collaboration may be directed to the corresponding author.

## Data

The study uses NHANES cycles H (2013–2014) and I (2015–2016), publicly available from the CDC at https://www.cdc.gov/nchs/nhanes/index.htm. Data are not included in this repository and must be accessed directly from the CDC.

Participants were adults aged 18 and over. The final analytic sample comprised 8,259 participants after excluding 2,656 currently pregnant individuals and 1,190 with insufficient diagnostic evidence. Weighted diabetes prevalence was 14.9%, comprising 981 previously diagnosed and 249 undiagnosed cases identified through laboratory confirmation — 33% more than self-report alone would have captured.

## Model Performance Summary

| Context | Best Model | Features | Test AUC (95% CI) | Sensitivity | Specificity | Brier Score |
|---|---|---|---|---|---|---|
| Mobile Screening | CatBoost | 8 | 0.832 (0.806–0.857) | 0.593 | 0.803 | 0.167 |
| GP Surgery | CatBoost | 11 | 0.862 (0.837–0.884) | 0.636 | 0.887 | 0.138 |
| Specialist Clinic | CatBoost | 16 | 0.866 (0.838–0.891) | 0.504 | 0.932 | 0.135 |

Traditional ML consistently outperformed deep learning across all three contexts. CatBoost achieved these results with 8–16 features; deep learning architectures required 29–63 features yet produced lower AUC in every context, trailing by 2.4 to 5.7 percentage points. The narrow performance gap between GP surgery and specialist clinic (0.4 percentage points) suggests that advanced laboratory panels offer marginal gains that may be difficult to justify in routine screening.

## Supplementary Methods

**Imbalance Handling and Context-Adaptive Configuration**

Weighted diabetes prevalence in NHANES was 11.6%, yielding a class ratio of approximately 7.6:1. Rather than applying synthetic oversampling, a cost-sensitive approach was adopted throughout. Four candidate strategies were evaluated under 5-fold survey-weighted cross-validation prior to model training: *lgb_balanced*, *lgb_custom_weights*, *xgb_balanced*, and *rf_balanced*. The xgb_balanced strategy was selected on the basis of consistent AUC performance across all three deployment contexts (mobile: 0.801±0.018; GP: 0.840±0.015; specialist: 0.837±0.020) and applied uniformly to the full model suite.

Logistic Regression regularisation was tightened for mobile screening (C=0.005, l1_ratio=0.8) to account for the sparser feature set and higher overfitting risk; GP surgery received no explicit adjustment and ran on the pipeline default (C=0.01, l1_ratio=0.7); specialist clinic used C=0.01 with l1_ratio=0.6. GP surgery and specialist clinic therefore shared the same regularisation strength, differing only in l1_ratio. In practice, the effect was negligible: Logistic Regression AUC was 0.813 (GP) versus 0.810 (specialist), a difference attributable to feature availability rather than regularisation. Cross-context comparisons for Logistic Regression should be interpreted with this in mind, though the paper's conclusions rest on CatBoost, for which hyperparameters were identical across all three contexts. The Gradient Boosting implementation was selected to handle missing values natively and accept sample weights directly during training, without requiring a separate imputation step.

In the specialist clinic context, PCA was applied to reduce the deep learning feature space from 63 to 10 principal components, retaining 95% of variance, to make the comparison with traditional ML computationally tractable. Traditional ML models used the full 16-feature selected set without dimensionality reduction.

Researchers seeking full hyperparameter specifications may contact the corresponding author.

## Fairness and Subgroup Performance

Male participants showed higher AUC (0.860) and sensitivity (66%) than female participants (AUC 0.778, sensitivity 53%). Among ethnic groups, Non-Hispanic Asian participants showed notably low sensitivity (33%) despite high specificity (96%), suggesting potential under-detection. Mexican American and Non-Hispanic Black participants showed better sensitivity (67% and 68% respectively). Full subgroup results are reported in the paper.

## TRIPOD+AI Compliance

This study follows TRIPOD+AI guidelines for transparent reporting of clinical prediction models using AI/ML methods.

> Collins GS, et al. TRIPOD+AI statement: updated guidance for reporting clinical prediction models that use regression or machine learning methods. *BMJ* 2024;385:e078378.

## Citation

```bibtex
@inproceedings{murphy2026diabetes,
  author    = {Murphy, Mary and Rafferty, Joseph and Wallace, Jonathan and Harper, Roy},
  title     = {Beyond Algorithmic Novelty: Deployment-Centred Diabetes Detection and Disease Progression Prevention Using Population-Representative Data},
  booktitle = {Advanced Information Networking and Applications. AINA 2026},
  series    = {Lecture Notes on Data Engineering and Communications Technologies},
  volume    = {301},
  pages     = {},
  publisher = {Springer Nature Switzerland},
  year      = {2026},
  doi       = {},
  note      = {Tables Repository: [GitHub URL]
}
```

## Licence and Enquiries

Copyright © [Year] Mary Murphy. All Rights Reserved.

The research pipeline and associated code are proprietary and are not included in this repository. They may not be reproduced, adapted, or used for commercial purposes without prior written permission from the author.

Enquiries regarding the research, potential collaboration, or licensing may be directed to: [murphy-m100@ulster.ac.uk]

NHANES data used in this study are publicly available from the CDC (https://www.cdc.gov/nchs/nhanes/index.htm) under their terms of use and are not redistributed here.

## Acknowledgements

Thanks to family, friends, faculty, staff and diabetes peers.
