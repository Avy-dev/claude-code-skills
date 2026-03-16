# Headless Mode Examples

Copy-paste commands for running Claude Code non-interactively (CI, scripts, cron).

## Bug Fix Pipeline
```bash
claude -p "Run bug-finder-refiner on the auth module" --allowedTools "Read,Grep,Glob,Write,Bash"
```

## Structured Bug Fix
```bash
claude -p "/bugfix 'Modal clips on iPhone SE'" --allowedTools "Read,Grep,Glob,Write,Bash,Edit"
```

## Code Review
```bash
claude -p "Review the last 3 commits for bugs and code smells" --allowedTools "Read,Grep,Glob,Bash"
```

## Test Suite
```bash
claude -p "Run pytest and report failures" --allowedTools "Read,Grep,Glob,Bash"
```

## Pre-Push Validation
```bash
claude -p "Run full test suite, lint, and report any issues" --allowedTools "Read,Grep,Glob,Bash"
```

## Notes
- `--allowedTools` controls which tools run without prompting
- Add `--output-format json` for machine-parseable output
- Use `--max-turns N` to limit agent iterations
- Pipe output to files: `claude -p "..." > report.txt 2>&1`
