# Runtimes and tooling: reentrancy and contract-bearing helpers

Research checked: **2026-10-02 UTC**. These are historical, human-led source snapshots, not recommended versions to install. Dates are commit dates unless stated otherwise. Upstream source and test mechanisms were inspected; upstream suites were not executed. Apply the target's current supported version, local policy, and failure model first.

Read only the relevant project below. Turn its lesson into a local invariant and executable counterexample; do not copy its architecture or treat historical code as defect-free.

## CPython: reentrancy, ownership, and measured optimization

**Pin:** v3.9.1, 2020-12-07, [`1e5d33e9b9b8631b36f061103a30208b206fd03a`](https://github.com/python/cpython/commit/1e5d33e9b9b8631b36f061103a30208b206fd03a), attributed to Łukasz Langa.

**Read:** [`list_ass_slice` in Objects/listobject.c](https://github.com/python/cpython/blob/1e5d33e9b9b8631b36f061103a30208b206fd03a/Objects/listobject.c#L598). It restores canonical state before reference decrements can run callbacks; that ordering explains a temporary recycle array. [listsort.txt](https://github.com/python/cpython/blob/1e5d33e9b9b8631b36f061103a30208b206fd03a/Objects/listsort.txt) records memory/comparison/movement tradeoffs across input shapes. [test_sort.py](https://github.com/python/cpython/blob/1e5d33e9b9b8631b36f061103a30208b206fd03a/Lib/test/test_sort.py) includes stability, exception, and mutation regressions.

**Apply:** Treat callbacks, destructors, comparison operators, and finalizers as potential reentry points. Ask what observers see during mutation and whether cleanup can execute user code. Demand representative workload evidence before replacing a specialized algorithm with a supposedly simpler one.

**Limits:** Reference counting and this interpreter's historical concurrency assumptions are domain-specific. Do not infer current free-threading safety from a 3.9 example. `listobject.c` visibly includes Argument Clinic generated regions; preserve the generator workflow instead of calling the complete file manually authored.

## Git: small contract-bearing helpers and reviewable patches

**Pin:** v2.29.2, 2020-10-29, [`898f80736c75878acc02dc55672317fcc0e0a5a6`](https://github.com/git/git/commit/898f80736c75878acc02dc55672317fcc0e0a5a6), attributed to Junio C Hamano.

**Read:** [strbuf.h](https://github.com/git/git/blob/898f80736c75878acc02dc55672317fcc0e0a5a6/strbuf.h) documents termination, embedded NULs, valid length/capacity, and ownership; [`strbuf_detach`/`strbuf_grow`](https://github.com/git/git/blob/898f80736c75878acc02dc55672317fcc0e0a5a6/strbuf.c#L69) implement transfer and growth. [CodingGuidelines](https://github.com/git/git/blob/898f80736c75878acc02dc55672317fcc0e0a5a6/Documentation/CodingGuidelines) favors local consistency and rejects style-only churn; [t/README](https://github.com/git/git/blob/898f80736c75878acc02dc55672317fcc0e0a5a6/t/README) describes regression tests and test organization.

**Apply:** Make invariants and ownership transitions explicit at small API boundaries. Ask whether a patch is reviewable as one behavior change and whether the test fails for the old behavior. Separate enabling cleanup from behavior changes when useful; preserve surrounding style.

**Limits:** Git's process-terminating allocation helpers are unsuitable as a universal library error policy. Public helpers are justified by a real shared invariant, not merely because two expressions look alike. A project's portability conventions are local constraints, not global rules.
