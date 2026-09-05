# Current Source Registry — v9

**Version:** `AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v9`  
**Updated:** 2026-08-31  
**Sources:** 156  
**Resolved locators:** 153  
**Unresolved:** 3 (`SRC-024`, `SRC-040`, `SRC-053`)

This file is a retrieval/index view for the current source registry. It is **not** a current project-state or execution-authority document.

## v9 additions

| ID | Source | Methodological relevance |
|---|---|---|
| SRC-150 | Runtime enforcement of regular timed properties by suppressing and delaying events | timed-runtime-enforcement reference |
| SRC-151 | Age of information optimization in cyber–physical systems with stateful packet management techniques | freshness / age-at-decision / age-at-application reference |
| SRC-152 | Network Calculus: A Theory of Deterministic Queuing Systems for the Internet | deterministic delay/backlog-bounds reference |
| SRC-153 | Adaptive Experiment Design for Nonlinear System Identification With Operational Constraints | constrained experiment-design reference |
| SRC-154 | Safety Beyond the Training Data: Robust Out-of-Distribution MPC via Conformalized System Level Synthesis | conformal/OOD predictive-control reference; includes a quadcopter benchmark |
| SRC-155 | Learning the Uncertainty Sets of Linear Control Systems via Set Membership: A Non-asymptotic Analysis | set-membership uncertainty reference |
| SRC-156 | Uncertainty quantification of set-membership estimation in control and perception: Revisiting the minimum enclosing ellipsoid | ellipsoidal outer-approximation reference |

## Exact locators

```text
SRC-150 https://doi.org/10.1016/j.scico.2016.02.008
SRC-151 https://doi.org/10.1016/j.adhoc.2023.103358
SRC-152 https://doi.org/10.1007/3-540-45318-0
SRC-153 https://doi.org/10.1109/LSP.2025.3639512
SRC-154 https://proceedings.mlr.press/v331/srinivasan26a.html
SRC-155 https://proceedings.mlr.press/v235/li24ci.html
SRC-156 https://proceedings.mlr.press/v242/tang24a.html
```

## Usage rule

These sources may support future work in timing/freshness, constrained experiment design, uncertainty or predictive control **only when the active roadmap identifies a measured project need**.

Do not infer that a method is active because its source exists in this registry.

Active research direction is defined by:

```text
../04_research/FUTURE_IMPLEMENTATION_ROADMAP.md
```

Current runtime/scientific state is defined by:

```text
../00_overview/CURRENT_STATUS.md
../00_overview/CURRENT_EXECUTION_LADDER_WM_20260905.md
```

## Authority boundary

Registry entries do not authorize:

```text
scientific execution
manifest or estimand changes
AURA / FAST/T1/C1 semantic changes
PX4 authority changes
World-Model training
SEALED access
production authority
```

## Adoption rule

```text
measured problem
→ relevant source
→ project-specific hypothesis
→ smallest credible baseline/mechanism
→ bounded shadow/offline/non-scientific test
→ retain only if repeat-supported benefit is demonstrated
```

The complete local v9 artifact contains 156 entries and retains existing source IDs without renumbering.
