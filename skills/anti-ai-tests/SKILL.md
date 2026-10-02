---
name: anti-ai-tests
description: Check behavior, regression coverage, and verification integrity for code reviews, bug fixes, and implementations. Use for correctness audits, missing edge or failure tests, tests that pass without proving behavior, and claims of completed validation. Prefer observable contracts and exact-state evidence over coverage quotas or assertion counts.
---

# Correctness and honest verification

Prove the claimed behavior with the smallest evidence that can distinguish correct code from the relevant defect.

## Scope and input

- Identify the requested behavior, affected code, callers, available tests, supported toolchain, and repository test instructions. Record the revision, dirty diff, and existing failures before editing.
- Default to audit. Add or change tests/code only within an explicit implementation, fix, or test-improvement request. Never weaken assertions or update snapshots merely to make a suite green.
- Read [test-design decisions](references/decisions.md) for ambiguous test level, mutation sensitivity, failure injection, or verification claims.

## Choose tests from the contract

1. State the behavior independently of its current implementation. Inspect side effects, durable state, errors, ownership, ordering, and authorization only where affected.
2. Select representative success and counterexamples: boundary values, invalid input, empty/absent states, partial failures, retries, cancellation, reentrancy, or concurrent schedules. Do not apply every category to unrelated code.
3. Choose the narrowest test layer that includes the failure mechanism. Use real transaction/storage/parser/runtime semantics where mocks would erase the risk. Fake unstable external dependencies at their actual boundary.
4. Check that each important assertion can fail when the relevant behavior is wrong. A test that exercises a mock of the target, catches every exception, only checks truthiness, or asserts a stubbed constant may be vacuous. Mocks, snapshots, and integration tests can still be legitimate when their oracle matches the contract.
5. Verify invariants across an operation’s whole outcome, including cleanup and unchanged state after failure. A correct return value alone cannot prove rollback or resource release.
6. For asynchronous behavior, prefer synchronization points, controllable clocks, and explicit schedules over lucky sleeps. Bound tests, clean up workers, propagate background exceptions, and make failures diagnosable.

## Run and record evidence

- Establish the baseline when practical. Run the targeted test and verify it was discovered and executed. An exit code of zero with zero tests, skipped paths, stale output, or an unrelated suite does not validate the claim.
- For a regression, demonstrate failure against original relevant code and success against the change when feasible. Use an isolated temporary copy/worktree; do not reset or overwrite user changes. If red/green proof is unavailable, say why.
- Run broader tests, build/type checks, lint/format checks, sanitizers, or property/fuzz tests in proportion to the changed risk and project support. State what each check covers. Compilation is not behavioral validation.
- Record exact commands, relevant toolchain, exit/results, collected/executed/skipped counts when available, and the tested HEAD plus diff identity. Hash a patch and any relevant untracked source/config when needed to identify the tested state.
- Recheck state after testing. If code/config or dependencies changed, rerun affected checks. Do not claim a different HEAD or uncommitted tree was tested.
- Preserve failures and classify them with evidence as introduced, pre-existing, flaky, or environmental. “Cannot reproduce” is not “fixed”; a missing dependency is not “passed.”

## Output contract

Report observed findings or the verified behavior, the decisive tests, exact-state results, and remaining gaps. Label planned, not run, failed, and passed distinctly. Give bounded assurance about inspected paths, never a claim that all bugs are gone. Keep raw logs out of the main answer unless useful; include a concise evidence artifact for substantial work.
