---
name: anti-ai-refactor
description: Plan and perform bounded refactoring, compatibility-sensitive cleanup, migrations, and release-readiness review with explicit behavior and recovery checks. Use when simplifying implementation, removing proven dead code, evolving API or storage formats, or assessing whether a change is safe to land or roll out. Audit first unless edits are requested; deployment and destructive actions need their own authority.
---

# Change less; preserve the contract

Make one coherent, reviewable change with a clear behavior delta and evidence. “Refactor” is not permission to redesign a repository or deploy to production.

## Scope and baseline

- Identify the requested outcome, permitted paths/behavior changes, current revision/diff, local rules, supported versions, and relevant baseline checks. Preserve user changes; never reset, stash, overwrite, or broadly format unrelated work.
- Default to read-only audit/plan. For authorized edits, state what remains invariant and what intentionally changes. If essential work exceeds scope, pause that portion and present the choice.
- Inspect callers, tests, dynamic registration, exported APIs, generated/vendor boundaries, feature/deployment configurations, and consumers before calling anything dead. No text matches is insufficient.

## Make the smallest credible change

1. Demonstrate the defect or maintenance cost with a path, counterexample, invariant, or actual change burden. Keep valid compatibility layers, error mapping, safety checks, telemetry, accessibility, and useful documentation.
2. Separate structural simplification from behavior changes where practical. Preserve errors, cleanup, ordering, cancellation, retry, serialization, lifetime, and supported platforms. Fewer lines is not the acceptance criterion.
3. Use existing language/project facilities. Do not introduce a new generic framework, dependency, unsafe shortcut, forced unwrap, detached work, or swallowed error merely to make the diff tidy.
4. Add the smallest meaningful regression or characterization tests. Characterization captures existing intentional behavior; it must not canonize an identified bug. Load anti-ai-tests only when evidence design needs help.
5. Review each coherent slice, then run appropriate targeted/broader checks. Avoid broad automatic rewrites and untested deletion scripts.

## Compatibility and durable changes

Skip this section for purely local reversible work. For API/ABI/wire/storage/configuration changes, read [evolution and recovery decisions](references/decisions.md).

- Identify independently updating consumers and all supported mixed-version combinations. Include queued events, offline clients, backups, plugins, and operational tools when relevant. Compilation or schema shape alone cannot prove semantic compatibility.
- Plan compatible expansion, convergence/backfill, switch, and retirement only where the actual system needs them. Define the source of truth and concurrent-write/retry behavior. A transactional local migration can be simpler than dual writes.
- Name the durable point after which code rollback cannot undo new records or external effects. Distinguish executable rollback, data recovery, and forward repair. Do not promise rollback without testing its assumptions.
- Treat production rollout, destructive cleanup, data access, and permission changes as separate actions requiring authority. A design or patch request does not authorize them.
- For an authorized rollout, use observable user outcomes, representative traffic, and explicit promote/hold/abort criteria. Missing observations or an empty canary are inconclusive. Do not invent universal percentages or bake times.

## Final verification and review

Run checks on the exact final revision plus diff and relevant untracked inputs; retain commands, counts/results, and environment limits. Demonstrate red/green when feasible in an isolated copy. Rerun affected checks after later edits or concurrent changes. Review the complete final diff for hidden behavior changes, API drift, test weakening, unsafe deletion, sensitive data, or unrelated churn.

## Output contract

Return the bounded change/findings, preserved and intentionally changed contracts, evidence actually obtained, and residual risks. Include migration/recovery decisions only when relevant. Separate blocker, optional improvement, and question. In audit mode return a patch plan without applying it; in repair mode provide the coherent patch and exact-state checks. Never claim untested production readiness or universal correctness.
