# COGX inspection validation

Feature commit: `8a77cf796b5ee99302d6c6c193ea2850026ca5c4`

The following is actual recorded command output from the feature checkout. It is text evidence, not a terminal screenshot. The full repository test suite was not run.

## Migration and CLI regression tests

Command:

```sh
LOG_LEVEL=ERROR TELEMETRY_DISABLED=1 .venv/bin/python -m pytest cognee/tests/unit/migration/ cognee/tests/cli_tests/cli_unit_tests/ -q --disable-warnings --tb=short --show-capture=no
```

```text
........................................................................ [ 15%]
........................................................................ [ 31%]
........................................................................ [ 47%]
........................................................................ [ 62%]
........................................................................ [ 78%]
........................................................................ [ 94%]
...........................                                              [100%]
459 passed, 30 warnings in 53.92s
```

## Repository pre-commit hooks

Run against the changed files:

```text
Trim Trailing Whitespace.................................................Passed
Fix End of Files.........................................................Passed
Check Yaml...........................................(no files to check)Skipped
Check for added large files..............................................Passed
ruff (legacy alias)......................................................Passed
ruff format..............................................................Passed
```

## Offline runnable example

Command:

```sh
LOG_LEVEL=ERROR TELEMETRY_DISABLED=1 .venv/bin/python examples/guides/inspect_cogx_archive.py
```

The example creates a temporary archive and deliberately alters its own fixture to demonstrate the checksum failure:

```text
Original archive:
{
  "directory": "/tmp/tmp83k0yoaz/company_memory",
  "cogx_version": "0.2",
  "source_system": "cognee",
  "record_counts": {
    "entity": 2,
    "fact": 1
  },
  "declared_counts": {
    "entity": 2,
    "fact": 1
  },
  "checksum_counts": {
    "valid": 3,
    "invalid": 0,
    "missing": 0
  },
  "contains_permissions": false,
  "diagnostics": [],
  "valid": true,
  "total_records": 3
}

After the demo deliberately alters a record:
{
  "directory": "/tmp/tmp83k0yoaz/company_memory",
  "cogx_version": "0.2",
  "source_system": "cognee",
  "record_counts": {
    "entity": 2,
    "fact": 1
  },
  "declared_counts": {
    "entity": 2,
    "fact": 1
  },
  "checksum_counts": {
    "valid": 2,
    "invalid": 1,
    "missing": 0
  },
  "contains_permissions": false,
  "diagnostics": [
    {
      "file": "facts.jsonl",
      "line": 1,
      "code": "checksum_mismatch",
      "severity": "error",
      "message": "Record checksum does not match its contents."
    }
  ],
  "valid": false,
  "total_records": 3
}
```
