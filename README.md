# Predicting Lung Cancer Survival: A Comparative Study of Statistical and Machine Learning Methods
This repository contains the code developed for the Master Thesis (MT). 
The objective of the MT was to predict the survival and key risk factors in lung cancer patients and to compare the performance of traditional statistical models for Survival Analysis (Cox and Weibull models) with Machine-Learning based models (Random Survival Forest, Gradient Boosting Survival and Neural Network-based model). 
The comparison was assessed through the Concordance-Index, the Integrated Brier Score and feature importance plots. 

The dataset used for this Thesis is a real dataset from "Hospital Universitari Arnau de Vilanova" (Lleida, Spain), and contains patients diagnosed with lung cancer from 2012 to 2021. Due to privacy concerns, patient data is not shared in this repository. 

This repository contains the following codes:

1. **Preprocessing-MT.ipynb**: Jupyter notebook in Python with the preprocessing steps followed to explore, clean and prepare the data for the Survival Analysis.
2. **survival_analysis-MT.html**: Rmarkdown file containing the R code for the traditional Survival Analysis (kaplan Meier Curves, Cox and Weibull models).
3. **ML_Survival_Analysis-MT.ipynb**: Juputer notebook in Python with the ML-based survival analysis (Random Survival Forest, Gradient Boosting Survival and DeepSurv-based model)

All the libraries, tables and plots can be found inside the code.

## 1. Preprocessing
The original dataset was in Excel format and contained 1,696 lung cancer patients and 27 variables. In summary, the processing steps implemented have been:

- Cleaning and Filtering : Removed irrelevant variables and those with high null values. Excluded patients without confirmed lung cancer as cause of death (based on ICD codes).
- Variable engineering: Obtained relevant columns such as the patient age at diagnosis, the event indicator (death or alive) and the time to the event (death). Censored patients were given the final study date (31/12/2021) as the event date.
- Data validation for consistency.
- Complete Exploratory Data Analysis.

The final dataset contained 1,451 patients and the following curated variables:

- Sex: Male or Female.
- Lung cancer tumor morphology types: Non-small cell carcinoma, Adenocarcinoma, Blastoma, Epidermoid, Small cell carcinoma, Squamous or others.
- Tobacco: Whether the patient smoked or not.
- Age group categories: [0-49], [50-59] , [60-69], [70-79], [80-~]

## 2. Traditional Survival Analysis
Traditional survival analysis techniques were applied to evaluate the overall survival (OS) among lung cancer patients, defined as the time in months from diagnosis to death or censoring. The analysis included the full dataset as well as male- and female-only subsets to explore sex-specific survival trends and confounding effects. 
This thesis explored non-parametric, semi-parametric and parametric survival models, as well as bivariate and multivariate models. 

This part was implemented in R (see *survival_analysis.html*).

This section contains:

- A **bivariate analysis** to indentify crude associations between categorical variables and survival outcomes using risk ratios.
- Overall survival curves and survival curves accross stratified groups obtained with the **Kaplan-Meier estimator**. Statistical significance assessed via the log-rank test.
- Several **Cox Proportional Hazards models**, with and without interaction terms. Cox model provided interpretable hazard ratios and the Proportional Hazards assumption was tested through the Schoenfeld residuals.
- The **Weibull regression model** was used to estimate survival time acceleration factors and capture time-dependent hazard dynamics.

Moreover, traditional models were compared between them using the Akaike Information Criterion (AIC).

## 3. ML-based Survival Analysis
ML models adapted for Survival Analysis were implemented to see if they could outperform traditional models. 
A nested 5-fold cross-validation was used for hyperparameter optimization and evaluation, with the concordance index (C-index)  as the primary performance metric. The final performance was reported as mean ± std of C-index.

Models explored include:

- **Random Survival Forest** (RSF)
- **Gradient Boosting Survival Analysis** (GBSA)
- **DeepSurv** based Neural Network


## Comparison
Traditional and ML-based models were compared using the following metrics:

- **Concordance Index** (C-index): Measures how well the model ranks patient survival times by comparing predicted risk scores with actual event order. Computed via 5-fold cross-validation for all models.
- **Integrated Brier Score** (IBS): Evaluates calibration and discrimination over time by averaging the squared differences between predicted survival probabilities and actual outcomes across a time grid. Assessed using boostraping methods and integrating over a follow-up period up to the 90th percentile of observed survival times.

In addition to these performance metrics, **feature importance** was assessed. In this case, Cox and Weibull used model coefficients, while ML models relied on permutation importance to measure the impact of each variable. DeepSurv, being a neural model, lacked direct interpretability.


## Key Results
- Sex is a significant predictor, with females observing a lower mortality risk and longer survival times.
- The risk of death increases with age, especially ≥80 years.
- Regarding tumor morphology, Blastoma and Small Cell Carcinoma (SCLC) are linked to the highest mortality, while Adenocarcinoma shows the highest survival.
- Smoking status is not significantly associated with survival in this particular case (highly due to misclassification bias, missing confounders, or post-diagnosis effects).
- Weibull model indicates decreasing hazard over time, which is aligned with the survival curves. There is high mortality at the begining, but survivors then tend to live longer. 

Comparison results are summarized in the following table:
![image](https://github.com/user-attachments/assets/84802f6c-0f3e-4a32-a5d9-6a59de09409a)


## Conclusions
It has been observed that there is no major differences between the performance of traditional models and ML models in this dataset. It is true that ML models stand out when there is high-dimensional data with a lot of predictors and non-linear complex relations. Therefore, the size of the present dataset may have hidden the power of ML models for survival. 

As a rule of thumb, for a given survival dataset with a small number of predictors, traditional models such as Cox should be considered first, as they are more interpretable, computationally faster and have a wide clinical acceptance.


