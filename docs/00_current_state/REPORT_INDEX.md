# Report Index — WM1 / AEGIS vNext

This index lists the **currently archived core reports** in this repository. Historical/intermediate reports that are not listed here remain in the project workspace/Kingston lineage and can be added later if needed.

## Current core reports

1. [`nhiem_vnext.wm1_v2r1_cross_topic_uxrce_update_head_of_line_blocking_root_cause_closure.md`](../03_reports/wm1_v2r1/nhiem_vnext.wm1_v2r1_cross_topic_uxrce_update_head_of_line_blocking_root_cause_closure.md) — closes the reproduced 148-ms uXRCE HOL mechanism caused by synchronous diagnostics logging.
2. [`nhiem_vnext.wm1_v2r1_canonical_sensorcombined_timestamp_mapping_discontinuity_root_cause_closure.md`](../03_reports/wm1_v2r1/nhiem_vnext.wm1_v2r1_canonical_sensorcombined_timestamp_mapping_discontinuity_root_cause_closure.md) — proves the 183018-us event was a Timesync mapping transition, not native source loss.
3. [`nhiem_vnext.wm1_v2r1_timestamp_semantics_owner_review_and_dual_domain_contract_freeze.md`](../03_reports/wm1_v2r1/nhiem_vnext.wm1_v2r1_timestamp_semantics_owner_review_and_dual_domain_contract_freeze.md) — freezes dual-domain native-source continuity + separate clock-alignment semantics.
4. [`nhiem_vnext.wm1_v2r1_dual_domain_timestamp_contract_implementation_and_qualification.md`](../03_reports/wm1_v2r1/nhiem_vnext.wm1_v2r1_dual_domain_timestamp_contract_implementation_and_qualification.md) — implements and qualifies Option A semantics offline/replay.
5. [`nhiem_vnext.wm1_v2r1_atomic_timesync_provenance_and_live_campaign_equivalent_soak.md`](../03_reports/wm1_v2r1/nhiem_vnext.wm1_v2r1_atomic_timesync_provenance_and_live_campaign_equivalent_soak.md) — identifies the live-wire provenance/interface gap and establishes WM1_DUAL_DOMAIN_TIMESTAMP_RECORD_V2.
6. [`nhiem_vnext.wm1_v2r1_atomic_sensorcombined_provenance_wire_implementation_and_live_8session_soak.md`](../03_reports/wm1_v2r1/nhiem_vnext.wm1_v2r1_atomic_sensorcombined_provenance_wire_implementation_and_live_8session_soak.md) — implements SensorCombinedStampedV1 and passes live single-session + 8-session soak.
7. [`nhiem_vnext.wm1_v2r1_statebank_aura_pre_science_bootstrap_root_cause_closure.md`](../03_reports/wm1_v2r1/nhiem_vnext.wm1_v2r1_statebank_aura_pre_science_bootstrap_root_cause_closure.md) — closes the StateBank/AURA startup readiness race with an explicit all-stream barrier.
8. [`nhiem_vnext.wm1_v2r1_fresh_randomized_pilot_retry_after_statebank_aura_bootstrap_closure.md`](../03_reports/wm1_v2r1/nhiem_vnext.wm1_v2r1_fresh_randomized_pilot_retry_after_statebank_aura_bootstrap_closure.md) — immutable pre-science failed root that revealed the specialized runner omitted `bootstrap_only=True`.
9. [`nhiem_vnext.wm1_v2r1_pilot_bootstrap_only_callsite_repair_and_exact_preflight.md`](../03_reports/wm1_v2r1/nhiem_vnext.wm1_v2r1_pilot_bootstrap_only_callsite_repair_and_exact_preflight.md) — closes the specialized-runner call-site defect; 51 tests PASS and exact real-runner pre-science qualification proves bootstrap returns before candidate/C1/E8/T_D paths.

## Current navigation

- Pipeline state: [`PIPELINE_CURRENT_STATE_20260829.md`](PIPELINE_CURRENT_STATE_20260829.md)
- Canonical architecture: [`../01_architecture/AURA_WISE_WM_AEGIS_VNEXT_MOVING_MODE_ARCHITECTURE.md`](../01_architecture/AURA_WISE_WM_AEGIS_VNEXT_MOVING_MODE_ARCHITECTURE.md)
- Source registry pointer: [`../02_source_registry/SOURCE_REGISTRY_CURRENT.md`](../02_source_registry/SOURCE_REGISTRY_CURRENT.md)
- Research synthesis: [`../04_research/CLOSED_LOOP_IDENTIFICATION_RESEARCH_SYNTHESIS.md`](../04_research/CLOSED_LOOP_IDENTIFICATION_RESEARCH_SYNTHESIS.md)

## Current next state

`PILOT_BOOTSTRAP_ONLY_CALLSITE_REPAIR_QUALIFIED_READY_FOR_OWNER_FRESH_PILOT_RETRY_REVIEW`

The next scientific gate is exactly one fresh randomized WM1 V2R1 pilot root under owner authorization. No training/SEALED/R1 is authorized by these readiness reports.

## Not archived here

Large raw runtime roots, telemetry, replay/capture bundles and datasets stay under `/media/nahhao74/KINGSTON`. This GitHub repository is for human-readable documentation, frozen decisions, source/provenance pointers and research synthesis.
