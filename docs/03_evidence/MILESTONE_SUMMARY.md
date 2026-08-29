# Milestone and Root-Cause Summary

This file is the compact human-readable audit trail for the Moving-Mode AURA–WISE–World Model–AEGIS vNext pipeline. Detailed reports, telemetry and runtime roots remain outside this repository, primarily under `/media/nahhao74/KINGSTON`.

## 1. Incremental candidate mechanism qualified

The candidate path was proven as a bounded additive augmentation to the active FAST/T1/C1 baseline rather than a hidden replacement path.

```text
NO_CANDIDATE_LIVE_PARITY=PASS
ZERO_CANDIDATE_LIVE_PARITY=PASS
NONZERO_INCREMENTAL_LIVE_PATH=PASS
STALE_FAIL_TO_BASELINE_LIVE=PASS
RESET_NO_STALE_REUSE_LIVE=PASS
H1000_V2_LIVE_REGRESSION=PASS
```

Architectural consequence:

```text
B = PX4 + AURA + FAST/T1/C1
u_total_requested = u_baseline + u_candidate
```

The requested-action composition is additive; the plant response is not assumed linear.

---

## 2. E8 pending-ACK precedence closed

A valid candidate generation waiting for exact ACK could previously be superseded by unrelated baseline publication. Pending-ACK precedence was repaired and live-qualified.

Treatment exposure is therefore defined by exact accepted candidate cycles bound to generation/session/reset/frontier identity, not intended publication alone.

---

## 3. Strict qualified-record serialization closed

Auxiliary non-finite diagnostics could previously break strict JSON persistence after otherwise valid runtime activity. Serialization was repaired so auxiliary diagnostics remain representable while required scientific non-finite fields still fail closed.

Persistence remains a scientific qualification gate.

---

## 4. uXRCE hot-path head-of-line blocking closed

Large SensorCombined delivery gaps were localized to synchronous high-rate logging/stdout flushing in the uXRCE update path. Hot-path diagnostics were bounded/removed while low-overhead counters were retained.

Subsequent source qualifications did not reproduce the same stall. Verbose Agent tracing remains disabled in scientific runtime because it perturbs timing.

---

## 5. Apparent 183018-us SensorCombined gap reclassified

The mapped-time jump was decomposed as:

```text
native source delta = 8000 us
timesync offset step = -175018 us
mapped apparent jump = 183018 us
```

Native source continuity was intact. This led to retirement of mapped timestamp continuity as source authority.

---

## 6. Dual-domain timestamp contract frozen

```text
SOURCE_CONTINUITY_DOMAIN = native PX4 source time + generation
CLOCK_ALIGNMENT_DOMAIN   = explicit causal mapping provenance
```

A mapping transition is not a native source gap. Cross-domain science requires both valid native source identity and valid clock alignment.

---

## 7. Atomic SensorCombined provenance wire qualified

`SensorCombinedStampedV1` was added as an observational wire carrying native source identity plus the exact sender mapping tuple captured atomically with the sample. Standard SensorCombined semantics were not changed.

A historical expected SHA string later proved to be a 62-character ledger transcription error rather than source/schema drift. The canonical current wrapper-message digest is:

```text
5cb31c66d07c9c2bdaa9a4a7c243f10e4e9e70423b28957dee87b10862a59d06
```

A format guard now rejects malformed expected SHA-256 values before identity comparison.

---

## 8. StateBank/AURA bootstrap readiness race closed

A fresh root stopped before science because the snapshot request could occur before AURA had entered StateBank.

Repair:

```text
all-required-stream readiness barrier
+ atomic snapshot-barrier recheck
```

Required streams remain:

```text
aura, imu, attitude, local_state, reference, controller, actuator
```

Eight fresh bootstrap-only lifecycles passed.

---

## 9. Specialized pilot `bootstrap_only` call site closed

The randomized-pilot session-start call omitted `bootstrap_only=True`, causing accidental entry into candidate/C1 logic before science.

The call site and observer were repaired. Exact preflight then demonstrated valid 7/7 bootstrap ACK with zero candidate/C1/E8 activity and zero manifest consumption.

---

## 10. Gazebo bridge startup service-discovery race closed

A fresh pilot stopped during model spawn with the PX4 `gz_bridge` one-second service timeout. Exact source and runtime timing showed that `/world/sim_world_a/create` could appear several seconds after asynchronous Gazebo launch.

The repair waits for the exact world-scoped create service with a bounded fail-closed readiness probe before invoking `gz_bridge`; it does not enlarge the bridge's internal timeout or change simulation physics.

Qualification:

```text
single startup PASS
8/8 startup lifecycles PASS
4 CALM + 4 GUST_E
bridge timeouts = 0
startup failures = 0
stale runtime after cleanup = 0
```

---

## 11. H1000 session-start lifecycle defect closed

After `bootstrap_only` was repaired, a later root timed out in pre-science H1000 waiting.

Deep cause:

```text
SESSION_START_BOOTSTRAP_STATUS_WRONGLY_SEEDED_CANDIDATE_ONLY_H1000_RELEASE_ANCHOR
```

A baseline bootstrap-only status was incorrectly stored as `_last_release`, even though no candidate release existed. The repair leaves `_last_release=None` after session-start bootstrap and only applies H1000 after a real candidate release.

Unchanged semantics:

```text
REFRACTORY_US=1_000_000
CANDIDATE_HISTORY=CANDIDATE_ONLY
FAST/T1/C1 baseline != candidate exposure
```

Focused regression suite: 68 tests PASS.

---

## 12. Native-disturbance scientific world semantics frozen

World-forensics showed that the current generated world contained a real `NativeDisturbanceSystem` plugin absent from the historical no-plugin frozen digest. The plugin is not inert: it can call `Link::AddWorldWrench()` and therefore represented genuine scientific-world semantic drift.

The owner selected the plugin-bearing world as the canonical vNext scientific world:

```text
WORLD_NAME=sim_world_a
CANONICAL_WORLD_SHA256=8b26be57f07380455071fe8f4f81797e8ca3b946bf407158ff91f0ac110f3b91
```

Context contract:

```text
CALM   = identical plugin-bearing world; zero/no disturbance command
GUST_E = identical plugin-bearing world; frozen predeclared +E stimulus
```

The runner now prepares the world, validates expected SHA-256 format, hashes the final generated bytes and fails before runtime on mismatch.

A fresh non-scientific CALM qualification passed startup, source attestation, StateBank 7/7, bootstrap ACK, zero native disturbance events, 35 m safety and cleanup.

---

## 13. Latest fresh pilot: C1 valid-offer frontier timeout

The latest fresh randomized root passed the major pre-science gates:

```text
WORLD_IDENTITY_GATE=PASS
GZ_SERVICE_READY=PASS
PX4_STARTUP=PASS
REQUIRED_RUNTIME_TOPICS=PASS
SOURCE_ATTESTATION=PASS
SOURCE_CONTINUITY=PASS_PRE_SCIENCE_ATTESTATION
CLOCK_ALIGNMENT=PASS_PRE_SCIENCE_ATTESTATION
ATOMIC_PROVENANCE=PASS_PRE_SCIENCE_ATTESTATION
STATEBANK_REQUIRED_STREAMS_PRESENT=7/7
BOOTSTRAP_SNAPSHOT_ACK=VALID_ACCEPTED
BLOCK1_SNAPSHOT_ACK=VALID_ACCEPTED
```

It then stopped at:

```text
PRE_SCIENCE_C1_VALID_OFFER_FRONTIER_TIMEOUT
```

Observed facts:

```text
C1 callbacks advanced
continuous-C1 records existed
canonical/published C1 source frontier remained 0
candidate offers = 0
candidate publishes = 0
E8 candidate transactions = 0
FIRST_SCIENTIFIC_T_D=NOT_REACHED
MANIFEST_SLOTS_CONSUMED=0
```

Three isolated diagnostics reported:

```text
unexpected_domain expected=epoch received=boot
```

This is a strong hypothesis for a stale timestamp-domain expectation or source-canonicalization defect, but the deep cause remains unresolved until the exact C1 path is reconstructed.

The latest root is correctly classified:

```text
VNEXT_WM1_V2R1_RANDOMIZED_PILOT_INVALID_INFRASTRUCTURE_PRE_SCIENCE
```

It is not a scientific failure.

---

## 14. Qualification process upgraded: full pre-science corridor

Repeated pre-science failures exposed a process weakness: component-specific preflights did not always exercise the exact transition later reached by the real pilot.

New integration rule:

```text
repair
-> deterministic regression
-> component qualification
-> FULL INTEGRATED PRE-SCIENCE CORRIDOR
-> randomized scientific root
```

The corridor must use the real pilot runner/probe and traverse:

```text
startup
-> source attestation
-> StateBank 7/7
-> session bootstrap_only
-> first-block snapshot
-> C1 valid-offer frontier
-> pre-offer source/clock/provenance
-> intentional stop before FIRST_SCIENTIFIC_T_D
```

Required zero-science accounting:

```text
FIRST_SCIENTIFIC_T_D=NOT_REACHED
CANDIDATE_OFFER_COUNT=0
CANDIDATE_PUBLISH_COUNT=0
E8_CANDIDATE_TRANSACTION_COUNT=0
SCIENTIFIC_BLOCKS=0
MANIFEST_SLOTS_CONSUMED=0
```

The randomized pilot must no longer be used as an integration test.

---

## 15. Current unresolved scientific gate

No accepted root has completed the frozen campaign:

```text
8 sessions
4 CALM + 4 GUST_E
12 blocks/session
96 blocks total
ZERO=48
P1=24
P2=24
```

Therefore the project still has no accepted treatment-SNR result for `G_action` and no scientifically authorized action-conditioned WM training.

Current exact next state:

```text
C1_VALID_OFFER_FRONTIER_ROOT_CAUSE
+
FULL_PRE_SCIENCE_CORRIDOR_CLOSURE
```

Only after this integrated corridor passes should another randomized pilot be run.

---

## 16. Future performance roadmap

After vNext-1 scientific closure, the planned evidence-driven sequence is:

```text
end-to-end latency decomposition
-> T_D -> T_A delay/state modeling
-> AURA detector shadow bake-off
-> F_B + G_action model ladder
-> history ablation
-> uncertainty calibration
-> bounded WISE candidate enumeration
-> event-triggered planning
-> TinyMPC / Koopman / RTI only if justified
-> AEGIS safety-envelope specification
-> low-dimensional online adaptation
-> formal safety projection
```

See `../04_research/FUTURE_IMPLEMENTATION_ROADMAP.md`.

## 17. Evidence retention policy

GitHub keeps the architecture, contracts, roadmap and compact audit trail. Detailed runtime reports, raw roots, telemetry, counters, replay bundles and scientific data remain in the project artifact hierarchy and Kingston storage.

For forensic reconstruction, prefer exact local source/build identities and retained raw evidence over generic upstream documentation or historical prose summaries.
