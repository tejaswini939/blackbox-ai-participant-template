# BB-020 — GK-02 Round 4 Reconstruction

## 1. Objective

The objective of Round 4 was to reconstruct a surrogate model of the hidden GK-02 scoring system using observations collected during Rounds 1 and 2, and then evaluate how well the reconstruction generalizes to the 80 Round-4 queries.

## 2. Data

The reconstruction training set contains the queries collected during the previous rounds:

- Round 1: 73 observations
- Round 2: 17 observations
- Total training observations: 90

The Round-4 evaluation set contains 80 observations.

The 80 Round-4 observations were kept out of model training for the primary holdout evaluation.

## 3. Features

The model uses the following GK-02 input features:

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

The target variable is the observed GK-02 priority score.

## 4. Reconstruction Approach

We investigated the observed GK-02 behaviour through controlled queries and then trained a regression-based surrogate model.

The final surrogate used for the Round-4 holdout evaluation was an Extra Trees Regressor with:

- 1000 trees
- random_state = 42
- one-hot encoding for the categorical `ward` feature
- all ten observed input features

The model was trained only on the 90 Round-1 and Round-2 observations before the Round-4 evaluation.

## 5. Observed GK-02 Behaviour

Our observations suggested the following relationships:

1. Age showed a positive relationship with score in the tested regions.
2. Prior visits showed a positive relationship with score in tested regions.
3. Years registered showed a negative relationship in the tested configurations.
4. Comorbidity ratio produced a substantial and context-dependent score change.
5. Ward produced a smaller observed effect in the tested configurations.
6. Some Round-2 experiments changed more than one input simultaneously, so interaction claims were treated cautiously.

These are empirical observations from the synthetic GK-02 system and are not claims about real-world hospital admission logic.

## 6. Round-4 Holdout Evaluation

The model trained on the 90 Round-1 and Round-2 observations was evaluated against the 80 Round-4 observations before those observations were added to the final full-data reconstruction.

### Results

| Metric | Result |
|---|---:|
| Round-4 observations | 80 |
| MAE | 0.1193 |
| R² | 0.1713 |
| Decision accuracy | 82.5% |

The highest observed DECLINE score was 0.4522 and the lowest observed APPROVE score was 0.4937.

The midpoint between these observations was approximately 0.4729 and was used as the surrogate decision cutoff.

## 7. Final Reconstruction

After completing the Round-4 holdout evaluation, the Round-4 observations were combined with the previous observations to form the complete available dataset:

- Round 1: 73
- Round 2: 17
- Round 4: 80
- Total: 170 observations

A final Extra Trees surrogate was then trained using all 170 observations.

The full-data training fit is reported separately from the Round-4 holdout evaluation and is not presented as evidence of generalization.

## 8. Limitations

The reconstruction is based on a relatively small number of observed queries compared with the continuous input space.

The Round-4 holdout results therefore provide a more meaningful indication of generalization than training-fit metrics.

The Extra Trees model should be interpreted as a behavioural surrogate of GK-02 rather than an exact recovery of the hidden implementation or model architecture.

## 9. Conclusion

Using the observations collected during Rounds 1 and 2, we reconstructed a surrogate model of GK-02 and evaluated it on 80 previously unseen Round-4 queries.

The reconstruction achieved:

- 82.5% decision accuracy
- 0.1193 MAE
- 0.1713 R²

The results show that the observed GK-02 behaviour can be approximated from the controlled queries collected during the earlier rounds, while also highlighting regions where additional observations would be needed for a more accurate reconstruction.
