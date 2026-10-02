---
name: anti-ai-docs
description: Audit and maintain repository documentation, README files, Markdown, design records, operational guides, and executable examples. Use for requested documentation cleanup, stale or duplicated docs, missing task guidance, or behavior changes needing documentation. Judge by audience, truth, ownership, consumers, and recovery value; never delete by extension, age, filename, or AI-like style.
---

# Documentation with a job

Keep the shortest complete path from a reader’s task to an accurate answer. Remove proven duplication and obsolete residue without erasing operational, contractual, or historical knowledge.

## Scope and inventory

- Default to audit. Edit, consolidate, move, archive, or delete only as authorized. If the cleanup request does not clearly authorize file deletion, propose the exact paths and ask before deleting. Creating this skill does not authorize a repository purge. Record scope, revision, existing changes, and local contribution/doc rules.
- For candidate documents identify purpose, audience, owner/source of truth, consumers, maintained status, and generated/vendor origin. Check navigation, inbound links, anchors, build/package inputs, CI examples, help output, and external/public consumers where discoverable.
- Protect legal/attribution material, security disclosure/policy, public API contracts, supported setup instructions, accessibility/localization guidance, release/migration notes, incident/recovery runbooks, and architectural decisions. Treat protection as a need for specific analysis, not blanket immunity from correction.

## Review by task and truth

1. Pick a concrete reader task: install, call an API, contribute, operate, recover, migrate, or understand a consequential decision. Check whether required prerequisites, ordered steps, expected outcome, failure handling, and limitations are available without guessing.
2. Verify factual claims against supported code/configuration, interfaces, versions, commands, and owned policy. A contradiction may expose a code regression or unresolved contract; do not silently make docs agree with broken behavior.
3. Separate tutorial, how-to, reference, and explanation needs where it improves navigation. Do not force four documents onto a small project or create a new framework around a short README.
4. Locate repeated information and choose a maintained source of truth. Preserve essential context at use sites and update actual references; removing one duplicate without a usable destination can make the system harder to operate.
5. Distinguish historical decisions from current instructions. Keep superseded ADRs and incident/migration history when they explain consequences or recovery; label their status and point to the successor rather than rewrite history as if the old choice never existed.
6. Classify each candidate as keep, correct, consolidate, archive/supersede, remove, or investigate unchanged. Require evidence that no needed knowledge, legal obligation, consumer, supported-version guidance, or recovery path is lost before proposing removal.

Read [documentation decisions](references/decisions.md) for deletion/consolidation, historical records, executable examples, or operational guides. Read [primary documentation guidance](references/sources.md) only for a relevant consumer/tool or disputed practice. Load anti-ai-comments only for actual source-comment or docstring work.

## Authorized changes and validation

- Patch the authoritative source; do not hand-edit generated output unless the workflow explicitly requires it. Keep scope and navigation coherent. Do not generate a new report/Markdown file for every tiny correction.
- Check links, anchors, cross-references, site/doc builds, extraction, package contents, executable examples, and copy/paste commands as applicable. Confirm examples were collected and executed; a successful zero-example build proves no examples. Run examples only in a safe disposable environment with required authority; do not execute destructive or production commands merely because they appear in docs.
- Check command exit codes and expected outcomes, not only syntax. Label commands that need unavailable credentials, platforms, services, or prerequisites as unverified.
- Preserve version-specific and public obligations. Do not assume that no inbound repository link proves no readers; published URLs, bookmarks, packaging, and outside consumers may exist.
- Recheck the diff for lost licenses, warnings, data recovery, supported-version instructions, and unrelated prose churn. Verify the exact changed state, record actual checks/results, and report gaps.

## Output contract

Return the task/audience and reviewed scope, high-value findings or concise changes, disposition evidence, and actual validation. For proposed deletion identify the lost information/consumer check and replacement if any. Recommend no change when supported. Never use file counts, deletion quotas, timestamps, word count, or guessed authorship as quality evidence.
