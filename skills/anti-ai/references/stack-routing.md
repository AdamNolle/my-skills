# Selective engineering stack

## Route by the unresolved question

Use one primary specialist, adding another only when its evidence is necessary. Each specialist can also be invoked directly. Load it by its exact name from the available skill catalog; do not assume every environment has every installed tool. References are optional deeper guidance, not a reading quota.

| Question | Primary skill | Add only when needed |
| --- | --- | --- |
| What behavior or boundary should callers rely on? | anti-ai-contracts | anti-ai-tests for a difficult oracle; anti-ai-refactor for mixed-version evolution |
| Does this implementation satisfy its contract? | anti-ai-tests | anti-ai-performance for an actual interleaving or measured resource claim |
| Does this change preserve authority and tenant isolation? | anti-ai-security | Dedicated Codex Security workflow for an actual formal scan/finding task |
| Why is this slow, leaking, racing, or not stopping? | anti-ai-performance | anti-ai-tests for regression design; anti-ai-contracts for unsettled lifetime semantics |
| Which comments are accurate and useful? | anti-ai-comments | anti-ai-docs only if information genuinely moves into a document |
| Which docs are used, true, and maintainable? | anti-ai-docs | anti-ai-comments only for source/docstring consumers; anti-ai-refactor for actual migration semantics |
| Can we simplify or evolve this safely? | anti-ai-refactor | anti-ai-contracts for unclear promises; anti-ai-tests for a difficult regression |

Ordinary code review and minor local simplification can stay in anti-ai. Do not summon architecture, performance, security, and release analysis just because code is present. A security scan request goes to the matching existing security skill rather than an abbreviated checklist here. Platform UI/accessibility, packaging, and language/tool-specific tasks can use their existing relevant skills without being duplicated into this stack.

## Minimal shared handoff

Pass only task-local facts that change the result:

- Intent and authority: audit/design/implement/repair/validate, allowed paths/behavior, and relevant external-action limits
- State: repository/path, HEAD and relevant dirty inputs, toolchain/environment, existing failures or concurrent edits
- Contract: observable success/failure, invariant, actual callers/consumers, unresolved choice
- Risk: concrete consequence, exposure, reversibility, and uncertainty; no fabricated numerical score
- Evidence: source paths/symbols, executed commands/results, missing checks, supported versions and source links when material
- Desired result: findings, a bounded plan, or an authorized patch, with compatibility/recovery implications only where relevant

A tiny task needs only a few lines. Do not ask every specialist to rebuild the entire inventory or repeat valid checks. Reuse evidence only if its inputs, scope, and tested state match; clearly distinguish cached evidence from a fresh execution. If later changes invalidate a check, rerun the affected check, not necessarily every unrelated suite.

## Size effort by consequence

- Local/reversible change: inspect affected behavior and consumers; focused checks and final diff review usually suffice.
- Contract-bearing change: resolve behavior, ownership, errors, compatibility, or concurrency; exercise relevant failure paths.
- Durable/widely exposed change: account for actual mixed versions, persistent state, recovery, trust boundaries, and operational evidence.
- Uncontrolled/irreversible uncertainty: stop the dependent action and surface the specific evidence or authority needed; continue safe independent work.

One changed line can be high risk; a large mechanical/generated change can be low risk if provenance and consumers are verified. Do not prescribe universal line, coverage, latency, or document-count thresholds.

## Review and stopping conditions

Finish when the scoped question is answered with evidence, the authorized patch is verified to the stated extent, or a precise blocker/decision prevents progress. Report inspected scope and a valid no-finding/no-change result. Separate a demonstrated defect, missing evidence, optional improvement, and unsupported speculation. Do not chase unrelated smells after the requested outcome is met.

Keep reports concise: outcome, consequential evidence, actual verification, residual risk or next decision. Provide maintainer-facing rationale, not private chain-of-thought or a transcript of internal deliberation. Never claim superiority to a company, infer authorship, or certify an entire system from sampled checks.
