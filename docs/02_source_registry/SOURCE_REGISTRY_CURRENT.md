# AURA–WISE–WM–AEGIS Source Registry — Current Pointer

## GitHub-archived registry identity

The latest **fully archived and hash-identified registry artifact currently present in this GitHub repository** remains:

```text
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v5
sources=123
resolved=120
unresolved=3
LOCAL_CANONICAL_JSON_SHA256=acc056ca6c475fb00c6173b54b7a4f779446c21588134bdb5ee23571fc1d16a2
LOCAL_CANONICAL_MD_SHA256=5d8221715ce87f61c089ed53e4c0666851551b4a1591c20d4fd2fc40d71df6b7
```

GitHub retrieval index:

- [`CURRENT_REGISTRY_V5.md`](CURRENT_REGISTRY_V5.md) — compact searchable v5 view with exact locators for post-v2 additions through `SRC-123`.

## Project-local research reference

The current future implementation roadmap references:

```text
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v7
```

However, the exact v7 machine-readable artifact, source count, resolved/unresolved counts and canonical hashes are **not archived in this repository at this revision**. Therefore this file does not invent or infer those identities from roadmap source IDs.

Until the exact v7 registry artifact is added and verified:

```text
project research roadmap reference = v7
latest GitHub-verifiable registry snapshot = v5
```

This distinction is deliberate provenance bookkeeping, not a downgrade of the project-local research state.

## Current research directions reflected by the roadmap

The project research set now informs work on:

```text
closed-loop randomized identification
PX4/uXRCE/ROS end-to-end latency
executor and scheduling determinism
AURA change-detection challengers
structured residual / temporal World Models
SINDY / Koopman / sparse GP dynamics
T_D -> T_A delay-aware prediction
uncertainty-aware MPC
bounded candidate enumeration
TinyMPC / RTI / event-triggered WISE
online low-dimensional adaptation
AEGIS safety filters / runtime assurance
```

Exact source title/locator promotion still requires the actual registry entry or independent source verification; the roadmap alone is not a locator authority.

## Archived Library snapshots

Historical entries `SRC-001..SRC-087` remain preserved under `library_snapshots/`:

```text
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v1.json
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v1.md
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v2(1).json
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v2(1).md
```

The v5 GitHub retrieval index extends this historical lineage.

## Unresolved internal identities

```text
SRC-024
SRC-040
SRC-053
```

No similar file should be silently substituted for these unresolved identities.

## Retrieval / authority rule

Prefer:

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
