# PID G3-A Multisine Experiment-Design Research — 2026-09-02

Branch: `research/pid-benchmark-20260901`

Status: authoritative PID-branch research/design memo for the transition from completed G3 mechanism/band qualification to the still-separate G3-A scientific acquisition contract. This document does **not** authorize runtime and does **not** change the V8 stage-ownership architecture.

## 1. Current authoritative state

```text
G0=PASS
G2=G2_PX4_NATIVE_FEASIBLE_ENVELOPE_CHARACTERIZED_READY_FOR_G3
G3_Q0_Q3=PASS
G3_MECHANISM_QUALIFICATION=PASS
G3_BAND_Q=PASS
G3A_SUPPORTED_PROBE_CEILING=2.0_HZ
G3_A_RUNTIME=NOT_AUTHORIZED
G3_IDENTIFICATION_CREDIT=0_UNTIL_FRESH_G3A
MODEL_TRAINING_CREDIT=0_UNTIL_FRESH_G3A
```

The scientific estimand remains

\[
u(t)=[a_N^{accepted}(t),a_E^{accepted}(t)]^T,
\qquad
y(t)=[v_N(t),v_E(t)]^T.
\]

PX4 N/E position/velocity feedback is bypassed during G3, while PX4 D control, acceleration-to-thrust/attitude projection, attitude, rate, allocation, actuators and vehicle dynamics remain inside the plant. Direct force and aerodynamic wind are zero in the initial center stratum. AURA/FAST/WM/WISE/AEGIS correction authority is inactive.

G2, Q0-Q3 and BAND-Q roots retain zero G3-A scientific/model-training credit.

## 2. Q0-Q3 mechanism qualification retained

Q0-Q3 established the external-acceleration action path and timing/authority contract at the center operating point. Q1/Q2 used approximately 0.12575 Hz single-axis excitation and Q3 used approximately 0.12575 Hz on N and 0.25151 Hz on E. These roots proved:

```text
accepted external acceleration is observable at the PX4 read boundary
N-only and E-only bounded response paths are valid
simultaneous rank-two N/E excitation is possible
0.35 m/s² global external-action norm is qualified for the tested mechanism
PX4 N/E velocity feedback/integrator is bypassed by the NaN per-axis contract
D/attitude/rate/allocation remain active
```

They do not receive model-training or G3-A dataset credit.

## 3. BAND-Q empirical closure

The authorized two-session full-harmonic orthogonal BAND-Q pair completed successfully.

```text
FINAL_STATE=G3_BAND_Q_QUALIFIED_READY_FOR_G3A_CONTRACT_FREEZE
BAND_Q_A=PASS
BAND_Q_B=PASS
PAIR=VALID_PAIR
SUPPORTED_LINES={0.125,0.250,0.500,1.000,2.000} Hz
G3A_SUPPORTED_PROBE_CEILING=2.0 Hz
```

Frozen BAND-Q design:

```text
base period T0=8.0 s
candidate harmonics={1,2,4,8,16}
record=5 periods=40 s
P0=transient/excluded
P1-P4=4 admitted repeated periods=32 s
global accepted-action norm <= 0.35 m/s²
A=(+m_N,+m_E)
B=(+m_N,-m_E)
manifest SHA256=6ee2e286194790580a704bcc9f2fb17cae316359b20072491702ccc7f11b46f7
```

All five candidate probe lines were consecutively supported under the prospective rule: paired input rank two, finite/conditioned input matrix, and direct-axis repeated-period 95% magnitude intervals with lower bounds above zero.

The observed paired input condition numbers were approximately 1.02-1.05 across all five lines. The direct-axis response magnitude decreased substantially toward 2 Hz, but remained distinguishable from zero at the 2 Hz probe. Therefore `2.0 Hz` is a **supported probe ceiling**, not a promoted FRF/model bandwidth and not proof that every intermediate frequency is scientifically supported.

No fixed or post-hoc magic coherence threshold was introduced.

## 4. BAND-Q timing and causal reconstruction

The primary input remains `U_accepted_NE` from the PX4 read-boundary audit. Scientific alignment is frozen as

```text
G3_INPUT_ALIGNMENT=CAUSAL_ACCEPTED_ACTION_ZOH
```

For every output timestamp `t_y`:

```text
U(t_y)=latest source-valid U_accepted with t_acc <= t_y
```

Nearest-neighbor future-action assignment, host-wall-time identification alignment and future-looking interpolation are forbidden.

BAND-Q retained continuous PX4 read-boundary evidence with 8-12 ms read-boundary spacing, no source gaps above the frozen qualification limit, no duplicate/non-monotonic timestamps, and diagnostic generation gaps that did not invalidate the held accepted action because the read-boundary stream remained continuous.

For primary FRF work, timestamp-native processing remains preferred:

- compute accepted ZOH input Fourier coefficients from true accepted-action intervals;
- estimate output harmonic coefficients directly at irregular `VehicleLocalPosition.timestamp_sample` times by harmonic least squares or an equivalent nonuniform Fourier estimator;
- preserve exact complex phase/delay provenance.

Uniform resampling is deferred to later VARX/PBSID preparation under a separately frozen causal interpolation/anti-alias policy.

## 5. Full-harmonic orthogonal G3-A design

The earlier disjoint-line Q3 design remains valid mechanism evidence but is not the preferred scientific 2×2 FRF design.

For G3-A use full-harmonic orthogonal pairs with the 2×2 Hadamard polarity pattern

\[
H_2=\begin{bmatrix}1&1\\1&-1\end{bmatrix}.
\]

For each phase realization:

```text
Pair A: u_N=+m_N(t), u_E=+m_E(t)
Pair B: u_N=+m_N(t), u_E=-m_E(t)
```

N and E use the same scientific frequency grid. A/B share frequency content, line amplitudes, phase realization, seed, period/source epoch definition and operating point; only the declared E polarity changes.

An A/B pair is an indivisible identification realization.

## 6. Critical matched-D operating-point rule

BAND-Q A and B used different D targets:

```text
A D_target=-4.2218318 m
B D_target=-3.2701380 m
```

Therefore the BAND-Q pair is qualification evidence only and must **not** be treated as a matched quantitative plant replicate.

For G3-A:

```text
ORTHOGONAL_PAIR_D_TARGET_MUST_MATCH=true
NO_OFFLINE_D_TARGET_COMPENSATION=true
```

The current downstream project contract recorded on this branch freezes:

```text
OPERATING_POINT_N/E=[0,0]
D_TARGET=-3.0 m (local NED down)
```

and requires the same exact D target across all five orthogonal pairs / all ten sessions. This `-3.0 m` value is a **contract value**, not a BAND-Q empirical result. Before any G3-A runtime, the project-side frozen manifest must be checked to match this value exactly and comparable D-settle eligibility must be proven prospectively.

## 7. Candidate scientific frequency grid after BAND-Q

The current downstream project contract recorded on this branch freezes the dense exact-harmonic candidate grid

```text
T0=8.0 s
Δf=0.125 Hz
f={0.125,0.250,0.375,0.500,0.625,0.750,0.875,1.000,
   1.125,1.250,1.375,1.500,1.625,1.750,1.875,2.000} Hz
```

This grid is bounded by the empirically supported 2 Hz probe ceiling. BAND-Q did **not** directly validate every intermediate line. Therefore every dense-grid line must independently pass fresh accepted-input, timing, uncertainty and repeatability rules in G3-A.

The line grid, line amplitudes, phase realizations and global scaling must be frozen before runtime and may not be reduced, reweighted or retuned from observed G3-A response.

## 8. Periodicity and realization structure

The G3-A candidate record structure remains:

```text
T0=8.0 s
5 periods/session
P0=entry/transient, excluded
P1-P4=4 admitted repeated scientific periods
40 s total excitation/session
32 s admitted periodic data/session
```

The planned scientific dataset is five independent orthogonal pairs / ten sessions:

```text
R1 pair -> TRAIN
R2 pair -> TRAIN
R3 pair -> TRAIN
R4 pair -> DEV
R5 pair -> HELD_OUT
```

Total:

```text
TRAIN=6 sessions
DEV=2 sessions
HELD_OUT=2 sessions
```

A pair must never be split across dataset partitions. Adjacent rows from one continuous realization must never be randomly split and treated as independent data.

## 9. Phase optimization and crest factor

Use deterministic phase-optimized multisines. For each realization:

```text
phase/seed identity is assigned prospectively
A/B use the same phase realization
only E polarity changes inside the pair
continuous-time/global norm bound is checked before runtime
max ||u_NE||_2 <= 0.35 m/s²
```

Phase optimization is an input-design tool only. Same-campaign phase/amplitude retuning after inspecting vehicle response is prohibited.

## 10. Scientific line-quality logic

Do not use a universal fixed coherence number such as `0.8` or `0.9` as the sole promotion criterion.

Per-line quality combines:

```text
INPUT SUPPORT
- accepted spectral line present and source-valid
- paired experiment rank 2
- prospectively acceptable conditioning
- finite accepted-action timing support

RESPONSE SUPPORT
- finite direct-axis complex response
- finite magnitude/phase uncertainty
- direct response distinguishable from zero/noise context under the frozen rule

REPEATABILITY
- repeated-period consistency
- independent phase-realization consistency
- held-out realization reserved for model validation
```

Cross-axis response may legitimately be small or zero and is not required to pass a nonzero-response test.

Coherence remains a frequency-specific diagnostic with a significance/uncertainty interpretation tied to the actual effective degrees of freedom.

## 11. Model-validation structure

Only fresh G3-A roots may receive `SCIENTIFIC_G3_A_DATASET_CREDIT=YES`.

After valid acquisition, the intended analysis ladder remains

```text
raw validity
→ causal accepted-action ZOH reconstruction
→ timestamp-native PSD/CSD / harmonic estimates
→ 2×2 MIMO FRF
→ VARX
→ PBSID/subspace/SVD
→ compact model
→ P0-P4 controller ladder
```

Prospective model validation uses complete held-out sessions and must include:

```text
one-step prediction
free-run prediction
residual whiteness
input-residual correlation
cross-axis residual audit
FRF agreement
source-delay/timing audit
model-order/regularization robustness
```

R² alone is insufficient for model promotion.

## 12. What this memo does not change

```text
V8 stage ownership remains authoritative.
G2 remains the native-PX4 operational baseline stage.
G3 remains accepted external [a_N,a_E] -> measured [v_N,v_E].
PX4 N/E position/velocity feedback remains bypassed in G3.
PX4 D/attitude/rate/allocation/actuator/vehicle dynamics remain in the G3 plant.
Direct force and aerodynamic wind remain zero in the initial center G3 stratum.
AURA/FAST/WM/WISE/AEGIS correction authority remains inactive.
PX4_BOOT_US/source time remains authoritative.
G2, Q0-Q3 and BAND-Q retain zero G3-A/model-training credit.
0.35 m/s² remains a qualified action cap, not an absolute plant limit.
```

## 13. Research Git authority

To preserve independent audit provenance:

```text
RESEARCH_GIT_UPDATE_AUTHORITY=CHATGPT_ONLY
LUNA_RESEARCH_COMMIT_AUTHORITY=false
LUNA_RESEARCH_PUSH_AUTHORITY=false
LUNA_RESEARCH_MERGE_AUTHORITY=false
```

Execution agents such as Luna may produce project-local evidence and may report

```text
RESEARCH_UPDATE_RECOMMENDED=true
```

with an exact proposed amendment, but they must not commit, push, merge or move the `Research-Documentation` research branch. Research updates are performed only after ChatGPT independently audits the supplied evidence.

Project runtime/implementation artifacts remain under `PID_Benchmark_Track/**`; large runtime data remain under `/media/nahhao74/KINGSTON/PID_Benchmark_Track`.

## 14. Key research references

1. Matt, J. J.; Altamirano, G. V., **System Identification of an Octocopter in Hover using Full-Harmonic Orthogonal Multisine Inputs**, AIAA AVIATION 2025 / NASA NTRS 20250006234. https://ntrs.nasa.gov/citations/20250006234

2. Grauer, J. A.; Boucher, M. J., **Aircraft System Identification from Multisine Inputs and Frequency Responses**, Journal of Guidance, Control, and Dynamics, 2020; NASA NTRS 20205003364. https://ntrs.nasa.gov/citations/20205003364

3. Mu, B.; Guo, J.; Wang, L. Y.; Yin, G.; Xu, L.; Zheng, W. X., **Identification of linear continuous-time systems under irregular and random output sampling**, Automatica 60 (2015), 100-114. DOI: 10.1016/j.automatica.2015.07.009.

## 15. Decision

```text
G3_Q0_Q3=PASS
G3_MECHANISM_QUALIFICATION=PASS
G3_BAND_Q=PASS
G3A_SUPPORTED_PROBE_CEILING=2.0_HZ
G3_A_EXCITATION_DESIGN=FULL_HARMONIC_ORTHOGONAL_PAIR
G3_A_PAIR_D_MATCH=REQUIRED
G3_PRIMARY_FRF_TIMING=TIMESTAMP_NATIVE_CAUSAL_ACCEPTED_ACTION_ZOH
G3_A_RUNTIME=NOT_AUTHORIZED
G3_MODEL_TRAINING_CREDIT=0_UNTIL_FRESH_G3A
RESEARCH_GIT_UPDATE_AUTHORITY=CHATGPT_ONLY
```
