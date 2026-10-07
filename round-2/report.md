# round-2 — Investigate

**Team:** BB-020
**Queries used:** 17

## What we investigated

Round 2 focused on determining whether individual features act independently or whether the effect of one feature changes depending on another feature.

We used controlled comparisons where possible and compared score changes when individual features were varied.

## What we concluded

The experiments confirm that several features have substantial effects on the priority score.

Age has a clear positive effect on the score, with diminishing increases at higher ages.

Prior_visits also has a strong positive effect in the tested region. In the Round 2 experiments, changing prior_visits produced a substantial score change.

Comorbidity_ratio also produced substantial score changes in the tested configurations.

However, the available Round 2 experiments do not yet provide enough clean factorial evidence to confidently claim that any particular pair of features is dependent or interacting.

Therefore, we distinguish between:

- features that clearly affect the score;
- features whose effects may be conditional;
- and relationships that remain unconfirmed.

## Experiments

### Prior_visits

Changing prior_visits while keeping the other intended factors fixed produced a substantial score change. This is consistent with the increasing relationship observed during Round 1.

In one Round 2 sequence, changing prior_visits produced a score change of approximately 0.2109.

### Comorbidity_ratio

Changing comorbidity_ratio in the tested configuration produced a substantial score change. This is consistent with the non-monotonic behaviour observed during Round 1.

### Interaction investigation

We attempted to investigate whether comorbidity_ratio and prior_visits interact.

The intended experiment required changing only these two features while holding the remaining features constant. Some of the recorded queries changed multiple variables simultaneously, so those observations are treated as confounded evidence rather than as proof of an interaction.

Consequently, we do not claim a confirmed comorbidity_ratio × prior_visits interaction.

## What we ruled out

We did not assume that two features are dependent merely because both affect the score.

We also did not treat queries where multiple variables changed simultaneously as clean evidence of pairwise interaction.

## What remains uncertain

- Whether comorbidity_ratio and prior_visits have a genuine interaction.
- Whether age interacts with baseline_score.
- Whether prior_visits interacts with years_registered.
- Whether ward changes the effect of other numerical features.
- Whether requested_beds is globally ignored or only inactive in the tested region.
- Whether additional higher-order interactions exist.

## Overall conclusion

The current evidence establishes several strong individual feature effects but is insufficient to confidently identify the complete interaction structure of the black-box model.

Further controlled 2x2 experiments would be required to distinguish independent effects from genuine feature interactions.
