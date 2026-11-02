# Snowflake Devs — RepoPilot

**RepoPilot** is a lightweight, dependency-free repository health scanner for the Snowflake Devs engineering workspace. It gives you a fast local check before committing code or asking an AI coding assistant to review a change.

## What it does

- Scans text files for common patterns that may indicate committed credentials.
- Reports large files and missing repository essentials.
- Runs PHP syntax checks when the PHP CLI is installed.
- Produces readable Markdown and machine-readable JSON reports.
- Keeps the scan local: no API calls, uploads, telemetry, or third-party packages.
- Avoids printing matched credential values in reports.

## Requirements

- Python 3.9 or newer
- Optional: PHP CLI for PHP syntax validation

## Run it

From the repository root:

```bash
python tools/repopilot/repopilot.py
```

Scan a different directory:

```bash
python tools/repopilot/repopilot.py /path/to/project
```

Choose a report directory:

```bash
python tools/repopilot/repopilot.py . --out ./audit-output
```

By default, reports are written to `.repopilot-reports/`:

- `repopilot-report.md`
- `repopilot-report.json`

The report directory is excluded by the root `.gitignore`.

## Exit codes

| Code | Meaning |
|---:|---|
| `0` | No high-severity findings from enabled checks |
| `1` | One or more high-severity findings |
| `2` | Invalid arguments or a scan/report error |

A high-severity finding should be reviewed before committing. A pattern match is not proof that a credential is live; verify it safely, remove exposed values, and rotate any credential that may have been exposed.

## Run tests

```bash
python -m unittest discover -s tests -v
python -m py_compile tools/repopilot/repopilot.py
```

GitHub Actions runs these checks on pushes and pull requests.

## AI-assisted engineering workflow

1. Run RepoPilot and save its report.
2. Give an AI assistant the task brief in [`docs/AI_ENGINEERING_TASKS.md`](docs/AI_ENGINEERING_TASKS.md), plus only the relevant source files.
3. Ask two assistants to solve the same task independently, using separate branches or worktrees.
4. Run the test suite against both implementations.
5. Compare them with [`docs/AI_RESPONSE_SCORECARD.md`](docs/AI_RESPONSE_SCORECARD.md).
6. Keep only changes that pass tests and human review.

Never provide credentials, private keys, customer records, production data, or confidential client code to an AI assistant. Check the terms and data-handling rules of any platform before submitting project material.

## Safety and limitations

RepoPilot is a small heuristic helper, not a full security product. It does not query vulnerability databases, understand every language, inspect dependency lockfiles for known CVEs, or prove that a repository is safe. Pattern matching can produce false positives and false negatives. Use dedicated secret scanning, dependency auditing, code review, and security testing for production systems.
