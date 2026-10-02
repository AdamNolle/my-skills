# C and Linux: lifetimes, failure paths, and justified complexity

Research checked: **2026-10-02 UTC**. Use these as inspection questions, not a universal C style guide. Linux is kernel C with subsystem-specific concurrency and allocation rules. Release tags below identify historical examples, not the latest or defect-free code. Resolve immutable commits when relying on a precise implementation in a new review.

## Review sequence

1. Trace each allocation, reference, descriptor, and lock through success, partial acquisition, failure, and ownership transfer. Give every resource exactly the releases its lifetime requires. Exercise failure after each meaningful acquisition.
2. Inspect integer widths, units, overflow, buffer capacities, zero-length inputs, partial I/O, and publication order before shortening checks or combining branches.
3. Choose the repository's cleanup idiom. Use meaningful staged cleanup labels when appropriate; do not free resources that were never acquired. In subsystems using scope cleanup, preserve declaration/destruction order and explicit ownership transfer. Do not mechanically mix or replace the two styles.
4. Compare similar paths for direction, locking, units, partial completion, and error semantics before deduplicating. Preserve intentional asymmetries.
5. Extract helpers when they reduce conceptual load or substantial duplication. Keep wrappers that enforce a real contract. Remove hypothetical flexibility only after verifying actual callers and extension commitments.
6. Keep rationale, invariants, and concurrency explanations. Do not impose kernel whitespace, typedef, function-length, macro, or allocation rules on unrelated C programs.

## Primary guidance and boundaries

- [Linux development guide, coding](https://docs.kernel.org/process/4.Coding.html): scrutinize flexibility without real callers and excessive indirection, while recognizing useful shared code. Its cross-OS hardware-wrapper guidance is a kernel constraint. Mature source also contains historical styles; neither copying all old code nor sweeping style cleanup is warranted.
- [Linux coding style, sections 6–8](https://www.kernel.org/doc/html/latest/process/coding-style.html): reason about conceptual complexity, staged cleanup, and explanatory comments. A long straightforward dispatch can be clearer than fragmented tiny helpers.
- [Linux scope-based cleanup helpers](https://docs.kernel.org/core-api/cleanup.html): `__free`, guards, reverse cleanup order, and ownership transfer are supported idioms with ordering obligations. Follow the subsystem and supported version, not a belief that all veteran C must use `goto`.
- [Submitting patches](https://docs.kernel.org/process/submitting-patches.html): keep a patch to one logical change and intermediate states buildable. Quantify performance claims and tradeoffs.
- [SQLite anomaly testing](https://sqlite.org/testing.html): use failure injection for allocation/I/O and integrity checks as a testing principle. Adapt the method to the module; do not import an entire database testing framework.

## Source-reading exemplars

- **Linux `v6.12`, `fs/eventfd.c`**: [source](https://raw.githubusercontent.com/torvalds/linux/v6.12/fs/eventfd.c). Inspect `do_eventfd` for allocation/descriptor/file lifetime transitions; `eventfd_ctx_fdget` and `eventfd_ctx_fileget` for distinct file/context references; `eventfd_poll` for the missed-wakeup and memory-ordering explanation. The apparent extra reference operations and long comment serve different obligations. This is an ownership example, not proof every precondition is safe.
- **Linux `v6.12`, `lib/kfifo.c`**: [source](https://raw.githubusercontent.com/torvalds/linux/v6.12/lib/kfifo.c). Inspect `__kfifo_alloc`, `kfifo_copy_in`, `kfifo_copy_out`, and user-copy helpers. Power-of-two capacity, split wraparound copies, publication barriers, and partially completed transfers constrain simplification. Do not turn two similar copies into one generic helper before accounting for those differences.

For generated C, identify the authoritative inputs and regeneration command. [SQLite's amalgamation](https://sqlite.org/amalgamation.html) is a deliberate generated distribution unit; file size and repetition alone say little about source architecture.

## Pre-2021 comparison

Linux **v5.10**, commit [`2c85ebc57b3e1817b6ce1a6b703928e113a90442`](https://github.com/torvalds/linux/commit/2c85ebc57b3e1817b6ce1a6b703928e113a90442), dated **2020-12-13**: compare [`fs/eventfd.c`](https://github.com/torvalds/linux/blob/2c85ebc57b3e1817b6ce1a6b703928e113a90442/fs/eventfd.c), `do_eventfd` and `eventfd_poll`, and [`lib/kfifo.c`](https://github.com/torvalds/linux/blob/2c85ebc57b3e1817b6ce1a6b703928e113a90442/lib/kfifo.c). Descriptor/context failure cleanup and missed-wakeup reasoning already explain complexity in this snapshot. Use it for historical provenance, not as a replacement for current supported cleanup APIs or subsystem policy.
