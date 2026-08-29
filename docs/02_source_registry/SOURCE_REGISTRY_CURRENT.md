# AURA–WISE–WM–AEGIS Source Registry — Current Pointer

Current authoritative registry identity:

```text
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v5
sources=123
resolved=120
unresolved=3
LOCAL_CANONICAL_JSON_SHA256=acc056ca6c475fb00c6173b54b7a4f779446c21588134bdb5ee23571fc1d16a2
LOCAL_CANONICAL_MD_SHA256=5d8221715ce87f61c089ed53e4c0666851551b4a1591c20d4fd2fc40d71df6b7
```

GitHub retrieval index:

- [`CURRENT_REGISTRY_V5.md`](CURRENT_REGISTRY_V5.md) — compact searchable v5 view with exact locators for all post-v2 additions (`SRC-088..SRC-123`), current authority rules, unresolved identities and research priorities.

Historical entries `SRC-001..SRC-087` remain preserved in the archived Library snapshots under `library_snapshots/`. Registry v5 extends that lineage rather than replacing historical evidence.

## Current v5 focus

### Runtime / latency

- PX4 v1.15 filter/control latency.
- uORB publish/subscribe and queue semantics.
- PX4 task/work-queue scheduling and non-blocking constraints.
- ROS 2 callback-chain response-time analysis.
- Linux PREEMPT_RT and scheduler-latency measurement.

### Predictive control / WISE

- acados and real-time iteration NMPC.
- TinyMPC and embedded quadrotor MPC.
- explicit MPC / multiparametric QP.
- event-triggered robust MPC.
- MPPI as a parallel sampling-based comparator.

### World model / learned dynamics

- TD-MPC2 continuous-control world models.
- physics-inspired temporal quadrotor dynamics.
- residual dynamics / sparse GP-MPC.
- SINDY-MPC and Koopman reduced-order modeling.
- quickest-change detection as future AURA latency research only.

## Archived Library snapshots

Exact historical filenames remain under `library_snapshots/`:

- `AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v1.json`
- `AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v1.md`
- `AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v2(1).json`
- `AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v2(1).md`

These are provenance snapshots and do not override v5.

## Unresolved internal identities

```text
SRC-024
SRC-040
SRC-053
```

No similar file should be silently substituted for these unresolved identities.

## Retrieval / authority rule

Prefer primary or official sources. For runtime causality:

```text
exact local source/build identity
+ captured raw telemetry/counters
> version-matched official source/docs
> primary scholarly source / DOI
> institutional source
> secondary synthesis
> issue/forum hypothesis
```

Issue/forum evidence is hypothesis-generation only unless exact local source/timing evidence matches. Project-internal Markdown is context/evidence and becomes normative only when explicitly frozen/promoted by project governance.

Research references do not authorize changes to frozen scientific/control semantics, safety limits, treatment design or production authority.