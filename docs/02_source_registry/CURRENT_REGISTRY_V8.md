# AURA–WISE–WM–AEGIS Source Registry v8 — GitHub Retrieval Index

## Registry identity

```text
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v8
UPDATED=2026-08-31
SOURCES=149
RESOLVED=146
UNRESOLVED=3
UNRESOLVED_IDS=SRC-024,SRC-040,SRC-053
```

The exact canonical v8 Markdown registry is maintained in the Detect and Response Project/File Library as `AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v8.md`.

This GitHub document is a retrieval/index view for the v8 promotion. No v8 JSON path or SHA256 is inferred unless an exact JSON artifact is separately verified/promoted.

No existing source ID was deleted or renumbered.

## v8 targeted additions — `SRC-142..SRC-149`

| ID | Topic | Intended project role | Boundary |
|---|---|---|---|
| `SRC-142` | Tarjan SCC / DFS graph algorithms | static/preflight detection of causal/dependency cycles | does not prove dependency graph completeness or scientific correctness |
| `SRC-143` | max-plus algebra for synchronized discrete-event systems | basis for latest-prerequisite / earliest-eligible source-time frontier | does not define Option-B numeric margins |
| `SRC-144` | MPC for max-plus-linear discrete-event systems | evidence for constrained event-time reasoning | not a replacement for WISE/PX4 MPC |
| `SRC-145` | strict temporal constraints with max-plus algebra | temporal-constraint validation / event scheduling after contract freeze | no authority to freeze scheduling policy |
| `SRC-146` | three-valued runtime verification | preserve `PASS / FAIL / UNKNOWN` under partial observations | bitmask implementation remains project-specific |
| `SRC-147` | sweep-based temporal interval joins | offline overlap validation for scientific intervals | offline validator, not live control law |
| `SRC-148` | Bentley–Ottmann sweep-line foundation | event-ordered active-set scanning foundation | one-dimensional timeline adaptation is project-specific |
| `SRC-149` | Network Flows | future constrained allocation / matching | not a Phase-0 runtime dependency |

## Evidence-gated adoption policy

```text
candidate algorithm
→ named project bottleneck / failure class
→ measurable KPI
→ simplest baseline comparison
→ shadow / offline / preflight evidence
→ retain only if benefit is demonstrated
```

Current priority:

```text
P0 after Q1 + owner freeze:
Tarjan SCC preflight
+ three-valued predicate monitoring
+ source-bound max-plus temporal frontier
+ atomic recheck / fail-closed eligibility

P0/P1 validation:
sweep-line interval audit

Future / conditional:
minimum-cost flow / matching
```

The roadmap v4 groups the live temporal/causal eligibility pieces under the project proposal **CTEE — Causal Temporal Eligibility Engine**. CTEE is not a new flight-control law and does not replace AURA, FAST/T1/C1, World Model, WISE, AEGIS or PX4 inner loops.

```text
CTEE_NAME=PROJECT_PROPOSAL
CTEE_PUBLICATION_NOVELTY=NOT_PROVEN
CTEE_PRIOR_ART_COMPONENTS=STRONG
```

CTEE must be benchmarked against simpler timed-state-machine/timed-guard implementations. If it provides no material correctness, tail-latency/jitter or engineering-complexity benefit, the simpler implementation should be retained instead.

## Explicitly not promoted

The following remain outside the active roadmap unless a matching project problem appears:

```text
Manacher
SPFA
generic Prim/Kruskal MST
Chu-Liu/Edmonds
centroid decomposition
heavy-light decomposition
Euler-tour/LCA
Kruskal reconstruction tree
Lagrange interpolation in causal eligibility/target construction
```

## Scientific/control authority boundary

Registry references are methodological/research inputs only. They do **not**:

```text
freeze Option B
select M_STABLE_US
select W_MAX_US
authorize 0B.3
change AURA semantics
change FAST/T1/C1
change E8 or H1000
change T_D/T_A
change PX4 authority
open SEALED
grant production authority
```

For exact runtime/scientific claims, local pinned source/tests/raw evidence and explicitly frozen project contracts remain authoritative.