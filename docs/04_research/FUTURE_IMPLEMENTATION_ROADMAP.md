# AURA–WISE–WORLD MODEL–AEGIS vNext
## Future Implementation and Research Roadmap

**Canonical active roadmap version:** `v6 — CALE causal-learning formalization / CEORP backend demotion — 2026-08-31`  
**Scope:** Moving Mode only  
**Research registry:** `AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v9`

Full session artifact:

```text
AURA_WISE_WM_AEGIS_FUTURE_IMPLEMENTATION_ROADMAP_v6_CALE_20260831.md
```

Detailed CALE research note:

[`CALE_CAUSAL_ACTION_LEARNING_ENGINE_20260831.md`](CALE_CAUSAL_ACTION_LEARNING_ENGINE_20260831.md)

Current execution ladder:

[`../00_overview/CURRENT_EXECUTION_LADDER_PHASE0B2_20260831.md`](../00_overview/CURRENT_EXECUTION_LADDER_PHASE0B2_20260831.md)

---

# 1. Canonical current state — unchanged by v6

```text
STRUCTURAL_CLEANUP=CLOSED
PHASE_0A_MECHANISM=CLOSED
PHASE_0B1=CLOSED

PHASE_0B2=OWNER_MARGIN_DECISION_REQUIRED

Q1_NOSCIENCE_NO_LAUNCH_CONTRACT=PREPARED
Q1_RUNTIME_AUTHORIZED=false
Q1_RUNTIME_EXECUTED=false

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
# RECOMMENDED, NOT FROZEN, NOT APPROVED

FRESH_SCIENCE=BLOCKED
SEALED=LOCKED_PRE_EVALUATION
production_authority=false
```

No CALE/CEORP work changes the exact next action.

---

# 2. Immediate Phase-0 execution order

```text
OWNER AUTHORIZE Q1 NOSCIENCE NO-LAUNCH RUNTIME
        ↓
Q1 bounded no-launch observation
        ↓
offline full-predicate characterization
        ↓
owner freezes M_STABLE_US / W_MAX_US / scheduling / source-time policy
        ↓
Tarjan dependency preflight
        ↓
CTEE vs timed-FSM vs timed-runtime-enforcement benchmark
        ↓
select smallest qualifying Option-B implementation
        ↓
deterministic regression
        ↓
bounded delayed-launch nonscientific qualification
        ↓
owner scientific review
        ↓
fresh randomized G_action science
        ↓
scientific analysis
        ↓
causal dataset acceptance
```

World-Model action-response training remains blocked until causal dataset acceptance.

---

# 3. v6 research change — CALE becomes the causal-learning abstraction

Claim-level novelty review found strong prior art for:

```text
Laguerre / Kautz / generalized orthonormal basis models
OBF observer + prediction
adaptive OBF/RLS
set-membership and event-triggered update
concurrent-learning informative data selection
closed-loop probing
state-space treatment-effect models
latent intervention-state models
embedded residual prediction
```

Therefore:

```text
CEORP_ORIGINAL_FORMULATION_NOVELTY=TOO_WEAK
CEORP_STATUS=BACKEND_CANDIDATE
CEORP_ROLE=CALE_OBF_IMPLEMENTATION_OPTION
```

The new primary research abstraction is:

```text
CALE — Causal Action Learning Engine
```

CALE is representation-agnostic. Its candidate novelty is not a particular filter; it is the ordering and enforcement of causal action-learning semantics.

---

# 4. CTEE / CTEE-F / CALE responsibility split

```text
CTEE
= may this event/action occur now?

CTEE-F
= if admissible now, will the information/plan remain fresh when consumed?

CALE
= may this completed transaction teach the action-response model?

WISE
= which admissible candidate should be selected?

AEGIS
= what bounded action may actually reach PX4?

PX4
= authoritative inner-loop execution
```

These must remain separate mechanisms.

---

# 5. CALE causal graph

CALE must preserve exact identities:

```text
pre-treatment context X(T_D), z_B(T_D)
        ↓
randomized assignment Z
        ↓
requested action U_requested
        ↓
AEGIS bounds / projection / freshness / generation / authority
        ↓
accepted action U_A at T_A
        ↓
closed-loop response
FAST / PX4 / allocation reactions included
        ↓
future outcome Y(T_A+h)
```

Never collapse:

```text
Z
U_requested
U_A
T_D
T_A
```

because in general:

\[
U_{requested} \ne U_A
\]

under projection, saturation, rejection, timing or authority mediation.

---

# 6. Action-blind pre-treatment context state

A future compact baseline/context state may be:

\[
z^B_{k+1}=A_\phi z^B_k+B_\phi \nu^B_k
\]

with the hard causal property:

\[
z^B(T_D)\in\mathcal F_{T_D^-}
\]

It must not use future treatment information to construct the pre-treatment state.

Forbidden inputs before state freeze include:

```text
future accepted action
future ACK
T_A
future outcome
post-treatment response
```

A FAST/context head may read:

\[
\hat d_{fast}=C_d z^B
\]

while treatment U enters only the separate action-effect head after the pre-treatment context is fixed.

---

# 7. Minimal action-effect model

Initial CALE work should remain linear-in-parameters:

\[
\psi_k(U)=
\begin{bmatrix}
z^B(T_D)\\
X(T_D)\\
U\\
z^B(T_D)\otimes U
\end{bmatrix}
\]

and:

\[
\hat G_h=\Theta_h^\top\psi_k(U)
\]

The scientific target remains:

\[
G^B_{action}(X,U,h)=Y(B+U,h)-Y(B+ZERO,h)
\]

with:

```text
B = active PX4 + AURA + FAST/T1/C1 baseline
```

CALE does not target a bare-airframe model.

---

# 8. Causal Learning Admission Gate

A transaction is not allowed to update parameters merely because it has high error or improves numerical rank.

Define:

\[
q_k^{causal}
=
q_k^{pre}
q_k^{treatment}
q_k^{target}
q_k^{provenance}
\]

Required classes include:

```text
PRE
T_D valid
pre-treatment eligibility valid
exact context session/reset/generation
no future sample

TREATMENT
assignment identity exact
requested action retained
accepted action U_A known
T_A known
ACK/acceptance valid
no stale-generation ambiguity

TARGET
same session/reset
source-bound horizon target exists
no prohibited carryover
no cross-root reconstruction

PROVENANCE
source continuity valid
clock/source-time provenance valid
generation binding exact
transaction identity exact
```

If any required predicate is FAIL or UNKNOWN:

\[
q_k^{causal}=0
\]

and exactly:

\[
\Theta_{k+1}=\Theta_k
\]

for that transaction.

This is intended to become a machine-checkable learning invariant.

---

# 9. Informativeness comes second

Only after:

\[
q_k^{causal}=1
\]

may CALE evaluate numerical informativeness.

\[
q_k^{learn}=q_k^{causal}\,q_k^{info}
\]

Possible q_info criteria:

```text
innovation magnitude
feature novelty
rank / minimum-singular-value improvement
condition-number improvement
context coverage improvement
```

Mandatory ordering:

```text
causal/provenance authority
        ↓
numerical informativeness
        ↓
parameter update
```

This is the central distinction from ordinary event-triggered adaptation, set-membership filtering, and concurrent-learning history selection.

---

# 10. Assignment effect versus accepted-action effect

CALE must preserve two distinct causal targets.

### Assigned-action / ITT-style head

\[
G_Z(X,Z,h)
\]

Question: what is the effect of assigning this randomized candidate under the frozen policy?

### Accepted-action / realized-exposure head

\[
G_U(X,U_A,h)
\]

Question: what is the response to the action that actually crossed the qualified execution boundary?

These are not automatically the same estimand.

Naive conditioning on U_A can destroy randomization because acceptance/projection may depend on state.

---

# 11. Randomization-as-IV research branch

Potential graph:

```text
Z randomized
    ↓
U_requested
    ↓
AEGIS / projection / constraints
    ↓
U_A
    ↓
Y
```

Research question:

> Can randomized assignment Z serve as an instrument for realized accepted action U_A when estimating an accepted-action closed-loop response model?

This branch must explicitly test/justify:

```text
relevance
exclusion restrictions
monotonicity / structural assumptions if required
effect heterogeneity
projection/saturation behavior
context conditioning
arm-neutral availability
```

No IV estimator is authorized merely because Z is randomized.

The frozen assigned-arm estimator remains the scientific reference until a later contract explicitly changes it.

---

# 12. Theory targets

### T1 — pre-treatment non-leakage

If:

\[
z^B(T_D)\in\mathcal F_{T_D^-}
\]

then the context state contains no future treatment information by construction.

### T2 — invalid-transaction non-update

If:

\[
q_k^{causal}=0
\]

then:

\[
\Theta_{k+1}=\Theta_k
\]

exactly.

### T3 — bounded adaptation

With bounded features/residuals and parameter projection, establish a bounded update law.

### T4 — causal identification conditions

Determine assumptions under which admitted randomized transactions identify:

```text
assigned-action causal effects
and/or
accepted-action effects using randomized assignment as an instrument
```

These are research targets, not currently proven properties.

---

# 13. CALE backend ladder

CALE must remain representation-agnostic:

```text
CALE-RIDGE
small offline/online linear reference

CALE-RLS
recursive linear-in-parameters

CALE-OBF
Laguerre/Kautz orthonormal backend
# CEORP-v3 maps here

CALE-DMDc
compact online dynamics challenger

CALE-SINDY
sparse equation challenger

CALE-KOOPMAN
small lifted model

CALE-TINY-MLP
only if structured models are insufficient
```

Representation choice comes after causal correctness.

---

# 14. Embedded/runtime target

Preferred first CALE implementation:

```text
fixed-order state
fixed fast-path iteration count
no dynamic allocation
no online backpropagation
no large matrix inversion on FAST path
no iterative optimizer in estimator
small bounded adaptation path
CPU implementation first
SIMD/HVX/fixed-point only if measured useful
```

Target deployment class:

```text
QCS8550 onboard compute
```

Embedded execution itself is not a novelty claim.

Desired engineering condition:

```text
predictor compute << transport/scheduling latency
```

---

# 15. CALE benchmark / go-no-go

Compare:

```text
A — frozen offline model, no online adaptation
B — ordinary error-triggered RLS/update
C — informative-only concurrent-learning style update
D — CALE causal-admission + informativeness update
```

Scientific-correctness metrics:

```text
invalid transaction update count
cross-generation update count
post-treatment leakage
wrong-action-label rate
requested-vs-accepted mismatch
```

Prediction metrics:

```text
G_action H20/H40/H80 error
CALM/GUST generalization
session generalization
accepted-action prediction error
```

Runtime metrics:

```text
CPU/update
CPU/prediction
p50/p95/p99/max
memory
allocations
deadline misses
```

Engineering metrics:

```text
state count
buffer count
branch count
causal-update LOC
forensic/debug time
```

Retain CALE only if causal admission prevents a real invalid-learning failure class or materially improves robustness/generalization at acceptable runtime cost.

Current state:

```text
CALE_STATUS=RESEARCH_HYPOTHESIS
CALE_PUBLICATION_NOVELTY=PLAUSIBLE_NOT_PROVEN
CALE_EXACT_PRIOR_ART_FOUND=false
CALE_COMPONENT_PRIOR_ART=VERY_STRONG
CALE_CURRENT_PHASE0_RUNTIME_DEPENDENCY=false
CALE_DOES_NOT_UNBLOCK_Q1=true

CEORP_STATUS=BACKEND_CANDIDATE
CEORP_ROLE=CALE_OBF_IMPLEMENTATION_OPTION
```

---

# 16. Other active research tracks retained from v5

```text
Tarjan SCC
→ static dependency-cycle validation

CTEE
→ live temporal/causal eligibility candidate

Sweep Line
→ offline scientific interval validation

CTEE-F
→ eligibility + Age of Information + prospective remaining-delay/freshness

CIBES
→ post-Phase-0 information-efficient constrained experiment design

Conformal / set-membership uncertainty
→ future WM/WISE/AEGIS uncertainty ladder
```

None changes current Phase-0 authority.

---

# 17. Updated long-term experiment sequence

```text
P0-Q1
NO-LAUNCH SHADOW CHARACTERIZATION
        ↓
P0-OWNER
M_STABLE / W_MAX / POLICY FREEZE
        ↓
P0-PREFLIGHT
TARJAN + CTEE/TIMED-ENFORCER BENCHMARK
        ↓
P0-NOSCIENCE
DELAYED-LAUNCH QUALIFICATION
        ↓
P0-SCIENCE
RANDOMIZED G_action PILOT + DATASET DECISION
        ↓
C0
CALE CAUSAL GRAPH + ESTIMAND / IV FORMALIZATION
        ↓
C1
CALE-RIDGE OFFLINE BASELINE
CAUSAL-ADMISSION vs UNRESTRICTED-UPDATE ABLATION
        ↓
C2
CALE BACKEND BENCHMARK
RLS / OBF / DMDc / SINDy / tiny model
        ↓
B2-OPTIONAL
CIBES UNDER NEW EXPERIMENT CONTRACT
        ↓
E1
END-TO-END LATENCY + FFT/FRF
        ↓
E1B
AoI + AGE-AT-APPLICATION + REMAINING-DELAY ENVELOPE
        ↓
E1C
CTEE-F SHADOW BENCHMARK
        ↓
E2
AURA DETECTOR SHADOW BAKE-OFF
        ↓
E3/E4
F_B + G_action MODEL LADDER
        ↓
E5/E6
HISTORY + T_D→T_A DELAY MODEL
        ↓
E7
UNCERTAINTY CALIBRATION
        ↓
E8+
WISE / EVENT-TRIGGERED PLANNING / LIGHTWEIGHT MPC
        ↓
LOW-DIMENSIONAL ADAPTATION
        ↓
FORMAL AEGIS SAFETY FILTER
```

---

# 18. v6 design principle

The research priority is now:

```text
causal estimand and treatment identity
>
learning-admission invariants
>
minimal numerical backend
>
higher model capacity
```

not:

```text
invent a more complicated filter first
```

The overall project principle remains:

```text
measure
→ identify a real bottleneck/failure class
→ introduce the smallest credible mechanism
→ compare against a simpler baseline
→ retain only if measurable benefit is real
```
