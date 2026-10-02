# Curated engineering corpus

Research checked: **2026-10-02 UTC**. Pick by the actual problem, not project prestige. Read one or two relevant guides and inspect only the cited symbols needed for the decision.

## Selection and provenance

- Select long-lived, publicly inspectable projects with substantive production use, review/contribution practices, and meaningful tests. Diversity of constraints matters: a kernel, transaction manager, GUI object tree, and application library solve different problems.
- Treat this as a curated teaching corpus, not a ranking or an exhaustive list of the world's best code. Longevity and popularity do not prove correctness.
- Use the pre-2021 snapshots below and in each guide as historical human-led development evidence. A commit date only bounds the revision being inspected; it cannot certify that every line was human-written or that no code generator was used. Never claim “100% human-made.”
- Use historical revisions for research, not deployment recommendations; old releases may contain fixed vulnerabilities or obsolete APIs. Preserve the original, newer Linux/LLVM/Rust/Swift sources for version-specific behavior. The historical counterparts are comparisons, not instructions to downgrade or imitate obsolete APIs.
- Distinguish pinned source facts from derived review questions. Inspect the target code, supported versions, and tests before turning a question into a finding. Local policy always wins over a source's unrelated constraints.
- Reuse concepts, not copied implementation. Review licensing before substantial reuse. Generated output, vendor snapshots, ABI bridges, exported adapters, and compatibility branches are legitimate until local evidence proves otherwise.

## Route by domain and failure mode

| Work to inspect | Read | Source families and useful questions |
| --- | --- | --- |
| Kernel C, resource acquisition, ring buffers | [C/Linux](c-linux.md) | Linux: who owns each resource on partial failure; what publication ordering is required? |
| C++ compiler/tools, error APIs, mmap | [C++/LLVM](cpp-llvm.md) | LLVM/Clang: what contract does a wrapper enforce; is a fallback intentional; what changes when assertions are disabled? |
| Rust ownership, unsafe boundaries | [Rust](rust.md) | Rust: is failure absence or error; which safe callers uphold unsafe invariants; what does a clone preserve? |
| Swift public APIs, task isolation | [Swift](swift.md) | Swift: can a suspension invalidate state; are cancellation and release assertions being honored? Use modern sources for concurrency. |
| Durable storage, caches, indexes, database lifecycle | [Storage/data](storage-data.md) | SQLite, PostgreSQL, Redis, LevelDB: atomicity, crash/order invariants, scan/cursor validity, cache identity, snapshot lifetime |
| Network parsing, async coordination, privilege boundaries | [Networking/concurrency](network-concurrency.md) | curl, OpenBSD/OpenSSH, Go, Tokio: bounded input, capability boundaries, cancellation ownership, registration/wakeup races |
| Runtime containers, tool interfaces, compatibility | [Runtimes/tooling](runtimes-tooling.md) | CPython, Git: callbacks/reentrancy during cleanup, buffer capacity vs length, ownership, narrow reviewable patches |
| Python web or transactional application code | [Python web](python-web.md) | Django: transaction scope, nested savepoint recovery, preserving original exceptions, stateful wrappers |
| Java/Kotlin libraries and network services | [JVM libraries](jvm.md) | Guava, Netty, JUnit, Okio: boundary validation, binary compatibility, partial messages, timeout/thread semantics, stream ownership |
| UI object lifetime, reactive callbacks, real-time queues | [UI/reactive/embedded](ui-embedded.md) | Qt, RxJS, Blender, FreeRTOS: teardown before callbacks, parent/child ownership, intrusive-structure invariants, task vs interrupt contracts |

Cross-reference only when the same failure mode truly transfers: CPython's reentrant destruction can inform callback cleanup; SQLite can suggest fault injection for an application transaction; neither mandates that another project adopt its architecture.

## Apply a precedent

1. State the target invariant and the source's relevant mechanism, in separate sentences.
2. Identify where the target's ownership, consistency, compatibility, or failure model differs.
3. Trace local callers and extension/configuration entry points. Preserve intentional asymmetries.
4. Test a representative success path and a boundary/failure/interleaving counterexample. Keep assertions about observable contracts.
5. Recommend the smallest justified change, or explicitly recommend leaving the code intact. Record source URL, symbol, revision, retrieval date, and applicability limit when the precedent materially affected the recommendation.
