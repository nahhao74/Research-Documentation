# Source Registry

This folder stores the source registry used to research, audit and extend the AURA–WISE–WM–AEGIS pipeline.

## Current registry identity

The current project-level authoritative identity is:

```text
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v3
sources=94
resolved=91
unresolved=3
SHA256=b4026e78d7da685c11503e594784b00091cdf2f86453bbca198589a2b0d640da
```

The repository currently preserves the available v1/v2 Library snapshots plus the current pointer/authority policy. Historical snapshots are retained for provenance; they do not override a newer authoritative registry.

## Archived Library snapshots

`library_snapshots/` contains:

```text
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v1.json
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v1.md
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v2(1).json
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v2(1).md
```

### v1

87 sources; locators were not yet resolved and were intentionally marked `NEEDS_EXACT_LOCATOR` rather than guessed.

### v2

87 sources; 84 exact/equivalent locators resolved and three internal identities remained unresolved:

```text
SRC-024
SRC-040
SRC-053
```

No similar internal Markdown file should be silently substituted for these identities.

## Retrieval policy

Prefer sources in this order:

```text
OFFICIAL / PRIMARY
-> canonical DOI / original repository
-> institutional repository
-> secondary synthesis for orientation
```

For runtime implementation questions, version-specific local evidence has higher authority than generic upstream documentation:

```text
exact local source/build SHA + captured runtime evidence
> official generic documentation
> issue/forum hypothesis
```

## Primary categories

The registry is intentionally multidisciplinary because the pipeline spans control, causal identification and learned predictive modeling.

Important categories include:

```text
px4_autopilot
uav_control
disturbance_observer
closed_loop_identification
micro_randomized_trials
small_sample_cluster_inference
world_model_ml
stability_robust_control
```

Use [`../04_research/RESEARCH_USAGE_GUIDE.md`](../04_research/RESEARCH_USAGE_GUIDE.md) for guidance on which category should answer which class of pipeline question.

## Important current source families

### PX4

Use official PX4 v1.15 docs/source and exact pinned local code when reasoning about:

- PositionControl;
- control allocation;
- message/reset semantics;
- uXRCE-DDS publication behavior;
- parameters and controller topology.

### eProsima Micro XRCE-DDS

Use official Client/Agent/streams/API documentation when reasoning about transport, output stream preparation, reliability and agent diagnostics.

### Closed-loop identification / randomized intervention

Use original closed-loop ID, experiment-design and MRT/causal-excursion literature for the `G_action` identification design.

### World model / planning

Use FlowPilot, WorldFly, DroneDreamer and related primary sources to inform temporal action representation, world-action prediction and receding-horizon planning.

## Registry maintenance rule

Do not invent a locator. Add or promote a source only when its identity is explicit or independently verified. Authority classification indicates provenance; it does not imply that every claim inside a source is correct.

Project-internal Markdown is context/evidence and becomes normative only when the project explicitly freezes or promotes it.