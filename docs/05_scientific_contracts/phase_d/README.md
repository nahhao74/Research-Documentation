# Phase-D B0 Frozen Scientific Contract

This directory is the documentation-layer binding for the frozen Phase-D B0 contract. Byte-canonical machine artifacts remain authoritative in the source repository and KINGSTON evidence roots; this repository stores verified hashes, design semantics, and navigation.

## Canonical identities

```text
BASELINE_ID=B0_PX4_AURA_FAST_T1_C1_CURRENT
METRIC_CONTRACT_ID=PHASE_D_B0_METRICS_V1_1
METRIC_CONTRACT_SHA256=5928dceea0a6e8e745f94282f7834bc268d0c10ccf61f2ccc4b424d43b78ed93
CAMPAIGN_MANIFEST_SHA256=dfab451d58f45e087fc9b25ab5eb9866bbff8e8d8ac6bd0475dffe25c1a2d247
CAMPAIGN_ROWS_SHA256=18a71d0f95447f0e47f0d4643a679e91ec0ffb9470f7930f52f13f384ff62c39
```

## Files

- [`CONTRACT_BINDINGS.json`](CONTRACT_BINDINGS.json) — verified source paths, SHA-256 identities, D0/PX4 bindings, and mirror policy.
- [`CAMPAIGN_DESIGN.md`](CAMPAIGN_DESIGN.md) — human-readable frozen eight-condition design, F0–F4 semantics, phase boundaries, and prewind rule.

Canonical source artifacts are bound by hash to:

```text
reports/phase_d_metrics_campaign_freeze_v1_1/phase_d_metrics_contract_v1_1.json
reports/phase_d_metrics_campaign_freeze_v1_1/phase_d_campaign_manifest_v1_1.json
```

They are intentionally not hand-copied here unless byte parity can be guaranteed.

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

Current implementation work may add only control-inert measurement/orchestration required to prove the fixed prewind evidence prefix and place the gate on the native-F0 path. It must not change scientific metric meanings, disturbance/reference, control mathematics, or retry policy.

## Related current evidence

- Current status: `../../00_overview/CURRENT_STATUS.md`
- Phase-D evidence index: `../../03_evidence/phase_d/README.md`
- Owner checkpoint decision: `../../03_evidence/phase_d/prewind/PREWIND_CHECKPOINT_DECISION_20260908.md`
