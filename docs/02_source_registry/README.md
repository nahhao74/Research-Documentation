# Source Registry

This folder stores the research-source index for the AURA–WISE–WM–AEGIS project.

It is **not** a runtime, scientific-state or roadmap authority.

## Current registry

```text
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v9
UPDATED=2026-08-31
sources=156
resolved=153
unresolved=3
UNRESOLVED_IDS=SRC-024,SRC-040,SRC-053
```

Use only:

- [`CURRENT_REGISTRY_V9.md`](CURRENT_REGISTRY_V9.md) — current retrieval/index view.

Older registry versions are retained by Git history, not as competing `CURRENT_*` files in `main`.

## What the registry is for

Use sources to support:

```text
PX4/runtime implementation interpretation
closed-loop identification methodology
causal/randomized experiment design
FAST challenger research
World-Model / WISE research
latency / freshness / uncertainty research
runtime assurance research
```

A source entry does not promote an algorithm into the active roadmap.

## Adoption rule

```text
named measured problem
→ source/literature review
→ smallest credible mechanism
→ shadow/offline/non-scientific evidence
→ repeat-supported benefit
→ owner/versioned promotion if required
```

If a method is not present in the active roadmap, treat it as reference material only.

## Authority hierarchy for research claims

```text
exact local source/build + captured runtime evidence
→ frozen project contract
→ version-matched official source/docs
→ original scholarly paper / canonical DOI
→ institutional repository
→ secondary synthesis
→ issue/forum/blog hypothesis
```

For current project state and execution permission, always use:

```text
../00_overview/CURRENT_STATUS.md
../00_overview/CURRENT_EXECUTION_LADDER_WM_20260905.md
```

## Unresolved internal identities

```text
SRC-024
SRC-040
SRC-053
```

Do not silently substitute another internal document for these identities.
