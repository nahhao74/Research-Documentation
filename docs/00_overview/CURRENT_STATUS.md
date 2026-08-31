# Current Status — 2026-08-31

## Executive state

The AURA–WISE–World Model–AEGIS vNext pipeline has completed its structural cleanup and roadmap normalization. The repository is no longer blocked by package ownership, duplicate WM1 semantics, compatibility adapters, or artifact-path ambiguity.

Current state:

```text
STRUCTURAL_CLEANUP=CLOSED_WITH_MINIMAL_PROVENANCE_FACADE
PHASE_0A_MECHANISM=CLOSED
ROADMAP=ESTABLISHED
CURRENT_BLOCKER=GUST_P1_AURA_REFERENCE_TRANSITION_ONSET_SUPPRESSION
0B.1_GUST_P1_SEMANTIC_FORENSIC=READY
FRESH_NOSCIENCE_PARITY=BLOCKED
FRESH_SCIENCE=BLOCKED
SEALED_STATE=LOCKED_PRE_EVALUATION
production_authority=false
```

The active blocker is now a scientific/semantic boundary, not repository structure or callback infrastructure.

## Current architecture

### Fast path

```text
PX4 / sensors / reference
-> AURA
-> AEGIS FAST / T1 / C1
-> PX4
```

PX4 inner loops remain authoritative. The World Model does not block first response.

### Predictive path

```text
PX4 / sensors / reference
-> StateBank always warm
-> predictive / World Model
-> bounded candidate refinement
-> AEGIS
-> PX4
```

Canonical predictive ownership now includes:

```text
predictive_core/predictive/statebank/core.py
predictive_core/predictive/nominal.py
predictive_core/predictive/artifact_registry.py
predictive_core/predictive/artifact_registry.v1.json
```

### WM1 causal/experiment path

Canonical shared WM1 semantics now live under `experiments/wm1/`:

```text
transaction
lifecycle
observations
contexts
H1000
serialization
```

The experiment flow is:

```text
context
-> observations
-> transaction
-> accepted exposure
-> release
-> H1000
-> accounting / thin runner
```

Historical AURA import-only compatibility adapters have been retired; internal compatibility adapters are now zero.

## WISE role

WISE is retained as an AS-IS diagnostic/reproduction sidecar, not a mandatory vNext control hop.

```text
wise_shadow = active diagnostic sidecar
HGDO = active diagnostic baseline
UIO = active diagnostic baseline
WISE_VNEXT_REQUIRED_HOP=false
```

Nominal predictive dynamics are no longer owned by WISE; the canonical implementation is in `predictive_core`.

## Structural cleanup closure

The physical ownership migration and repository cleanup are closed with a minimal provenance facade.

Closed work includes:

```text
StateBank canonical ownership
nominal predictive canonical ownership
WM1 shared behavior extraction
accepted-status duplicate removal
historical report dependency decoupling
5 internal compatibility adapters retired
versioned artifact registry/resolver v1
_DELETE_STAGING accounting closure
canonical roadmap/document workflow restoration
```

The final artifact-call migration classified the remaining direct World Model artifact references. Migratable references use the fail-closed artifact registry; exact historical paths are retained only where provenance or compatibility requires them.

`4_WORLD_MODEL/artifacts/` remains intentionally present as a path-bound provenance facade. It is not treated as an active source tree or a general runtime-data store.

Root `.runtime_*` symlinks and the retained WISE diagnostic paths likewise remain only because current callers, entrypoints, tests, or path-bound lineage still require them.

## Validation baseline after structural closure

Latest reported regression state:

```text
AURA=717 PASS
WISE=115 PASS
AEGIS=247 PASS
PREDICTIVE_CORE=5 PASS
WORLD_MODEL=264 PASS
ROOT=22 PASS
WM1_FOCUSED=78 PASS
py_compile=PASS
git diff --check=PASS
Codegraph=FRESH
```

No live/scientific campaign was run to obtain these maintenance results.

```text
SCIENTIFIC_EXECUTION=NOT_RUN
IMMUTABLE_KINGSTON_EVIDENCE_MUTATION=NONE
GUST_P1_SEMANTICS_CHANGED=false
H1000_SEMANTICS_CHANGED=false
CONTROL_MATHEMATICS_CHANGED=false
```

## Phase 0A mechanism state

The causal/runtime mechanism work preceding scientific identification is closed.

```text
runtime/source/clock/provenance=CLOSED
StateBank causal bootstrap=CLOSED
C1/E8 exact accepted-action path=CLOSED
T_D -> T_A timing contract=CLOSED
exact treatment continuation=CLOSED
H1000 refractory semantics=CLOSED
CALM/GUST repeated-decision mechanism=CLOSED
```

Do not reopen these gates without new evidence of an actual regression.

## Live nonscientific parity state

Current parity evidence remains:

```text
DETERMINISTIC_PARITY=6/6_PASS
LIVE_NOSCIENCE_PARITY=5/6
```

Observed live outcomes:

```text
CALM ZERO=VALID
CALM P1=VALID
CALM P2=VALID
GUST ZERO=VALID
GUST P2=VALID
GUST P1=BLOCKED
```

The remaining GUST P1 failure is not callback starvation and is not a DDS-loss conclusion.

## Current scientific blocker

The exact blocker is:

```text
GUST_P1_AURA_REFERENCE_TRANSITION_ONSET_SUPPRESSION
```

Known behavior:

```text
native GUST event exists
-> AURA enters REFERENCE_TRANSITION
-> AURA emits reason=reference_transition_not_disturbance
-> no qualified DISTURBANCE_ONSET is produced
-> GUST P1 cannot satisfy the current event-qualified treatment gate
```

This is a semantic/timing interaction at the AURA/reference-transition/event-qualification boundary.

Changing any of the following would be a scientific/semantic decision and therefore requires owner review:

```text
AURA event/reference-transition meaning
GUST treatment eligibility/timing
native external-event vs AURA qualification binding
validity criteria
```

## Immediate next gate — 0B.1

The roadmap-authorized next task is:

```text
0B.1 GUST P1 semantic forensic
```

Goal:

```text
identify the first causal divergence unique to GUST P1
compare GUST ZERO / P1 / P2 native-source chronology
trace reference transition vs native GUST onset
classify the root as:
- intended AURA semantics
- implementation contradiction to frozen semantics
- timing/design interaction
- insufficient evidence
```

The expected output is an owner semantic decision package. No semantic implementation is authorized by this gate.

Candidate resolution families to compare include:

```text
A. change AURA event/reference-transition semantics
B. keep AURA semantics and change GUST arm/treatment timing
C. separate native external-event identity from AURA disturbance qualification
D. another source-grounded alternative
```

Each option must be evaluated for impact on:

```text
G_action
P1/P2
baseline B
randomization
pre-treatment conditioning
AURA semantics
FAST/T1/C1
H1000
validity criteria
```

## Gates after 0B.1

No later gate is READY yet.

Dependency order is:

```text
0B.1 semantic forensic
-> 0B.2 owner semantic decision
-> 0B.3 approved implementation
-> 0B.4 deterministic regression
-> 0B.5 fresh bounded nonscientific 6-case parity
-> 0B.6 scientific pilot owner review
-> 0B.7 fresh randomized scientific pilot
-> 0B.8 scientific analysis
-> 0B.9 causal dataset acceptance
```

Fresh nonscientific parity must reach:

```text
6/6 VALID
```

before a new scientific pilot is reviewed.

## Scientific campaign remains pending

The intended action-conditioned experiment remains conceptually:

```text
worker A
8 sessions = 4 CALM + 4 GUST_E
12 blocks/session
96 blocks total
ZERO=48
P1=24
P2=24
```

The scientific estimand remains:

```text
G_action(X,U,h)
=
Y(B+U,h) - Y(B+ZERO,h)

B = active PX4 + AURA + FAST/T1/C1 baseline
```

Future FAST/PX4 reaction after the candidate is part of the closed-loop treatment response, not automatically a confounder.

A technically valid campaign does not automatically imply `CAUSAL_DATASET_ACCEPTED`.

Scientific analysis must distinguish:

```text
CLEAR
WEAK
NOT_RESOLVED
```

and `NO_IMPROVEMENT` must not be interpreted as proof of no physical action effect.

## World Model roadmap after causal closure

World Model development remains dependency-gated behind causal data acceptance.

The intended decomposition is:

```text
Stage A:
T_D -> X_hat_A

F_nominal:
X -> nominal future response

G_action:
(X,U) -> incremental closed-loop response
```

Development order:

```text
causal dataset acceptance
-> Stage A / F_nominal / G_action baselines
-> model qualification
-> read-only latency characterization / later optimization where allowed
-> predictive shadow integration
-> ActivePlan monitor / invalidate / replan
-> safety and control-margin qualification
-> separate authority gate
```

World Model must never own first response, event detection, safety authority, or PX4 inner-loop authority.

## Storage and provenance

Significant runtime/capture/dataset/training/intermediate artifacts remain under:

```text
/media/nahhao74/KINGSTON
```

`/home` is for source, tests, small deterministic fixtures, configuration and canonical documentation.

Immutable/path-bound evidence is not moved merely for cosmetic cleanup.

## Authority boundaries

```text
SEALED_ACCESS_BOUNDARY=LOCKED_PRE_EVALUATION
SEALED_PAYLOAD_OPENED=false
FRESH_SCIENCE=BLOCKED
MODEL_TRAINING_ACTION_RESPONSE=BLOCKED_PENDING_CAUSAL_DATA_ACCEPTANCE
production_authority=false
```

Historical failed scientific roots remain immutable.

## Current direction

The project is back on the main scientific roadmap. Structural cleanup is no longer the active workstream.

The immediate sequence is:

```text
GUST P1 semantic forensic
-> owner semantic decision
-> smallest approved implementation
-> deterministic regression
-> fresh 6-case nonscientific parity
-> only then scientific pilot review
```
