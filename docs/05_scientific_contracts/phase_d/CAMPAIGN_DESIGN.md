# Phase-D B0 Campaign Design

## Scope

Phase-D characterizes the frozen baseline:

```text
B0 = PX4 + AURA + current FAST/T1/C1
```

It does not select or implement a FAST challenger.

## Eight frozen scientific conditions

| Slot | Repeat | Worker | Profile | Canonical row |
|---:|---:|---|---|---|
| 1 | 1 | A | STEADY_E | `E8FASTLAT_R1:r1:STEADY_E:A` |
| 2 | 1 | A | GUST_E | `E8FASTLAT_R1:r1:GUST_E:A` |
| 3 | 2 | B | GUST_E | `E8FASTLAT_R1:r2:GUST_E:B` |
| 4 | 2 | B | STEADY_E | `E8FASTLAT_R1:r2:STEADY_E:B` |
| 5 | 3 | A | GUST_E | `E8FASTLAT_R1:r3:GUST_E:A` |
| 6 | 3 | A | STEADY_E | `E8FASTLAT_R1:r3:STEADY_E:A` |
| 7 | 4 | B | STEADY_E | `E8FASTLAT_R1:r4:STEADY_E:B` |
| 8 | 4 | B | GUST_E | `E8FASTLAT_R1:r4:GUST_E:B` |

Balance: four STEADY and four GUST conditions; four rows per worker; both profiles represented on both workers.

## Frozen stimulus/reference

```text
reference body_flu = [0.55, 0.0] m/s
native disturbance world_enu = [0.7, 0.0, 0.0] N
torque = [0, 0, 0] Nm
target link = base_link
event start = 3.0 s
observation horizon = 10.5 s
```

Profiles:

```text
STEADY: rise=0.35 s, duration=6.5 s, fall=0.5 s
GUST:   rise=0.12 s, duration=1.5 s, fall=0.22 s
```

## Frozen causal frontiers

```text
F0 = native disturbance application
F1 = first qualified AURA response
F2 = first exact FAST/T1/C1/E8 emission linked to F1
F3 = first exact PX4 accepted correction linked to F2
F4 = first causally linked ActuatorMotors response
F5 = diagnostic only
```

Latencies:

```text
L_AURA
L_AEGIS
L_ACCEPT
L_ACTUATOR
L_F0_F3
L_F0_F4
```

## Measurement phases

Report separately:

```text
ONSET
SUSTAINED
CLEAR_RECOVERY
```

No pooled controller-goodness score is defined.

## Censoring and validity

Missing F1/F2/F3/F4 is retained as stage-specific censored evidence. Measurement loss is not interpreted as control success. No recovery inside the horizon is retained as `CENSOR_RECOVERY_NOT_ACHIEVED`.

## Prewind rule

The frozen contract requires the qualified `PHASE_D0_PHASE_D_READINESS_V3 / V3_RUNTIME_START_EVENT_V2` gate before native F0.

The owner-approved prospective evidence population is:

```text
[first canonical V2-eligible evaluation, scheduled native F0)
```

At the one-shot F0 opportunity, PASS alone opens F0 eligibility. FAIL/UNKNOWN/infrastructure-invalid block F0 and stop the row. No 20-second dwell, F0 shift, favorable-state wait, dynamic prefix shortening, or retry-until-PASS is allowed.

## Threshold policy

```text
PERFORMANCE_THRESHOLDS=NONE_FROZEN_DESCRIPTIVE_ONLY
ADAPTIVE_CHANGES=false
RETRY_UNTIL_FAVORABLE=false
FAST_CHALLENGER_SELECTION=NOT_PERFORMED
```
