# Qualified lifecycle migration

`QualifiedActionLinkLifecycle` is the sole owner of qualified C1 arming, immutable transport retry, exact terminal-status matching, and native `T_A` binding. `Stage1Probe` retains only snapshot/profile/record orchestration; `ContiguousEngineeringRunner` explicitly opts into the bounded mode and delegates its initial and parent-linked release offers to the same lifecycle.

The deterministic static matrix covers exact/retry/timeout/mismatch/ZERO and 3/5/7 event-generation identities. No ROS runtime, SITL, PX4, scientific acquisition, model operation, or SEALED access occurred.

## Final status

```text
TASK_RESULT=QUALIFIED_LIFECYCLE_MIGRATION_COMPLETE
QUALIFIED_TRANSACTION_HOOK_IMPLEMENTED=FULL
STAGE1PROBE_USES_SHARED_HOOK=true
CONTIGUOUS_RUNNER_USES_SHARED_HOOK=true
LIVE_RUNNER_ARBITRARY_SOURCE_ALLOWED=false
RELEASE_TRANSACTION_IMPLEMENTED=true
RELEASE_USES_SHARED_HOOK=true
LEGACY_EQUIVALENCE_PASS=true
LEGACY_FRONTIER_SELECTION_EQUIVALENT=true
LEGACY_ACK_MATCH_EQUIVALENT=true
LEGACY_T_A_EQUIVALENT=true
LEGACY_RETRY_EQUIVALENT=true
LEGACY_TIMEOUT_EQUIVALENT=true
LEGACY_GENERATION_PROGRESSION_EQUIVALENT=true
LEGACY_3C_EQUIVALENT=true
LEGACY_5C_EQUIVALENT=true
LEGACY_7C_EQUIVALENT=true
LEGACY_EVENT_ONLY_BEHAVIOR_UNCHANGED=true
RUNTIME_SMOKE_EXECUTED=false
SCIENTIFIC_ACQUISITION_EXECUTED=false
G_ACTION_CAUSAL_STATUS=UNQUALIFIED
```

Validation:

```text
ROS imports PASS
py_compile PASS
100 focused tests PASS
task-scoped git diff --check PASS
```

Next task:

```text
G_ACTION_CONTIGUOUS_MINIMAL_RUNTIME_QUALIFICATION
```
