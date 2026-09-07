# Milestone and Root-Cause Summary

This is the compact canonical audit trail. Large runtime roots and telemetry remain under `/media/nahhao74/KINGSTON`; detailed D0 V3 task reports are retained under `docs/03_evidence/d0_v3/`.

For current state use:

```text
../00_overview/CURRENT_STATUS.md
../00_overview/CURRENT_EXECUTION_LADDER_D0_V3_20260907.md
```

## Historical foundation retained

Before the current D0 V3 campaign, the project had already established:

```text
bounded additive AEGIS candidate architecture
PX4 control authority
exact candidate/exposure identity
native-source vs clock-mapping separation
StateBank startup/causal barriers
Option-B Direct Guard
WM reverse-index → graph → Tarjan SCC → fixed-point peeling validity engine
continuous-C1 replay/recovery
post-reset E8 source-causal pairing
native-event CLEAR lifecycle
next_status source-frontier repair
```

The randomized WM1 `G_action` scientific campaign still has no accepted complete 8-session/96-block root. Scientific inference/training remains blocked.

## D0 V3 milestone sequence — 2026-09-06

### 1. Concrete V3 production backend qualified

`V3_CANONICAL_DATA0_BACKEND_V1` bound the V3 orchestrator to canonical DATA0 lifecycle ownership without duplicating control or validity semantics.

### 2. Root `_01` — startup trace-path ordering defect

```text
RESULT=INVALID_MEASUREMENT_INFRASTRUCTURE
FIRST_CAUSAL_DIVERGENCE=orchestrator read trace before canonical writer created it
```

Repair introduced explicit startup trace-source states. Missing trace before owner readiness became `NOT_YET_AVAILABLE`; post-owner absence remained fail-closed.

### 3. Root `_02` — owner operand evidence blind spot

```text
TOTAL=4001
VALID=2833
UNEXPLAINED=1168
```

Offline attribution proved evaluator joins were correct; the missing boundary was owner-side causal operands, not join logic.

### 4. AURA owner operand ledger completed

`V3_AURA_OWNER_OPERAND_LEDGER_V1` now records actual M3 ordered predicates, runtime-gate operands and owner-time lookup provenance at the real decision sites.

No new blocking hot-path I/O was added; control-inert regression passed.

### 5. Downstream correspondence completed

`V3_DOWNSTREAM_EVALUATION_CORRESPONDENCE_V1` propagates the existing diagnostic identity:

```text
aura-shadow:<sequence>
```

through W20 → C1 → E8 while keeping it strictly distinct from control frontier, generation, reset and audit identities.

### 6. Root `_03` — E8 evidence existed but was not ingested

```text
TOTAL=4000
VALID=2885
UNEXPLAINED=1115
```

AURA owner evidence was working. The first missing boundary was E8 correspondence living in `e8_ingress_evidence.jsonl` but outside canonical V3 evaluator ingestion.

### 7. E8 secondary evidence ingestion + collector cleanup qualified

Canonical model:

```text
SECONDARY_APPEND_ONLY_EVIDENCE_SOURCE_CONSUMED_BY_V3_ORCHESTRATOR
NO_TRACE_LINE_COPY
```

Historical `_03` analysis found exact E8 correspondence for 1084/1084 previously unproven evaluations.

Reference collector ownership/finalization was also closed: cleanup cannot PASS while the owned collector remains alive.

### 8. Root `_04` — causal observability nearly closed

```text
TOTAL=4001
ACCOUNTED=4001
VALID=2879
EXPLAINED_ATTITUDE_FRESHNESS=1094
UNEXPLAINED=28
```

Integrity:

```text
W20=4001/4001
C1=4001/4001
E8 exact diagnostic IDs=4001/4001
E8 exact C1 IDs=4001/4001
E8 unmatched=0
duplicates=0
omissions=0
malformed=0
drops=0
gaps=0
writer errors=0
collector=dead_and_reaped
```

Residual 28:

```text
2 M3 attitude NO_CANDIDATE
1 M3 CAUSAL_EXPECTED_AVAILABLE=false
18 motor_warmup
5 motor_command_stale_or_missing
2 W20 no-baseline/not-ready with nonzero provenance frontier
```

### 9. Global explained-cause coverage audit

The audit proved the residual states were not one homogeneous evaluator bug.

Important findings:

```text
motor_command_stale_or_missing -> NOT_READY_CONTROL_UNAVAILABLE
W20 no-baseline/nonzero frontier -> contract-valid provenance-only not-ready state; no fabricated frontier
aura runtime-gate COMMAND_FRESH and deployable motor-command freshness are distinct domains
```

### 10. V3 explained-cause registry V1 frozen prospectively

Registry:

```text
V3_EXPLAINED_CAUSE_REGISTRY_V1
```

Known reason coverage:

```text
KNOWN_REASON_COUNT=24_PLUS_UNKNOWN_FAIL_CLOSED_FALLBACK
UNMAPPED_KNOWN_REASON_COUNT=0
```

Read-only `_04` replay under the prospective registry:

```text
VALID=2879
EXPLAINED=1116
NOT_READY=5
UNKNOWN=1
```

The one UNKNOWN remains `CAUSAL_EXPECTED_AVAILABLE=false` pending fresh owner-time expected-setpoint provenance.

### 11. Expected/motor provenance probe `_01` — precollector startup failure

```text
RESULT=PROBE_INCONCLUSIVE_INVALID_MEASUREMENT_INFRASTRUCTURE_PRECOLLECTOR
FIRST_FAILED_PROOF_OBLIGATION=runtime_source_counter_attestation:trace_attestation_marker_missing
ROOT_CAUSE=CANONICAL_DATA0_PRECOLLECTOR_TRACE_ATTESTATION_MARKER_ABSENT
```

Collector never started. Therefore the probe provides no evidence about expected-state or motor-command behavior.

This is a measurement/lifecycle issue, not a new FAST/control finding.

## Current milestone gate

The immediate task is a global offline DATA0 precollector startup/attestation contract audit rather than another one-line marker patch.

After that:

```text
one successful expected/motor provenance probe
→ final expected-state disposition
→ motor-command NOT_READY root-cause decision
→ final registry/evaluator freeze
→ one fresh D0 root
→ Phase D only after formal D0 closure
```

## Evidence retention rule

Formal roots and failed probes remain immutable historical evidence. Prospective registries/evaluator changes do not retroactively relabel historical roots.
