# Networking and concurrency: bounded work and lifecycle contracts

Research checked: **2026-10-02 UTC**. These are historical, human-led source snapshots, not recommended versions to install. Dates are commit dates unless stated otherwise. Upstream source and test mechanisms were inspected; upstream suites were not executed. Apply the target's current supported version, local policy, and failure model first.

Read only the relevant project below. Turn its lesson into a local invariant and executable counterexample; do not copy its architecture or treat historical code as defect-free.

## curl: bounded interfaces and protocol edge cases

**Pin:** curl-7_74_0, 2020-12-09, [`e052859759b34d0e05ce0f17244873e5cd7b457b`](https://github.com/curl/curl/commit/e052859759b34d0e05ce0f17244873e5cd7b457b), attributed to Daniel Stenberg.

**Read:** [`Curl_dyn_init`, `dyn_nappend`, and `Curl_dyn_free`](https://github.com/curl/curl/blob/e052859759b34d0e05ce0f17244873e5cd7b457b/lib/dynbuf.c). Buffer length, capacity, size policy, failure cleanup, and reuse are handled by one small abstraction. [unit1653.c](https://github.com/curl/curl/blob/e052859759b34d0e05ce0f17244873e5cd7b457b/tests/unit/unit1653.c) separately demonstrates URL parser tests for valid and malformed IPv6, zone IDs, and port syntax. Current [contribution guidance](https://curl.se/dev/contribute.html) asks for bounded patches, matching style, tests or an explicit verification account.

**Apply:** Ask who owns a buffer, what input-size limit applies, how failure changes its state, and whether malformed/empty/boundary inputs are tested. Keep policy and cleanup consistent instead of cloning ad hoc growth logic. Review arithmetic and caller assumptions independently.

**Limits:** This historical helper is not a proven safe drop-in implementation. Its presence in curl does not prove every arithmetic operation correct for arbitrary callers. Protocol compatibility branches can be necessary; do not remove them because they look defensive. The cited parser tests are evidence of edge-case testing, not direct tests of `dyn_nappend`.

## OpenBSD/OpenSSH: reducing authority before parsing

**Pin:** OpenBSD source mirror snapshot 2020-10-18, [`48e6b99de5b6c21f9b0b7846d7b5c7e4959b6e13`](https://github.com/openbsd/src/commit/48e6b99de5b6c21f9b0b7846d7b5c7e4959b6e13). Commit attribution is `djm`; its message records review by `markus@`. This is a dated source snapshot, not the exact OpenBSD 6.8 release tree.

**Read:** [`privsep_preauth_child` and `privsep_preauth` in sshd.c](https://github.com/openbsd/src/blob/48e6b99de5b6c21f9b0b7846d7b5c7e4959b6e13/usr.bin/ssh/sshd.c). Sensitive keys are demoted and the network-facing child is restricted before it processes unauthenticated traffic. [connect-privsep.sh](https://github.com/openbsd/src/blob/48e6b99de5b6c21f9b0b7846d7b5c7e4959b6e13/regress/usr.bin/ssh/connect-privsep.sh) tests privilege-separation/sandbox connections across allocator settings because libc changes can affect the sandbox. [style(9)](https://man.openbsd.org/style.9) is supplementary project-local style guidance.

**Apply:** Draw the trust boundary: what inputs are untrusted, what secrets and OS privileges are reachable, and when authority can be reduced. Preserve restrictions and explicit failure handling during “simplification.” Test system-library assumptions at the boundary.

**Limits:** Portability and OS-specific security primitives matter. Do not paste an old daemon's sandbox, chroot, cryptographic, or authentication code into another app. Do not confuse a short implementation with a small attack surface or style conformity with security.

## Go: cancellation as an explicit resource contract

**Pin:** go1.15.6, 2020-12-03, [`9b955d2d3fcff6a5bc8bce7bafdc4c634a28e95b`](https://github.com/golang/go/commit/9b955d2d3fcff6a5bc8bce7bafdc4c634a28e95b), attributed to Carlos Amedee. Metadata links a Gerrit review and names reviewer Dmitri Shuralyov; it also contains automated test-result attribution.

**Read:** [`context.go`, especially `cancelCtx.cancel` and `WithDeadline`](https://github.com/golang/go/blob/9b955d2d3fcff6a5bc8bce7bafdc4c634a28e95b/src/context/context.go#L394), plus the package contract at the top of that file. Cancellation releases parent/child references and timers; it is not just a boolean. [context_test.go](https://github.com/golang/go/blob/9b955d2d3fcff6a5bc8bce7bafdc4c634a28e95b/src/context/context_test.go) covers simultaneous/interlocked cancellations, canceled parents, and removal from parents.

**Apply:** For every spawned task or deadline, identify who cancels it and who waits for completion. Trace cancellation through the call graph and ensure every exit releases its resources. Prefer explicit request context over hidden mutable service state.

**Limits:** `context.Value` is not an untyped dependency-injection container. Cancellation is cooperative, and notification that work should stop does not prove the worker has stopped. Use current APIs when implementing; the historical sample is a reasoning model.

## Tokio: explicit notification semantics and schedule exploration

**Pin:** tokio-1.0.1, 2020-12-25, [`2330edc875ed8b873b6ffc4686feef1534658f79`](https://github.com/tokio-rs/tokio/commit/2330edc875ed8b873b6ffc4686feef1534658f79), attributed to Alice Ryhl; the release-preparation commit references PR #3347.

**Read:** [`Notify`, `Notified`, and state constants in notify.rs](https://github.com/tokio-rs/tokio/blob/2330edc875ed8b873b6ffc4686feef1534658f79/tokio/src/sync/notify.rs). The public contract distinguishes one stored permit from a counted event stream; state, waiters, and pinning requirements are explicit. [loom_notify.rs](https://github.com/tokio-rs/tokio/blob/2330edc875ed8b873b6ffc4686feef1534658f79/tokio/src/sync/tests/loom_notify.rs) explores notification, multiple waiters, and dropping a waiting future through Loom models.

**Apply:** Ask whether events accumulate or coalesce, which operation registers interest, and what happens if a waiter is canceled. Test interleavings and dropped futures instead of depending on sleeps to happen in a favorable order. Keep unsafe synchronization invariants near the implementation.

**Limits:** Tokio was already established by this baseline, but its 1.0 release was new, so do not equate maturity of the project with a long-stable 1.x API at that time. Loom explores the chosen bounded model, not every program behavior. Do not hand-roll unsafe synchronization when an existing safe primitive fits.
