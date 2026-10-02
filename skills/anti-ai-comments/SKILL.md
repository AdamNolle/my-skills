---
name: anti-ai-comments
description: Review and improve source comments, API documentation comments, and docstrings by verified truth and reader value. Use for noisy comments, boilerplate, stale TODOs, unsupported claims, commented-out code, or requested comment cleanup. Preserve contracts, rationale, examples, legal notices, directives, safety, accessibility, and localization context. Never infer AI authorship or apply comment quotas.
---

# Comments that earn their place

Keep accurate information where its reader needs it. “Why, never what” is too crude: API behavior, algorithm mechanics, examples, and tool directives can all be essential.

## Scope and consumers

- Default to audit. Edit or delete only within an authorized cleanup or implementation scope. Read the containing unit, callers/tests, repository conventions, supported tools, and relevant history; preserve unrelated changes.
- Identify the actual reader or consumer: caller, maintainer, operator, translator, documentation generator, runtime reflection, compiler, linter, formatter, coverage tool, or test harness.
- Protect legal/copyright/SPDX notices, generated markers and source-of-truth templates, public API contracts, unsafe proofs, tool directives, accessibility/localization guidance, and intentionally invalid examples. “Protect” means review under their own obligations, not never correct an error.

## Evaluate each candidate

1. State the precise claim: units, bounds, error behavior, ownership, lifetime, ordering, allowed execution context, retry/durability guarantee, performance, or rationale.
2. Verify it against implementation, callers, tests, supported specification, and history where needed. A mismatch can be a code bug or broken public promise; do not rewrite the comment to bless a regression.
3. Identify what knowledge would be lost. Keep non-obvious invariants, safety assumptions, negative constraints, useful examples, stable domain meaning, and reasons for awkward choices. Preserve long explanations when they make complex code reviewable.
4. Investigate obvious syntax narration, empty templates, copied wrong symbols, unsupported praise, conversation residue, obsolete process notes, abandoned implementations, and stale TODOs. Treat these as leads, never a word/length/style blacklist.
5. Choose **keep**, **rewrite**, **consolidate/move**, **remove**, or **investigate unchanged**. Remove only when no unique useful information or legal/tool role remains and deletion is authorized. Prefer a local constraint summary plus a durable source link when moving long rationale.

Read [decision examples and protected consumers](references/decisions.md) for ambiguous cases. Read [primary guidance](references/sources.md) only for the language/tool or claim under review. Load anti-ai-docs only when a document-level change is genuinely needed.

## Patch and validate

- Keep the smallest information-preserving patch. Do not invent measurements, owners, issue numbers, compatibility reasons, or safety proofs. An uncertain claim deserves investigation, not authoritative replacement prose.
- For a TODO/workaround, verify the completion condition in the supported platform/dependency matrix. Old dates and closed issues alone are insufficient. Removing a reminder does not perform the unresolved task.
- Preserve syntax-sensitive placement and grouping. Change generated comments at their source. Public docstrings can be runtime data; source comments can define tests or generated output.
- Validate affected consumers: parser/type/build checks, docs build, doctests/rustdoc/examples, directive-driven tests, generated-output checks, localization grouping, or reflected docstrings as applicable. Inspect rendered API docs and links when changed. Compilation alone cannot validate prose or directive preservation.
- Review the final diff for lost contracts, examples, licenses, semantics, and unrelated formatting. Record actual commands/results and exact checked state; label unrun checks. A comment-only token comparison is supplemental, not proof that consumers are unaffected.

## Output contract

Report consequential corrections/removals with location, reader, evidence, disposition, and verification. Separate misinformation from optional polishing; a no-change result is valid. Measure quality by truthful useful information and maintained contracts, never by deleted lines, “AI probability,” or mandatory comment density.
