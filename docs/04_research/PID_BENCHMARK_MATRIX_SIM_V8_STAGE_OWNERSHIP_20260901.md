# PID Benchmark Matrix — Simulation v8 — Stage Ownership — 2026-09-01

Branch: `research/pid-benchmark-20260901`

Parent: `PID_BENCHMARK_MATRIX_SIM_V7_SINGLE_NE_LOOP_20260901.md`

This document supersedes v7 as the active stage-ownership matrix. v7 and all
earlier matrices remain historical records; they are not deleted or rewritten.

## 1. Research objective

The PID branch is an independent lightweight replacement-architecture
benchmark against the exact qualified baselines:

```text
C0     = PX4 native
C_FAST = exact qualified AURA + FAST/T1/C1 revision
C_FULL = exact qualified predictive AURA/FAST/WM/WISE/AEGIS revision when available
PID    = qualified replacement N/E velocity-controller arm
```

The PID arm is not an AURA add-on. It reuses project infrastructure and the
same declared vehicle/world/runtime identity, while keeping controller
ownership explicit at every research stage.

## 2. Frozen model and control axes

```text
PRIMARY_CONTROLLER_MODEL_AXES=N,E
D_AXIS_PRIMARY_MODEL=false
PX4_D_AXIS_ALTITUDE_CONTROL=true
YAW_MODEL_FEATURE=false
PRIMARY_YAW_POLICY=fixed_explicit_reference
```

The active operating domain remains:

```text
v_N ∈ {-3, 0, +3} m/s
v_E ∈ {-2.5, 0, +2.5} m/s
```

D position, vertical velocity, tilt, thrust and allocation/headroom remain
safety and coupling observables. They become model or scheduling variables
only when held-out evidence establishes material explanatory value.

## 3. Stage ownership

| Stage | Sole N/E loop | N/E setpoint ownership | Purpose |
|---|---|---|---|
| G0 | boundary under test | external bounded acceleration insertion | prove the mixed-axis accepted-action boundary |
| G1 | none | path/cross-track guidance outputs velocity reference | define bounded path → velocity semantics; not a velocity PID |
| G2 | PX4 native N/E velocity controller | finite native N/E velocity, external N/E acceleration NaN | characterize the native closed-loop direct-force operational envelope |
| G3 | external designed excitation, no native N/E velocity feedback | bounded external `a_N/a_E` | identify `G_NE: [a_N,a_E] → [v_N,v_E]` |
| P0–P4 | one qualified external N/E velocity controller | external controller produces `a_N/a_E` | benchmark the replacement controller ladder |
| Final benchmark | arm-specific declared owner | exact controller identity per arm | compare C0/C_FAST/C_FULL when qualified and PID champion |

The single-loop rule is stage conditional:

```text
SINGLE_NE_LOOP_RULE_IS_STAGE_CONDITIONAL=true

WHEN_EXTERNAL_NE_CONTROLLER_ACTIVE:
    PX4_NE_VELOCITY_CONTROLLER=BYPASSED

DURING_G2:
    PX4_NE_VELOCITY_CONTROLLER=SOLE_NE_VELOCITY_LOOP
```

It is incorrect to disable the native N/E velocity controller during G2 merely
because it is bypassed later in G3 and P0–P4.

## 4. G0 — external acceleration boundary qualification

G0 proves the future external-controller boundary only:

```text
position_N/E     = NaN
velocity_N/E     = NaN
acceleration_N/E = finite bounded external command
position_D       = finite fixed altitude target
velocity_D       = NaN unless exact local PX4 semantics require otherwise
acceleration_D   = NaN external
```

G0 must show accepted finite external N/E acceleration, inactive N/E native
position/velocity feedback, active D/attitude/rate/allocation chain, source
lineage and no hidden N/E integrator. G0 evidence is not native-PX4 G2 force
evidence.

## 5. G1 — bounded path guidance

G1 maps path/cross-track error to bounded `v_N_sp/v_E_sp`. It does not create a
second N/E velocity loop. A pure constant-velocity G2 cell has no invented
cross-track metric unless it declares a spatial path reference.

## 6. G2 — PX4-native direct-force operational envelope

G2 uses one unchanged native PX4 controller through acquisition, settled
pre-force baseline, direct force and recovery:

```text
position_N/E     = NaN
velocity_N/E     = finite target_v_N/E
acceleration_N/E = NaN
position_D       = fixed qualified D target
velocity_D       = NaN unless exact local semantics require otherwise
acceleration_D   = NaN external
yaw              = fixed explicit reference
```

Direct-force campaign values remain `0, 3, 6, 9, 12, 15 N` with the signed
absolute NED directions `+N, -N, +E, -E, NE, NW, SE, SW`. The direct-force
objective is a native-PX4 operational/safety envelope, not an absolute
vehicle physical maximum:

```text
PX4_NATIVE_OPERATIONAL_FORCE_ENVELOPE
```

The old temporary-PID roots are capture/lifecycle diagnostics only and receive
zero corrected G2 or G3 identification credit. The v7 `0–15 N` research
objective is retained; adaptive boundary search is preferred over blind
Cartesian completion. A force boundary stops that operating-point/direction
escalation path, and unrun higher levels are not measured failures.

G2 metrics are phase-separated into acquisition, settled baseline, force active
and recovery. Hard operational boundary, D-axis safety, source validity,
actuator/allocation state and native-PX4 tracking/recovery are reported
separately. CALM anchors distinguish nominal moving-flight demand from added
force demand.

## 7. G3 — 2×2 external-excitation identification

G3 starts only after G2 supplies a sufficiently characterized safe/common
native-PX4 operating envelope. G3 is a different experiment:

```text
position_N/E     = NaN
velocity_N/E     = NaN
acceleration_N/E = designed bounded external excitation
```

Target:

\[
G_{NE}: [a_N,a_E]^T \rightarrow [v_N,v_E]^T.
\]

Preferred analysis chain:

```text
orthogonal N/E multisine
→ FFT / PSD / cross-spectrum / coherence
→ 2×2 MIMO FRF
→ VARX
→ PBSID/subspace/SVD reduction
→ compact state-space or local family only when evidence requires it
```

Native-PX4 G2 data must not be silently promoted to replacement-plant G3 data.
G3 requires accepted external acceleration, valid source/session lineage,
observable `v_N/v_E`, adequate excitation/coherence and no native N/E velocity
feedback ownership in the identification window.

## 8. P0–P4 — replacement controller ladder

Only after G3/model validation:

```text
P0 fixed robust 2DOF PID
P1 robust-optimized 2DOF PID
P2 gain-scheduled PID
P3 P2 + feedforward
P4a P3 + classical INDI
P4b P3 + actuator/control-effectiveness-aware INDI
```

For every P0–P4 runtime:

```text
PX4_NE_POSITION_CONTROLLER = BYPASSED
PX4_NE_VELOCITY_CONTROLLER = BYPASSED
ONE_EXTERNAL_NE_VELOCITY_CONTROLLER = ACTIVE
```

No temporary substitute controller is allowed before G3/P0 qualification.

## 9. Baseline and final comparison

Final comparisons use matched, held-out cases, exact runtime identity and
explicit arm ownership:

```text
C0 vs C_FAST vs C_FULL (when qualified) vs PID champion
```

A PID controller may later extend beyond the native-PX4 G2 envelope. Such an
extension is characterized as a controller-dependent extension and does not
retroactively change the interpretation of the C0/native envelope.

## 10. Superseded recommendations

Earlier recommendations that treated a 3×3 NED controller/model as the active
primary design are historical only. They are superseded by the N/E-only
primary axes above. In particular, no D-axis external PID or 3×3 NED velocity
loop may be introduced into G2, G3 or P0–P4 without a separately documented
owner decision and evidence revision.

The following remain historical and are not deleted:

```text
PID_BENCHMARK_MATRIX_SIM_V7_SINGLE_NE_LOOP_20260901.md
earlier signed-NED matrices and implementation-readiness recommendations
```

## 11. Current gate state

```text
G0=PASS
G2_STAGE_OWNERSHIP=PX4_NATIVE_NE_VELOCITY_BASELINE
G2_MECHANISM_QUALIFICATION=PASS
G2_ADAPTIVE_ENVELOPE=PENDING_NATIVE_CALM_DIRECTION_FORCE_COVERAGE
G3=BLOCKED_ON_G2
P0_TO_P4=BLOCKED_ON_G3
C_FAST_BENCHMARK=BLOCKED_ON_LADDER
```

The next G2 work is adaptive native-PX4 coverage: complete CALM anchors,
measure assist/opposing/cross-force signed directions, then escalate only the
informative paths through the declared force ladder. No G3 acquisition is
authorized by this document.
