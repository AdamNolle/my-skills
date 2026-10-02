# Boundary decisions

## Worked example: tenant-scoped cache

A route authorizes access to `(tenant, document)` but a shared cache keys only by `document_id`. If identifiers can overlap or authority differs between tenants, a cached result may cross the boundary. Trace identifier guarantees, cache lifetime, and actual lookup order before declaring a defect. The remedy must preserve identity through every cache/read path and test allowed same-tenant access plus cross-tenant denial or separation.

Counterexample: an immutable public content-addressed blob keyed by a cryptographic digest may be intentionally global. A missing tenant field is not automatically a bug. Verify that access control is applied to references and that the blob itself is authorized for this sharing model.

## Worked example: removing a “duplicate” check

A controller validates a path for usability; a storage boundary validates the canonical resolved target against the permitted root. Removing the second check because both reject `..` can erase defense against alternate callers, encodings, or symlinks. Trace each boundary and the filesystem race model. A string prefix check is not necessarily an adequate containment mechanism, and a parser check is not authorization.

Counterexample: two identical checks in one immutable, linear scope may be redundant when no mutation, callback, alternate caller, or changed trust boundary intervenes. Document that evidence; do not preserve duplication as a ritual.

## Worked example: shorter failure handling

Replacing an authorization service error with “allow” improves apparent availability while changing the security policy. Fail-open may be explicitly designed for a low-risk optional feature; do not assume it for privileged operations. Identify the consequence and ask for a policy choice if the requirement is unavailable.

## Triage rubric

- **Demonstrated defect:** a reachable, in-scope counterexample violates a stated boundary after existing controls are considered.
- **Candidate:** a plausible path exists, but a deployment assumption, invariant, caller, or mitigation remains unresolved. State the missing evidence.
- **Hardening:** no demonstrated violation; narrower authority or better isolation could reduce future risk. Keep separate from defect severity.
- **Keep intact:** the protection has a current threat model, compatibility requirement, or safe failure purpose; superficial verbosity does not justify deletion.

## Precedent and limits

[OpenSSH pre-authentication privilege separation, pinned OpenBSD snapshot](https://github.com/openbsd/src/blob/48e6b99de5b6c21f9b0b7846d7b5c7e4959b6e13/usr.bin/ssh/sshd.c) illustrates reducing authority before processing unauthenticated input. Inspect `privsep_preauth_child` and `privsep_preauth`; the historical daemon and OS mechanisms are not a portable implementation recipe.

The parent anti-ai corpus inspected this source on 2026-10-02. Consult the project’s current security policy, official supported APIs, and the dedicated security workflow for decisions beyond bounded review. Do not copy historical cryptographic or authentication implementation.

## Further primary guidance

Research checked 2026-10-02. [Microsoft SDL](https://www.microsoft.com/en-us/securityengineering/sdl/practices) combines design review, threat modeling, and testing; completing a checklist is not an audit result. [NIST SSDF v1.1](https://csrc.nist.gov/pubs/sp/800/218/final) supplies lifecycle practices, not a guarantee or claim about the latest edition. [OWASP input validation](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html), [authorization](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html), and [secrets management](https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet.html) help distinguish exact enforcement duties and safe evidence handling. Apply only the mechanisms relevant to the target’s intended public/private policy.

Review new dependency/build-time execution and provenance where a change introduces it. A pin does not prove a dependency secure; a scanner match requires exposure triage. Run repository/package hooks only in an authorized environment and treat source/test output as untrusted data. Credential changes, incident response, live exploitation, production modification, and broad scans remain separate authorized tasks.
