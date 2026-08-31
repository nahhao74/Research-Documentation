# Algorithmic Research Expansion — 2026-08-31

## AURA–WISE–WORLD MODEL–AEGIS vNext

**Purpose:** record the technical rationale, boundaries, hypotheses, benchmark plans, and source basis for the v9/v5 algorithmic research additions.

**This document is research-only.** It does not authorize Q1, Option B, scientific runtime, control-law changes, or production authority.

---

# 1. Current canonical boundary

```text
PHASE_0B2=OWNER_MARGIN_DECISION_REQUIRED

Q1_NOSCIENCE_NO_LAUNCH_CONTRACT=PREPARED
Q1_RUNTIME_AUTHORIZED=false
Q1_RUNTIME_EXECUTED=false

OPTION_B_CONTRACT_READY=false
IMPLEMENTATION_AUTHORIZED=false

M_STABLE_US=UNFROZEN
W_MAX_US=UNFROZEN

FRESH_SCIENCE=BLOCKED
SEALED=LOCKED_PRE_EVALUATION
production_authority=false
```

No algorithm in this note can substitute for the missing Q1 no-GUST trajectory.

---

# 2. Research question A — Is CTEE actually better than prior art?

CTEE currently proposes:

```text
three-valued source-bound predicates
→ max-plus eligibility frontier
→ readiness/blocker diagnostics
→ atomic generation/session/reset recheck
→ commit / recompute / fail closed
```

However, timed runtime-enforcement literature already supports delaying and suppressing events to satisfy timed specifications.

Therefore CTEE must be benchmarked against a timed-automata/runtime-enforcement baseline, not only against ad-hoc `if`/timeout code.

## 2.1 CTEE's only defensible potential contribution

Not:

```text
"delay until a timing rule passes"
```

Potentially:

```text
pre-treatment causal eligibility
+
source-time / session / reset / generation provenance
+
arm-neutrality by construction
+
max-plus earliest admissible frontier
+
atomic generation-safe commit
+
bounded fail-closed execution
```

for randomized closed-loop UAV interventions.

## 2.2 Required benchmark

```text
A  ad-hoc current state machine
B  conventional timed FSM
C  timed-automata/runtime-enforcement baseline
D  CTEE
```

Metrics:

```text
premature commit
stale-generation commit
UNKNOWN→PASS coercion
session/reset mismatch commit
eligible→commit p50/p95/p99/max
jitter
CPU
memory
deadline misses
invalid-root count
code/state complexity
forensic time
```

If CTEE is not materially better, retire it.

---

# 3. Research question B — CTEE-F

## 3.1 Gap left by CTEE

CTEE answers whether an action is causally admissible **now**.

It does not fully answer whether the data/plan will remain fresh by the time PX4 actually consumes it.

That gap matters because:

```text
StateBank snapshot
→ WM compute
→ WISE compute
→ AEGIS mediation
→ DDS/PX4 scheduling
→ actual acceptance
```

can age the underlying information substantially.

## 3.2 Proposed CTEE-F

**CTEE-F = Causal Temporal Eligibility and Freshness Engine**

Inputs:

```text
CTEE eligibility frontier
source timestamp / age per required item
plan creation time
expiry/freshness budget
measured or justified remaining-delay envelope
session/reset/generation/provenance
```

Core concept:

\[
Age_{consume,i}
=
Age_{now,i}
+
D_{remaining}
\]

Require:

\[
Age_{consume,i}\le Age_{max,i}
\]

for each critical information object.

## 3.3 Expected effect

Potential improvements:

```text
stale-plan execution ↓
prediction applied to wrong state ↓
deadline failures ↓
tail age-at-application ↓
unnecessary reuse of stale plans ↓
effective closed-loop accuracy ↑ indirectly
```

## 3.4 Why Age of Information

AoI explicitly measures freshness of the information available to a monitor/controller, rather than only network latency.

Project candidates:

```text
state_age
reference_age
AURA_age
WM_input_age
prediction_age
plan_age
age_at_PX4_acceptance
```

## 3.5 Why Network Calculus

A plan can be fresh now but become stale before consumption.

A bounded remaining-delay estimate can support a prospective freshness test.

Network Calculus is only admissible if local arrival/service assumptions are measured and validated. It is not a magic bound generator for ROS 2.

## 3.6 Benchmark

Compare:

```text
TTL-only
measured-current-age only
current-age + empirical p99 remaining delay
CTEE-F + justified deterministic delay envelope
```

Evaluate:

```text
stale-at-acceptance rate
tracking degradation under induced scheduling load
deadline miss
recompute rate
plan reuse efficiency
CPU overhead
```

---

# 4. Research question C — CIBES

## 4.1 Motivation

Current Phase-0 science uses a frozen randomized probing design. That should not change.

After Phase-0 closure, later experiments may benefit from choosing safe probing actions that maximize information rather than using equally informative-looking probes everywhere.

## 4.2 Proposed CIBES

**CIBES = Causal Information-Budgeted Excitation Scheduler**

Concept:

\[
U^*
=
\arg\max_U \operatorname{ExpectedInformationGain}(U)
\]

subject to:

```text
candidate-authority limits
vehicle-state operational constraints
CTEE admissibility
carryover/refractory rules
context support
newly frozen randomization/design rule
PX4 authority
```

## 4.3 Expected effect

```text
treatment SNR ↑
G_action identifiability ↑
useful information / flight ↑
required sessions ↓
low-information exposures ↓
```

## 4.4 Critical causal boundary

Adaptive action choice can itself create bias.

Therefore CIBES is not a drop-in optimizer. A new causal/randomization design must define:

```text
decision policy
selection probability
availability
logging
analysis estimand
weights/adjustment if required
```

CIBES is post-Phase-0 only.

---

# 5. Research question D — uncertainty method ladder

## 5.1 Why this matters

WISE should not optimize only a point estimate from the World Model.

A candidate with a slightly better mean but very high uncertainty can be worse than a slightly less optimal but well-supported candidate.

## 5.2 Ladder

```text
U0 residual empirical quantiles
U1 heteroscedastic mean/variance
U2 bootstrap/ensemble
U3 conformal error bounds
U4 set-membership uncertainty
U5 tractable ellipsoidal outer bound
```

## 5.3 Conformal track

Potential benefit:

```text
empirical coverage
OOD-aware uncertainty
reduced overconfidence
risk-aware WISE scoring
```

A 2026 L4DC paper demonstrates conformalized robust OOD MPC on a 12D quadcopter among its benchmarks.

Do not copy its controller architecture directly; use it as evidence that conformal error sets can be integrated into nonlinear robust planning.

## 5.4 Set-membership track

Potential benefit:

```text
explicit set of dynamics consistent with bounded-noise data
robust-control compatible uncertainty
clear worst-case interpretation under assumptions
```

Risk:

```text
sets may be conservative
nonlinear sets may be expensive
noise bounds may be hard to justify
```

Minimum-enclosing-ellipsoid research provides one possible tractable downstream approximation.

## 5.5 Benchmark

```text
coverage
conditional coverage by context
OOD coverage
set width / conservatism
G_action prediction utility
WISE closed-loop utility
constraint-violation rate
runtime
```

---

# 6. Integration order

```text
NOW
Q1 NO-LAUNCH
    ↓
OWNER OPTION-B FREEZE
    ↓
CTEE / TIMED-ENFORCER BENCHMARK
    ↓
DELAYED-LAUNCH NOSCIENCE
    ↓
FRESH RANDOMIZED SCIENCE
    ↓
CAUSAL DATASET ACCEPTANCE

THEN
Latency / FFT / FRF
    ↓
AoI / age-at-application
    ↓
remaining-delay envelope
    ↓
CTEE-F shadow benchmark

POST-PHASE-0 RESEARCH
CIBES

WORLD MODEL
uncertainty ladder
    ↓
conformal / set-membership if justified

WISE / AEGIS
consume calibrated uncertainty/freshness
```

---

# 7. Source mapping

- SRC-150 — Falcone et al., *Runtime enforcement of regular timed properties by suppressing and delaying events*, DOI 10.1016/j.scico.2016.02.008.
- SRC-151 — Prandel & Barreto, *Age of information optimization in cyber–physical systems with stateful packet management techniques*, DOI 10.1016/j.adhoc.2023.103358.
- SRC-152 — Le Boudec & Thiran, *Network Calculus: A Theory of Deterministic Queuing Systems for the Internet*, DOI 10.1007/3-540-45318-0.
- SRC-153 — *Adaptive Experiment Design for Nonlinear System Identification With Operational Constraints*, DOI 10.1109/LSP.2025.3639512.
- SRC-154 — Srinivasan, Leeman & Chou, *Safety Beyond the Training Data: Robust Out-of-Distribution MPC via Conformalized System Level Synthesis*, L4DC 2026.
- SRC-155 — Li et al., *Learning the Uncertainty Sets of Linear Control Systems via Set Membership: A Non-asymptotic Analysis*, ICML 2024.
- SRC-156 — Tang, Lasserre & Yang, *Uncertainty quantification of set-membership estimation in control and perception: Revisiting the minimum enclosing ellipsoid*, L4DC 2024.

---

# 8. Adoption principle

```text
measure
→ identify a real bottleneck
→ choose the smallest credible method
→ compare against a simpler baseline
→ reject if KPI does not improve
```

No algorithm is promoted because it is mathematically interesting.
