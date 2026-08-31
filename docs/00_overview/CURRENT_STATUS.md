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

## Closed 0B.1 causal conclusion

```text
C_TIMING_DESIGN_INTERACTION_BETWEEN_FROZEN_REFERENCE_TRANSITION_PRECEDENCE
AND_THE_PREOFFER_NATIVE_EVENT_WINDOW
```

P1 never reaches candidate offer, scientific `T_D`, ACK or accepted exposure. The failure is therefore not a P1 treatment effect.

## Why historical evidence is insufficient

Existing GUST traces support only reference-side delayed-scheduling chronology. The native GUST already occurred before the proposed delayed-launch stability window, so post-event AURA/C1/reset state cannot stand in for the unexposed no-GUST counterfactual.

```text
FULL_PREDICATE PASS=0
FULL_PREDICATE FAIL=0
FULL_PREDICATE UNKNOWN=3
```

Historical evidence also lacks an exact source-domain `nominal_requested_launch_source_us`, so no defensible full-predicate wait distribution or `W_MAX_US` runtime envelope can be frozen.

## Current next gate — Q1

```text
Q1 — BOUNDED NON-SCIENTIFIC NO-LAUNCH SHADOW QUALIFICATION
```

Q1 must:

```text
identify nominal GUST launch opportunity
→ record nominal_requested_launch_source_us in PX4_BOOT_US
→ suppress native GUST for the complete bounded window
→ observe reference / AURA / C1 / session-reset / continuity / provenance
→ evaluate GUST_PREOFFER_REFERENCE_STABILITY_V1 in SHADOW ONLY
→ terminate without GUST launch, treatment, scientific T_D or manifest use
```

Critical invariant:

```text
NO_NATIVE_GUST_AFTER_NOMINAL_OPPORTUNITY
```

Successful Q1 can create evidence for:

```text
nominal_requested_launch_source_us
first_full_predicate_eligible_source_us
full_counterfactual_wait_us
M_STABLE_US sensitivity
candidate W_MAX full-predicate offline coverage
```

Q1 still cannot establish actual delayed-launch runtime feasibility.

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

Detailed execution-only pointer:

[`CURRENT_EXECUTION_LADDER_PHASE0B2_20260831.md`](CURRENT_EXECUTION_LADDER_PHASE0B2_20260831.md)

## Scientific target remains unchanged

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + FAST/T1/C1 baseline
```

Post-treatment FAST/PX4 reactions remain part of the realized closed-loop treatment response. Action-conditioned World-Model training remains blocked pending causal dataset acceptance.

## Active roadmap — v5

Current roadmap pointer:

[`../04_research/FUTURE_IMPLEMENTATION_ROADMAP.md`](../04_research/FUTURE_IMPLEMENTATION_ROADMAP.md)

Roadmap v5 adds research tracks without changing Phase-0 authority:

```text
CTEE benchmark strengthened with timed runtime-enforcement prior art
CTEE-F = causal eligibility + freshness + justified remaining-delay envelope
Age of Information / age-at-application characterization
Network Calculus as a candidate deterministic-delay methodology
CIBES for post-Phase-0 information-efficient constrained excitation
Conformal / set-membership uncertainty ladder for future WM/WISE/AEGIS
```

### CTEE status

```text
CTEE_NAME=PROJECT_PROPOSAL
CTEE_PUBLICATION_NOVELTY=NOT_PROVEN
CTEE_PRIOR_ART_COMPONENTS=STRONG
```

After Q1 evidence and owner Option-B freeze, compare:

```text
ad-hoc state machine
vs conventional timed FSM
vs timed-automata/runtime-enforcement baseline
vs CTEE
```

Retain CTEE only if it demonstrates material correctness, tail-latency/jitter or engineering/forensic benefit.

### CTEE-F status

```text
CTEE_F_STATUS=RESEARCH_HYPOTHESIS
CTEE_F_CURRENT_PHASE0_DEPENDENCY=false
CTEE_F_DOES_NOT_UNBLOCK_Q1=true
```

CTEE-F belongs after Phase-1 timing/freshness characterization, not in the current Q1/Option-B gate.

### CIBES status

```text
CIBES_STATUS=POST_PHASE0_RESEARCH_HYPOTHESIS
CURRENT_FROZEN_SCIENCE_CHANGE_AUTHORIZED=false
```

CIBES may only be explored under a newly frozen post-Phase-0 experiment design.

Detailed research note:

[`../04_research/ALGORITHMIC_RESEARCH_EXPANSION_CTEE_F_CIBES_UNCERTAINTY_20260831.md`](../04_research/ALGORITHMIC_RESEARCH_EXPANSION_CTEE_F_CIBES_UNCERTAINTY_20260831.md)

## Source Registry

Current research authority:

```text
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v9
UPDATED=2026-08-31
SOURCES=156
RESOLVED=153
UNRESOLVED=3
UNRESOLVED_IDS=SRC-024,SRC-040,SRC-053
```

v9 adds `SRC-150..SRC-156` for timed enforcement, Age of Information, Network Calculus, adaptive experiment design, conformal robust OOD MPC and set-membership uncertainty.

Registry index:

[`../02_source_registry/CURRENT_REGISTRY_V9.md`](../02_source_registry/CURRENT_REGISTRY_V9.md)

Research references remain methodological inputs. They do not freeze Option B, select `M_STABLE_US/W_MAX_US`, authorize 0B.3, alter AURA/FAST/T1/C1/E8/H1000/T_D/T_A, change PX4 authority, open SEALED or grant production authority.

## Long-term roadmap after causal dataset acceptance

```text
end-to-end latency + FFT/FRF
→ AoI / age-at-application / remaining-delay envelope
→ CTEE-F shadow benchmark
→ AURA detector bake-off
→ World Model model ladder
→ history and T_D→T_A delay ablations
→ uncertainty ladder
→ WISE candidate enumeration / event-triggered planning
→ TinyMPC / Koopman / RTI only if justified
→ low-dimensional online adaptation
→ formal AEGIS safety/runtime assurance
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
