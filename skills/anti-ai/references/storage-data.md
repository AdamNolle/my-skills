# Storage and data: atomicity, ordering, and ownership

Research checked: **2026-10-02 UTC**. These are historical, human-led source snapshots, not recommended versions to install. Dates are commit dates unless stated otherwise. Upstream source and test mechanisms were inspected; upstream suites were not executed. Apply the target's current supported version, local policy, and failure model first.

Read only the relevant project below. Turn its lesson into a local invariant and executable counterexample; do not copy its architecture or treat historical code as defect-free.

## SQLite: durable state and testable invariants

**Pin:** 3.34.0, 2020-12-01. Canonical Fossil source ID `a26b6597e3ae272231b96f9982c3bcc17ddec2f2b6eb4df06a224b91089fed5b`; Git mirror commit `384f5c26f48b92e8bfcb168381d4a8caf3ea59e7`. The [release record](https://www.sqlite.org/releaselog/3_34_0.html) and [pinned manifest](https://github.com/sqlite/sqlite/blob/384f5c26f48b92e8bfcb168381d4a8caf3ea59e7/manifest.uuid) agree.

**Read:** [src/pager.c](https://github.com/sqlite/sqlite/blob/384f5c26f48b92e8bfcb168381d4a8caf3ea59e7/src/pager.c), especially its rollback-journal invariants and named state transitions (`syncJournal`, `sqlite3PagerCommitPhaseOne`, `pager_error`). [test/crash.test](https://github.com/sqlite/sqlite/blob/384f5c26f48b92e8bfcb168381d4a8caf3ea59e7/test/crash.test) simulates I/O damage during commit rather than merely checking successful transactions. [Testing rationale](https://www.sqlite.org/testing.html) describes distinct harnesses, fuzzing, fault injection, and as-delivered builds.

**Apply:** Name durable-state invariants before simplifying a transaction path. Ask what survives a failure between each write/sync/rename and which test exercises that boundary. Keep mode assumptions adjacent to the invariant.

**Limits:** Those pager invariants explicitly exclude WAL, MEMORY, and OFF journal modes. SQLite warns that full MC/DC coverage is expensive and may be inappropriate for ordinary applications. Its [historical README](https://github.com/sqlite/sqlite/blob/384f5c26f48b92e8bfcb168381d4a8caf3ea59e7/README.md) explicitly describes generated source, including parsers and the amalgamation; do not classify the amalgamation as handwritten or edit generated output instead of its source.

## PostgreSQL: concurrency reasoning near the implementation

**Pin:** REL_13_1, commit date 2020-11-09, [`6daf725a9c66e880fd76d25279ce00710535e030`](https://github.com/postgres/postgres/commit/6daf725a9c66e880fd76d25279ce00710535e030), attributed to Tom Lane.

**Read:** [nbtree/README](https://github.com/postgres/postgres/blob/6daf725a9c66e880fd76d25279ce00710535e030/src/backend/access/nbtree/README) and [`_bt_moveright` in nbtsearch.c](https://github.com/postgres/postgres/blob/6daf725a9c66e880fd76d25279ce00710535e030/src/backend/access/nbtree/nbtsearch.c#L245). The README explains why shared buffers require departures from the published algorithm, the lock-order constraints, and how scans remain correct during splits. [Isolation-test instructions](https://github.com/postgres/postgres/blob/6daf725a9c66e880fd76d25279ce00710535e030/src/test/isolation/README) describe multi-session schedules because ordinary regression queries cannot model those interactions.

**Apply:** Before deleting a retry, lock, pin, or apparent duplicate check, identify the concurrent operation that makes it necessary. Ask whether the new behavior is covered by an explicit interleaving, not only a sequential unit test. Explain deviations from textbook algorithms in domain terms.

**Limits:** Storage-engine concurrency machinery is not a template for simple application code. A long explanatory comment can be essential; verbosity alone is not a defect. Preserve SQL, transaction, recovery, and binary-format contracts when refactoring.

## Redis: data-structure-shaped code and latency budgets

**Pin:** 6.0.9, 2020-10-27, [`25214bd7dc2f4c995d76020e95180eb4e6d51672`](https://github.com/redis/redis/commit/25214bd7dc2f4c995d76020e95180eb4e6d51672), attributed to Oran Agra.

**Read:** [`dictRehash`, `dictRehashMilliseconds`, and `_dictRehashStep`](https://github.com/redis/redis/blob/25214bd7dc2f4c995d76020e95180eb4e6d51672/src/dict.c). The two-table migration state is explicit; empty-bucket scanning is capped, safe iterators inhibit migration, and comments explain copy-on-write constraints. The [pinned README](https://github.com/redis/redis/blob/25214bd7dc2f4c995d76020e95180eb4e6d51672/README.md) starts its source tour with data structures and describes its Tcl test infrastructure.

**Apply:** Start with the actual data and transition costs. Ask whether background work is truly bounded, whether an iterator remains valid, and what work occurs on a request's critical path. Preserve a performance-motivated detail only when its constraint is identifiable.

**Limits:** A rehash step moves a bucket, whose chain may contain many entries; the empty-visit cap is not a hard real-time latency guarantee. Event-loop/global-state choices do not generalize automatically to threaded or distributed systems. Use this historical licensed snapshot for analysis, not assumptions about present Redis licensing or implementation.

## LevelDB: narrow storage contracts and realistic failure models

**Pin:** 1.22, 2019-05-03, [`78b39d68c15ba020c0d60a3906fb66dbf1697595`](https://github.com/google/leveldb/commit/78b39d68c15ba020c0d60a3906fb66dbf1697595), attributed to Chris Mumford.

**Read:** [include/leveldb/db.h](https://github.com/google/leveldb/blob/78b39d68c15ba020c0d60a3906fb66dbf1697595/include/leveldb/db.h) defines DB, iterator, snapshot, ownership, concurrency, and failure semantics. [`log::Writer::AddRecord`/`EmitPhysicalRecord`](https://github.com/google/leveldb/blob/78b39d68c15ba020c0d60a3906fb66dbf1697595/db/log_writer.cc) keep physical log framing and assertions local. [fault_injection_test.cc](https://github.com/google/leveldb/blob/78b39d68c15ba020c0d60a3906fb66dbf1697595/db/fault_injection_test.cc) tracks filesystem state at sync and discards unsynced data/files.

**Apply:** Distinguish visibility, process persistence, and crash durability. Ask whether a flush actually implies sync, what ownership outlives the parent handle, and whether a test models the failure claimed. Use narrow interfaces with explicit lifetimes instead of generic layers that obscure the contract.

**Limits:** This embedded ordered store is not a distributed consensus system. Fault-injection tests validate a model; they do not prove every hardware/filesystem configuration. A virtual interface is useful here because it expresses real environment seams, not because all projects need an interface for every class.
