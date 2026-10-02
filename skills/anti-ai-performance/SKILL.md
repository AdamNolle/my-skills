---
name: anti-ai-performance
description: Investigate measured performance and concurrency behavior in code reviews, optimizations, async lifecycle bugs, races, deadlocks, and cancellation. Use for latency, throughput, memory, allocation, queueing, ownership, or synchronization questions. Require representative measurement for performance claims and explicit schedules or invariants for concurrency claims.
---

# Measure work; explain interleavings

Optimize the actual resource under constraint. Preserve ownership, cancellation, fairness, and backpressure rather than trading them away for a shorter implementation.

## Input and scope

- Identify the symptom or target, workload, scale, latency/throughput/memory budget, supported runtime, and affected paths. Distinguish measured facts from hypotheses.
- Default to audit or investigation. Apply changes only when an optimization/fix is authorized. Preserve unrelated edits and record the exact baseline state.
- Read [measurement and lifecycle decisions](references/decisions.md) only for the relevant problem. Do not run a generic checklist over unrelated paths.

## Performance track

1. Reproduce the relevant workload and choose a metric tied to user impact. Separate cold/warm paths, throughput, tail latency, memory/allocations, and resource utilization as appropriate.
2. Profile or instrument before choosing an optimization. Trace where work, blocking, allocation, I/O, or contention accumulates. State the leading hypothesis and what would falsify it.
3. Compare like-for-like baseline and candidate: same workload/data, toolchain, release/debug mode, hardware/resource limits, warmup, caching, and observability overhead. Repeat enough to see variance; report sample size and spread, not invented certainty.
4. Check whether the optimization merely shifts cost, weakens correctness, worsens tail latency, increases memory, or depends on unrealistic cache/data conditions. Preserve intentional copies and bounded work until their purpose is understood.
5. Re-run correctness checks and representative measurements on the exact final change. If measurement is unavailable, label the benefit a hypothesis; do not promise a percentage or say “faster.”

## Concurrency and async track

1. Identify mutable state, owners, tasks/threads, synchronization, and the invariant being protected. Name the relevant linearization/publication point where applicable.
2. Trace success, cancellation, timeout, partial failure, shutdown, and callback reentrancy. Every spawned task/resource needs an owner and completion/cleanup policy. Cancellation requested is not cancellation completed.
3. Examine actual interleavings at awaits, callbacks, locks, atomics, task registration, notification, and retry boundaries. An actor or one event-loop thread does not eliminate reentrancy across suspension.
4. Check lock ordering, holding locks across blocking/user callbacks, stale state after suspension, coalesced versus counted events, bounded queues/backpressure, and background exception propagation only where relevant.
5. Construct a decisive schedule with barriers/events/controllable clocks; use race detectors or model tests when supported. Arbitrary sleeps and one clean stress run are insufficient proof. Do not hand-roll synchronization or weaken ordering without a valid memory-model argument.

## Output contract

Report the verified symptom, affected state/work path, evidence, smallest justified change or experiment, and residual risk. Include benchmark conditions/results and tested revision/diff for performance claims; include invariant and concrete schedule for concurrency findings. Label unavailable tools and unexecuted tests. A clean detector run is bounded evidence, not proof of race freedom.
