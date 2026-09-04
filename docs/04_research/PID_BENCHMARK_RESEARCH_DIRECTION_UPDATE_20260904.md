# PID Benchmark Research Direction Update — 2026-09-04

## Purpose

This note updates the downstream research direction of the independent PID benchmark branch after the latest G3 locked-parent structural forensic and a focused literature review on gain scheduling / LPV control, INDI, disturbance rejection, closed-loop identification, and latency-sensitive UAV control.

This note **does not authorize runtime, new acquisition, R4/R5 response access, model promotion, controller promotion, or changes to the canonical AURA–WISE–WM–AEGIS Phase-0 authority**.

The branch objective remains:

> Find the smallest controller architecture that can achieve fast response, strong tracking and disturbance rejection, low end-to-end latency, deterministic runtime, and robust behavior over the required UAV operating envelope.

---

# 1. Current G3 authority

The latest owner-provided G3 forensic artifacts are:

```text
result(2).json
reverse_peeling_structural_ledger_v3.json
G3_LOCKED_PARENT_STRUCTURAL_ADEQUACY_FORENSIC.md
```

Current classification:

```text
G3_MULTIPLE_STRUCTURAL_MECHANISMS_REMAIN_MIXED_UNRESOLVED
SELECTED_MODEL=NONE
MODEL_PROMOTION=NOT_RUN
NEW_RUNTIME=NOT_RUN
NEW_DATA=false
R4_RESPONSE_USED=false
R5_RESPONSE_OPENED=false
STAGE2_RUNTIME=NOT_AUTHORIZED
```

Current next gate:

```text
READY_FOR_TARGETED_STRUCTURAL_DISCRIMINATION_DESIGN_OWNER_REVIEW
```

The current conclusion is therefore **not** that one LPV model, one higher-order plant, one hidden-state model, or one nonlinear model has been identified.

The evidence supports the narrower statement:

```text
A single locked stationary low-order parent is structurally inadequate,
but the mechanism producing the inadequacy remains mixed / not separated.
```

Active structural hypotheses remain:

```text
H1 — memory / modal-timescale inadequacy
H2 — trajectory / polarity-dependent dynamics
H3 — session / operating-regime-dependent dynamics
H4 — nonlinear / amplitude-regime effects
H5 — plant / noise separation inadequacy
H6 — cross-axis / MIMO structural inadequacy
H7 — hidden non-retained runtime state / history
```

Current evidence status is approximately:

```text
H1  HIGH_PRIORITY_UNRESOLVED
H2  SUPPORTED mechanism evidence, but not a complete explanation
H3  HIGH_PRIORITY_UNRESOLVED
H4  UNRESOLVED_EXISTING_DATA_INSUFFICIENT
H5  HIGH_PRIORITY_UNRESOLVED
H6  UNRESOLVED_EXISTING_DATA_INSUFFICIENT
H7  HIGH_PRIORITY_UNRESOLVED
```

A major forensic signature is:

```text
one-step prediction can be very good
while deterministic free-run prediction remains poor
and residual structure persists over physical lags.
```

This is compatible with accumulated state-transition / structural mismatch, but does **not** by itself prove that model order must increase or that one specific hidden state is missing.

---

# 2. Consequence for downstream controller design

The previous branch direction toward gain scheduling remains plausible, but it must now be stated more carefully.

Current G3 supports:

```text
plant dynamics are not adequately represented by one simple stationary parent
```

It does **not** yet support:

```text
plant dynamics vary as a known function G(s; z) of a qualified measurable z
```

Therefore LPV / gain scheduling is a **downstream hypothesis**, not a current G3 conclusion.

The next structural work must determine whether there exists a scheduling state `z` that is:

```text
measurable before the control action
causal / response-independent
available online at deployment
repeat-supported
well covered by the data support envelope
physically or structurally tied to dynamics variation
not merely a proxy for session identity
not a post-treatment consequence
```

Only after this is established should the identification target be promoted from

\[
G_{NE}(s)
\]

to

\[
G_{NE}(s;z)
\]

or an LPV state-space representation

\[
\dot x=A(z)x+B(z)u,
\qquad
y=C(z)x+D(z)u.
\]

---

# 3. Updated preferred architecture hypothesis

For the stated objective of fast response + good tracking + low end-to-end latency, the strongest **downstream benchmark hypothesis** is now:

\[
\boxed{
\text{causally-qualified robust gain-scheduled 2-DOF PID}
+
\text{delay-synchronized actuator-aware INDI}
}
\]

Working short name:

```text
RGS-2DOF-PID + synchronized actuator-aware INDI
```

This is a hypothesis to benchmark, not a promoted controller.

The reason for this split is functional:

```text
RGS / LPV PID:
    nominal tracking and operating-envelope variation

INDI:
    fast measured-response incremental correction

causal / system-identification layer:
    determine valid model family and valid scheduling variables
```

The causal / identification layer must **not** sit on the first-response realtime path.

---

# 4. LPV / robust gain scheduling role

If structural discrimination establishes a valid scheduling state `z`, use

\[
K(z)=[K_p(z),K_i(z),K_d(z)]
\]

instead of one global gain triplet.

Preferred implementation order remains:

```text
GS0 — fixed robust PID baseline
GS1 — discrete local-gain table
GS2 — linear / barycentric interpolation
GS3 — smooth polynomial / spline schedule
GS4 — LPV-qualified robust schedule
GS5 — RBF / tiny ANN compression only if simple schedules fail
```

Do not begin with ANN scheduling.

A scheduler must include:

```text
bounded gain range
bounded gain-rate change
bumpless transfer
integrator-state continuity
anti-windup
filtered derivative on measurement
scheduler-state validity / freshness
OOD fallback to a certified baseline
```

A gain schedule that works at frozen operating points but creates spikes or instability during transitions fails qualification.

---

# 5. PID synthesis and tuning requirement

The G2/G3 test PID is not a production-tuned controller. Production tuning must remain offline and robust.

For each qualified local plant / operating region, build a plant ensemble

\[
\mathcal G_i=\{G_i^{(1)},\ldots,G_i^{(M)}\}
\]

covering at least the qualified uncertainty dimensions relevant to the branch:

```text
identification uncertainty
payload / mass
transport and execution delay
actuator bandwidth
control effectiveness
sensor / filter variation
battery / thrust effectiveness
aerodynamic variation
```

Candidate robust objective:

\[
K_i^*=
\arg\min_K
\left[
\mathbb E_{G\in\mathcal G_i}J(G,K)
+\lambda\operatorname{Var}(J)
+\mu\max_{G\in\mathcal G_i}J(G,K)
\right]
\]

with hard rejection of unstable candidates.

The cost must not optimize tracking error alone. It should include:

```text
ITAE / IAE
peak error
overshoot
settling time
control effort
command total variation
jerk
saturation exposure
robustness / worst-case penalty
```

Frequency-domain robustness should be added to qualification where the identified model supports it:

```text
gain margin
phase margin
delay margin
crossover bandwidth
peak sensitivity Ms = ||S||∞
complementary sensitivity / high-frequency amplification
control sensitivity
```

A controller with slightly slower nominal settling but materially better delay and robustness margins may be preferable to a nominally faster but fragile candidate.

---

# 6. Why INDI is promoted as the strongest fast augmentation hypothesis

INDI is attractive for this branch because fast correction is based on measured incremental response rather than requiring a long-horizon free-running plant prediction at every control update.

Conceptually:

\[
\dot y\approx \dot y_0+G_u\Delta u
\]

and

\[
\Delta u\approx G_u^{-1}(\nu-\dot y_0).
\]

This is especially relevant to the current G3 signature where one-step behavior can be captured much better than long free-run behavior.

However, INDI is **not** assumed to solve G3 structural identification. G3 must still determine the nominal / parameter-varying dynamics needed for controller synthesis and fair comparison.

INDI benchmark variants should be ordered as:

```text
INDI0 — classical measured-response INDI
INDI1 — delay-synchronized / filtered INDI
INDI2 — actuator-dynamics-aware INDI
INDI3 — bounded control-effectiveness-adaptive INDI
```

No higher variant is retained unless ablation shows a measurable benefit.

---

# 7. Latency qualification for INDI and all fast paths

Low controller arithmetic time is not sufficient.

The branch must freeze four separate latency metrics:

```text
T_compute
    source sample → controller output

T_accept
    source sample → PX4 accepted-action boundary

T_effect
    physical event / disturbance onset → first useful corrective physical action

T_recover
    physical event / disturbance onset → defined recovery threshold
```

Report at least:

```text
p50
p95
p99
max
jitter
```

Champion selection should prioritize `T_effect` and `T_recover`, not only `T_compute`.

For INDI specifically, qualification must measure:

```text
measurement source age
acceleration / derivative estimate age
filter group delay
relative delay between measured-response and command paths
actuator model / actuator response delay
control-effectiveness freshness
PX4 accepted-action latency
synchronization mismatch
```

Increasing controller update frequency does not automatically improve physical response if sensor, filter, actuator, or command-path delays dominate.

---

# 8. AURA / FAST relationship to INDI

AURA/FAST and INDI must not be treated as identical algorithms.

Their architectural roles overlap sufficiently that the PID branch should first treat them as **alternative fast augmentation arms**.

After a qualified nominal fixed or scheduled PID exists, benchmark:

```text
F0 — nominal PID only
F1 — nominal PID + INDI
F2 — nominal PID + AURA/FAST
```

Only if F1 and F2 show complementary residual benefit should the branch open:

```text
F3 — nominal PID + INDI + AURA/FAST
```

Do not assume F3 is better simply because it contains more mechanisms.

Ablation metrics must include:

```text
T_effect
T_recover
peak deviation
ITAE / integrated disturbance error
command TV / jerk
saturation exposure
control-allocation / motor headroom
CPU / update cost
jitter
source age / freshness
```

The existing branch rule remains authoritative:

```text
retain each layer only if an ablation shows a measurable benefit.
```

---

# 9. DOB / ESO / additional observers

DOB / NDO / ESO remain lower-priority optional augmentations.

Reason:

```text
INDI and AURA/FAST already target fast model-error / disturbance rejection.
An additional observer may duplicate function and add filtering / estimation delay.
```

Open an observer arm only if a qualified residual disturbance component remains after simpler fast-path candidates are evaluated.

---

# 10. Causal model / causal-identification role

The causal model is not proposed as a large realtime action generator.

Its role is scientific and supervisory:

```text
identify which variables are valid pre-treatment moderators / scheduling variables
separate assigned, requested and accepted action identities
preserve source-time causal order
prevent post-treatment variables from being used as causes
avoid session labels being mistaken for physical scheduling states
admit learning only from causally valid transactions
```

The relevant causal chain is:

\[
X(T_D),z(T_D)
\rightarrow
U_{requested}
\rightarrow
U_A(T_A)
\rightarrow
Y(t>T_A).
\]

For G3, the action identity remains the qualified accepted PX4-side action boundary with source-time causal accepted-action ZOH.

A candidate `z` for LPV scheduling must be available before the relevant action / response and cannot be selected solely because it correlates with downstream response.

This preserves compatibility with the broader CALE research direction in the main project without making CALE a current PID runtime dependency.

---

# 11. Updated benchmark ladder

The controller ladder is revised to make the fast-path alternatives explicit:

```text
P0 — FIXED-2DOF-PID
     one robust globally tuned controller

P1 — LOCAL-ROBUST-2DOF-PID
     local robust gains only after structural / operating regions are qualified

P2 — RGS-2DOF-PID
     causally-qualified bounded gain schedule

P3 — RGS-2DOF-PID-FF
     optional bounded physics / identified feedforward

P4A — RGS-2DOF-PID-INDI
      synchronized actuator-aware measured-response fast augmentation

P4B — RGS-2DOF-PID-AURA-FAST
      existing disturbance-estimate fast augmentation under matched authority

P5 — COMBINED-FAST-AUGMENTATION
     only if P4A and P4B show repeat-supported complementary benefit

P6 — DOB / observer augmentation
     only if a residual disturbance class remains

P7 — ANN / RBF gain-surface compression
     only if low-dimensional deterministic schedules are inadequate
```

Current downstream champion **hypothesis before evidence**:

```text
P4A — RGS-2DOF-PID + synchronized actuator-aware INDI
```

This is not a promotion decision.

---

# 12. Immediate scientific order

The current branch must **not** jump directly to P2/P4A implementation.

The required order is:

```text
1. G3 targeted structural discrimination design
2. determine whether a deployable causal scheduling state z exists
3. establish a credible stationary plant or plant family / uncertainty ensemble
4. synthesize and qualify robust fixed PID baseline
5. synthesize scheduled / LPV PID only if z is supported
6. benchmark fast augmentation arms under identical authority and timing
7. retain only layers with repeat-supported benefit
```

If no adequate measurable `z` is found, the preferred fallback is:

```text
robust fixed 2-DOF PID
+
qualified fast incremental correction
```

rather than forcing an unsupported LPV schedule.

---

# 13. Targeted structural-discrimination principle

The next G3 campaign should maximize discrimination between hypotheses, not simply collect more sessions.

Conceptual contrasts include:

```text
same operating state / different trajectory history
    → trajectory / memory discrimination

same trajectory family / different operating regime
    → regime dependence

same trajectory and regime / controlled amplitude contrast
    → amplitude nonlinearity

same physical experiment / richer source-state retention
    → hidden runtime-state testability

qualified low-frequency / longer-timescale excitation
    → memory / modal-timescale discrimination
```

These are design principles only. Exact acquisition, source instrumentation, amplitude, duration, R4/R5 access, and runtime authority remain subject to owner review.

---

# 14. Literature support

The following sources strengthen the downstream architecture hypothesis but do not override project evidence or authorize implementation.

## Gain scheduling / LPV

1. Wilson J. Rugh, Jeff S. Shamma, **Research on Gain Scheduling**, Automatica 36(10), 2000, 1401–1425. DOI: `10.1016/S0005-1098(00)00058-3`.
   - Survey of gain scheduling, flight-control applications, linearization-based scheduling and LPV approaches.

2. Jeff S. Shamma, Michael Athans, **Gain Scheduling: Potential Hazards and Possible Remedies**, IEEE Control Systems Magazine 12(3), 1992, 101–107. DOI: `10.1109/37.165527`.
   - Important warning that frozen-point controller quality does not by itself guarantee safe behavior when scheduling variables vary.

3. Pierre Apkarian, Pascal Gahinet, Greg Becker, **Self-Scheduled H∞ Control of Linear Parameter-Varying Systems: A Design Example**, Automatica 31(9), 1995, 1251–1261. DOI: `10.1016/0005-1098(95)00038-X`.
   - Formal LPV / self-scheduled robust-control foundation.

## INDI / UAV disturbance rejection

4. Ewoud J. J. Smeur, Guido C. H. E. de Croon, Qiping Chu, **Cascaded Incremental Nonlinear Dynamic Inversion for MAV Disturbance Rejection**, Control Engineering Practice 73, 2018, 79–90. DOI: `10.1016/j.conengprac.2018.01.003`.
   - Quadrotor flight experiments demonstrate strong disturbance-rejection performance for cascaded INDI and explicitly compare against PID.

5. T. S. C. Pollack, Spilios Theodoulis, Xuerui Wang, **Duality Between Incremental Nonlinear Dynamic Inversion and Quasi-Linear Parameter-Varying Control**, Journal of Guidance, Control, and Dynamics 49(7), 2026, 1897–1913. DOI: `10.2514/1.G009559`.
   - Provides a q-LPV framework for robust analysis / synthesis of INDI and NDI-based closed-loop systems; supports viewing LPV and INDI as potentially compatible rather than unrelated design paradigms.

6. Tom Aantjes, Till M. Blaha, Spilios Theodoulis, Ewoud J. J. Smeur, **Robust H∞ Controller Design for INDI-Controlled Quadrotor Using Online Parameter Identification**, ICUAS 2026, 561–568. DOI: `10.1109/ICUAS69441.2026.11598602`.
   - Demonstrates a robust gain-scheduled cascaded controller around an INDI-controlled quadrotor with online parameter identification and flight-test evaluation.

---

# 15. Current decision summary

```text
G3_CURRENT_STATE=
    G3_MULTIPLE_STRUCTURAL_MECHANISMS_REMAIN_MIXED_UNRESOLVED

CURRENT_GATE=
    READY_FOR_TARGETED_STRUCTURAL_DISCRIMINATION_DESIGN_OWNER_REVIEW

LPV_STATUS=
    DOWNSTREAM_HYPOTHESIS_REQUIRES_CAUSALLY_QUALIFIED_SCHEDULING_STATE

INDI_STATUS=
    STRONGEST_FAST_AUGMENTATION_HYPOTHESIS_FOR_PID_BRANCH

AURA_FAST_STATUS=
    FAST_AUGMENTATION_COMPARATOR_ALTERNATIVE_TO_INDI_BEFORE_COMBINATION

CAUSAL_MODEL_STATUS=
    IDENTIFICATION_AND_SUPERVISORY_ROLE_NOT_FIRST_RESPONSE_RUNTIME_PATH

CURRENT_CHAMPION_HYPOTHESIS=
    CAUSALLY_QUALIFIED_RGS_2DOF_PID_PLUS_SYNCHRONIZED_ACTUATOR_AWARE_INDI

SELECTED_CONTROLLER=NONE
IMPLEMENTATION_AUTHORIZED=false
NEW_RUNTIME_AUTHORIZED=false
```
