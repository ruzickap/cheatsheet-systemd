# AGENTS.md

Single-document LaTeX project: `systemd_cheatsheet.tex` compiles to a PDF
cheatsheet for `systemd`. There is no application code.

General contribution standards (commit messages, branching, linting, security
scanning) live in the user-global `~/.config/opencode/AGENTS.md`, which is
already loaded. This file only records repo-specific facts.

## Build

- Build via Docker, not a local TeX install: `./run.sh` runs `make` inside
  `ghcr.io/xu-cheng/texlive-full`. Default targets: `jpg pdf png svg mostlyclean`.
- `./run.sh pdf` builds only the PDF; `./run.sh -h` shows options. Override the
  image/targets with `LATEX_DOCKER_IMAGE` / `LATEX_MAKE_TARGETS`.
- The image is amd64-only (no arm64); on Apple Silicon Docker runs it emulated.
- Bare `make` / `make lint` / `make pretty` only work if a full TeX toolchain
  (`pdflatex`, `chktex`, `latexindent`) is on `PATH` — otherwise use Docker or
  the devcontainer.

## Do not edit (vendored upstream)

- `Makefile` (makefile4latex by Takahiro Ueda) and `.gitignore` are vendored and
  carry `latest-raw-url` / `make upgrade` tags. Do not hand-edit them; they are
  meant to be regenerated from upstream.

## Editing the cheatsheet

- All content is in `systemd_cheatsheet.tex` (`tabularx` tables aligned with
  spaces). Run `make lint` (chktex) and `make pretty` (latexindent) through
  Docker/devcontainer before committing.
- `.chktexrc` disables warnings 1, 8, 12, 44 — keep using `\tt`/`\newline` style
  already in the file rather than "fixing" what chktex is told to ignore.

## CI gotchas

- CI quality is one job: **MegaLinter** (`.mega-linter.yml`), not the individual
  tools run directly. It runs on every push except `main`.
- Markdown is checked with `rumdl` + `lychee` (markdownlint/markdown-link-check
  are disabled). Bash/shell/sh code blocks inside Markdown are extracted to
  `bash_commands_from_md.sh` and shellcheck'd — keep fenced shell snippets valid.
- `latex-build` workflow only runs on `**.tex` changes and never on `main`.
- Releases are automated by release-please (`release-type: simple`) on `main`;
  it builds the PDF and attaches it to the GitHub release. Bump versions via
  conventional commits, not manual edits.
