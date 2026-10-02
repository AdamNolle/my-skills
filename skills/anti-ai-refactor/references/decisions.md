# Evolution and recovery decisions

## A small refactor does not need a rollout ceremony

Renaming a private local variable with known callers may need only normal checks. Changing one timeout, shared cache key, persisted enum, or public default can change a broad contract. Select rigor by consequence, exposure, reversibility, and uncertainty, not line count.

## Worked example: stable errors behind a wrapper

A wrapper maps two provider SDK errors to one stable application exception and preserves cancellation. Removing it because it only has one current provider breaks callers and future retry classification. Simplify inside the boundary if useful; test real error and cancellation behavior. A private pure forwarding function with no separate consumer/invariant may legitimately collapse.

## Worked example: persisted field rename

Old and new binaries can coexist. A source rename from `name` to `display_name` compiles but may fail against old records or readers. Enumerate actual old/new reader-writer combinations, choose a compatibility path, define authority if both representations exist, and verify migration interruption plus concurrent writes. Retire the old field only when supported consumers and recovery obligations end.

For an offline app owning its entire store, a versioned transactional migration with tested restoration may be sufficient. Do not impose a distributed migration pattern without a mixed-version requirement.

## Worked example: retry after a timeout

A timed-out write may already have committed. Adding an unbounded retry or retrying at several layers can duplicate the business effect or exhaust capacity. Establish idempotency/deduplication, shared deadline, error classification, and commit visibility first. HTTP method names alone do not establish the complete business contract.

## Worked example: rollout with no evidence

A candidate serves no relevant requests and emits no errors. That is insufficient to promote. Observe the affected operation and version/cohort, account for delayed/background effects, and compare the agreed guardrails. A feature flag that hides UI may leave background writes active. Code rollback does not undo durable side effects.

## Recovery rubric

- Identify the last reversible point and what remains after each failure phase.
- For backfills, check restart safety, bounded batches, progress, and protection against clobbering fresher writes. Equal row counts do not prove semantic equivalence.
- Exercise allowed mixed-version pairs and representative historical records/events.
- Test restore/recovery assumptions in a disposable environment when authorized; possessing a backup is not evidence it restores correctly.
- If reversal loses valid new state, define containment and forward repair instead of a fictitious one-click rollback.
- Stop dependent execution if authority, source of truth, supported consumer behavior, destructive effects, or required observation is unresolved; continue safe investigation.

## Primary sources and limits

Research checked 2026-10-02. Apply the target system’s version and deployment model first.

- [Google small changes](https://google.github.io/eng-practices/review/developer/small-cls.html): prefer one self-contained functioning change, not an arbitrary line-count quota.
- [Google review standard](https://google.github.io/eng-practices/review/reviewer/standard.html): improve code health without demanding perfection or elevating preference to defect.
- [Google AIP-180](https://google.aip.dev/180): source, wire, and semantic compatibility differ; independently updating consumers matter.
- [Microsoft API versioning](https://github.com/microsoft/api-guidelines/blob/vNext/azure/VersioningGuidelines.md): behavior changes can be breaking even without schema changes. Azure-specific governance is not a required local process.
- [Swift library evolution](https://www.swift.org/blog/library-evolution/): separately distributed binary frameworks differ from co-built application modules; verify supported toolchain/settings.
- [RFC 9110 idempotent methods](https://www.rfc-editor.org/rfc/rfc9110.html#section-9.2.2): automatic repetition depends on semantics; idempotency does not promise identical responses or no incidental effects.
- [Database migration and fallback](https://docs.cloud.google.com/architecture/database-migration-concepts-principles-part-2): client semantics and intervening writes matter beyond data checks. Large database moves are not every migration.
- [Apple staged Core Data migrations](https://developer.apple.com/documentation/coredata/staged-migrations): use supported versioned facilities and meaningful domain values for existing data; verify OS availability.
- [Google SRE canarying](https://sre.google/workbook/canarying-releases/): limited exposure needs meaningful evaluation; do not copy example thresholds.
- [Microsoft safe deployment](https://learn.microsoft.com/en-us/azure/well-architected/operational-excellence/safe-deployments): operational evidence and stateful recovery matter; select a window from actual usage and risk.
