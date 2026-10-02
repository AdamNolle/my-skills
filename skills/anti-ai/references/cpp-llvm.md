# C++ and LLVM/Clang: contracts, ownership, and useful abstraction

Research checked: **2026-10-02 UTC**. LLVM/Clang and the C++ Core Guidelines have different error and runtime policies. Apply the target project's supported standard and conventions first. Historical release-tag examples below are not a claim about the latest release or universal best practice.

## Review sequence

1. Identify ownership, borrowing, invalidation, exception guarantees, and destruction order. Prefer existing RAII handles and explicit ownership over scattered cleanup; do not replace safe types with raw pointers merely to reduce layers.
2. Preserve distinctions between fallible external input and internal invariants. Assertions and unreachable markers cannot validate untrusted input, and required side effects cannot live only inside assertions.
3. Evaluate types, templates, interfaces, and wrappers by the invariant or real variation they support. Keep ABI bridges and real test seams. Prefer simple local code over speculative extensibility, but do not ban classes, templates, or forwarding functions.
4. Follow the established error policy. Propagate meaningful failure/context; use deliberate fallback only when its contract is visible. Do not introduce exceptions into exception-free LLVM code or remove exceptions from unrelated C++ to imitate LLVM.
5. Separate a behavior fix from naming, movement, and formatting churn. Measure performance with a comparable workload, build settings, and repeated runs; state variability and limits.

## Primary guidance and boundaries

- [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines): use C.2/C.4 for invariant-bearing types and member boundaries; R.1/R.3/R.5/R.21 for scoped resources and ownership; T.2 for justified genericity; Per.1–Per.6 for measured performance. These are guidelines, not the ISO standard or an audited implementation. Raw-pointer nonownership is a convention to verify in legacy code.
- [LLVM Coding Standards](https://llvm.org/docs/CodingStandards.html): prefer clear control flow, distinguish recoverable failures from impossible states, and respect assertions-disabled builds. LLVM's restrictions on exceptions/RTTI and its casting facilities are project policies, not language rules.
- [LLVM Programmer's Manual](https://llvm.org/docs/ProgrammersManual.html): inspect the error-handling section for `Error`/`Expected`, checked/forwarded failure, typed context, and justified `cantFail`. A success-shaped empty value is not an acceptable substitute for error propagation unless the API explicitly specifies it.
- [LLVM Developer Policy](https://llvm.org/docs/DeveloperPolicy.html) and [Code-Review Policy](https://llvm.org/docs/CodeReview.html): make localized, reviewable changes and provide the tests and explanation needed to assess consequences.
- [LLVM Testing Guide](https://llvm.org/docs/TestingGuide.html): isolate regression behavior, use a focused pipeline, and retain relevant target assumptions. Generated FileCheck assertions are supported and are not inherently junk.
- [LLVM Benchmarking Tips](https://llvm.org/docs/Benchmarking.html): control sources of variance without assuming low noise removes bias. Do not change privileged or security-sensitive machine settings as part of ordinary cleanup; use allowed measurements and report limitations.

## Source-reading exemplars

- **`llvmorg-18.1.8`, `llvm/lib/Support/MemoryBuffer.cpp`**: [source](https://raw.githubusercontent.com/llvm/llvm-project/llvmorg-18.1.8/llvm/lib/Support/MemoryBuffer.cpp). Inspect `getMemBufferCopyImpl`, `shouldUseMmap`, `getFileAux`, and `getOpenFileImpl`. Empty data, allocation failure, file volatility, null-termination, page boundaries, short reads, and EOF all explain branches. A seemingly repeated status check can avoid another syscall. Historical allocation techniques here do not justify copying them into application code without need.
- **`llvmorg-18.1.8`, `clang/lib/Tooling/CommonOptionsParser.cpp`**: [source](https://raw.githubusercontent.com/llvm/llvm-project/llvmorg-18.1.8/clang/lib/Tooling/CommonOptionsParser.cpp). Inspect `init`, `create`, and `ArgumentsAdjustingCompilations`. The factory propagates initialization failure, while compilation-database autodetection has a visible intentional fallback. Forwarding methods preserve a real database interface. Check callers before treating fallback or delegation as redundant.

Check [TableGen's authoritative records/backends](https://llvm.org/docs/TableGen/index.html) before touching generated output. Preserve the generation path rather than hand-editing repetitive derived files.

## Pre-2021 comparison

LLVM **llvmorg-11.0.0**, commit [`176249bd6732a8044d457092ed932768724a6f06`](https://github.com/llvm/llvm-project/commit/176249bd6732a8044d457092ed932768724a6f06), dated **2020-10-07** (tag dated **2020-10-12**): [`MemoryBuffer.cpp`](https://github.com/llvm/llvm-project/blob/176249bd6732a8044d457092ed932768724a6f06/llvm/lib/Support/MemoryBuffer.cpp), `shouldUseMmap`, already distinguishes volatility, page boundaries, and null termination, with intentional duplicated status work to avoid a syscall. [`CommonOptionsParser.cpp`](https://github.com/llvm/llvm-project/blob/176249bd6732a8044d457092ed932768724a6f06/clang/lib/Tooling/CommonOptionsParser.cpp), `create`, propagates initialization errors while the legacy constructor has different fatal-error semantics. Similar-looking entry points can preserve different public contracts. Do not copy old allocation machinery or infer current APIs from this snapshot.
