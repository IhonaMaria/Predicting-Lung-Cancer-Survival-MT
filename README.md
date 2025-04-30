# Predicting Lung Cancer Survival: A Comparative Study of Statistical and Machine Learning Methods
This repository contains the code developed for the Master Thesis (MT). 
The objective of the MT was to predict the survival and key risk factors in lung cancer patients and to compare the performance of traditional statistical models for Survival Analysis (Cox and Weibull models) with Machine-Learning based models (Random Survival Forest, Gradient Boosting Survival and Neural Network-based model). 
The comparison was assessed through the Concordance-Index, the Integrated Brier Score and feature importance plots. 

The dataset used for this Thesis is a real dataset from "Hospital Universitari Arnau de Vilanova" (Lleida, Spain), and contains patients diagnosed with lung cancer from 2012 to 2021. Due to privacy concerns, patient data is not shared in this repository. 

This repository contains the following codes:

- Preprocessing.ipynb : Jupyter notebook with the preprocessing steps followed to explore, clean and prepare the data for the Survival Analysis.
- survival_analysis.html : Rmarkdown file containing the R code for the traditional Survival Analysis (kaplan Meier Curves, Cox and Weibull models).
- ML_Survival_Analysis.ipynb : Juputer notebook with the ML-based survival analysis (Random Survival Forest, Gradient Boosting Survival and DeepSurv-based model)

All the libraties, tables and plots can be found inside the code.

## Preprocessing
The original dataset was in Excel format and contained 1,696 lung cancer patients and 27 variables. In summary, the processing steps implemented have been:

- Cleaning and Filtering : Removed irrelevant variables and those with high null values. Excluded patients without confirmed lung cancer as cause of death (based on ICD codes).
- Variable engineering to obtain relevant columns such as the patient age at diagnosis, the event indicator (death or alive) and the time to the event (death). Censored patients were given the final study date (31/12/2021).
- Data validation for consistency.
- Complete Exploratory Data Analysis.

The final dataset contained 1,451 patients and the following curated variables:

- Sex: Male or Female.
- Tumor morphology: Non-small cell carcinoma, Adenocarcinoma, Blastoma, Epidermoid, Small cell carcinoma, Squamous or others.
- Tobacco: Yes or No.
- Age group: [0-49], [50-59] , [60-69], [70-79], [80-~]

## Traditional Survival Analysis
Traditional survival analysis techniques were applied to evaluate overall survival (OS) among lung cancer patients, defined as the time in months from diagnosis to death or censoring. The analysis included the full dataset as well as male- and female-only subsets to explore sex-specific survival trends and confounding effects. 
This thesis explored non-parametric, semi-parametric and parametric survival models, as well as bivariate and multivariate models. 

This part was all implemented in R (see survival_analysis.html).

This section contains:

- A **bivariate analysis** to indentify crude associations between categorical variables and survival outcomes using risk ratios.
- Overall survival curves and survival curves accross stratified groups obtained with the **Kaplan-Meier estimator**. Statistical significance assessed via the log-rank test.
- Several **Cox Proportional Hazards models**, with and without interaction terms. Cox model provided interpretable hazard ratios. Proportional Hazards assumption was tested through the Schoenfeld residuals.
- The **Weibull regression model** was used to estimate survival time acceleration factors and capture time-dependent hazard dynamics.

Moreover, traditional models were compared between them using the AIC. 

## ML-based Survival Analysis

ML models adapted for Survival Analysis were developed to see if they could outperform traditional ones. 
A nested 5-fold cross-validation was used for hyperparameter optimization (for the tree-based models) and evaluation, with the concordance index  as the primary performance metric. The final performance was reported as mean ± std of C-index

Models explored include:

- **Random Survival Forest** (RSF)
- **Gradient Boosting Survival Analysis** (GBSA)
- **DeepSurv** based Neural Network


## Comparison

Traditional and ML-based models were compared using the following metrics:

- **Concordance Index** (C-index): Measures how well the model ranks patient survival times. Computed via 5-fold cross-validation for all models.
- **Integrated Brier Score** (IBS): Evaluates calibration + discrimination over time. Assessed using boostraping methods and integrating over a time grid.

In addition to these performance metrics, **feature importance** was assessed: Cox and Weibull used model coefficients, while ML models relied on permutation importance to measure the impact of each variable. DeepSurv, being a neural model, lacked direct interpretability.


## Results
