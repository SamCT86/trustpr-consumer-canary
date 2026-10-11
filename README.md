# TrustPR public consumer canary

This repository is a fixture-only public consumer of
`SamCT86/trustpr-action@3ac08375bed6380285fcfd8019cef0cb7931dda8`.
It exercises the published Action on real `pull_request` events. It is not the
TrustPR product repository and contains no application, build, or npm scripts.

The workflow has only `actions: read`, `checks: read`, `contents: read`, and
`pull-requests: read`. It uses no repository secrets and never checks out or
executes PR code. It checks out only the immutable public Action source to import
that version's receipt verifier. It reads one bounded public GitHub PR-files
metadata response as an independent changed-path oracle.

Two real PR cases are expected:

| Case | PR author and original workflow actor | Exact changed paths |
| --- | --- | --- |
| Same repository | `SamCT86` | `README.md` |
| Dependabot | `dependabot[bot]` | `package.json`, `package-lock.json` |

For the first case, create a normal branch in this repository and append a short
canary note to this README only. For the second case, GitHub Dependabot opens a
real npm version-update PR from the fixture's exact `picocolors` version `1.0.0`.
The dependency is fixture data only; it is not a TrustPR product dependency and
is never installed or executed by this workflow. The initial lockfile must be
generated with `npm install --package-lock-only --ignore-scripts` before
publication. No `node_modules` directory belongs in this repository.

The Dependabot case tests a real Dependabot event and its restricted token
context. Its head and base repository identities are recorded. It is not a fork
PR and must not be reported as fork evidence. Manual workflow dispatches and
synthetic event payloads are not canary evidence.

Successful verification requires the exact event subject, a verified core
digest, `PARTIAL` receipt status, a complete Git diff with the expected paths,
matching public PR-files metadata and new blob identities, and a byte-identical
rendered summary. Receipts remain `UNAUTHENTICATED` and `POLICY_UNBOUND`, with
`merge_authorized: false`. `TEST_EXECUTION_NOT_ESTABLISHED` remains explicit.
An internally consistent receipt does not establish external authenticity or
authorize a merge.

The caller workflow uploads the receipt, summary, event identities, public files
metadata, and verification result for seven days. This uploader is a consumer
evidence step, not part of the TrustPR product. Actual run links, artifact bytes,
and provider readback are required before claiming either case passed.

Same-repository canary: one README change exercises the immutable public Action.
