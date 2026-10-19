---
name: lda-qda-classification-analysis
description: Analyze a tabular classification dataset using LDA, QDA, and PCA variants with cross-validation and a reserved test set.
---

# SKILL: LDA/QDA Classification Analysis

## Invocation
Load this skill only when explicitly requested with a phrase such as
"apply my LDA/QDA skill" or "invoke the LDA/QDA skill." Do not load
automatically. The user supplies only the dataset file path at invocation
time. The agent inspects the data and determines the response variable
and predictors independently before running the analysis.

---

## Step 1: Data Exploration

Before any modeling, read the dataset and report:

1. **Dimensions**: number of rows and columns.
2. **Variable types**: identify which columns are numeric, binary, or
   categorical (character/factor).
3. **Response variable**: identify the most likely classification target
   — typically a binary or low-cardinality integer/factor column. If
   ambiguous, choose the column that appears last or is most clearly
   a label. State your choice and the reason.
4. **Predictors**: use all remaining columns as predictors. Convert any
   character or factor columns to numeric dummy variables (0/1). Note
   which columns were converted and how.
5. **Class distribution**: report the count and proportion of each class.
   Note any imbalance.
6. **Summary statistics**: report mean and standard deviation of each
   numeric predictor.

All decisions made in this step must be stated clearly before any
modeling begins.

---

## Step 2: Preprocessing

1. Dummy-code all character or factor predictors using training-fold
   data only; apply the same encoding to validation and test folds.
2. Standardize all predictors (zero mean, unit variance) using
   training-fold statistics. Apply the same scaling to validation/test
   folds.
3. For PCA variants, retain enough principal components to explain at
   least 90% of training-fold variance. Record the number of components
   kept for each fold.

---

## Step 3: Candidate Models

Fit all four candidates and compare via cross-validation:

| ID | Model |
|----|-------|
| M1 | LDA on original predictors |
| M2 | QDA on original predictors |
| M3 | PCA → LDA (≥90% variance retained) |
| M4 | PCA → QDA (≥90% variance retained) |

All preprocessing is estimated inside each CV fold using training
observations only.

---

## Step 4: Data Split

- Use the random seed supplied at invocation. If none is supplied,
  use seed 42 and state this.
- Hold out 20% of observations as a **test set** before any modeling.
- Use the remaining 80% as the **training set** for all CV and model
  selection steps.
- Never use the test set until the final evaluation of the selected model.

---

## Step 5: Cross-Validation Procedure

- Apply **5-fold cross-validation** on the training set.
- Use the **same fold assignments** for all four candidates.
- Metric: **misclassification rate** (proportion misclassified in each
  held-out fold).
- Report mean CV misclassification rate ± standard deviation across
  folds for each candidate.
- Select the candidate with the lowest mean CV misclassification rate.

  Break ties by preferring the simpler model (M1 > M3 > M2 > M4).

---

## Step 6: Final Model and Test Evaluation

- Refit the selected model (and all preprocessing) on all training
  observations.
- Evaluate the fitted model **once** on the reserved test set.
- Report the test misclassification rate as the estimated prediction
  error. Do not revisit or re-tune after seeing the test result.

---

## Step 7: Required Report (≤ 1 printed page)

Produce a concise report with exactly the following sections:

### 1. Data and Candidate Models
One short paragraph describing the dataset (dimensions, response,
predictors, any coding applied), the four candidate models, and the
CV strategy (folds, metric, seed).

### 2. Cross-Validation Results (Table)
A table with columns: Model, Mean CV Error, SD CV Error.
Bold or mark the selected model's row.

### 3. CV Error Comparison (Figure)
A bar chart or dot plot showing mean CV misclassification rate for each
candidate, with error bars (±1 SD). Label axes clearly. Include the
random seed in the caption.

### 4. Selected Model and Test Performance
One paragraph stating which model was selected, why (CV results), and
the test misclassification rate. Include the confusion matrix on the
test set.

### 5. Findings and Limitations
Two to four sentences interpreting the result: which classes are hardest
to separate, whether PCA helped or hurt, and any notable limitations
(sample size, class imbalance, linearity assumption).

---

## Constraints

- Use only LDA, QDA, or their combination with PCA.
- No variable names from any specific dataset may appear in this skill.
- The report must fit on one printed page; the confusion matrix may be
  compact (numbers only).
- State the random seed explicitly in the report.
- The skill is dataset-agnostic: it applies to any tabular classification
  dataset, not only the dataset used at time of writing.