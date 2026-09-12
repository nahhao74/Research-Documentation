# Current Status — 2026-09-12

## Executive summary

Detect & Response is now past the earlier E8 pre-offer, native ACK localization, and native-expiry blockers. The active engineering frontier is **native candidate consumption scheduling inside PX4**.

Canonical baseline and scientific target remain:

```text
B = PX4 + AURA + FAST/T1/C1
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
```

```text
CURRENT_BASELINE_B=PX4+AURA+FAST/T1/C1
FAST_ACTIVE_BASELINE=true
AURA_EXECUTION_PHASE_V1=CANONICAL
ATTITUDE_MAX_AGE_US=10000

SCIENTIFIC_ACQUISITION_EXECUTED=false
G_ACTION_CAUSAL_STATUS=UNQUALIFIED
CONTIGUOUS_EXPOSURE_SOURCE_QUALIFIED=false
WM_CONTROL_WRITE=false
WISE_ENABLED=false
AEGIS_WM_AUTHORITY=false
```

PX4 remains final control authority. World Model / WISE remains predictive-only and has no production control authority.

---

## 1. Canonical pipeline

```text
Sensors / PX4 / Reference
  ├─> AURA ─> FAST/T1/C1 ───────────────────────────────┐
  │                                                     │
  └─> StateBank ─> World Model / WISE ─> bounded U_plan│
                                                        v
                                      AEGIS candidate path ─> PX4 ─> UAV
```

World Model conclusion remains unchanged:

```text
F_ENGINEERING_STATUS=USEFUL_SHORT_HORIZON_ENGINEERING_PREDICTION
G_ENGINEERING_STATUS=NO_PREDICTIVE_GAIN_AT_CURRENT_EFFECTIVE_ACTION_SUPPORT
PRIMARY_G_LIMITATION=INSUFFICIENT_EFFECTIVE_ACTION_HORIZON
MODEL_CAPACITY_INCREASE_JUSTIFIED=false
```

The current priority is therefore treatment support and timing, not increasing model capacity.

---

## 2. Canonical treatment/event semantics

The current native event contract distinguishes:

```text
T_U_E8 = first exact source-bound E8 application of the assigned candidate
T_C    = exact native controller consumption cycle for that lineage
T_N    = exact native gate-accepted incorporation into control computation
T_A    = exact lifecycle-bound accepted native status evidence
```

These are different semantic events even when some timestamps happen to coincide.

Treatment expiry remains fixed at:

```text
T_EXP = T_U_E8 + assigned_duration
```

Duration is never restarted at `T_C`, `T_N`, or `T_A`.

---

## 3. Native absolute-expiry enforcement — qualified

The previous defect where an old queued candidate could be incorporated after its treatment deadline has been repaired.

Canonical wire contract:

```text
AEGIS_CANDIDATE_ABSOLUTE_EXPIRY_WIRE_V1
```

Native behavior:

```text
candidate eligible iff native_now_us < candidate_expiry_source_us
candidate rejected iff native_now_us >= candidate_expiry_source_us
```

The expiry gate applies only to the candidate contribution. FAST and the baseline bridge remain independently active.

Qualified result:

```text
POST_EXPIRY_NATIVE_CANDIDATE_INCORPORATION_COUNT=0
NO_DURATION_REBASE_PASS=true
EXPIRED_CANDIDATE_BASELINE_PRESERVATION_PASS=true
ZERO_PARITY_PASS=true
```

Therefore the current blocker is no longer expiry enforcement.

---

## 4. Lockstep host↔native timing bridge — qualified

The EVENT_BRACKET bridge is now qualified using independent native ingress/dequeue/gate holdouts and retained native ULogs under KINGSTON.

Fresh qualification root:

```text
/media/nahhao74/KINGSTON/g_action_lockstep_holdout_20260912_001500/
```

Key result:

```text
CLOCK_BRIDGE_AVAILABLE=true
BRIDGE_INDEPENDENT_HOLDOUT_PASS=true
HOLDOUT_CONTAINMENT_PASS=true
DECOMPOSITION_CLOSURE_STATUS=CLOSED_WITHIN_QUALIFIED_UNCERTAINTY
```

For the three consumed fresh lineages, the measured decomposition was:

```text
SOURCE_TO_E8            = 0–4 ms
E8_TO_NATIVE_INGRESS    = 4–8 ms
NATIVE_INGRESS_TO_DEQUEUE = 12–16 ms
NATIVE_DEQUEUE_TO_GATE  = 0 ms
```

Thus the dominant post-E8 latency component is now source-qualified as:

```text
NATIVE_INGRESS_TO_DEQUEUE_DOMINANT
```

Attitude export/freshness may still affect whether an opportunity is eligible before E8, but it is not the demonstrated cause of the dominant post-E8 delay.

---

## 5. Current root cause — native phase + queue backlog

The latest PX4 scheduling review localized the 12–16 ms native ingress→dequeue delay.

Observed target waits:

```text
Probe 01: 8 ms phase + 8 ms backlog = 16 ms
Probe 03: 4 ms phase + 8 ms backlog = 12 ms
Probe 04: 8 ms phase + 8 ms backlog = 16 ms
```

Classification:

```text
PHASE_PLUS_BACKLOG
```

Mechanism:

```text
candidate ingress
→ uORB multi-slot queue
→ MulticopterPositionControl is triggered by vehicle_local_position, not candidate ingress
→ each eligible controller cycle reads one unread candidate message
→ uORB update() returns the oldest still-retained unread sample
→ newer target candidate can wait behind older baseline / invalid / continuation payloads
```

Measured local behavior:

```text
native candidate ingress ≈ 200 Hz
eligible position-controller consumption ≈ 100 Hz
candidate queue depth = 4
consumption = one unread sample per eligible control cycle
```

Therefore the current problem is not simply “PX4 scheduler is slow”. The demonstrated avoidable component is that the controller can spend an additional cycle consuming an older queued payload while a newer candidate lineage is already resident in native PX4.

---

## 6. Current recommended intervention

The current recommendation is **not** to change treatment duration, source rate, controller frequency, or queue depth blindly.

The next protocol decision is:

```text
NEXT_TASK=OWNER_REVIEW_LATEST_VALUE_CANDIDATE_CONSUMPTION_PROTOCOL
```

The candidate intervention is to review a latest-value / supersession consumption semantic for the existing controller path so that obsolete queued correction state does not unnecessarily delay the current relevant state.

Any future implementation must preserve these invariants:

```text
newer invalid/release state must suppress older candidate state
never fall back to an older valid nonzero candidate
never restart T_EXP
absolute expiry remains checked at the native gate
session/reset invalidation remains authoritative
FAST/baseline remain independent
skipped/superseded generations must not be falsely reported as accepted ACKs
```

This is a protocol-semantic decision and is **not yet implemented or qualified**.

---

## 7. What is closed vs. what is still open

### Closed / qualified

```text
AURA V1 remains canonical
T_U_E8 / T_C / T_N / T_A event separation established
native ACK-loss root cause localized
candidate-scoped absolute expiry enforcement qualified
post-expiry candidate incorporation eliminated
EVENT_BRACKET host↔native clock bridge qualified
independent holdout containment qualified
post-E8 latency decomposition closed for fresh consumed lineages
native ingress→dequeue identified as dominant component
phase + backlog mechanism demonstrated
```

### Still open

```text
latest-value / supersession native consumption semantics
whether the current queue-consumption protocol should be changed
fresh qualification after any approved protocol change
scientifically usable contiguous 8/12 ms treatment support
G_action causal identification and SNR
World Model control authority
WISE production authority
```

---

## 8. Hard invariants

```text
PX4 remains final authority
FAST remains active baseline
AURA_EXECUTION_PHASE_V1=CANONICAL
ATTITUDE_MAX_AGE_US=10000
T_EXP=T_U_E8+assigned_duration
SOURCE_RATE_CHANGED=false
TREATMENT_DURATION_CHANGED=false
WM_CONTROL_WRITE=false
WISE_ENABLED=false
AEGIS_WM_AUTHORITY=false
SCIENTIFIC_ACQUISITION_EXECUTED=false
G_ACTION_CAUSAL_STATUS=UNQUALIFIED
CONTIGUOUS_EXPOSURE_SOURCE_QUALIFIED=false
large runtime/data artifacts=/media/nahhao74/KINGSTON
historical INVALID roots remain immutable
```

## Current status

```text
STATUS=BLOCKED_ON_NATIVE_CANDIDATE_CONSUMPTION_PROTOCOL_DECISION
FIRST_MATERIAL_BLOCKER=old queued correction payload can consume an eligible controller cycle before the newer relevant candidate state
NEXT_TASK=OWNER_REVIEW_LATEST_VALUE_CANDIDATE_CONSUMPTION_PROTOCOL
```
