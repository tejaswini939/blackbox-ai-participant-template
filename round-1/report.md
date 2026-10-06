# round-1 — Observe

**Team:** BB-020
**Queries used:** 73

## What we concluded

The BLACKBOX system is a synthetic hospital-admission triage service that maps patient-record inputs to a priority score between 0 and 1 and an APPROVE/DECLINE decision.

Our experiments show that several inputs affect the score and that their effects are not all simple or monotonic.

The clearest observations were:

- Age increases the priority score, with a smaller increase at higher ages.
- Years_registered has a strong negative effect in the tested region.
- Prior_visits increases the score, with possible saturation.
- Requested_beds appears to have a small positive effect.
- Baseline_score does not show a simple monotonic relationship in the observed experiments.

We do not have enough evidence to identify the exact model architecture, mathematical formula, decision threshold, or all feature interactions.

## How we got there

We first investigated age by changing it across several values while keeping the remaining inputs approximately fixed.

The observed scores were:

- Age 46.215 → 0.6595
- Age 53.625 → 0.6986
- Age 64.740 → 0.7599
- Age 72.150 → 0.7677
- Age 75.000 → 0.7714

The score increased consistently with age, while the size of the increase became smaller at higher ages. This suggests a positive effect with saturation.

We then investigated years_registered. Reducing years_registered from 40 to 0 produced an observed score increase of approximately 0.1015 in a high-scoring configuration. This was one of the strongest effects observed.

Prior_visits was also varied. In one configuration, approximately 4.6, 17.8 and 20 prior visits produced scores of 0.9201, 0.9593 and 0.9599 respectively. This suggests an increasing effect with diminishing change near the upper end.

An experiment involving requested_beds showed that reducing the value to 1 produced an approximately 0.0186 decrease in score. Therefore, in the tested region, requested_beds appears to have a small positive effect.

Baseline_score was explored across a broad range. The resulting scores did not consistently increase or decrease with baseline_score, so we did not treat it as a simple monotonic feature.

A high-scoring configuration reached approximately 0.9599 with age 75, baseline_score 900, comorbidity_ratio 0, dependants 6, prior_visits 20, recent_admissions 0, requested_beds 0, vitals_index 100, ward A and years_registered 0.

## What we ruled out

### Higher baseline_score always means higher priority

The observed baseline_score experiments did not support a simple monotonic relationship.

### Higher comorbidity_ratio necessarily means higher priority

A high-scoring configuration used comorbidity_ratio = 0, so the available evidence does not support treating higher comorbidity_ratio as automatically beneficial.

### requested_beds is necessarily a penalty

The observed experiment showed a small decrease when requested_beds was reduced to 1, indicating that the feature can have a positive effect in the tested region.

### The exact model family can already be identified

The available observations are insufficient to distinguish confidently between possible model families.

## What we are still unsure about

- The exact APPROVE/DECLINE decision threshold.
- The exact mathematical relationship for each feature.
- Whether baseline_score is truly non-monotonic or whether interactions contribute to the observed variation.
- The exact effect of comorbidity_ratio.
- The isolated effects of vitals_index, ward, recent_admissions and dependants.
- Whether important interactions exist between features.
- The exact model architecture.
