\# CS4928 Lab 2 Evidence



\## Baseline



\- Pinned SHA: `cf437186b2ae2d9100a187e0df3cce3a163e9b4b`

\- Upstream `mvn test`: PASS

\- `CS4928Lab2CheckTest`: PASS

\- `CS4928\_BASELINE` tag: created



\## Problem A



Problem A: validateName mixes general validation with package-type-specific normalisation. Goal: move the varying name-normalisation algorithm behind a stable policy while preserving every existing result.



The intended structure was to introduce a stable `PackageNamePolicy` interface and a `PackageNamePolicies` resolver containing reusable PRESERVE, LOWERCASE, PUB and PYPI policies.



\## Refactoring A



\- Added `PackageNamePolicy.java`.

\- Added `PackageNamePolicies.java`.

\- Changed `PackageURL.validateName()` to retain the general empty-name validation and delegate name normalisation to the appropriate policy.

\- Full `mvn test`: PASS

\- `CS4928Lab2CheckTest`: PASS

\- `git diff --check`: PASS

\- Refactoring A commit: `3050128c73787f0f18eff7ccf4b2a84e2adb3cd8`



Before → after: Package-type-specific name normalisation was moved out of `validateName()` into reusable policies, while the existing name-normalisation behaviour was preserved.



\## Problem B



Qualifier key canonicalisation was duplicated because both `parseQualifiers` paths converted qualifier keys to lowercase. However, the two parsing methods have different responsibilities and should remain separate: the Map path retains its filtering/overwrite behaviour, while the encoded-string path retains its duplicate-key detection.



\## Refactoring B



\- Added the private `canonicalizeQualifierKey(String key)` helper.

\- Both `parseQualifiers` methods now use the helper for qualifier-key lowercasing.

\- The two parsing algorithms remain separate.

\- Full `mvn test`: PASS

\- `CS4928Lab2CheckTest`: PASS

\- `git diff --check`: PASS

\- Refactoring B commit: `790178b`



Before → after: Qualifier-key lowercasing now has a single source of truth, while the Map and encoded-string parsing behaviours deliberately remain separate.



\## Problem C



The wider package-type variation in `validateNamespace(...)`, `validateVersion(...)` and `verifyTypeConstraints(...)` is deliberately left for later so that this refactoring remains small, bounded, testable and easy to understand.



\## Final Judgement



I would keep both changes in a real pull request. A benefit is that the varying normalisation rules are now more localised and duplicated qualifier-key canonicalisation has been removed. A trade-off is that Refactoring A introduces additional files and abstractions for behaviour that was previously contained in one method.



Passing tests are necessary because they provide evidence that the existing behaviour has been preserved, but passing tests alone do not prove that the new design is clearer, simpler or more maintainable.



\## Repository



\- GitHub repository URL: `https://github.com/DanielKirwan26/cs4928-lab2`

\- Refactoring B commit SHA: `790178b`



The final HEAD SHA will be shown to the coordinator using `git rev-parse HEAD` after the evidence commit.

