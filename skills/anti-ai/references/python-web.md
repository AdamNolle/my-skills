# Python web applications: transactional and extension contracts

Research checked: **2026-10-02 UTC**. Historical human-led example; source and test code were inspected, not executed upstream. Read the target's framework/backend version and local application contracts first. Pair this guide with storage/data only when durable state is involved.

## Review sequence

1. Trace the unit of work from input validation through all writes, exceptions, commit/rollback, post-commit effects, and cache invalidation. Verify externally observable state on partial failure, not only a function's return value.
2. Identify what each context manager, decorator, adapter, or wrapper owns: transaction, authorization, connection state, lifetime, compatibility, or a genuine extension boundary. Do not flatten it until that contract is proven redundant.
3. Distinguish recovering inside a transaction from allowing its manager to observe an exception. Identify the owner of each nested rollback/savepoint and preserve the original error when cleanup also fails. Do not turn failure into success-shaped data unless the API explicitly allows partial results.
4. Inspect multi-tenant/request-local identity and cache keys, invalidation, mutable defaults, and shared state only where applicable. Names that match are not necessarily the same resource across tenants or contexts.
5. Exercise a successful unit, a failure after earlier work, reuse after failure, and existing compatibility entry points. Use the real database/backend for transaction semantics where feasible; mocks alone cannot demonstrate rollback.

## Django 3.1

**Pin:** [`ec5bc3a991bf6f2b9afe9b1b4068e4a1e62001c2`](https://github.com/django/django/commit/ec5bc3a991bf6f2b9afe9b1b4068e4a1e62001c2), **2020-08-04**. Selected for explicit transaction behavior, public extension contracts, tests, and contribution practice in an application framework.

- **Read:** [`django/db/transaction.py`](https://github.com/django/django/blob/ec5bc3a991bf6f2b9afe9b1b4068e4a1e62001c2/django/db/transaction.py), `Atomic.__enter__`, `Atomic.__exit__`, and `mark_for_rollback_on_error`. Savepoint nesting, rollback ownership, exception preservation, and restoration of reusable connection state justify real machinery around a call.
- **Validate:** [`tests/transactions/tests.py`](https://github.com/django/django/blob/ec5bc3a991bf6f2b9afe9b1b4068e4a1e62001c2/tests/transactions/tests.py) covers nested commit/rollback combinations, wrapper reuse, broken transactions, and savepoint leakage. Use these as test-design questions, not a mandate to copy Django's suite.
- **Review and compatibility:** [Unit-test guidance](https://github.com/django/django/blob/ec5bc3a991bf6f2b9afe9b1b4068e4a1e62001c2/docs/internals/contributing/writing-code/unit-tests.txt) and [release/deprecation policy](https://github.com/django/django/blob/ec5bc3a991bf6f2b9afe9b1b4068e4a1e62001c2/docs/internals/release-process.txt).
- **Boundary:** Django's state machine is private and backend-sensitive. Applications should use supported transaction APIs, not reimplement its internals. Thread-local behavior does not establish async-task isolation. The general cache and tenant checks above are transfer questions for application contracts, not a claim that the cited transaction implementation is a caching guide.
- **License/generation:** [BSD 3-Clause](https://github.com/django/django/blob/ec5bc3a991bf6f2b9afe9b1b4068e4a1e62001c2/LICENSE). Inspect migration, template, generated, and vendor ownership before editing; no repository-wide authorship certification is implied.
