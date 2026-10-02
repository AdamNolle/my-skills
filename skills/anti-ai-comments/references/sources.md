# Primary comment guidance

Research checked 2026-10-02. These sources express project conventions or demonstrate particular uses; none proves universal comment rules. Read only the relevant source and respect the target repository and supported toolchain. Examples below are summarized, not copied implementation.

- [Linux coding style, commenting](https://www.kernel.org/doc/html/latest/process/coding-style.html#commenting): function behavior and data explanations can be useful; avoid obvious micro-level narration. Kernel conventions are not blanket application rules.
- [kernel-doc](https://docs.kernel.org/doc-guide/kernel-doc.html): document calling/execution context and lock obligations; validate documentation extraction separately from compilation.
- [LLVM commenting](https://llvm.org/docs/CodingStandards.html#commenting): usable public contracts, useful examples/rationale, and parameter annotations; preserve license headers and distinguish purposeful examples from abandoned code.
- [Go doc comments](https://go.dev/doc/comment): caller-visible behavior, zero values, special cases, and concurrency guarantees belong in public docs. Symbol-name summaries are a local convention, not automatically noise.
- [PEP 8 comments](https://peps.python.org/pep-0008/#comments) and [PEP 257](https://peps.python.org/pep-0257/): accuracy and relevant call contracts matter; docstrings are runtime attributes.
- [Rust API documentation](https://rust-lang.github.io/api-guidelines/documentation.html) and [standard-library safety comments](https://std-dev-guide.rust-lang.org/policy/safety-comments.html): examples, errors, panics, caller obligations, and local safety proofs have distinct purposes.
- [Swift API Design Guidelines](https://www.swift.org/documentation/api-design-guidelines/): assess clarity at use sites and use documentation to reveal design ambiguity; legitimate summaries can describe what is returned.
- [Google C++ comments](https://google.github.io/styleguide/cppguide.html#Comments): distinguish declaration contracts from implementation rationale; connect TODOs to a real issue/context and completion condition.
- [Fluent localization comments](https://firefox-source-docs.mozilla.org/l10n/fluent/review.html#comments): translator context and grouping have consumers different from code readers.
- [WAI-ARIA keyboard interface](https://www.w3.org/WAI/ARIA/apg/practices/keyboard-interface/#focusabilityofdisabledcontrols): seemingly unusual focus behavior may carry an accessibility obligation; inspect the component context.
- [LLVM FileCheck](https://llvm.org/docs/CommandGuide/FileCheck.html#tutorial) and [TypeScript expected errors](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-9.html#ts-expect-error-comments): some comments are test instructions, not inert prose.

Empirical caution: [Rani et al. systematic review](https://scg.unibe.ch/archive/papers/Rani22c.pdf) describes multiple quality dimensions and substantial context/evaluation limits. [Jabrayilzade et al. comment-smell study](https://link.springer.com/article/10.1007/s10664-023-10425-5) reports contextual exceptions to smell classifications. Neither supports a universal deletion score or authorship inference.
