# G-action Progress — 2026-09-10

## Scope

Compact milestone trail for the current World Model `G_action` branch. Large raw artifacts remain on KINGSTON.

## Milestone ladder

```text
V1 structured World Model
  -> V1.1 context compatibility
  -> runtime target / Stage-A architecture review
  -> V1.2 explicit delay
  -> G residual/effective-horizon diagnosis
  -> Acquisition V2R1 contract revision
  -> source-semantics closure
  -> owner approval for bounded contiguous exposure
  -> bounded-contiguous state machine implementation
  -> ROS environment repair
  -> contiguous caller/runner plumbing
  -> QualifiedOffer / QualifiedAcceptance / QualifiedActionLinkLifecycle
  -> Stage1 + contiguous migration to shared lifecycle
  -> deterministic legacy equivalence PASS
  -> NEXT: minimal fresh runtime qualification
```

## Key conclusions

### World Model

```text
F_ENGINEERING_STATUS=USEFUL_SHORT_HORIZON_ENGINEERING_PREDICTION
G_ENGINEERING_STATUS=NO_PREDICTIVE_GAIN_AT_CURRENT_EFFECTIVE_ACTION_SUPPORT
MODEL_CAPACITY_INCREASE_JUSTIFIED=false
G_ACTION_CAUSAL_STATUS=UNQUALIFIED
```

### Primary G limitation

```text
PRIMARY_G_LIMITATION=INSUFFICIENT_EFFECTIVE_ACTION_HORIZON
```

H40 is entirely pre-action under the current decision-relative timing interpretation. H80 provides only sparse, short post-action exposure; held-out clean longer-horizon support is absent.

### Historical temporal-plan semantics

```text
INTER_CYCLE_PERSISTENCE=PROVEN_EVENT_ONLY
PULSE_3C=3 accepted events
HOLD_SHORT_5C=5 accepted events
HOLD_LONG_7C=7 accepted events
EXPOSURE_DURATION_DERIVABLE=false
```

`OFFER_RETRY_INTERVAL_NS=20_000_000` is transport retry, not command cadence.

### Acquisition V2R1

Current V2 is parked and immutable. V2R1 is a separate proposal that separates assigned plan, offered sequence, accepted sequence, observed exposure and release lifecycle. It is not executed.

### Bounded contiguous primitive

Owner-approved mode:

```text
BOUNDED_CONTIGUOUS_CANDIDATE_EXPOSURE_V1
```

Implemented semantics:

```text
accepted T_A in px4_boot_us
-> same candidate on each qualified active callback
-> bounded expiry / fail-closed ZERO
-> explicit parent-linked release
```

Legacy `EVENT_ONLY_V1` remains unchanged.

### Shared transaction lifecycle

Current canonical runtime owners:

```text
QualifiedOffer
QualifiedAcceptance
QualifiedActionLinkLifecycle
```

Final migration result:

```text
QUALIFIED_TRANSACTION_HOOK_IMPLEMENTED=FULL
STAGE1PROBE_USES_SHARED_HOOK=true
CONTIGUOUS_RUNNER_USES_SHARED_HOOK=true
LIVE_RUNNER_ARBITRARY_SOURCE_ALLOWED=false
RELEASE_TRANSACTION_IMPLEMENTED=true
RELEASE_USES_SHARED_HOOK=true
```

Legacy equivalence:

```text
frontier selection PASS
ACK matching PASS
T_A PASS
retry PASS
timeout PASS
generation progression PASS
3C PASS
5C PASS
7C PASS
EVENT_ONLY behavior unchanged
```

Latest migration validation:

```text
100 focused tests PASS
ROS imports PASS
py_compile PASS
task-scoped diff PASS
```

## Runtime status

```text
RUNTIME_SMOKE_EXECUTED=false
CONTIGUOUS_EXPOSURE_SOURCE_QUALIFIED=false
V2R1_TEMPORAL_LADDER_NOW_DERIVABLE=false
```

Next task:

```text
G_ACTION_CONTIGUOUS_MINIMAL_RUNTIME_QUALIFICATION
```

Planned scope is exactly one engineering ZERO bounded plan and one engineering nonzero 20 ms bounded plan with source-bound exposure ledger, release, landing and cleanup.

20 ms is an engineering qualification value only, not a frozen scientific horizon.

## MRT status

The intended future `G_action` acquisition should be formalized as a constrained Micro-Randomized Trial after live qualification of the contiguous primitive.

```text
historical WM1 dataset=NOT MRT
old V2=MRT-like design; not executed
V2R1=constrained MRT-like proposal; not executed
formal MRT G campaign=NOT EXECUTED
```

## Scientific boundary

```text
B = PX4 + AURA + FAST/T1/C1
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
FAST remains active
FAST_REMOVAL_EVIDENCE=NO
WM_CONTROL_WRITE=false
WISE_ENABLED=false
AEGIS_WM_AUTHORITY=false
SCIENTIFIC_ACQUISITION_EXECUTED=false
SEALED_PAYLOAD_OPENED=false
```

For the full current handoff see `../../00_overview/CURRENT_STATE_CHECKPOINT_20260910_G_ACTION_CONTIGUOUS_MRT_PREP.md`.
