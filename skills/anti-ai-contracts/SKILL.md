---
name: anti-ai-contracts
description: Evidence-led architecture and contract review for APIs, state models, abstraction boundaries, compatibility, and data ownership. Use when reviewing a design or change, simplifying wrappers, planning an interface, or investigating boundary ambiguity. Audit first; do not impose a new architecture or erase legitimate seams. Use anti-ai for broad cleanup routing.
---

# Contract-led design

Make the governing contract visible before changing the structure. A simple implementation can implement a demanding contract; a small interface can conceal dangerous state.

## Scope and input

- Accept a concrete design, repository path, patch, or API plus the requested outcome. Read local instructions, supported versions, callers, tests, and existing extension conventions.
- Default to read-only audit. Edit only when requested, within the stated scope. Record the current revision and worktree changes; preserve unrelated work.
- Recover missing contract details from sources before asking. If a consequential choice cannot be inferred, present that choice instead of inventing requirements.

## Review sequence

1. Name the observable promises: inputs/outputs, errors, ownership/lifetime, ordering, idempotency, concurrency, and compatibility. Include wire/storage/ABI contracts only where applicable.
2. Trace one successful path and one failed or interrupted path across real callers and boundaries. Identify who owns each state transition and where a partial result can escape.
3. Classify each disputed abstraction by its actual job: invariant/capability, lifetime, variation, compatibility, test/environment seam, or mere forwarding. One implementation does not prove uselessness. Similar syntax does not prove shared semantics.
4. Check representation against domain states. Distinguish unknown, absent, failed, empty, and zero. Prefer an existing type or small explicit state model over booleans that admit invalid combinations; do not invent a framework for a local function.
5. Inspect public and dynamic consumers before removal: imports, exported symbols, schemas, migrations, reflection, plugin/configuration registrations, feature flags, deployed versions, and downstream promises. No textual callers is not proof of dead code.
6. Compare the smallest viable options against the actual failure model. Keep the current design if a change only moves complexity, breaks a contract, or relies on a speculative future requirement.

Read [decision examples](references/decisions.md) only for a disputed boundary, state-model, or abstraction decision. Load anti-ai-tests for test strategy or anti-ai-refactor for a requested cross-boundary edit; do not automatically load both.

## Authorized design or repair

State the intended invariant and changed behavior first. Preserve contracts by default, including exception identity, cancellation, ordering, and externally observable timing where promised. Design with local idioms and supported versions. Mark open choices and migration implications; do not label a proposal implemented.

For code changes, run targeted contract tests and applicable build/type checks on the exact final revision plus diff. Verify test collection, retain command/results, and label unrun checks. Recheck the final diff for accidental API, schema, or semantic drift.

## Output contract

Return the scope, a compact contract map, and only consequential findings. For each finding include location, real consumer/path, violated promise, concrete example, impact, confidence, smallest remedy, and verification. Separate defects from optional design tradeoffs. Report a clean review without manufacturing a redesign.
