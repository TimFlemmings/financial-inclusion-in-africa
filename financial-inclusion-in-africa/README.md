# Predicting Financial Inclusion in Africa

## Project Overview

This project explores financial inclusion in **Kenya, Rwanda, Tanzania, and Uganda** using household survey data and machine learning.

The project has three main objectives:

1. **Predict bank-account ownership** using machine learning models.
2. **Describe the state of financial inclusion** across the four countries.
3. **Identify the characteristics most strongly associated with bank-account ownership** and assess where the model performs well or poorly.

The work was developed as part of a collaborative machine-learning project using the Zindi Financial Inclusion in Africa dataset.

---

## Dataset

The data contains:

- **23,524 labelled respondents** in `Train.csv`
- **10,086 unlabelled respondents** in `Test.csv`
- A binary target variable: `bank_account`
- Demographic, geographic, education, employment, and access-related features

Key explanatory variables include:

- Country
- Survey year
- Rural / urban location
- Age
- Gender
- Household size
- Relationship to household head
- Marital status
- Cellphone access
- Education level
- Employment type

The `uniqueid` field is used only as an identifier and is excluded from model training.

A key feature of the dataset is **class imbalance**: only about **14.1%** of respondents in the training data report having a bank account.

---

## Project Workflow

### 1. Data Understanding and Exploration

We explored:

- Target-class distribution
- Bank-account ownership by country
- Rural vs. urban account ownership
- Account ownership by education level
- Account ownership by employment type
- Data types, missing values, duplicates, and feature distributions

The exploratory analysis showed substantial differences in account ownership across countries and population groups.

### 2. Train / Validation Split

The labelled training data was split into:

- **80% training data**
- **20% held-out validation data**

The split was stratified so that the proportion of account holders remained consistent across the two samples.

The competition `Test.csv` was kept separate and used only for final predictions.

### 3. Preprocessing

The modelling pipeline included:

- Standardisation of numeric variables
- One-hot encoding of categorical variables
- Separation of predictors from the target variable
- Removal of `uniqueid` from model inputs

Preprocessing was included inside the machine-learning pipeline to avoid data leakage during cross-validation.

### 4. Model Comparison

We compared three classification approaches:

- Logistic Regression
- Random Forest
- XGBoost

A Dummy Classifier was also used as a baseline.

Each model was evaluated using the same cross-validation framework and validation data.

### 5. Hyperparameter Tuning

XGBoost achieved the strongest overall performance and was selected for further tuning using `RandomizedSearchCV`.

The tuned model retained strong overall performance while slightly improving recall for account holders.

---

## Final Model Performance

On the held-out validation set, the tuned XGBoost model achieved approximately:

| Metric | Result |
|---|---:|
| Accuracy | **89.1%** |
| Precision | **71.7%** |
| Recall | **37.2%** |
| MAE / overall classification error | **10.9%** |

The model is therefore relatively **precise but conservative**:

- When it predicts that someone has a bank account, it is correct about **72% of the time**.
- However, it identifies only about **37% of all actual account holders** at the default classification threshold.

This means the model is more suitable as a **prioritisation tool** than as an automated exclusion or decision rule.

---

## Error Analysis

The confusion matrix showed that the main weakness of the model is **false negatives**.

On the held-out validation set:

- True Negatives: 3,946
- False Positives: 97
- False Negatives: 416
- True Positives: 246

The model therefore misses a substantial share of real account holders.

Performance also differs across population groups.

Examples include:

- Lower recall for respondents with little or no formal education
- Lower recall for rural respondents than urban respondents
- Lower recall for female respondents than male respondents
- Substantially lower recall in Rwanda than in Kenya, Tanzania, and Uganda

These differences are important when considering how the model could be used operationally.

---

## Model Interpretation

Permutation importance was used to assess which original features contribute most to predictive performance.

The strongest predictive signals were:

1. Education level
2. Employment type
3. Country
4. Cellphone access
5. Age

These should be interpreted as **predictive relationships**, not causal effects.

The descriptive analysis and model interpretation both point to education, employment, geography, and access to technology as important dimensions of financial inclusion in this dataset.

---

## Classification Threshold

The default classification threshold is `0.50`.

Because the model has relatively high precision but low recall, alternative thresholds can be tested to explore the trade-off between:

- Identifying more actual account holders
- Producing more false-positive predictions

The appropriate threshold depends on the intended business or policy use case.

---

## Collaboration Workflow

The project was developed collaboratively.

We initially worked **separately on the data-analysis and machine-learning components**, allowing each person to explore the problem independently and develop their own results.

We then:

1. Shared and compared our findings
2. Reviewed differences in methodology and interpretation
3. Agreed on the strongest analytical and modelling approach
4. Selected the notebook that provided the clearest basis for the final presentation
5. Consolidated the final findings, visuals, recommendations, and model interpretation

This workflow allowed us to combine independent analysis with a shared final decision-making process.

---

## Final Training and Test Predictions

After model selection and validation, the selected pipeline was retrained using **all labelled observations in `Train.csv`**.

The final model was then used to predict `bank_account` for the unlabelled `Test.csv`.

These predictions were formatted into the required submission structure for Zindi.

---

## Key Findings

- Bank-account ownership is low overall, with only around **14%** of respondents reporting an account.
- Ownership varies substantially across countries.
- Education and employment status show some of the strongest relationships with account ownership.
- The final model performs well overall but has limited recall for account holders at the default threshold.
- Model performance is uneven across population groups, which should be considered before any operational use.

---

## Recommendations

### Use the model to prioritise, not automate

Use model predictions to focus attention on higher-probability cases while keeping human judgement in the decision process.

### Align the threshold with the objective

If the priority is to identify more potential account holders, use a lower threshold and accept more false positives. If false positives are costly, use a more conservative threshold.

### Improve before scaling

Strengthen model performance for groups where recall is currently weaker, enrich the available data, and validate performance across countries before wider deployment.

---

## Future Work

Future development should focus on:

- Selecting a classification threshold aligned with the intended use case
- Testing additional strategies for class imbalance
- Adding richer predictive features where available
- Monitoring performance across population subgroups
- Validating the model on newer or external data
- Comparing whether country-specific models improve performance

---

## Repository Notes

The repository should contain the notebooks, data-processing steps, modelling workflow, visual outputs, and final submission files used in the project.

Where possible, keep exploratory work separate from the final presentation notebook so that the final workflow remains reproducible and easy to review.
