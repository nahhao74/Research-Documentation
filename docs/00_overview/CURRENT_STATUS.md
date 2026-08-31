# Current Status — 2026-08-31

## Executive state

The project remains in **Phase 0B.2**. Structural/mechanism work and the GUST-P1 causal forensic are closed. Historical offline characterization reached its evidence limit; the immediate next gate is the owner-authorized bounded Q1 no-launch characterization.

```text
STRUCTURAL_CLEANUP=CLOSED
PHASE_0A_MECHANISM=CLOSED
PHASE_0B1=CLOSED

PHASE_0B2=OWNER_MARGIN_DECISION_REQUIRED

OPTION_B_DIRECTION=PREFERRED
OPTION_B_CONTRACT_READY=false
IMPLEMENTATION_AUTHORIZED=false

REFERENCE_STABILITY_PREDICATE=
GUST_PREOFFER_REFERENCE_STABILITY_V1

AUTHORITATIVE_TIME_DOMAIN=PX4_BOOT_US
# selected/proposed source-time authority
# still inside owner freeze boundary

M_STABLE_US=UNFROZEN
W_MAX_US=UNFROZEN

EVENT_SCHEDULING_POLICY=
DELAY_RESCHEDULE_WITHIN_BLOCK
# RECOMMENDED
# NOT_FROZEN
# NOT_APPROVED

COUNTERFACTUAL_IDENTIFIABILITY=REFERENCE_ONLY
POST_NATIVE_EVENT_CONTAMINATION_AUDIT=LIMITED
FULL_PREDICATE_COUNTERFACTUAL_SUPPORT=NOT_IDENTIFIABLE
W_MAX_FULL_PREDICATE_OFFLINE_SUPPORT=UNRESOLVED
W_MAX_RUNTIME_FEASIBILITY=UNPROVEN

CHARACTERIZATION_STATE=
PREPARED_FOR_OWNER_NOSCIENCE_RUNTIME_AUTHORIZATION

Q1_NOSCIENCE_NO_LAUNCH_CONTRACT=PREPARED
Q1_RUNTIME_AUTHORIZED=false
Q1_RUNTIME_EXECUTED=false

PHASE_0B3_IMPLEMENTATION=BLOCKED
PHASE_0B4_DETERMINISTIC_REGRESSION=BLOCKED
PHASE_0B5_FRESH_NOSCIENCE_PARITY=BLOCKED
PHASE_0B6_SCIENTIFIC_PILOT_OWNER_REVIEW=BLOCKED
PHASE_0B7_FRESH_RANDOMIZED_SCIENCE=BLOCKED
PHASE_0B8_SCIENTIFIC_ANALYSIS=BLOCKED
PHASE_0B9_CAUSAL_DATASET_ACCEPTANCE=BLOCKED

FRESH_SCIENCE=BLOCKED
SEALED=LOCKED_PRE_EVALUATION
MANIFEST_SLOTS_CONSUMED=0
SCIENTIFIC_EXECUTION=NOT_RUN

AEGIS_AUTHORITY=0
ROUND_Z_AUTHORITY=0
production_authority=false
```

The active workstream remains Phase-0 causal-data qualification. This is not a World-Model blocker and is not the historical C1-frontier timeout.

## Canonical architecture

### Fast path

```text
PX4 / sensors / reference
-> AURA
-> AEGIS FAST / T1 / C1
-> PX4
```

The fast path must remain independently functional. PX4 inner loops remain authoritative.

### Predictive path

```text
PX4 / sensors / reference
-> StateBank always warm
-> World Model / predictive refinement
-> WISE / bounded candidate planning
-> AEGIS
-> PX4
```

The World Model is not the current development gate and must never block first response.

## 0B.1 closure — exact causal conclusion

```text
C_TIMING_DESIGN_INTERACTION_BETWEEN_FROZEN_REFERENCE_TRANSITION_PRECEDENCE
AND_THE_PREOFFER_NATIVE_EVENT_WINDOW
```

GUST ZERO and P2 regain qualified Moving evidence after the reference transition clears. GUST P1 loses Moving detector active evidence while the transition is still active, then never obtains a qualified `DISTURBANCE_ONSET`.

P1 never reaches candidate offer, scientific `T_D`, ACK, or accepted exposure. The failure is therefore not a P1 treatment effect.

## Completed 0B.2 offline characterization

The existing immutable GUST evidence supports only:

```text
COUNTERFACTUAL_IDENTIFIABILITY=REFERENCE_ONLY
POST_NATIVE_EVENT_CONTAMINATION_AUDIT=LIMITED
```

Reference-side stable chronology can be derived, but historical GUST already occurred before the proposed delayed-launch waiting trajectory. Post-event AURA/C1/reset state cannot stand in for the no-GUST counterfactual.

```text
FULL_PREDICATE PASS=0
FULL_PREDICATE FAIL=0
FULL_PREDICATE UNKNOWN=3
```

Historical evidence also lacks an exact `nominal_requested_launch_source_us` in `PX4_BOOT_US`. Therefore no valid source-domain wait distribution or `W_MAX_US` offline coverage envelope can yet be computed.

```text
M_STABLE_US=UNFROZEN
W_MAX_US=UNFROZEN
W_MAX_RUNTIME_FEASIBILITY=UNPROVEN
```

The existing 500 ms reference-change exclusion constant is not promoted into Option B merely because it exists.

## Current next gate — Q1

```text
Q1 — BOUNDED NON-SCIENTIFIC NO-LAUNCH SHADOW QUALIFICATION
```

Q1 contract is prepared but runtime is not authorized or executed.

For GUST_E, Q1 must create a true unexposed waiting trajectory:

```text
session/block ready
-> identify predeclared nominal GUST launch opportunity
-> record nominal_requested_launch_source_us in PX4_BOOT_US
-> suppress native GUST for the entire bounded observation window
-> observe reference / AURA / C1 / session-reset / continuity / provenance
-> evaluate GUST_PREOFFER_REFERENCE_STABILITY_V1 in SHADOW ONLY
-> terminate without GUST launch, treatment, scientific T_D or manifest accounting
```

Critical invariant:

```text
NO_NATIVE_GUST_AFTER_NOMINAL_OPPORTUNITY
```

CALM remains a negative/control context for observability/support. It must not be turned into a synthetic GUST case.

Q1 no-launch mode is observational/fail-safe: predicate `PASS`, `FAIL` or `UNKNOWN` must never release the suppressed GUST. Normal scheduling behavior must remain unchanged when Q1 mode is disabled.

Successful Q1 can create evidence for:

```text
nominal_requested_launch_source_us
first_full_predicate_eligible_source_us
full_counterfactual_wait_us
M_STABLE_US sensitivity
candidate W_MAX full-predicate offline coverage
```

It still cannot establish actual delayed-launch runtime feasibility because it does not execute delayed native launch, AURA onset binding, `T_D`, offer, ACK/exposure, release and H1000.

## Phase-0 dependency ladder

```text
0A mechanism / structural closure                         CLOSED
0B.1 GUST-P1 timing/design forensic                       CLOSED
0B.2a historical/reference-only characterization          COMPLETE TO EVIDENCE LIMIT
0B.2b Q1 no-launch shadow preparation                     PREPARED
0B.2c Q1 live no-launch characterization                  OWNER AUTHORIZATION REQUIRED
0B.2d owner numeric margin/policy freeze                  PENDING
0B.3 Option-B implementation                              BLOCKED
0B.4 deterministic regression                             BLOCKED
0B.5 bounded delayed-launch nonscience qualification      BLOCKED
0B.6 owner scientific-pilot review                        BLOCKED
0B.7 fresh randomized science                             BLOCKED
0B.8 randomized scientific analysis                       BLOCKED
0B.9 causal dataset acceptance                            BLOCKED
```

No extra scientific smoke/corridor should be invented between these gates unless a concrete unresolved validity condition requires it.

## Scientific target remains unchanged

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + FAST/T1/C1 baseline
```

Post-treatment FAST/PX4 reactions remain part of the realized closed-loop treatment response.

Action-conditioned World-Model training remains blocked pending causal dataset acceptance.

## Active roadmap — v4 CTEE formalization

The active roadmap revision is:

```text
AURA_WISE_WM_AEGIS_FUTURE_IMPLEMENTATION_ROADMAP_v4_CTEE_20260831.md
```

The exact full artifact is maintained in the Detect and Response Project/File Library. GitHub uses `docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md` as the stable pointer and a v4 retrieval/index view.

The key v4 change is that **after Q1 evidence and explicit owner Option-B freeze**, implementation becomes evidence-gated:

```text
Q1 observed no-GUST waiting trajectory
-> owner freezes M_STABLE_US / W_MAX_US / scheduling / source-time policy
-> static dependency graph extraction
-> Tarjan SCC preflight
-> CTEE benchmark / eligibility qualification
   - PASS / FAIL / UNKNOWN
   - source-bound max-plus T_eligible
   - readiness/blocker diagnostic
   - atomic generation-safe recheck
-> bounded delayed-launch nonscience qualification
```

CTEE is not active now and is not automatically promoted:

```text
CTEE_NAME=PROJECT_PROPOSAL
CTEE_PUBLICATION_NOVELTY=NOT_PROVEN
CTEE_PRIOR_ART_COMPONENTS=STRONG
```

It must be compared against the current/ad-hoc state machine and a conventional timed FSM/timed-guard implementation. If the simpler implementation is equivalent, use it and retire CTEE.

Algorithm responsibilities remain distinct:

```text
Tarjan SCC = static structural/dependency validation
CTEE       = candidate live temporal/causal eligibility
Sweep Line = offline scientific timeline/interval validation
MCMF       = future conditional allocation only
```

None of these algorithms may choose the scientific contract.

## Source Registry

Current research authority:

```text
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v8
UPDATED=2026-08-31
SOURCES=149
RESOLVED=146
UNRESOLVED=3
UNRESOLVED_IDS=SRC-024,SRC-040,SRC-053
```

v8 adds `SRC-142..SRC-149` for Tarjan SCC, max-plus timing, three-valued runtime verification, sweep-line interval validation and conditional network-flow allocation.

Research references remain methodological inputs. They do not freeze Option B, select `M_STABLE_US/W_MAX_US`, authorize 0B.3, change AURA/FAST/T1/C1/E8/H1000/T_D/T_A, change PX4 authority, open SEALED or grant production authority.

Do not invent an unverified v8 JSON path or SHA256.

## Long-term roadmap

After Phase-0 causal dataset acceptance:

```text
end-to-end latency + FFT/FRF characterization
-> AURA detector shadow bake-off
-> World Model v1 model ladder
-> StateBank history ablation
-> T_D -> T_A delay-aware prediction
-> uncertainty-aware prediction
-> WISE-0 candidate enumeration
-> event-triggered WISE
-> TinyMPC / Koopman / RTI only if justified
-> low-dimensional online adaptation
-> formal AEGIS safety/runtime assurance
```

Primary design rules:

```text
minimum complexity that produces demonstrable closed-loop benefit
better causal data > larger model
```

## Storage and authority boundaries

Significant runtime/capture/dataset/training/intermediate artifacts remain under:

```text
/media/nahhao74/KINGSTON
```

`/home` is for source, tests, small deterministic fixtures, configuration and canonical documentation.

```text
SEALED_ACCESS_BOUNDARY=LOCKED_PRE_EVALUATION
FRESH_SCIENCE=BLOCKED
MODEL_TRAINING_ACTION_RESPONSE=BLOCKED_PENDING_CAUSAL_DATA_ACCEPTANCE
production_authority=false
```

Failed scientific roots remain immutable.