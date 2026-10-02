# Comment decisions

## Worked example: useful “what”

A public function’s summary says it returns a reversed view. Its generated API page does not include the implementation. Keep a concise behavior summary and any lifetime/error caveats even if the function name is descriptive. A private comment saying “increment count by one” beside `count += 1` has no additional value unless the counting rule itself is non-obvious.

## Worked example: false confidence

“Thread-safe update” does not identify the synchronization contract. If all callers hold a particular lock and readers observe publication under that lock, state that verified relationship. If a caller violates it, report a correctness issue rather than write reassuring prose. A `SAFETY` label similarly needs a local proof of the actual bounds, lifetime, alignment, aliasing, or initialization obligation; a comment cannot repair unsound code.

## Worked example: compatibility TODO

A fallback exists until the minimum supported server has a bulk endpoint. Preserve the TODO while supported old servers remain. Remove or rewrite it only after checking the actual version matrix and whether the fallback’s work is complete. Do not infer completion from a closed upstream issue or fabricate an issue identifier.

## Worked example: intentional invalid code

A source comment contains an example of incorrectly joining separately allocated buffers, explaining why it is unsafe. Keep the counterexample. A dormant old parser with no current instructional/tool/migration purpose is a deletion candidate after checking ownership and history. Similar-looking commented code can have opposite dispositions.

## Protected consumers

- **Legal/provenance:** retain licenses, SPDX, copyright and attribution absent separately authorized substantiated work.
- **Generated/tooling:** locate generators and templates; identify build tags, `go:generate`, encoding/shebang lines, formatter/linter/coverage directives, source maps, and purity hints before edits.
- **Tests:** preserve LLVM `RUN:`/`CHECK:` and deliberate `COM:` handling, `@ts-expect-error`, doctests, rustdoc tests, example output markers, and parser fixtures. Removing text can silently weaken validation.
- **Public/runtime documentation:** preserve caller contracts, errors, preconditions, ownership, and useful examples; Python docstrings may be consumed through `__doc__`.
- **Accessibility/localization:** retain reasons for focus restoration, keyboard behavior, reduced motion, forced colors, or assistive-technology workarounds; translator comments serve readers without code context.
- **Algorithms/concurrency:** preserve theory-of-operation, invariants, lock order, memory ordering, numerical assumptions, rejected alternatives, and paper-to-code mappings that maintainers need.

## Disposition rubric

Keep accurate, useful information. Rewrite a verified useful core that is unclear or stale. Consolidate duplicated truths only after identifying a maintained home and preserving readers/links. Remove proven valueless residue only in scope. Leave uncertain contracts unchanged and state the missing evidence.

Treat words like “atomic,” “secure,” “constant-time,” “never fails,” and “O(1)” as specific claims requiring a scoped meaning, not as automatic removal signals. Reject comment-density targets and authorship classification. A tutorial may legitimately explain syntax; a production caller may need a short public summary; a tricky algorithm may need a long proof.
