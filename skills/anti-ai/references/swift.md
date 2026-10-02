# Swift: API clarity, isolation, and lifecycle contracts

Research checked: **2026-10-02 UTC**. Pinned source: **swift-6.2-RELEASE**, commit `1ff1cc1170617ab23ab74aa8b741c8daca1903f6` ([commit metadata](https://api.github.com/repos/swiftlang/swift/git/commits/1ff1cc1170617ab23ab74aa8b741c8daca1903f6)). Treat this as a historical example, not the latest release. Respect language mode, SDK, deployment target, availability, and the repository's UI/concurrency conventions.

## Review sequence

1. Keep optionality, thrown errors, and `Result` aligned with the API's meaning. Do not wrap every `async throws` operation in `Result` or turn errors into empty values with `try?` by reflex. Preserve useful error context and deliberate recovery.
2. Audit force unwraps, assertions, and preconditions by contract. Debug assertions cannot validate production input. Preserve intentional programmer-contract failures rather than silently returning. Avoid side effects inside assertion conditions.
3. Identify each task's owner, lifetime, completion/error observation, and cancellation policy. Use structured children where their lifetime belongs to an enclosing operation; retain an explicitly owned unstructured task at a lifecycle/event boundary when appropriate.
4. Treat every `await` as a possible state-change boundary. Actor isolation does not make a multi-suspension operation atomic. Revalidate assumptions or gate a post-await commit on current state. Check stale completions, cancellation, cleanup, and reentrancy.
5. Do not use detached tasks, dispatch wrappers, `@unchecked Sendable`, or `nonisolated(unsafe)` merely to silence diagnostics. Investigate the isolation and ownership contract.
6. Evaluate protocols, factories, generics, `some`, and `any` by required substitution, identity, or heterogeneous storage. Preserve actual compatibility/test seams. Check call-site clarity and argument labels; do not import Linux names or Rust error conventions mechanically.
7. Treat access, availability, `@inlinable`, `@usableFromInline`, and `@frozen` as compatibility decisions. Preserve public contract docs, invariants, cancellation/lifetime notes, and accessibility behavior.

## Primary guidance and boundaries

- [Swift API Design Guidelines](https://www.swift.org/documentation/api-design-guidelines/): prefer clarity at use sites and useful declaration documentation. A rule against redundant implementation narration does not justify deleting API summaries.
- [SE-0413, typed throws](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0413-typed-throws.md#when-to-use-typed-throws): ordinary `throws` remains the better default for most code. Consider typed throws for stable exhaustively handled error sets, generic pass-through, or constrained environments; exposing a concrete dependency error can restrict evolution.
- [Language guide: concurrency](https://github.com/swiftlang/swift-book/blob/main/TSPL.docc/LanguageGuide/Concurrency.md): inspect task structure, cooperative cancellation, and actor suspension semantics. A cancellation request does not guarantee the operation has stopped.
- [Opaque and boxed protocol types](https://github.com/swiftlang/swift-book/blob/main/TSPL.docc/LanguageGuide/OpaqueTypes.md): choose underlying-type identity versus varying conforming values deliberately.
- [Library evolution](https://www.swift.org/blog/library-evolution/): even private stored representation can be ABI-relevant in a frozen public struct. Do not add performance annotations or change representation speculatively.

## Source-reading exemplars

All links below use the pinned commit above:

- [Assert.swift](https://github.com/swiftlang/swift/blob/1ff1cc1170617ab23ab74aa8b741c8daca1903f6/stdlib/public/core/Assert.swift): `assert` conditions are not evaluated under `-O`; `precondition` checks differ under `-Ounchecked`. Preserve release-mode semantics.
- [Result.swift](https://github.com/swiftlang/swift/blob/1ff1cc1170617ab23ab74aa8b741c8daca1903f6/stdlib/public/core/Result.swift): success/failure cases and conformances coexist with older ABI map implementation. Apparent duplication can be required compatibility code.
- [TaskCancellation.swift](https://github.com/swiftlang/swift/blob/1ff1cc1170617ab23ab74aa8b741c8daca1903f6/stdlib/public/Concurrency/TaskCancellation.swift): a handler can race with the operation; a pre-canceled task still invokes the operation. Preserve lock/continuation obligations and retained executor-inheritance ABI shims.
- [TypeCheckConcurrency.cpp](https://github.com/swiftlang/swift/blob/1ff1cc1170617ab23ab74aa8b741c8daca1903f6/lib/Sema/TypeCheckConcurrency.cpp), executor-conformance checking: distinguish migration-compatible old implementations from mutually recursive defaults that provide no work. Old-looking paths require behavioral inspection before removal.

Use actual repository build/test commands and supported platform configurations. [Actor-isolation tests](https://github.com/swiftlang/swift/blob/1ff1cc1170617ab23ab74aa8b741c8daca1903f6/test/Concurrency/actor_isolation_swift6.swift) and [cancellation tests](https://github.com/swiftlang/swift/blob/1ff1cc1170617ab23ab74aa8b741c8daca1903f6/test/Concurrency/async_cancellation.swift) demonstrate explicit language-mode and diagnostic coverage; compile/diagnostic success is not runtime race proof. Use controlled before/after measurements for performance claims, as illustrated by the [benchmark suite](https://github.com/swiftlang/swift/blob/1ff1cc1170617ab23ab74aa8b741c8daca1903f6/benchmark/README.md). Label unavailable Apple SDKs, compilers, simulator tests, and runtime evidence honestly.

## Pre-2021 comparison

Swift **swift-5.3-RELEASE**, commit [`e7c2f897ad2766b1664b56f3a77d939b844dd135`](https://github.com/swiftlang/swift/commit/e7c2f897ad2766b1664b56f3a77d939b844dd135), dated **2020-09-16**: [`Assert.swift`](https://github.com/swiftlang/swift/blob/e7c2f897ad2766b1664b56f3a77d939b844dd135/stdlib/public/core/Assert.swift) distinguishes debug-only assertions from release preconditions, and [`Result.swift`](https://github.com/swiftlang/swift/blob/e7c2f897ad2766b1664b56f3a77d939b844dd135/stdlib/public/core/Result.swift) preserves success/failure mapping and public documentation. These support build-mode and API-contract review. This snapshot predates Swift's structured-concurrency implementation; **do not use it as evidence about actors or task cancellation**. Retain the modern sources above for those questions.
