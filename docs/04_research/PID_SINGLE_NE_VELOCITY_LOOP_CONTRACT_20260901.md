# PID Single N/E Velocity Loop Contract — 2026-09-01

Branch: `research/pid-benchmark-20260901`

## Decision

The PID research branch shall not stack a new N/E velocity PID on top of the PX4 N/E velocity PID.

There must be exactly one N/E velocity feedback controller when the experimental controller is active.

```text
EXTERNAL_NE_VELOCITY_CONTROLLER = ACTIVE
PX4_NE_POSITION_CONTROLLER = BYPASSED
PX4_NE_VELOCITY_CONTROLLER = BYPASSED
PX4_D_AXIS_CONTROLLER = ACTIVE
PX4_ATTITUDE_CONTROLLER = ACTIVE
PX4_RATE_CONTROLLER = ACTIVE
PX4_CONTROL_ALLOCATION = ACTIVE
```

This preserves the normal cascade across different physical states while preventing duplicate feedback on the same velocity error.

## Active NED command boundary

External controller owns only N/E velocity tracking:

\[
[v_{N,sp},v_{E,sp}]
\rightarrow
C_{NE}
\rightarrow
[a_{N,sp},a_{E,sp}].
\]

The PX4-facing setpoint contract during external N/E control is:

```text
position_N     = NaN
position_E     = NaN
velocity_N     = NaN
velocity_E     = NaN
acceleration_N = finite external command
acceleration_E = finite external command
```

Do not simultaneously send finite N/E velocity and finite external acceleration during the primary architecture. In PX4 this would make the finite acceleration act as feedforward to the PX4 velocity controller, recreating two N/E feedback paths.

## D-axis ownership

The D axis is not part of the experimental N/E model/control objective.

Preferred primary campaign contract:

```text
position_D     = fixed z/altitude reference
velocity_D     = NaN unless required by the exact PX4 mode contract
acceleration_D = NaN external
```

PX4 therefore retains altitude/vertical control and internally generates the D-axis acceleration required to combine with the external N/E acceleration vector.

D-axis quantities remain mandatory safety/coupling monitors:

```text
z / D position error
v_D
vertical acceleration
thrust
peak tilt
motor/control-allocation headroom
saturation count/duration
```

A controller that improves N/E tracking while causing unacceptable altitude loss, vertical drift, saturation, or headroom collapse fails the benchmark.

## Yaw

Yaw is not a model feature or scheduling variable.

Primary identification/control campaign shall use an explicit fixed yaw reference rather than an omitted/NaN yaw reference, so heading drift cannot contaminate N/E identification.

```text
YAW_MODEL_FEATURE=false
YAW_SCHEDULING_VARIABLE=false
PRIMARY_YAW_REFERENCE=FIXED
```

Separate yaw-invariance tests may later repeat selected cases at multiple headings.

## Guidance loop is not a second velocity PID

The outer path/cross-track logic is a guidance law that modifies the desired N/E velocity vector; it is not a second controller acting on the same velocity error.

Example bounded guidance law:

\[
e_\perp=(I-tt^T)(p-p_{line})
\]

\[
v_{ct}=-v_{ct,max}\tanh(e_\perp/e_0)
\]

\[
v_{sp}=v_{nominal}+v_{ct}.
\]

Only the single N/E velocity controller converts `v_sp - v` into N/E acceleration.

## Experimental plant boundary

The preferred identified plant is:

\[
G_{NE}: [a_N,a_E]\rightarrow[v_N,v_E].
\]

The plant intentionally contains:

```text
PX4 acceleration-to-thrust/attitude conversion
PX4 attitude controller
PX4 rate controller
PX4 control allocation
actuator dynamics
vehicle dynamics/aerodynamics
PX4 D-axis altitude control interaction
```

It intentionally excludes the PX4 N/E position and velocity controllers.

This avoids circular identification of the controller that the research branch is attempting to replace.

## G0 exact-local smoke gate

Public PX4 v1.15.4 source supports the intended mixed-axis semantics, but the project uses an exact local PX4 revision and runtime integration. Therefore this architecture remains source-supported but not runtime-qualified until an exact-local smoke is completed.

### G0-A — external N/E acceleration path

Use a bounded small excitation, for example:

\[
a_N=A\sin(2\pi f t),\quad a_E=0
\]

while altitude and yaw references are fixed.

Require evidence for:

```text
external a_N accepted at PX4 boundary
N/E velocity setpoints remain NaN/inactive
no PX4 N/E velocity-PID contribution
no hidden N/E integrator accumulation
D-axis controller remains active
attitude/rate/allocation remain active
vehicle responds on N as expected
```

### G0-B — PX4-native velocity path control comparison

For forensic comparison only, run a separate case using PX4-native N/E velocity control with external N/E acceleration disabled.

Do not combine G0-A and G0-B control semantics in the benchmark controller.

### Forbidden mixed case

```text
velocity_N/E = finite
external acceleration_N/E = finite PID output
```

is not an authorized primary architecture because it reintroduces the PX4 N/E velocity feedback loop.

## Runtime assertions

When the external N/E controller is active, the bridge/runtime should fail closed if the setpoint semantics violate the single-loop contract.

Equivalent assertions:

```text
isnan(position_N)
isnan(position_E)
isnan(velocity_N)
isnan(velocity_E)
isfinite(acceleration_N)
isfinite(acceleration_E)
```

The exact local message/interface representation must be audited before implementation; do not assume middleware NaN propagation without verification.

## Interaction with PID / INDI / H-infinity challengers

All candidate N/E controllers share the same single-loop boundary:

```text
P0 fixed 2DOF PID
P1 robust optimized PID
P2 gain-scheduled PID
P3 PID + feedforward
P4a PID + classical INDI
P4b PID + actuator/control-effectiveness-aware INDI
H0 low-order structured H-infinity
H1 low-order structured H-infinity + INDI
E0 PID + ESO/DOB challenger
```

No candidate is allowed to gain performance by leaving the PX4 N/E velocity controller active beneath it.

## Acceptance state

```text
SINGLE_NE_VELOCITY_LOOP_ARCHITECTURE=FROZEN_FOR_G0_SMOKE
DOUBLE_NE_VELOCITY_PID=PROHIBITED
MIXED_AXIS_PX4_BOUNDARY=SOURCE_SUPPORTED_RUNTIME_UNQUALIFIED
G0_EXACT_LOCAL_SMOKE=REQUIRED
FULL_CONTROLLER_IMPLEMENTATION=BLOCKED_ON_G0
CANONICAL_MAIN_CHANGED=false
PRODUCTION_AUTHORITY=false
```
