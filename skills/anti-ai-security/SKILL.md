---
name: anti-ai-security
description: Triage trust, tenant, authority, input, output, and secret boundaries during engineering reviews or simplification. Use for security-sensitive code changes and cleanup that may remove safeguards. This is bounded boundary review, not a full security scan or certification; route formal audits, vulnerability validation, and fixes to the installed Codex Security workflows.
---

# Preserve trust boundaries

Treat security checks as contracts to explain and test, not clutter to remove. A short path can still carry too much authority.

## Input and scope

- Identify the requested change, relevant entry points, assets, principals, tenant identity, and threat assumptions. Read local security policy and supported deployment model.
- Default to read-only triage. Do not exploit live systems, expose secrets, change permissions, or implement a security fix merely because a review found a candidate issue.
- Separate a demonstrated violation, a plausible candidate needing validation, and optional hardening. Do not infer exploitability or severity from a dangerous API name alone.

## Inspect the boundary, not just the sink

1. Trace untrusted input and authenticated identity through parsing, validation, authorization, canonicalization, storage, caches, background jobs, and output where affected.
2. State the authorization predicate over principal, operation, resource, and tenant. Check it at the authoritative boundary before side effects. Authentication alone does not authorize an object operation.
3. Verify that resource lookup, cache keys, de-duplication, batching, and async work carry the necessary tenant/authority context. Do not infer isolation from a parameter’s name.
4. Examine the exact parser/encoder context and size/work budgets. Parameterization, canonicalization, escaping, and path checks solve different problems. Avoid “sanitize everything” without specifying the destination.
5. Track secrets and privilege lifetimes. Preserve redaction, narrow capabilities, constant-time primitives, safe defaults, audit evidence, and fail-closed behavior when they serve a real contract. Never print credentials to demonstrate a finding.
6. Follow both denial and legitimate-success paths. Double checks may protect against a race or different trust boundary; investigate before deduplicating them. “Internal” is not proof of trusted input.
7. Inspect the smallest counterexample safely and locally when authorized. Record the call path, preconditions, protective controls, impact, and missing evidence. Do not claim a vulnerability from an unexecuted hypothetical exploit.

Read [boundary decisions](references/decisions.md) only for an unresolved tenant/cache, validation, authority, or safeguard-removal question.

## Route specialized work

If the user asks for a formal repository or scoped security audit, use `codex-security:security-scan`; for a pull request/commit/branch/working-tree diff use `codex-security:security-diff-scan`; for explicitly exhaustive repository scans use `codex-security:deep-security-scan`. Follow those skills’ intake and completion rules rather than emulating them here.

For an existing finding use the matching triage/validation workflow; use `codex-security:fix-finding` only for an explicitly requested security fix and `codex-security:verify-fix` only for explicitly requested verification. If unavailable, state that limitation and keep this output a bounded manual boundary review. Never label triage a completed scan.

## Output contract

Return the inspected scope, concise boundary map, prioritized evidence-backed findings/candidates, and the smallest next check or authorized remedy. Include exact locations, reachable path, assumptions, legitimate controls, and uncertainty. For changed code, require exact-state behavioral and negative tests; do not certify security based on a green build or claim the absence of all vulnerabilities.
