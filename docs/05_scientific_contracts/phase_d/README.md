# Phase-D B0 Frozen Scientific Contract

This directory is the documentation-layer binding for the frozen Phase-D B0 science plus approved qualification amendments. Byte-canonical machine artifacts remain authoritative in the source repository and KINGSTON evidence roots; this repository stores verified hashes, human-readable semantics, and explicit prospective qualification lineage.

## Canonical scientific identities

```text
BASELINE_ID=B0_PX4_AURA_FAST_T1_C1_CURRENT
METRIC_CONTRACT_ID=PHASE_D_B0_METRICS_V1_1
METRIC_CONTRACT_SHA256=5928dceea0a6e8e745f94282f7834bc268d0c10ccf61f2ccc4b424d43b78ed93
CAMPAIGN_MANIFEST_SHA256=dfab451d58f45e087fc9b25ab5eb9866bbff8e8d8ac6bd0475dffe25c1a2d247
CAMPAIGN_ROWS_SHA256=18a71d0f95447f0e47f0d4643a679e91ec0ffb9470f7930f52f13f384ff62c39
```

## Files

- [`CONTRACT_BINDINGS.json`](CONTRACT_BINDINGS.json) — verified source paths, SHA-256 identities, D0/PX4 bindings, and mirror policy.
- [`CAMPAIGN_DESIGN.md`](CAMPAIGN_DESIGN.md) — human-readable frozen eight-condition scientific design and F0–F4 metric semantics.
- [`PREWIND_QUALIFICATION_V2.md`](PREWIND_QUALIFICATION_V2.md) — approved prospective qualification/admission revision using fixed checkpoint `C`.

Canonical source artifacts are hash-bound to:

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
physical F0 = native Gazebo application truth
```

A 20-second Phase-D prewind dwell is not frozen or required.

## Qualification amendment V2

The earlier prospective attempt to use the complete live population `[V2_ELIGIBLE,F0)` was superseded after source/frontier audits showed that the ordinary Phase-D path cannot know the exact physical-F0 source frontier before command emission and cannot guarantee the late pre-F0 suffix is live-complete at that earlier decision boundary.

Current prospective admission rule:

```text
PREWIND_POPULATION_V2=[V2_ELIGIBLE,C)
```

with:

```text
[V2,C) = prewind admission
[C,F0)  = transition observation
[F0,...) = scientific response
```

`C` must be prospectively frozen, source-owned, control-independent, non-adaptive, and immutable for the bound execution design.

The qualification revision is explicitly accounted as:

```text
SCIENTIFIC_EXPERIMENT_DESIGN_CHANGED=false
QUALIFICATION_ADMISSION_CONTRACT_CHANGED=true
PREWIND_QUALIFICATION_CONTRACT_DELTA=[V2,F0) -> [V2,C)
CONTROL_SEMANTIC_DELTA=NONE
FAST_SEMANTIC_DELTA=NONE
DISTURBANCE_SEMANTIC_DELTA=NONE
PHASE_D_METRIC_SEMANTIC_DELTA=NONE
```

The concrete canonical binding of `C`, live owner upper-watermark, writer flush, C1/E8 reconciliation, and exact execution harness are still pending. Runtime is not authorized.

## Related current evidence

- Current status: `../../00_overview/CURRENT_STATUS.md`
- Phase-D evidence index: `../../03_evidence/phase_d/README.md`
- Current checkpoint-C decision: `../../03_evidence/phase_d/prewind/PREWIND_CHECKPOINT_C_DECISION_20260908.md`
- Source-frontier audit: `../../03_evidence/phase_d/prewind/SOURCE_FRONTIER_BINDING_AUDIT_20260908.md`
- Scheduled-frontier audit: `../../03_evidence/phase_d/prewind/SCHEDULED_FRONTIER_AUDIT_20260908.md`
