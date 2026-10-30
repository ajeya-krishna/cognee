# Feature contribution plan

## Goal

Build useful open-source features through design, implementation, documentation, examples, validation, and maintainer review. Use separate research, implementation, test, and review agents where they improve the work. Judge progress by useful capabilities and accepted contributions.

## Current feature: offline COGX archive inspection

- Code: https://github.com/ajeya-krishna/cognee/tree/feat/cogx-archive-inspection
- Implementation commit: 8a77cf796b5ee99302d6c6c193ea2850026ca5c4
- Current head after comment cleanup: 188f44f20
- Guide: https://github.com/ajeya-krishna/cognee/blob/feat/cogx-archive-inspection/docs/cogx-inspection.md
- Demo: https://github.com/ajeya-krishna/cognee/blob/feat/cogx-archive-inspection/examples/guides/inspect_cogx_archive.py
- Upstream PR against dev: https://github.com/topoteretes/cognee/pull/6452
- Status: PARKED at the user's request on 2026-10-30. The user created PR #6452; it is open and awaits maintainer review. Focus has moved to feature opportunities in other repositories.
- Validation: 459 migration and CLI tests passed (30 warnings), including 34 new inspection/CLI cases. Ruff and applicable pre-commit hooks passed. The runnable demo passed. Full repository suite was not run.
- The separate proposal issue attempt returned HTTP 403. The user subsequently created PR #6452 directly.
- PR creation was completed by the contributor. 34 affected tests were rerun after comment/docstring cleanup and passed. No upstream CI result or maintainer acceptance is implied by local test results.
- The previous Cypher property-name fix PR #6396 remains parked; this feature is a separate branch.

## Working approach

1. Identify a user workflow and inspect existing capabilities and maintainer guidance.
2. Check issue discussions and competing implementations before taking a feature.
3. Define a bounded opt-in capability and explicit compatibility rules.
4. Implement the public surface, docs, examples, and tests.
5. Use independent testing and review; reproduce edge cases rather than guessing.
6. Report exact validation scope and remaining limitations.
7. Seek maintainer feedback and follow through on accepted changes.

For this feature, three sub-agents researched opportunities, implemented the inspection core, wrote independent tests, and reviewed edge cases. The parent integrated the CLI, docs, runnable example, fresh-process checks, review fixes, and publication.

## Original proposal text

## User workflow

Before importing a COGX archive, an operator should be able to inspect its inventory and available integrity checks without writing to a memory store. This is useful when transferring portable memory between instances or checking an archive before a restore.

## Working prototype

[Feature branch](https://github.com/ajeya-krishna/cognee/tree/feat/cogx-archive-inspection) · [Guide](https://github.com/ajeya-krishna/cognee/blob/feat/cogx-archive-inspection/docs/cogx-inspection.md) · [Runnable demo](https://github.com/ajeya-krishna/cognee/blob/feat/cogx-archive-inspection/examples/guides/inspect_cogx_archive.py)

```bash
cognee-cli inspect-archive backup_cogx --json
```

```python
from cognee.migration import inspect_cogx_archive

report = inspect_cogx_archive("backup_cogx")
print(report.valid, report.record_counts, report.checksum_counts)
```

The prototype adds:
- Manifest/version and typed record schema checks.
- Complete comparison of an explicitly declared record inventory.
- Verification of available record checksums.
- Structured diagnostics with filenames and line numbers.
- A human-readable CLI report and JSON output, with nonzero exit status on errors.
- Presence-only detection of permissions.json, without reading account credentials.
- A documented example showing both a valid archive and a changed-record failure.

## Compatibility and scope

The inspector is opt-in and leaves import behavior unchanged. It accepts the reader's legacy manifest/checksum omissions as unverified coverage rather than corruption. It checks unpacked directories, streams record files, initializes no database, and invokes no models. It does not resolve fact references, validate restored permissions, authenticate the sender, or guarantee a database-specific import will succeed.

## Validation

459 migration and CLI unit/regression tests pass, including 34 new inspection and CLI cases, two of which execute the real CLI in fresh processes and check that no stores or archive mutations appear. Ruff and all applicable pre-commit hooks pass. The runnable demo passes. The full repository suite was not run.

The implementation is a new proposal, not a previously approved roadmap item. Related portability context: #3396 and #3400. Searches for COGX inspection/preflight and inspect_cogx_archive did not surface an existing implementation.

Would an opt-in local inspector fit the migration API and CLI? The working branch is available to review the proposed interface and behavior before agreeing on inclusion.

