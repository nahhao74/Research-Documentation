# Phase-D B0 Frozen Scientific Contract

This directory contains the frozen Phase-D B0 measurement/campaign contract package.

## Canonical identities

```text
BASELINE_ID=B0_PX4_AURA_FAST_T1_C1_CURRENT
METRIC_CONTRACT_ID=PHASE_D_B0_METRICS_V1_1
METRIC_CONTRACT_SHA256=5928dceea0a6e8e745f94282f7834bc268d0c10ccf61f2ccc4b424d43b78ed93
CAMPAIGN_MANIFEST_SHA256=dfab451d58f45e087fc9b25ab5eb9866bbff8e8d8ac6bd0475dffe25c1a2d247
```

## Files

- `phase_d_metrics_contract_v1_1.json` — machine-readable frozen metric/readiness/censoring contract.
- `phase_d_campaign_manifest_v1_1.json` — machine-readable eight-condition frozen campaign.
- `PHASE_D_METRICS_AND_CAMPAIGN_REPORT_V1_1.md` — human-readable freeze report.
- `PHASE_D_PROVENANCE_CORRECTION_REPORT.md` — provenance-only correction; metric/campaign semantics unchanged.

## Frozen science

```text
B0 = PX4 + AURA + current FAST/T1/C1
planned scientific conditions = 8
performance thresholds = NONE_FROZEN_DESCRIPTIVE_ONLY
adaptive changes = false
retry until favorable = false
FAST challenger selection = NOT_PERFORMED
```

The contract requires the qualified `PHASE_D0_PHASE_D_READINESS_V3 / V3_RUNTIME_START_EVENT_V2` one-shot readiness gate before scheduled native disturbance.

A 20-second Phase-D prewind dwell is **not** frozen or required.

The current implementation work is allowed to add only control-inert measurement/orchestration necessary to prove the fixed prewind evidence prefix and place the gate on the native-F0 path. It must not change the scientific metric meanings, disturbance/reference, control mathematics, or retry policy.

For current execution semantics and open blocker, read:

- `../../00_overview/CURRENT_STATUS.md`
- `../../03_evidence/phase_d/prewind/PREWIND_CHECKPOINT_DECISION_20260908.md`
