# Contract decisions

## Decision rubric

- **Keep a seam** when it owns a real invariant, resource lifetime, compatibility promise, substitution boundary, or independently evolving policy.
- **Collapse forwarding** only after showing that no distinct contract, external consumer, injection seam, or likely change boundary is lost. Evaluate the net call path rather than counting layers.
- **Split state or responsibility** when concrete combinations are invalid or a change repeatedly requires unrelated policies to move together. A long file alone is insufficient.
- **Keep duplication** when two paths have different reasons to change. Share the stable invariant, not every repeated token.
- **Ask for a product decision** when callers disagree about behavior, durability, allowed failure, latency, or compatibility. Do not silently select the easiest implementation.

## Worked example: transaction-shaped wrapper

`store.save(order)` calls a repository, registers a post-commit event, and maps a backend error to a stable public error. Replacing it with `db.insert(order)` is shorter but removes the event/commit boundary and error contract. Inspect rollback behavior and caller retries. Retain the wrapper or move each obligation explicitly; forwarding shape is not redundancy evidence.

Counterexample: a private helper adds no validation or adaptation and only forwards identical arguments to another private function. If all callers are statically known and tests cover error/lifetime behavior, collapsing it may improve navigation. That does not justify removing a public compatibility alias.

## Worked example: absent versus unavailable

An empty search result means the query succeeded with zero matches. A dependency timeout means the answer is unknown. Returning `[]` for both simplifies callers while silently changing the contract. Choose an existing error/result convention; test both successful emptiness and dependency failure. Do not add a custom type hierarchy if the language/framework already supplies the needed distinction.

## Worked example: one implementation, real boundary

An environment interface with one production implementation can permit deterministic clock or filesystem fault tests. Preserve it when those tests exercise a real failure model. Conversely, an unused factory, registry, and abstract base introduced for an imaginary second provider impose maintenance costs with no current consumer. Propose the smallest removal, accounting for exported/plugin consumers first.

## Precedents and limits

Use the target repository before external examples. The anti-ai corpus includes LevelDB’s public ownership and environment contracts, Django’s transaction state machine, and Rust’s absence/error distinctions. Inspect the relevant corpus guide only if it resolves an actual question.

- [LevelDB public DB API, pinned 1.22](https://github.com/google/leveldb/blob/78b39d68c15ba020c0d60a3906fb66dbf1697595/include/leveldb/db.h): ownership, snapshot, and concurrency promises justify narrow interfaces. Embedded-store contracts do not imply distributed consistency.
- [Django transaction implementation, pinned 3.1](https://github.com/django/django/blob/ec5bc3a991bf6f2b9afe9b1b4068e4a1e62001c2/django/db/transaction.py): nested failure/recovery is a real reason for wrappers. Applications should use supported APIs rather than transplant internals.
- [Rust API Guidelines: future proofing](https://rust-lang.github.io/api-guidelines/future-proofing.html): public type and representation decisions constrain evolution. Verify current language and repository compatibility policy before applying.

Historical sources were inspected in the parent corpus on 2026-10-02; they are reasoning precedents, not recommended dependency versions or proof of universal superiority.
