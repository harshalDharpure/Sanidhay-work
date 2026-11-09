# SisFall and KFall: results for the paper

**Status: exploratory working tables, not confirmation of submission readiness.** The two datasets were examined during development. All headline scores are pooled across five subject-disjoint outer test folds; the pooled percentage is calculated from all test predictions, not the arithmetic mean of fold percentages. These results concern separately fitted models and different tasks. The confidence-gated retrieval system is a numerical routing rule; its reported classification results do not use an LLM.

## 1. Headline classification results

| Dataset | Evaluation task | Method | Recordings | Accuracy (%) | F1 (%) | Recall (%) | Specificity (%) |
|---|---|---|---:|---:|---:|---:|---:|
| SisFall | Nine-second known-start trial-prefix classification | Corrected gated retrieval | 4,500 | **97.00** | **96.14** | 93.49 | 99.33 |
| KFall | Impact-selected fall windows and separately selected ADL control windows | Cross-fitted gated retrieval | 5,075 | **99.63** | **99.59** | 99.53 | 99.71 |

**Interpretation:** These percentages are not directly comparable: the KFall fall-window selection uses the true fall label and impact annotation, while SisFall uses a trial prefix. The KFall score is not streaming accuracy; neither score proves real-world clinical performance. The model family is similar, but the fitted models, head inputs, observation windows and training procedures are not identical across datasets.

## 2. Corrected SisFall results for all five folds

Simulated acceleration noise for inner out-of-fold neural, physical and quality features was regenerated using statistics from the inner training subjects. Existing detector checkpoints were reused; the classifier head, threshold and gate were refitted. Each retained test recording appears once across the five folds.

| Outer fold | Recordings | Accuracy (%) | F1 (%) | Recall (%) | Specificity (%) |
|---|---:|---:|---:|---:|---:|
| 0 | 946 | 98.20 | 97.73 | 97.60 | 98.60 |
| 1 | 942 | 95.65 | 94.22 | 89.07 | 100.00 |
| 2 | 946 | 96.41 | 95.34 | 92.80 | 98.77 |
| 3 | 932 | 96.14 | 94.93 | 90.35 | 100.00 |
| 4 | 734 | 99.05 | 98.83 | 98.67 | 99.31 |
| **Pooled** | **4,500** | **97.00** | **96.14** | **93.49** | **99.33** |

| Corrected SisFall method | Accuracy (%) | F1 (%) | Recall (%) | Specificity (%) |
|---|---:|---:|---:|---:|
| Primary classifier | 96.09 | 95.03 | 93.55 | 97.78 |
| Gated retrieval | 97.00 | 96.14 | 93.49 | 99.33 |
| Always retrieve | 96.02 | 94.87 | 92.05 | 98.67 |

The inner and outer detector training and normalization still differ. This post-hoc rerun does not establish independent agent superiority. Predictions and 15 sampled corrected physical/quality features were checked; verification was not exhaustive. [Corrected experiment report](../results/sisfall_mamba/subject_crossfit_noise_corrected_20261101/REPORT.md); [summary](../results/sisfall_mamba/subject_crossfit_noise_corrected_20261101/summary.json); [verification](../results/sisfall_mamba/subject_crossfit_noise_corrected_20261101/verification.json).

## 3. KFall results for all five folds

| Outer fold | Recordings | Accuracy (%) | F1 (%) | Recall (%) | Specificity (%) |
|---|---:|---:|---:|---:|---:|
| 0 | 959 | 100.00 | 100.00 | 100.00 | 100.00 |
| 1 | 959 | 99.69 | 99.66 | 99.32 | 100.00 |
| 2 | 957 | 99.27 | 99.21 | 98.43 | 100.00 |
| 3 | 1,102 | 99.36 | 99.31 | 99.80 | 99.00 |
| 4 | 1,098 | 99.82 | 99.81 | 100.00 | 99.66 |
| **Pooled** | **5,075** | **99.63** | **99.59** | **99.53** | **99.71** |

| KFall impact-window method | Accuracy (%) | F1 (%) | Recall (%) | Specificity (%) |
|---|---:|---:|---:|---:|
| Primary classifier | 99.49 | 99.45 | 99.45 | 99.52 |
| Gated retrieval | 99.63 | 99.59 | 99.53 | 99.71 |
| Always retrieve† | 99.92 | 99.91 | 99.96 | 99.89 |
| Sensor features only† | 99.63 | 99.60 | 99.70 | 99.56 |

† Post-hoc controls from an earlier experiment using the same outer folds and records, not rerun as part of the cross-fitted head study; compute budgets were not matched. These controls must be reported with that qualification. [KFall cross-fitted summary](../results/kfall/impact_crossfit_20261101/summary.json); [controls](../results/research_submission_audit_20261101/kfall_matched_baselines.json).

## 4. KFall streaming: separate event-based evaluation

| Matching rule and method | Matched / eligible falls | Event recall (%) | ADL alarm edges | Edges / monitored hour | Median delay among matches (s) |
|---|---:|---:|---:|---:|---:|
| New alarm, impact to impact + 2 s: primary | 1,566 / 2,341 | 66.89 | 52 | 10.53 | 0.22 |
| New alarm, impact to impact + 2 s: gated retrieval | 1,814 / 2,341 | 77.49 | 43 | 8.70 | 0.22 |
| Onset through impact + 2 s: retrained detector | 2,340 / 2,341 | 99.96 | 35 | 7.08 | 0.88 |
| Onset through impact + 2 s: confidence-routed advisor | 2,338 / 2,341 | 99.87 | 16 | 3.24 | 1.06 |
| Onset through impact + 2 s: earlier detector, fixed confirmation | 2,332 / 2,341 | 99.62 | 16 | 3.24 | 1.19 |

The two matching rules answer different questions; onset-based matching includes alarms before impact. Policies and detector versions also differ. All use approximately 4.94 monitored hours of short scripted ADLs; delays exclude processing and communication. The advisor streams were precomputed, calls are simulated, and policy search spaces differ. These are offline replay results, not real-world false-alarm rates. [Strict post-impact audit](../results/kfall/crossfit_alarm_edge_audit_20261101/summary.json); [advisor study](../results/kfall/confidence_advisor_20261101/summary.json).

## 5. Manuscript status and interpretation

The existing draft `RCDP_JBHI_Overleaf/research_v6/main.tex` already includes both datasets, but its abstract and classification table cite the earlier SisFall run (96.11% accuracy and 95.05% F1), not the later corrected run above. The corrected run still needs a consistent manuscript, methods, table, uncertainty, and figure update before being used as the primary result. The agent's advantage over simpler controls and clinical external validation are not established. Historical test exposure, unknown independent participant overlap, mismatched endpoints, and the documented limitations must be disclosed. Author reports written KFall authorization; issuer/date/terms and secondary-analysis ethics details remain to be documented. This file collects results and does not assert JBHI submission readiness.

## 6. Additional earlier experiments and ablations

The tables below reproduce the prior working compilation to preserve **all** its results. Its SisFall 96.11% headline and related baseline table refer to the earlier run, not the corrected run in Sections 1–2; they must not be combined as if generated by the same refit. Other rows are distinct experiments with their own protocols. Their notes, negative results and limitations are part of the evidence.

---

# Results tables from the current manuscript draft

**Status:** Working tables; not a claim of submission readiness. Both cohorts were previously examined during development. Subject-cross-fitting limits one source of in-sample bias but does not establish independent validation. Percentages are calculated from pooled predictions unless explicitly labeled by fold.

## Table 1. SisFall subject-cross-fitted nine-second trial-prefix results

*Gated retrieval; each fold contains different held-out subjects. All 4,500 retained recordings are evaluated once across the five folds.*

| Outer fold | Recordings, n | Accuracy (%) | F1 (%) | Recall (%) | Specificity (%) |
|---|---:|---:|---:|---:|---:|
| 0 | 946 | 97.46 | 96.83 | 97.87 | 97.20 |
| 1 | 942 | 95.33 | 93.77 | 88.27 | 100.00 |
| 2 | 946 | 96.30 | 95.21 | 92.80 | 98.60 |
| 3 | 932 | 96.14 | 94.93 | 90.35 | 100.00 |
| 4 | 734 | 95.10 | 94.30 | 99.33 | 92.17 |
| **Pooled** | **4,500** | **96.11** | **95.05** | **93.49** | **97.85** |

Trial labels describe whole recordings, not independently timed falls. Inner and outer detector training and normalization differ.

### Follow-up: aligned inner/outer training budgets

*Outer models were retrained with the same sampling and training budgets as the inner detectors; inner CPU and outer GPU mixed-precision training still differ.*

| Method | Recordings, n | Accuracy (%) | F1 (%) | Recall (%) | Specificity (%) |
|---|---:|---:|---:|---:|---:|
| Primary detector | 4,500 | 95.36 | 94.26 | 95.44 | 95.30 |
| Gated retrieval | 4,500 | 95.91 | 94.91 | 95.33 | 96.30 |
| Always retrieve | 4,500 | 96.02 | 94.87 | 92.05 | 98.67 |

The paired subject-cluster bootstrap gated-minus-primary accuracy interval was -0.02 to +1.11 percentage points, conditional on fitted models. The cohort had been previously examined.

## Table 2. KFall subject-cross-fitted impact-window results

*Gated retrieval; fall windows were selected using impact annotations and ADL control windows were selected separately. All 5,075 recordings appear once across five subject-disjoint test folds.*

| Outer fold | Recordings, n | Accuracy (%) | F1 (%) | Recall (%) | Specificity (%) |
|---|---:|---:|---:|---:|---:|
| 0 | 959 | 100.00 | 100.00 | 100.00 | 100.00 |
| 1 | 959 | 99.69 | 99.66 | 99.32 | 100.00 |
| 2 | 957 | 99.27 | 99.21 | 98.43 | 100.00 |
| 3 | 1,102 | 99.36 | 99.31 | 99.80 | 99.00 |
| 4 | 1,098 | 99.82 | 99.81 | 100.00 | 99.66 |
| **Pooled** | **5,075** | **99.63** | **99.59** | **99.53** | **99.71** |

These are impact-selected window classifications, not online detection accuracy. Results in Tables 1 and 2 use different tasks and are not directly comparable.

## Table 3. Matched classification baselines and retrieval ablations

*Pooled results. SisFall rows share the nine-second prefix task. KFall cross-fitted rows share the impact-window task.*

| Dataset and task | Method | Accuracy (%) | F1 (%) | Recall (%) | Specificity (%) |
|---|---|---:|---:|---:|---:|
| SisFall prefix | Primary detector | 95.38 | 94.18 | 93.55 | 96.60 |
| SisFall prefix | Gated retrieval | 96.11 | 95.05 | 93.49 | 97.85 |
| SisFall prefix | Always retrieve | 96.02 | 94.87 | 92.05 | 98.67 |
| KFall impact window | Primary detector | 99.49 | 99.45 | 99.45 | 99.52 |
| KFall impact window | Gated retrieval | 99.63 | 99.59 | 99.53 | 99.71 |
| KFall impact window | Always retrieve† | 99.92 | 99.91 | 99.96 | 99.89 |
| KFall impact window | Sensor features only† | 99.63 | 99.60 | 99.70 | 99.56 |

† Post hoc baselines from the earlier impact-window experiment on the same outer folds and records. They were not rerun as part of the cross-fitted head study; always-retrieve uses training-subject sensor features, while the sensor-only classifier has its own calibration threshold. No compute-budget matching was performed.

## Table 4. KFall offline streaming replay: new post-impact alarm

*A detection requires a new positive rising edge from annotated impact through two seconds after impact. Earlier alarms do not count in this strict post-impact metric.*

| Method | Eligible falls, n | New alarm matches, n | Event recall (%) | Median delay among matches (s) | ADL alarm edges, n | Monitored ADL hours | Alarm edges / monitored hour |
|---|---:|---:|---:|---:|---:|---:|---:|
| Primary detector | 2,341 | 1,566 | 66.89 | 0.22 | 52 | 4.94 | 10.53 |
| Gated retrieval | 2,341 | 1,814 | 77.49 | 0.22 | 43 | 4.94 | 8.70 |

Five fall recordings lack a full two seconds of follow-up. The matching rule excludes alarms that start before impact, including potentially useful alarms after fall onset. The rate excludes 1.8 seconds of warmup per ADL recording, has no refractory rule, and is measured on short scripted recordings rather than free-living monitoring. Delay excludes processing and communication time.

### Complementary KFall replay: alarms from onset through impact + 2 seconds

*This is a different matching window from Table 4 and includes potentially useful pre-impact alarms after fall onset. Policies are selected on calibration subjects; the data remain exploratory.*

| Method | Eligible falls, n | Matched falls, n | Event recall (%) | Median onset-to-alarm among matches (s) | ADL edges, n | Edges / monitored hour |
|---|---:|---:|---:|---:|---:|---:|
| Retrained detector | 2,341 | 2,340 | 99.96 | 0.88 | 35 | 7.08 |
| Confidence-routed advisor | 2,341 | 2,338 | 99.87 | 1.06 | 16 | 3.24 |
| Earlier detector, selected fixed confirmation | 2,341 | 2,332 | 99.62 | 1.19 | 16 | 3.24 |

The advisor and detector policies use different candidate-grid sizes. The advisor is a numerical routing rule, with both model streams precomputed; its 126 simulated calls do not establish runtime or energy savings. The fitted-model subject-bootstrap intervals comparing advisor with the earlier fixed control include zero for recall and ADL alarm-rate differences. None of these results demonstrates independent clinical validation.

## Table 5. Common 1.8-second prefix and source-only transfer

*The same observation budget is used for all rows, with whole-trial labels. Within-dataset rows use separately trained models; transfer rows use source-trained models without target calibration.*

| Training → evaluation | Method and operating point | Recordings, n | Accuracy (%) | F1 (%) | Recall (%) | Specificity (%) |
|---|---|---:|---:|---:|---:|---:|
| SisFall → SisFall | Gated, 5:1 cost threshold | 4,500 | 60.67 | 65.16 | 92.05 | 39.79 |
| SisFall → SisFall | Gated, calibration F1† | 4,500 | 63.71 | 66.11 | 88.60 | 47.15 |
| KFall → KFall | Gated, 5:1 cost threshold | 5,075 | 78.34 | 80.48 | 96.55 | 62.70 |
| KFall → KFall | Gated, calibration F1† | 5,075 | 82.66 | 83.01 | 91.65 | 74.94 |
| SisFall → KFall | Gated, source-only ensemble | 5,075 | 53.02 | 66.29 | 99.91 | 12.72 |
| KFall → SisFall | Gated, source-only ensemble | 4,500 | 44.58 | 56.35 | 89.54 | 14.66 |

† Threshold maximizes calibration F1 subject to at least 90% calibration recall; held-out recall is not guaranteed. The within-dataset head uses in-sample neural training scores; both cohorts were historically exposed. The two detector training protocols differ. The reverse transfer uses a causal anti-aliasing filter with nominal 80 ms delay to convert SisFall to 100 Hz; resampling differs by direction. Prefix classification is not event detection; most KFall falls occur after this budget.

## Sources and interpretation

- [SisFall cross-fitted results](../results/sisfall_mamba/subject_crossfit_20261101/summary.json)
- [SisFall aligned-training follow-up](../results/sisfall_mamba/matched_crossfit_20261101/summary.json)
- [KFall cross-fitted results](../results/kfall/impact_crossfit_20261101/summary.json)
- [KFall new-alarm replay](../results/kfall/crossfit_alarm_edge_audit_20261101/summary.json)
- [KFall advisor routing](../results/kfall/confidence_advisor_20261101/summary.json)
- [KFall matched controls](../results/research_submission_audit_20261101/kfall_matched_baselines.json)
- [Common-prefix operating points](../results/research_v2/common_prefix_operating_points_20261101/summary.json)
- [Source-only transfer](../results/research_v2/sisfall_to_kfall_prefix_transfer_20261101/summary.json)

- [Reverse source-only transfer](../results/research_v2/kfall_to_sisfall_prefix_transfer_20261101/summary.json)

These tables do not demonstrate novelty, clinical performance, independent external validation, or journal acceptance. The successfully reported path uses retrieval voting, not an LLM. The author reports full written KFall authorization; its terms remain to be documented.


## Additional manuscript table. KFall onset-based replay and persistence

A new alarm is matched from annotated fall onset through two seconds after impact. This differs from the strict post-impact table above. ADL monitoring covers 4.94 hours of short scripted recordings; delay is measured among matched falls and excludes processing and communication.

| Policy | Falls matched / eligible | Event recall (%) | ADL edges / monitored hour | Median onset-to-alarm delay (s) |
|---|---:|---:|---:|---:|
| Primary | 2,330 / 2,341 | 99.53 | 10.53 | — |
| Gated, one window | 2,333 / 2,341 | 99.66 | 8.70 | 0.88 |
| Gated, selected persistence | 2,325 / 2,341 | 99.32 | 1.62 | 1.40 |

## Additional manuscript results. Quality-aware SisFall routing

The model predicts trial labels from known-start prefixes under simulated sensor conditions. Policy selection uses a different calibration-subject subset from gate training; observed budgets differ. The gyroscope-availability flag is idealized and specialist outputs were precomputed.

| Condition | Method | Accuracy (%) | F1 (%) | Recall (%) | Observed seconds | Simulated calls / trial |
|---|---|---:|---:|---:|---:|---:|
| clean | fixed_primary | 97.00 | 96.14 | 93.38 | 9.00 | 0.000 |
| clean | confidence | 95.11 | 93.95 | 94.94 | 7.37 | 0.000 |
| clean | quality_agent | 95.89 | 94.85 | 94.77 | 7.54 | 0.158 |
| gyro_missing | fixed_primary | 92.80 | 91.41 | 95.83 | 9.00 | 0.000 |
| gyro_missing | confidence | 88.82 | 87.45 | 97.44 | 7.42 | 0.000 |
| gyro_missing | quality_agent | 91.73 | 90.31 | 96.44 | 7.79 | 0.273 |
| acceleration_noise | fixed_primary | 95.76 | 94.73 | 95.55 | 9.00 | 0.000 |
| acceleration_noise | confidence | 90.07 | 88.58 | 96.38 | 7.06 | 0.000 |
| acceleration_noise | quality_agent | 91.67 | 90.21 | 96.05 | 7.26 | 0.083 |

## Additional manuscript results. KFall quality-aware agent versus Bhatti-inspired baseline

The two policies use the same tuning subjects and threshold/persistence grids, but model training, policy searches and computation budgets differ. The baseline is an implementation inspired by Bhatti, not the authors’ official model or original phase task. Alarms are matched from fall onset through impact plus two seconds.

| Method | Falls matched / eligible | Event recall (%) | ADL alarm edges | ADL edges / hour | Median onset-to-alarm delay (s) |
|---|---:|---:|---:|---:|---:|
| Quality-aware agent | 2,340 / 2,341 | 99.96 | 33 | 6.68 | 0.89 |
| Bhatti-inspired detector | 2,338 / 2,341 | 99.87 | 15 | 3.04 | 0.93 |

The paired subject-bootstrap interval for agent-minus-baseline recall was 0.00 to +0.26 percentage points; the alarm-rate interval was +0.61 to +6.99 edges/hour. These are exploratory, conditional on fitted models and historically exposed data, and do not establish clinical superiority.

## Additional manuscript results. Advisor action ablation

These post-hoc ablations retain the advisor’s selected confidence bands and persistence. The alternatives were not independently optimized.

| Decision within routed band | Falls matched / eligible | ADL alarm edges |
|---|---:|---:|
| Always negative | 2,336 / 2,341 | 14 |
| Consult advisor | 2,338 / 2,341 | 16 |
| Always positive | 2,339 / 2,341 | 18 |

Source files: [quality-aware SisFall](../results/sisfall_mamba/quality_gated_followup_20261101/summary.json), [matched KFall comparison](../results/kfall/matched_calibration_audit_20261101/summary.json), and [advisor action ablation](../results/kfall/confidence_advisor_20261101/action_ablation.json).


## Additional manuscript results. SisFall sequential specialist follow-up

Pooled accuracy for the first observe-or-consult policy and confidence-only comparison; sensor corruptions are simulated. These policies use assumed loss, and expert outputs were precomputed.

| Condition | Sequential agent accuracy (%) | Confidence-only accuracy (%) |
|---|---:|---:|
| Clean | 94.69 | 95.69 |
| Simulated gyroscope loss | 92.47 | 90.27 |
| Simulated acceleration noise | 90.09 | 91.78 |

## Additional manuscript results. KFall confirmation-policy follow-ups

These exploratory controls use onset-through-impact-plus-two-second matching on 2,341 eligible falls and short scripted ADL recordings. Candidate searches differ and results are not a clinical evaluation.

| Policy | Matched falls | ADL edges | ADL edges / hour | Median onset-to-alarm delay (s) |
|---|---:|---:|---:|---:|
| Adaptive agreement / confirmation | 2,333 | 38 | 7.69 | 0.88 |
| Fixed confirmation, matched calibration constraint | 2,332 | 16 | 3.24 | 1.19 |
| Physical-evidence gate | 2,332 | 16 | 3.24 | 1.08 |

The physical-evidence and fixed-confirmation policies each matched the same total number of falls, but not exactly the same cases; their median paired delay difference among jointly matched falls was zero.


## Additional manuscript results. Matched random consultation audit

The post-hoc random control preserves the agent’s observation stage and specialist-call count within each fold and simulated condition. It uses test condition labels and is not a deployable policy; its draw range is not a confidence interval for population performance.

| SisFall condition | Agent accuracy (%) | Mean random-route accuracy (%) | Random draws meeting or exceeding agent |
|---|---:|---:|---:|
| Clean | 95.89 | 95.75 | 216 / 2,000 |
| Simulated gyroscope loss | 91.73 | 91.17 | 0 / 2,000 |
| Simulated acceleration noise | 91.67 | 91.40 | 11 / 2,000 |

The result isolates routing choices conditional on the agent’s chosen stages. It does not establish an improvement over optimized fixed policies, prospective performance or measured computation savings. [Audit source](../results/sisfall_mamba/quality_gated_followup_20261101/matched_random_audit.json).
