# AI Agent Guidelines

## Project Overview

This is a **LaTeX document project** (`cheatsheet-systemd`) that produces
a systemd command reference cheatsheet as PDF, JPG, PNG, and SVG. The
primary source is `systemd_cheatsheet.tex`. The build system uses GNU
Make (makefile4latex) and Docker with TeXLive Full.

## Build Commands

```bash
# Full build via Docker (PDF + JPG + PNG + SVG)
./run.sh

# Build only PDF via Docker
./run.sh pdf

# Verbose Docker build
./run.sh --verbose pdf

# Direct Make targets (requires local TeXLive + poppler-utils)
make pdf         # Build PDF
make jpg png svg # Build image formats
make mostlyclean # Clean temporary files, keep outputs
make clean       # Clean everything including outputs
make lint        # Run ChkTeX LaTeX linter
make pretty      # Format LaTeX with latexindent
make help        # Show all available targets
```

## Lint and Validation Commands

```bash
# LaTeX linting
chktex systemd_cheatsheet.tex

# Shell script linting and formatting
shellcheck run.sh
shfmt --case-indent --indent 2 --space-redirects --diff run.sh

# Markdown linting
rumdl ./*.md

# Link checking
lychee --config lychee.toml .

# JSON linting (supports comments)
jsonlint --comments .github/renovate.json5

# GitHub Actions workflow validation
actionlint

# Full CI linting (runs all linters via MegaLinter)
# This runs in CI only; see .mega-linter.yml for configuration
```

There are **no tests** in this project. Validation is done through
linting and successful LaTeX compilation.

## Code Style Guidelines

### General

- **Indentation**: 2 spaces everywhere (no tabs)
- **Line length**: Wrap at 72 characters for Markdown and commit
  messages; no strict limit for LaTeX or shell scripts
- **Final newline**: All files must end with a newline
- **Trailing whitespace**: Trim trailing whitespace

### LaTeX (`.tex`)

- Lint with `chktex` (config in `.chktexrc`)
- Format with `latexindent` via `make pretty`
- Disabled ChkTeX warnings: n1 (command terminated with space),
  n8 (dash length), n12 (interword spacing), n44 (booktabs)
- Keep document structure simple; use standard packages

### Shell Scripts (`.sh`)

- Start with `#!/usr/bin/env bash` and `set -euo pipefail`
- **Variables**: UPPERCASE with braces (`${MY_VARIABLE}`)
- **Formatting**: `shfmt --case-indent --indent 2 --space-redirects`
- **Linting**: `shellcheck` (SC2317 excluded globally)
- Quote all variable expansions: `"${VARIABLE}"`
- Redirect errors to stderr: `>&2`
- Use `command -v` to check command existence
- Use `cat << EOF` heredocs for multi-line output

### Markdown (`.md`)

- Must pass `rumdl` checks (config in `.rumdl.toml`)
- Wrap lines at 72 characters
- Use proper heading hierarchy (no skipped levels)
- Include language identifiers in code fences
- Shell code blocks are extracted and validated with `shellcheck`
  and `shfmt` during CI
- `CHANGELOG.md` is auto-generated; do not edit manually

### JSON / YAML

- JSON: must pass `jsonlint --comments`
- YAML: use `---` document separator at top of files
- Both: 2-space indentation

### GitHub Actions Workflows

- Validate with `actionlint` after any modification
- **Pin all actions to full SHA commits**, never tags
- Set `permissions: read-all` as default; override minimally
- Always set `timeout-minutes` on jobs (5 or 10)
- All workflows must support `workflow_dispatch`
- Add a descriptive comment at the top of each workflow file

## Security Scanning

CI runs these security scanners:

- **Checkov**: IaC scanner (skip `CKV_GHA_7`)
- **DevSkim**: Pattern scanner (ignore DS162092, DS137138)
- **KICS**: Fails only on HIGH severity
- **Trivy**: HIGH/CRITICAL only, ignores unfixed vulnerabilities
- **CodeQL**: Runs on pushes to main and weekly
- **OSSF Scorecard**: Runs on pushes to main and weekly

## Version Control

### Commit Messages

Use **conventional commit** format with these rules:

- Types: `feat`, `fix`, `docs`, `chore`, `refactor`, `test`,
  `style`, `perf`, `ci`, `build`, `revert`
- Format: `<type>: <description>` (imperative mood, lowercase)
- Subject line: maximum 72 characters, no trailing period
- Body: wrap at 72 characters, explain what and why
- Reference issues: `Fixes #123`, `Closes #123`, `Resolves #123`

```text
feat: add automated dependency updates

- Implement Dependabot configuration
- Configure weekly security updates

Resolves: #123
```

### Branching

Follow [Conventional Branch](https://conventional-branch.github.io/)
format: `<type>/<description>`

- `feature/` or `feat/`: new features
- `bugfix/` or `fix/`: bug fixes
- `hotfix/`: urgent fixes
- `release/`: release prep (e.g., `release/v1.2.0`)
- `chore/`: non-code tasks

Use lowercase, hyphens, no consecutive/leading/trailing hyphens.
Include issue numbers when applicable: `feature/issue-123-description`

### Pull Requests

- Always create as **draft** initially
- Title must follow conventional commit format
- Include clear description of changes and motivation
- Link related issues with `Fixes`, `Closes`, or `Resolves`
