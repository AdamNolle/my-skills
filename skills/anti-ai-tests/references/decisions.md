# Test-design decisions

## Find the actual oracle

For each test ask: what externally meaningful observation distinguishes success from the bug? Name that observation before choosing the fixture or mock. Independence matters more than test length or assertion quantity.

- **Pure transformation:** use representative values plus boundaries and algebraic properties supported by the contract; avoid recreating the implementation as the oracle.
- **Persistence:** assert the committed/recovered state with the real backend or a documented fault model. A mock `rollback.assert_called_once()` proves a call, not recovered data.
- **Compatibility:** exercise supported old inputs/clients and public errors where the change crosses a version boundary.
- **Security boundary:** verify deny behavior and absence of side effects for unauthorized principals, plus legitimate access. Coordinate with the dedicated security workflow for a formal assessment.
- **Concurrent state:** identify the decisive interleaving and control it. A thousand unsynchronized passes cannot establish the absence of a race.

## Worked example: the swallowed error

A batch handler catches a write exception and returns the successful item count. The request contract says all items commit or none do. A test that stubs the database and checks the count misses the defect. Insert an earlier item, induce a later write failure, and inspect persistence through an independent read. Preserve the exception/retry contract. A partial-success API would require different expectations, so discover the contract first.

Counterexample: a unit test mocking an HTTP transport to verify URL encoding is appropriate if URL construction is the behavior under test. Do not demand a live network simply because a mock appears.

## Worked example: snapshot with a real purpose

A reviewed golden file can validate a stable wire format or compiler diagnostic. Keep it if readers depend on that output and changes are reviewed semantically. Reject automatic regeneration that accepts every difference without understanding it. Test volatile fields separately or normalize only what the contract declares irrelevant.

## Worked example: a green command proves too little

`pytest -k cancelled` exits with no selected tests in the project’s wrapper. Report “cancellation tests not executed,” inspect collection, then run the intended suite. Do not replace the wrapper’s output with a guessed pass count. A unit suite passing at revision A does not certify a subsequently amended patch B.

## Sensitivity and practical limits

Temporarily perturb the implicated behavior in an isolated copy when safe, or demonstrate the old implementation’s failure. Do not mutate a user’s working tree or production service for proof. A targeted mutation that survives can expose a weak oracle; surviving arbitrary mutants do not mandate an exhaustive mutation campaign.

Property/fuzz tests explore their generators and budget. Race detectors observe instrumented executions. Bounded model checks cover their model. Coverage records executed paths, not correctness. Explicitly state those limits when making a claim.

## Primary precedents

- [SQLite testing](https://www.sqlite.org/testing.html): independent harnesses, fault injection, and delivered-build checks illustrate matching evidence to the failure model, not a requirement to adopt SQLite’s full rigor for every application.
- [LevelDB actual pinned fault-injection source](https://github.com/google/leveldb/blob/78b39d68c15ba020c0d60a3906fb66dbf1697595/db/fault_injection_test.cc): distinguish synced from unsynced state in crash models.
- [Tokio schedule tests, pinned 1.0.1](https://github.com/tokio-rs/tokio/blob/2330edc875ed8b873b6ffc4686feef1534658f79/tokio/src/sync/tests/loom_notify.rs): explicit notification/drop schedules are more informative than sleeps; bounded models are not universal proof.

The parent corpus inspected these sources on 2026-10-02; use current supported APIs when implementing.

## Further primary guidance and integrity checks

Research checked 2026-10-02. [Google testing overview](https://abseil.io/resources/swe-book/html/ch11.html) and [unit testing](https://abseil.io/resources/swe-book/html/ch12.html) support behavior-focused tests, appropriate scope, and explicit flakiness limits; company-scale thresholds are not mandatory. [LLVM test guidance](https://llvm.org/docs/TestingGuide.html) illustrates intentional expected results and directive-driven execution. [pytest exit codes](https://docs.pytest.org/en/stable/reference/exit-codes.html) identify no-tests-collected explicitly; other runners and wrappers require their own interpretation.

Inspect failed pipelines, unawaited async assertions, all-skipped suites, wrong installed packages/binaries, stale build output, and filters that omit the changed package when relevant. Match CI evidence to its recorded PR head or merge revision and build inputs. Trusted cache reuse can be evidence if inputs match; label it cached rather than freshly executed. Include staged, unstaged, and relevant untracked/generated inputs in state identity. A test-list or dry-run command is not an execution.

Use [change-focused mutation](https://testing.googleblog.com/2021/04/mutation-testing.html), [property testing](https://github.com/google/fuzztest/blob/main/doc/fuzz-test-macro.md), or [bounded fuzzing](https://llvm.org/docs/LibFuzzer.html) when they target a real oracle or input-space gap. Check justified properties and independent anchors; do not derive the expected answer by repeating the production implementation. No arbitrary mutation/coverage quota is required.
