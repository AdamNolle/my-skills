# Rust: ownership, useful failure types, and soundness

Research checked: **2026-10-02 UTC**. Pinned source: Rust **1.90.0**, commit `1159e78c4747b02ef996e55082b704c09b970588` ([tag metadata](https://api.github.com/repos/rust-lang/rust/git/tags/d8401009a052a5efaed3f5c901c76dd733c04fbe)). This is a historical teaching snapshot, not a claim about the latest version. Respect the target edition, MSRV, features, target platforms, and lint policy. Compiler internals are not automatic application-design recommendations.

## Review sequence

1. Distinguish expected failure, ordinary absence, and violated programming invariants. Preserve useful `Result`/`Option` semantics and error sources. Avoid success-shaped defaults, discarded `.ok()`, and string-matched error handling without a contract.
2. Inspect production `unwrap`, `expect`, indexing, and panic paths with their callers. Reject untrusted input through fallible APIs; keep intentional invariant failures when justified. Do not automatically change poison recovery or known-valid literal/test assumptions.
3. Map borrowed versus owned values and destruction/lock scope. Borrow for inspection; take ownership when retaining. Investigate clones before either adding them to appease the compiler or deleting them to reduce allocations. A snapshot or decoupled lifetime can require a clone.
4. Require a real capability, variation, invariant, substitution seam, or public contract for traits/newtypes/layers. Choose generics, `impl Trait`, or `dyn Trait` for their actual tradeoffs. Do not mechanically erase a public abstraction because it has one current implementation.
5. Audit unsafe operations together with surrounding safe code and privacy boundaries. Prove alignment, initialization, aliasing, bounds, allocation/lifetime, and drop obligations. A `SAFETY` comment or successful compilation does not establish soundness.
6. Preserve public/unsafe API docs, examples, error/panic conditions, and rationale. Review visibility, trait bounds/implementations, representation, and signatures for downstream compatibility before simplification.

## Primary guidance and boundaries

- [Rust Book, panic decisions](https://doc.rust-lang.org/stable/book/ch09-03-to-panic-or-not-to-panic.html): decide by failure contract, not a universal panic ban.
- [API Guidelines: interoperability](https://rust-lang.github.io/api-guidelines/interoperability.html): give public error types useful `Error`/`Display` behavior and sources, with applicable `Send`/`Sync`. Do not invent a large error hierarchy for every private helper.
- [API Guidelines: flexibility](https://rust-lang.github.io/api-guidelines/flexibility.html), [future proofing](https://rust-lang.github.io/api-guidelines/future-proofing.html), and [documentation](https://rust-lang.github.io/api-guidelines/documentation.html): request only needed capabilities, preserve extension/representation boundaries, and document contracts.
- [Rustonomicon, working with unsafe](https://doc.rust-lang.org/nomicon/working-with-unsafe.html): soundness can depend on safe code beyond the textual unsafe block.
- [Clippy usage](https://doc.rust-lang.org/clippy/usage.html): run repository-selected checks and inspect suggestions; do not enable every restriction/pedantic lint or automatically apply all fixes.

## Source-reading exemplars

- **`library/alloc/src/vec/mod.rs`**: [pinned source](https://github.com/rust-lang/rust/blob/1159e78c4747b02ef996e55082b704c09b970588/library/alloc/src/vec/mod.rs). Inspect `pop`, `try_reserve`, `as_mut_ptr`, and `set_len`. Absence, allocation failure, and invalid capacity/index contracts need different APIs. Pointer access avoids an intermediate reference for aliasing reasons; initialized elements and capacity remain caller obligations for `set_len`. A shorter delegation can change soundness.
- **`library/std/src/sync/poison/mutex.rs`**: [pinned source](https://github.com/rust-lang/rust/blob/1159e78c4747b02ef996e55082b704c09b970588/library/std/src/sync/poison/mutex.rs). Poisoning and recovery concern potentially broken invariants; unconditional recovery is not inherently safer than a deliberate panic.
- **`compiler/rustc_infer/src/infer/mod.rs`**: [pinned source](https://github.com/rust-lang/rust/blob/1159e78c4747b02ef996e55082b704c09b970588/compiler/rustc_infer/src/infer/mod.rs). Inspect `get_region_var_infos`: its clone preserves information needed by later compiler phases. Retain the rationale rather than enforcing a no-clone slogan.

Use repository test commands. Consider compile-fail/API tests, sanitizers, or Miri when relevant and supported. [rustc UI tests](https://rustc-dev-guide.rust-lang.org/tests/ui.html) illustrate explicit expected diagnostics and reviewed output updates, not a mandate to adopt compiler infrastructure. [rustc profiling guidance](https://rustc-dev-guide.rust-lang.org/profiling.html) supports choosing measurements for the resource under investigation. Report tools/tests that could not run.

## Pre-2021 comparison

Rust **1.49.0**, commit [`e1884a8e3c3e813aada8254edfa120e85bf5ffca`](https://github.com/rust-lang/rust/commit/e1884a8e3c3e813aada8254edfa120e85bf5ffca), dated **2020-12-29** (tag dated **2020-12-31**): [`library/alloc/src/vec.rs`](https://github.com/rust-lang/rust/blob/e1884a8e3c3e813aada8254edfa120e85bf5ffca/library/alloc/src/vec.rs), `pop`, `set_len`, and `try_reserve`, separates absence, initialization invariants, and allocation failure. **`try_reserve` is unstable in this snapshot**; use the target MSRV and current stability evidence before recommending an API. Historical unsafe implementation is material to inspect, never a shortcut around today's soundness rules.
