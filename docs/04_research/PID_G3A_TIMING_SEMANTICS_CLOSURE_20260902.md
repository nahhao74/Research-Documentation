# PID G3-A timing semantics closure — 2026-09-02

Branch: `research/pid-benchmark-20260901`

This memo records the audited timing-semantics closure for the G3-A center scientific acquisition contract. It does not authorize model fitting, controller implementation, or any change to V8 stage ownership.

## 1. Incoming authority

```text
RESEARCH_HEAD_BEFORE_THIS_UPDATE=1510f19ddcdd5562a638625df7fe26f89323e338
G3_Q0_Q3=PASS
G3_BAND_Q=PASS
G3A_SUPPORTED_PROBE_CEILING=2.0_HZ
G3A_CONTRACT_SCIENTIFIC_DESIGN=SUBSTANTIVELY_ACCEPTED
G3A_RUNTIME_AUTHORIZATION=false
```

The timing amendment was performed offline only. No G3-A runtime, FRF/VARX/PBSID, or controller fitting was executed. Research-Documentation was not modified by the execution agent.

## 2. Resolved timing semantic split

The prior ambiguity between raw accepted-action observation continuity, held-action duration, and action/state timing has been closed.

These are distinct objects:

```text
RAW_ACCEPTED_READ_BOUNDARY_GAP
UNIQUE_ACTION_CHANGE_INTERVAL
ACTION_TO_STATE_ASSOCIATION_DELAY
```

A long unique-action-change interval is valid under causal zero-order hold when the raw PX4 read-boundary audit continues to prove the same accepted value. It is not a source gap.

A missing raw read-boundary interval cannot be bridged by assuming that the previously observed action continued.

The scientific alignment remains:

```text
G3_INPUT_ALIGNMENT=CAUSAL_ACCEPTED_ACTION_ZOH
```

For each output timestamp `t_y`, use the latest source-valid accepted action whose PX4 read-boundary timestamp satisfies `t_acc <= t_y`.

## 3. Source-grounded raw continuity rule

The six immutable qualification roots Q0-Q3 and BAND-Q A/B were re-audited under the separated timing semantics.

The qualified maximum raw PX4 accepted read-boundary spacing is:

```text
QUALIFIED_RAW_READ_BOUNDARY_MAX_GAP_US=12000
```

The prospective G3-A raw continuity rule is frozen as:

```text
RAW_READ_BOUNDARY_MAX_GAP_US=24000
```

This is a 2x engineering margin over the qualified 12 ms maximum. The margin applies only to the raw `PositionControllerPublicationAudit` read-boundary observation stream. It is not an allowance for a 24 ms action-change period and it is not derived from sparse postprocessed samples.

If the raw accepted read-boundary gap exceeds 24 ms in a scientific G3-A root, accepted-action provenance across that interval is not guaranteed and the root must fail closed.

## 4. Legacy timing values retired from acceptance authority

The previously retained values:

```text
132000 us
92000 us
```

are legacy diagnostics and no longer have G3-A acceptance-ceiling authority.

They must not be used to:

```text
admit missing raw accepted-action evidence
bridge a raw source discontinuity
justify causal ZOH through an unobserved interval
```

Action/state delay and held-action duration remain reportable diagnostics under their own semantics.

## 5. Validation state

Reported offline closure evidence:

```text
Q0-Q3/BAND-Q roots audited=6/6 PASS
focused timing tests=17 PASS
full PID suite=61 PASS
JSON=PASS
compile=PASS
links=PASS
whitespace=PASS
Codegraph=UP_TO_DATE
runtime launched=NONE
Research-Documentation writes by execution agent=NONE
```

The timing closure is therefore accepted as an implementation-preserving scientific-contract amendment.

## 6. G3-A state after timing closure

```text
G3A_TIMING_SEMANTICS=PASS
RAW_READ_BOUNDARY_MAX_GAP_US=24000
LONG_ZOH_HOLD_WITH_CONTINUOUS_RAW_AUDIT=VALID
LEGACY_132MS_ACCEPTANCE_AUTHORITY=false
LEGACY_92MS_ACCEPTANCE_AUTHORITY=false
G3A_CENTER_SCIENTIFIC_CONTRACT=READY_FOR_OWNER_RUNTIME_AUTHORIZATION
G3A_RUNTIME_STARTED=false
G3_IDENTIFICATION_CREDIT=0
MODEL_TRAINING_CREDIT=0
```

No additional system-identification research is required before the exact frozen G3-A acquisition manifest is executed. The remaining step is owner runtime authorization plus a final exact-identity/preflight check.

## 7. Research Git authority

```text
RESEARCH_GIT_UPDATE_AUTHORITY=CHATGPT_ONLY
LUNA_RESEARCH_COMMIT_AUTHORITY=false
LUNA_RESEARCH_PUSH_AUTHORITY=false
LUNA_RESEARCH_MERGE_AUTHORITY=false
```

Execution agents may report `RESEARCH_UPDATE_RECOMMENDED=true`, but may not modify the Research-Documentation branch.
