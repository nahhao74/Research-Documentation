# Control and Action Path

## 1. Purpose

This document defines how disturbance estimates and predictive candidate actions enter the current closed-loop control system without replacing PX4 authority or the qualified FAST/T1/C1 baseline.

## 2. Baseline control path

The active baseline used both operationally and scientifically is:

```text
Sensors / reference
      -> AURA disturbance estimate
      -> AEGIS FAST/T1/C1
      -> bounded acceleration-correction request
      -> PX4 PositionControl
      -> _accelerationControl()
      -> attitude/rate/control allocation
      -> actuators
```

The scientific baseline is denoted:

```text
B = PX4 + AURA + FAST/T1/C1
```

The baseline is **not disabled** during action-identification experiments.

## 3. FAST semantics

The current fast correction is conceptually:

```text
P_fast,NE = -d_fast,NE
```

where `d_fast` is the causal short-horizon disturbance increment estimated by AURA.

FAST is intended for immediate response. It is not the full predictive controller and does not own low-level flight-control authority.

The insertion point is the qualified PX4 acceleration setpoint/correction boundary before `_accelerationControl()`. This keeps the implementation inside a bounded acceleration domain and lets native PX4 convert the requested acceleration into downstream thrust/attitude behavior.

## 4. T1/C1 retained baseline

T1/C1 remains active as part of the deployed baseline during randomized candidate experiments. This matters scientifically because the target is the effect of an **incremental action under the active controller**, not an isolated candidate in a controller-free plant.

The retired replacement-style branch must not be reintroduced. Candidate semantics are additive augmentation, not replacement of FAST/T1/C1.

## 5. Candidate action path

The predictive candidate uses a bounded additive request:

```text
u_total_requested = u_baseline + u_candidate
```

The candidate is accepted only when its identity is bound to the current session/generation/reset/source frontier and the downstream E8/ACK contract is satisfied.

Important distinction:

```text
requested composition: additive and exact
physical response: nonlinear closed loop
```

Therefore the system does not infer physical superposition from request composition.

## 6. Frozen pilot action arms

Current pilot arms are:

```text
ZERO = exactly 0 accepted nonzero candidate cycles
P1   = 0.008 m/s^2 for exactly 5 accepted cycles
P2   = 0.012 m/s^2 for exactly 7 accepted cycles
```

Directions:

```text
+E
-E
+N
```

The action envelope remains bounded at the already qualified candidate mechanism. No adaptive scaling, compensation cycles, automatic amplitude increase, post-hoc re-dose or arm relabeling is allowed inside the frozen pilot.

## 7. E8 and exact exposure

Scientific exposure is defined at the qualified accepted candidate boundary, not by an intended command that was never accepted.

E8 provides exact candidate identity/ACK semantics. A valid pending candidate awaiting exact ACK must not be overwritten by unrelated baseline publication. Invalid or stale candidate identity fails closed.

For scientific validity:

```text
assigned arm
  -> exact offer identity
  -> exact accepted candidate cycles
  -> explicit release
  -> H1000 candidate-history closure
```

Any P1/P2 accepted-cycle mismatch after scientific `T_D` invalidates the scientific root.

## 8. PX4 downstream reaction

A candidate can change trajectory/state, which can cause FAST, PX4 feedback loops, attitude controller and allocator behavior to react. These post-treatment responses are part of the realized closed-loop causal effect.

They are not automatically confounders.

Correct interpretation:

```text
pre-treatment FAST/PX4 state -> context / moderator candidate
post-treatment FAST/PX4 response -> treatment-mediated closed-loop response
```

## 9. Constraints and projection

Candidate feasibility must be audited at the actual control boundary. Relevant downstream diagnostics include:

- requested candidate norm/component limits;
- acceleration projection/clipping;
- PX4 control constraints;
- allocator saturation/activity;
- control timing/deadline behavior.

Absence of constraint activity in a mechanism qualification does not prove absence under every scientific session. The randomized pilot therefore records constraint/projection/saturation activity as part of treatment-SNR interpretation.

## 10. Operational degradation behavior

If WM/WISE has no acceptable fresh plan:

```text
u_candidate = 0
```

while the baseline remains active:

```text
AURA + FAST/T1/C1 + PX4
```

This is the required safe degradation mode for the predictive layer.

## 11. Authority boundary

The candidate path may suggest bounded acceleration refinement, but it does not own:

- motors;
- raw thrust;
- attitude setpoint authority outside the qualified PX4 path;
- rate loops;
- control allocation policy.

Any future design that moves authority across those boundaries is a material control/safety semantic change and requires explicit owner review.