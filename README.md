# Mice Protein Expression Classification

## Problem Statement
Down syndrome (modeled in mice via a trisomic genotype) affects learning and memory, and its effects can be studied through protein expression levels in the brain. Given expression measurements for 77 proteins, can a mouse's experimental condition - genotype, treatment, and behavioral context - be predicted from the biology alone, without being told the condition directly?

## What This Project Does
Classifies mice into one of 8 experimental classes using their protein expression profile (`Data_Cortex_Nuclear.csv`, the public UCI "Mice Protein Expression" dataset - 1080 samples, 77 protein markers). Includes data cleaning, correlation-based and model-based feature selection, and a comparison across five classifiers (Random Forest, Logistic Regression, Decision Tree, SVM, k-NN), with hyperparameter tuning on the best-performing model.

## Approach
- **Cleaning:** drops rows with missing values across the 77 protein markers; label-encodes the target and categorical attributes.
- **Feature selection:** correlation-based pruning of highly redundant protein features, followed by Random Forest feature importance ranking to identify the top predictive proteins.
- **Modeling:** trains and compares Random Forest, Logistic Regression, Decision Tree, SVM, and k-NN on a held-out test split, then runs a `GridSearchCV` hyperparameter search on Random Forest.

## A Note on the Feature Set
The dataset's `Genotype`, `Treatment`, and `Behavior` columns are **not predictive signal** - the `class` label is literally defined as their combination (verified directly: every unique Genotype/Treatment/Behavior combination maps to exactly one class value). They're excluded from the model's input features here; including them would let a model trivially decode the label from itself instead of learning anything from the actual protein expression data.

## Tech Stack
Python, pandas, NumPy, scikit-learn, seaborn/matplotlib.

## How to Run
```bash
pip install pandas numpy scikit-learn matplotlib seaborn
```
Open `Datacortex.ipynb` in Jupyter and run all cells - the dataset (`Data_Cortex_Nuclear.csv`) is included in this repo.

## Results
After excluding the label-defining columns, test accuracy: Random Forest 99.1%, Logistic Regression 100%, Decision Tree 82.0%, SVM 100%, k-NN 97.3%, tuned Random Forest 98.2%. High accuracy from protein expression alone is consistent with published research on this dataset - the Decision Tree's comparatively lower score is expected, since a shallow tree generalizes less well than the ensemble/kernel methods here.
