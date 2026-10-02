# Measurement and lifecycle decisions

## Worked example: an “unnecessary” copy

A parser copies a small input slice so the retained result cannot reference a reused receive buffer. Removing the copy lowers a microbenchmark allocation count but corrupts data after the next read. Establish buffer ownership and lifetime first. Benchmark a legitimate owned/borrowed alternative under the actual retention pattern; do not optimize the wrong contract.

Counterexample: a repeated immutable copy inside a hot loop may be removable when no mutation, aliasing, lifetime, or API promise depends on it. Show its contribution in the profile and compare representative end-to-end behavior.

## Worked example: single-threaded stale state

An async handler reads `generation`, awaits a network result, then updates the current view without checking whether the request was superseded. A newer request can finish first on the same thread. Schedule both completions deterministically and assert which result may become visible. A mutex around the initial read does not solve logical staleness; choose cancellation, generation validation, or serialized ownership according to the contract.

## Worked example: notification is not a queue

A notification primitive may store one permit and coalesce repeated notifications. Treating it as a counted event stream loses work if the consumer expects one wake per job. Inspect the primitive’s documented version-specific semantics and the queue predicate. Test registration, notification, consumption, and waiter cancellation schedules. A predicate loop with a properly synchronized queue may be correct; do not replace a sound design just because notifications coalesce.

## Measurement rubric

- **Claim an improvement** only for the measured workload, metric, state, and conditions. Report absolute values as well as percentages when useful.
- **Investigate noise** when the effect is small relative to variance, environment changes, or instrumentation overhead. Do not cherry-pick the fastest run.
- **Keep bounded work** when it protects latency/fairness even if bulk throughput improves without it. A bucket-step cap may still allow long collision chains.
- **Prefer the existing primitive** when it supplies the required guarantees. More atomics or fewer locks do not imply higher performance or correctness.
- **Refuse an unsupported guarantee** when available evidence does not cover the scheduler, hardware memory model, or workload being claimed.

## Primary precedents

- [Go cancellation lifecycle, pinned 1.15.6](https://github.com/golang/go/blob/9b955d2d3fcff6a5bc8bce7bafdc4c634a28e95b/src/context/context.go): cancellation owns child references and timers, not merely a boolean. Use supported current APIs.
- [Tokio notification contract, pinned 1.0.1](https://github.com/tokio-rs/tokio/blob/2330edc875ed8b873b6ffc4686feef1534658f79/tokio/src/sync/notify.rs): distinguish stored permits, waiter registration, and drop obligations. Historical implementation is not a recipe for custom unsafe synchronization.
- [Redis incremental rehash, pinned 6.0.9](https://github.com/redis/redis/blob/25214bd7dc2f4c995d76020e95180eb4e6d51672/src/dict.c): inspect migration state, safe iterators, and work bounds; a step is not a hard real-time guarantee.

The parent corpus inspected these sources on 2026-10-02. Consult the supported runtime and local conventions before applying an API or memory-model claim.

## Further primary tools and measurement traps

Research checked 2026-10-02. [Google Benchmark](https://github.com/google/benchmark/blob/main/docs/user_guide.md) documents repetitions, warmup, interleaving, and optimization hazards. Ensure the operation is executed rather than constant-folded, time setup deliberately, and check scale/distribution, timer resolution, offered load, and sample sufficiency for tail claims. CPU time, wall time, throughput, and energy answer different questions. Interleave comparable trials where environmental drift matters.

[Apple performance tests](https://developer.apple.com/documentation/xcode/writing-and-running-performance-tests) and [SwiftUI Instruments](https://developer.apple.com/videos/play/wwdc2025/306/) illustrate platform-appropriate evidence; they do not generalize a UI measurement to server workloads. [Swift concurrency](https://github.com/swiftlang/swift-book/blob/main/TSPL.docc/LanguageGuide/Concurrency.md) documents cooperative cancellation and isolation; a blocking mutex and async-aware primitive have different progress properties. Do not enforce a slogan against all locks across await without analyzing the actual primitive and intended serialization.

[Microsoft Coyote](https://github.com/microsoft/coyote/blob/main/docs/concepts/concurrency-unit-testing.md) explores declared nondeterminism and replays schedules within its supported model. [ThreadSanitizer](https://clang.llvm.org/docs/ThreadSanitizer.html), [KCSAN](https://docs.kernel.org/dev-tools/kcsan.html), and [Miri](https://github.com/rust-lang/miri/blob/master/README.md) have different instrumentation/model/platform limits. A clean run cannot prove universal race freedom or soundness.
