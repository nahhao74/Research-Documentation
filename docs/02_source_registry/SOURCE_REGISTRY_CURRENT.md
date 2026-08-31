# AURA–WISE–WM–AEGIS Source Registry — Current Pointer

## Current registry identity

The current project research registry is:

```text
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v8
UPDATED=2026-08-31
sources=149
resolved=146
unresolved=3
LIBRARY_ARTIFACT=AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v8.md
```

Unresolved internal identities remain:

```text
SRC-024
SRC-040
SRC-053
```

GitHub retrieval index:

- [`CURRENT_REGISTRY_V8.md`](CURRENT_REGISTRY_V8.md) — current v8 retrieval/index view, including `SRC-142..SRC-149` and evidence-gated algorithm adoption policy.
- [`CURRENT_REGISTRY_V7.md`](CURRENT_REGISTRY_V7.md) — historical v7 retrieval view through `SRC-141`.
- [`CURRENT_REGISTRY_V5.md`](CURRENT_REGISTRY_V5.md) — historical retrieval index through `SRC-123`.
- `library_snapshots/` — historical v1/v2 snapshots.

The exact v8 Markdown artifact exists in the Detect and Response Project/File Library and is the current research authority. A v8 machine-readable JSON artifact/path/hash has not been promoted into this repository, so none is inferred here.

## Registry lineage

```text
v1  87 sources; locators initially unresolved
v2  87 sources; 84 resolved / 3 unresolved
v3  SRC-088..SRC-094: pinned PX4/uXRCE/eProsima runtime references
v4  SRC-095..SRC-103: PX4/Gazebo + uncertainty/planning/residual research
v5  SRC-104..SRC-123: latency, scheduling, embedded predictive control, World Model research
v6  SRC-124..SRC-127: learned residual MPC, online dynamics, latency-aware control, safety filter
v7  SRC-128..SRC-141: DDS/ROS 2 latency, change detection, learned/adaptive disturbance modeling, runtime assurance
v8  SRC-142..SRC-149: dependency correctness, causal temporal eligibility, three-valued monitoring, interval validation, conditional network-flow allocation
```

No existing source ID was deleted or renumbered in v8.

## v8 algorithmic-infrastructure direction

The v8 set adds methodological support for:

```text
Tarjan SCC
  -> static/preflight dependency-cycle detection

three-valued monitoring
  -> PASS / FAIL / UNKNOWN preservation

max-plus temporal reasoning
  -> source-bound earliest eligibility frontier

Sweep Line
  -> offline scientific interval/exposure validation

MCMF / network flow
  -> future constrained allocation only when a real allocation problem exists
```

Roadmap v4 groups the live temporal/causal eligibility proposal under **CTEE — Causal Temporal Eligibility Engine**. CTEE is a project proposal, not a new flight-control law and not an authorized production component.

```text
CTEE_NAME=PROJECT_PROPOSAL
CTEE_PUBLICATION_NOVELTY=NOT_PROVEN
CTEE_PRIOR_ART_COMPONENTS=STRONG
```

CTEE remains benchmark-gated against simpler timed-state-machine/timed-guard baselines.

## Evidence-gated adoption rule

```text
candidate algorithm
-> named project bottleneck / failure class
-> measurable KPI
-> simplest baseline comparison
-> shadow / offline / preflight evidence
-> retain only if benefit is demonstrated
```

## Retrieval / authority rule

Prefer:

```text
exact local source/build identity
+ captured raw telemetry/counters
> explicitly frozen project contract
> version-matched official source/docs
> primary scholarly source / DOI
> institutional source
> secondary synthesis
> issue/forum hypothesis
```

Issue/forum evidence is hypothesis-generation only unless exact local source/timing evidence matches. Research references do not authorize changes to frozen scientific/control semantics, safety limits, treatment design, acceptance criteria, SEALED, or production authority.