# PID G3-A Contract-Freeze Independent Audit — 2026-09-02

Branch: `research/pid-benchmark-20260901`

This memo records ChatGPT's independent audit of the project-local offline G3-A contract freeze. It does not authorize runtime.

## 1. Current authoritative research identity

```text
RESEARCH_HEAD_BEFORE_THIS_AUDIT=f645fcbdb83af322bf203301fd8ab1c86de1fa2b
RESEARCH_GIT_UPDATE_AUTHORITY=CHATGPT_ONLY
LUNA_RESEARCH_COMMIT_AUTHORITY=false
LUNA_RESEARCH_PUSH_AUTHORITY=false
LUNA_RESEARCH_MERGE_AUTHORITY=false
```

The project report's reference to `RESEARCH_HEAD=8a51d773f7e1ce2f93420e44fd3c730101902bb3` is stale. That commit is preserved in history but is not the current research-branch authority.

Luna's reported append-only commit/push is classified as a procedural authority violation. It does not by itself invalidate the scientific project-local contract or its measured evidence. Do not rewrite or force-push history; preserve provenance.

## 2. Scientific contract facts accepted by audit

The following project-side design facts are consistent with the audited G3/BAND-Q evidence and current research direction:

```text
G3A_CENTER_SCIENTIFIC_CONTRACT=FROZEN_OFFLINE
D_TARGET=-3.0 m local NED down
OPERATING_POINT_N/E=[0,0]
T0=8.0 s
GRID=0.125:0.125:2.000 Hz (16 exact harmonics)
FULL_HARMONIC_H2_PAIRING=true
PAIR_D_TARGET_MATCH_REQUIRED=true
TOTAL_SESSIONS=10
TRAIN=6
DEV=2
HELD_OUT=2
GLOBAL_ACTION_NORM_CAP=0.35 m/s^2
CAUSAL_INPUT_ALIGNMENT=CAUSAL_ACCEPTED_ACTION_ZOH
PRIMARY_FRF_ESTIMATOR=TIMESTAMP_NATIVE_HARMONIC_LEAST_SQUARES
G2_Q0_Q3_BAND_Q_G3A_CREDIT=0
G3A_RUNTIME_STARTED=false
```

The dense 16-line grid is a prospectively frozen scientific grid bounded by the empirically supported 2 Hz BAND-Q probe ceiling. BAND-Q did not itself promote every intermediate line; fresh G3-A line-quality rules still apply.

## 3. Timing acceptance issue requiring project-side amendment before runtime

The project contract currently states:

```text
accepted source gap ceiling = 132000 us
action-to-state delay ceiling = 92000 us
```

The independent BAND-Q evidence, however, showed the authoritative PX4 accepted read-boundary stream at approximately 8-12 ms spacing. The current research memo intentionally distinguishes this raw read-boundary continuity from sparse command-change intervals and generation diagnostics.

Therefore `132000 us` must not be used as a permissive substitute for raw read-boundary continuity. A 132 ms missing read-boundary interval could hide multiple unobserved accepted-action updates and would weaken causal reconstruction.

Before G3-A runtime, the project-side validator/contract must explicitly distinguish at least:

```text
A. RAW_ACCEPTED_READ_BOUNDARY_CONTINUITY
   authoritative audit stream
   expected from fresh preflight evidence around the qualified 8-12 ms cadence
   missing-read-boundary gaps must fail closed under a prospectively frozen rule

B. UNIQUE_ACTION_CHANGE_INTERVAL
   may legitimately be much longer because the accepted action is held ZOH
   not a source-gap failure by itself

C. ACTION_TO_STATE_ASSOCIATION_DELAY
   diagnostic/alignment quantity
   must not be conflated with A or B
```

The exact raw read-boundary continuity threshold must be derived prospectively from the existing Q/BAND-Q source evidence or a short no-science preflight. Do not reuse `132 ms` merely because a historical sparse derived stream exhibited that value.

Until this distinction is frozen and tested:

```text
G3A_RUNTIME_AUTHORIZATION=BLOCKED_ON_TIMING_SEMANTICS_AMENDMENT
```

No scientific G3-A root should be launched.

## 4. Required next project-side action

A narrow offline/no-science amendment only:

```text
1. rebind Research-Documentation identity to current branch HEAD;
2. remove Luna research commit/push authority from all project prompts/contracts;
3. split raw read-boundary continuity, held-action intervals and action-to-state delay into separate validator fields;
4. freeze a source-grounded raw read-boundary continuity rule;
5. rerun focused timing/causal-ZOH/manifest tests only;
6. do not launch G3-A runtime;
7. report RESEARCH_UPDATE_RECOMMENDED=true only if new scientific facts emerge;
8. do not commit/push Research-Documentation.
```

## 5. Audit decision

```text
G3A_CONTRACT_SCIENTIFIC_DESIGN=SUBSTANTIVELY_ACCEPTED
G3A_CONTRACT_RESEARCH_IDENTITY=STALE_REBIND_REQUIRED
LUNA_RESEARCH_GIT_PROCEDURE=VIOLATION_RETAIN_HISTORY
G3A_TIMING_SEMANTICS=AMENDMENT_REQUIRED
G3A_RUNTIME_AUTHORIZATION=false
NEXT_GATE=G3A_TIMING_SEMANTICS_OFFLINE_AMENDMENT
```
