# AURA–WISE–WM–AEGIS Source Registry — Current Pointer

Current authoritative registry:

```text
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v3
sources=94
resolved=91
unresolved=3
SHA256=b4026e78d7da685c11503e594784b00091cdf2f86453bbca198589a2b0d640da
```

The full registry is maintained as both Markdown and JSON in the project source registry. This repository records the current identity and the source-governance rules required to interpret the archived pipeline reports.

## Unresolved internal identities

```text
SRC-024
SRC-040
SRC-053
```

No similar file should be silently substituted for these unresolved identities.

## Important source classes currently used

### PX4 runtime / control semantics

- PX4 v1.15 official documentation.
- PX4 v1.15.4 `dds_topics.h.em`.
- PX4 v1.15.4 `uxrce_dds_client.cpp`.
- PX4 control-allocation and position-control source/docs.
- Exact local source/build SHA identities in runtime reports override generic upstream behavior when they diverge.

### Micro XRCE-DDS

- eProsima Micro XRCE-DDS Client streams/API documentation.
- eProsima Micro XRCE-DDS Agent CLI/trace documentation.
- PX4 issue reports are hypothesis-generation only unless exact local-version evidence matches.

### Closed-loop identification / randomized experiments

Registry includes closed-loop system-identification literature, input/experiment design, micro-randomized trial methodology, causal excursion effect estimation, and few-cluster inference references.

### UAV world models / planning

Key current references include FlowPilot, WorldFly, DroneDreamer, MPC/NMPC and disturbance-observer / adaptive-control references. These inform future temporal `U_plan`, predictive rollouts and robust planning but do not override frozen runtime semantics.

## Retrieval / authority rule

Prefer primary or official sources for scientific and implementation claims. Secondary syntheses are orientation only.

For runtime causality:

```text
exact local source/build identity
+ captured raw telemetry/counters
> generic upstream documentation
> issue/forum hypotheses
```

Best-effort transport semantics establish that loss is possible; they do not prove a specific observed loss event.

Project-internal Markdown is context/evidence and becomes authority only when explicitly frozen/promoted by owner/project governance.
