# BB-020 — GK-02 Final Reconstruction

## Objective

Reconstruct a surrogate model of the hidden GK-02 scoring system using observations collected during Rounds 1 and 2, then evaluate it on previously unseen Round-4 queries.

## Data

- Round 1: 73 observations
- Round 2: 17 observations
- Training data for model selection: 90 observations
- Round 4 holdout: 80 observations
- Full available dataset after holdout evaluation: 170 observations

The Round-4 observations were kept unseen during model selection and tuning.

## Features

The reconstruction uses:

- age
- baseline_score
- comorbidity_ratio
- dependants
- prior_visits
- recent_admissions
- requested_beds
- vitals_index
- ward
- years_registered

## Reconstruction and Model Selection

A 5-fold cross-validation model comparison was performed using only the 90 Round-1 and Round-2 observations.

The tested approaches included:

- Extra Trees
- Random Forest
- Gradient Boosting
- HistGradientBoosting
- Hyperparameter variations
- Targeted interaction features

The selected model was the base Extra Trees surrogate:

- ExtraTreesRegressor
- n_estimators = 1000
- random_state = 42
- n_jobs = -1
- one-hot encoding for ward
- base features without additional interaction terms

Cross-validation performance of the selected configuration:

- CV MAE: 0.012187
- CV R²: 0.907177
- CV R² standard deviation: 0.118839

No tested alternative produced a better defensible result on the available training data, so the base Extra Trees model was retained.

## Round-4 Holdout Evaluation

The frozen model was trained only on the 90 Round-1 and Round-2 observations and then evaluated on the 80 previously unseen Round-4 observations.

| Metric | Result |
|---|---:|
| Training observations | 90 |
| Unseen Round-4 observations | 80 |
| MAE | 0.120072 |
| R² | 0.166570 |
| Decision accuracy | 82.50% |

These are the primary unseen-data generalization results.

## Decision Limitation

All 90 Round-1 and Round-2 training observations are APPROVE observations.

Therefore, the available training labels do not contain a DECLINE class from which an exact hidden decision boundary can be learned.

The value 0.6150 is therefore described only as a **surrogate decision cutoff**, corresponding to the lowest observed approved training score.

It is not claimed to be the true hidden-system threshold.

The 82.50% Round-4 decision accuracy should consequently be interpreted with this limitation in mind.

## Final Refit

After the Round-4 holdout evaluation was completed and recorded, the selected Extra Trees surrogate was refit using all 170 available observations.

The full-data refit is a final reconstruction artifact and is not used as evidence of unseen-data generalization.

## Observed Behaviour

Earlier controlled observations indicated:

- age showed a positive relationship with score in tested regions;
- prior_visits showed a positive relationship in tested regions;
- years_registered showed a negative relationship in a tested configuration;
- comorbidity_ratio produced substantial, context-dependent score changes;
- ward produced smaller observed changes in tested configurations.

These are empirical observations of the synthetic GK-02 system and are not claims about real-world hospital admission logic.

## Limitations

The reconstruction is based on a relatively small number of observations compared with the continuous input space.

In particular, the R1+R2 training set does not contain DECLINE examples, limiting what can be inferred about the hidden decision rule.

The Round-4 holdout therefore provides the most important test of score generalization available in this experiment.

## Conclusion

The final BB-020 reconstruction uses an Extra Trees surrogate selected through cross-validation on the 90 earlier observations and evaluated on 80 previously unseen Round-4 observations.

The primary holdout results are:

- MAE = 0.120072
- R² = 0.166570
- Decision accuracy = 82.50%

The model is presented as a behavioural surrogate rather than an exact recovery of the hidden implementation.
