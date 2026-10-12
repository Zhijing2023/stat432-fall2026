---
name: lda-qda-analysis
description: >-
  Use only when explicitly invoked to compare LDA and QDA
  classifiers and produce a concise statistical report.
---

# LDA/QDA analysis

Ask for the dataset, response, and predictor set if they are not
supplied. Do not assume a particular dataset or variable name. Use
LDA, QDA, or combinations of their predicted class probabilities;
a representation such as PCA is allowed before either classifier.

Reserve a test set before comparing candidates. Within the remaining
training data, use the same stratified cross-validation folds for
every candidate. Fit every transformation, including imputation,
scaling, and PCA, using only the fitting portion of each fold. Apply
its fitted parameters to that fold's validation observations. Select
candidate settings using mean cross-validation misclassification
rate, with a stated tie rule. Never use test results to revise
candidates or settings.

Refit the selected transformation and classifier on all training
observations. Predict the reserved test set once and report its
misclassification rate, number of errors, and sample size. Include
ordinary LDA and QDA as comparison candidates. Check class counts
and covariance rank before fitting QDA; report failures rather than
substituting another classifier silently. Keep the random seed,
folds, and candidate settings reproducible.

Produce a clearly readable analysis report of at most one printed
page. Include a figure comparing validation performance, a compact
table of candidate results, and numerical results for the selected
model on the test set. Briefly state the candidates, validation
design, selected model, main finding, and limitations. Use a
model-based explanation rather than claiming a tiny difference
establishes superiority.
