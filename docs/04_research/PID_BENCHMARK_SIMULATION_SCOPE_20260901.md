# PID Benchmark — Simulation Scope Override — 2026-09-01

This file narrows the active scope of `research/pid-benchmark-20260901` for the current simulation-only phase.

## Active rule

```text
CURRENT_ENVIRONMENT=SIMULATION
BATTERY_MODELING_REQUIRED=false
BATTERY_SCHEDULING_VARIABLE=false
BATTERY_UNCERTAINTY_SWEEP=false
BATTERY_BENCHMARK_SCENARIO=false
```

Battery-related variables mentioned in the parent research note are **deferred real-flight variables**, not active simulation inputs.

## Active scheduling vector

Start with the smallest scheduling state:

\[
\rho_0=[v_x,v_y,|v|]
\]

Only promote dimensions when held-out evidence shows a material dynamics variation that the smaller schedule cannot represent.

Candidate promoted variables, in preferred order, are:

```text
acceleration / desired acceleration
attitude or tilt
motor/control-allocation headroom
control-effectiveness estimate
```

Battery is excluded from the current promotion ladder.

## Active identified-model uncertainty ensemble

For each operating region, model uncertainty should cover only variables relevant in simulation:

```text
identification uncertainty
mass / payload variation
thrust / control-effectiveness variation
aerodynamic drag / damping variation
actuator bandwidth / lag
transport/controller delay
sensor/filter variation
N/E coupling
```

Do not add battery-voltage or battery-state variation to the current ensemble.

## Active scenario matrix

### Nominal

```text
hover
constant velocity
steps
reversals
ramps
sine
chirp
N/E diagonal motion
```

### Disturbance

```text
GUST ±N / ±E
continuous wind
wind onset/change/clear
crosswind during reference transitions
```

### Model variation

```text
payload / mass variation
thrust/control-effectiveness reduction
actuator bandwidth variation
aerodynamic drag variation
sensor noise/filter variation
```

### Timing

```text
callback/executor delay injection
transport delay variation
source-age variation
```

### Constraint region

```text
saturation
motor/control-allocation headroom reduction
tilt-limit approach
aggressive velocity reversal
```

## Why battery is excluded

In the current simulator, battery is not needed to answer the main benchmark question:

> Can the PID-centered branch match or beat the current AURA/FAST/T1/C1 path on accuracy, disturbance rejection, accepted-action latency, runtime, and implementation complexity?

Keeping a battery dimension would enlarge the identification and gain-scheduling state space without adding useful discriminatory power unless the simulator explicitly models battery-dependent thrust/control effectiveness.

If a later simulation introduces a validated battery-to-thrust/actuator model, battery may be reintroduced only if an ablation shows that direct battery scheduling explains dynamics variation not already captured by measured control effectiveness or motor headroom.

## Real-flight future

Battery becomes relevant again for hardware flight because voltage sag and state-of-charge may change actuator authority and thrust response.

Preferred real-flight policy:

```text
first measure whether battery materially changes control effectiveness
then decide whether to schedule directly on battery
or indirectly on measured thrust/control effectiveness / motor headroom
```

Direct battery scheduling is not automatically preferred.

## Authority

```text
SIMULATION_SCOPE_OVERRIDE=true
CANONICAL_PIPELINE_CHANGED=false
CURRENT_PHASE0_GATE_UNCHANGED=true
PRODUCTION_AUTHORITY=false
```
