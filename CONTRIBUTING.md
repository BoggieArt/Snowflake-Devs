# Contributing

Thanks for improving RepoPilot and the Snowflake Devs engineering workspace.

## Project principles

1. Keep the core dependency-free and compatible with Python 3.9+.
2. Keep scanning read-only: never change files in the target repository.
3. Never include matched credential values or source snippets in a report.
4. Bound file reads and skip binary, generated, and dependency directories.
5. Give each finding a severity, an understandable explanation, and a practical next step.
6. Add regression tests for every new scanner rule, including false-positive cases.
7. Be explicit about limitations. A clean report is not a security certification.

## Before opening a pull request

```bash
python -m unittest discover -s tests -v
python -m py_compile tools/repopilot/repopilot.py
python tools/repopilot/repopilot.py .
```

Review the generated report, inspect the diff, and do not commit local scan output or sensitive data.
