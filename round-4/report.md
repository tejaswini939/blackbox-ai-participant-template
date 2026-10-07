# Reconstruction Report: GK-02 Hidden System
### Team: BB-020

## 1. Executive Summary
We have completed the reconstruction of the synthetic GK-02 scoring logic. By isolating the holdout Round 4 dataset (80 queries) and training exclusively on the combined Round 1 + Round 2 dataset (90 queries), we maintained strict competition integrity.

## 2. Model Selection (5-Fold Cross-Validation on 90 R1+R2 observations)
- **Best Model**: Ridge Regression (Base)
- **CV MAE**: 0.022749
- **CV R²**: 0.782017 ± 0.223254

## 3. Performance on Unseen Round-4 Holdout
- **Mean Absolute Error (MAE)**: 0.096965
- **R² Score**: 0.482976
- **Decision Accuracy**: 93.75%

## 4. Decision Limit Caveat
- **Surrogate decision cutoff**: 0.6150
- **Note**: Since the training data (R1+R2) contains exactly zero DECLINE observations, the exact true decision boundary cannot be mathematically learned from the training labels. The value 0.6150 serves strictly as a 'Surrogate decision cutoff' representing the lowest observed approved training score.

## 5. Final Refit
- **Status**: Trained on all 170 observations strictly AFTER holdout validation was documented.
