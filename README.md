# Mice Protein Expression: Probabilistic Classification of Down Syndrome Genotype

**Statistical Learning — Master in Statistics for Data Science**

## Overview

This project builds a classification pipeline to distinguish trisomic (Down Syndrome) from control mice based on the mices protein expression profiles. Using probabilistic classifiers like **Logistic Regression**, **Gaussian Naive Bayes**, **LDA**, and **QDA** we identify which of 77 measured proteins carry the strongest biological signal for genotype classification.

Link to the dataset: [Mice Protein Expression&](https://archive.ics.uci.edu/dataset/342/mice+protein+expression)

## Methodological Highlights

**Leakage-safe preprocessing.** All imputation and scaling parameters are derived exclusively from the training set — including a biologically-informed imputation strategy that computes missing protein values from genotype × treatment subgroup medians, rather than naive global averages.

**Principled missing-data handling.** Features with >5% missingness are dropped entirely rather than imputed, based on the reasoning that high missingness in proteomic data likely reflects *Missing Not at Random* (MNAR) detection-limit effects — imputing these would silently bias results.

**Multicollinearity control.** Highly correlated proteins (r > 0.70) are pruned using `findCorrelation`, empirically justified by QDA's numerical instability above this threshold — directly motivated by covariance matrix invertibility requirements.

**Stability-aware feature selection.** A 5-fold cross-validated ANOVA-based feature selection pipeline (`SelectKBest`) is wrapped in a *stability selection* scheme: features are ranked by how consistently they're selected across folds, not just by a single split — reducing selection bias. We additionally map QDA's empirical breakdown point (rank deficiency beyond ~15 features) to pin down a safe, optimal K = 9.

**Effect size over raw differences.** Beyond p-values, Cohen's *d* is used to quantify biological relevance of each protein's discriminative power — surfacing APP_N as the standout marker (d ≈ 1.21).

## Exploratory Findings

- Balanced target classes (52.8% / 47.2%) — accuracy is a trustworthy metric here
- Reasonable normality of top discriminative features (validated via Q-Q plots), supporting Gaussian model assumptions
- PCA reveals substantial overlap between classes in low dimensions (10–17 PCs needed for 80–90% variance) — justifying the use of multivariate probabilistic models over simple linear separators

## What's Next

The second half extends this pipeline into full model training, 10-fold robust cross-validation, ROC/AUC comparison, and medically-motivated threshold tuning — with QDA emerging as the top performer (Accuracy 88.3%, AUC-leading ROC curve).
```
