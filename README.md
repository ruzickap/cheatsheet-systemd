# Systemd Cheatsheet

[![Build Status](https://github.com/ruzickap/cheatsheet-systemd/actions/workflows/latex-build.yml/badge.svg)](https://github.com/ruzickap/cheatsheet-systemd/actions/workflows/latex-build.yml)
[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/ruzickap/cheatsheet-systemd)

This repository hosts a comprehensive cheatsheet for `systemd`, the init system
and service manager for Linux operating systems. The cheatsheet is generated
from the `systemd_cheatsheet.tex` LaTeX file and serves as a quick reference
guide, ideally used in its PDF form.

Download the PDF (and PNG, JPG, SVG) from the
[latest release](https://github.com/ruzickap/cheatsheet-systemd/releases/latest).

It provides a valuable resource for users transitioning to `systemd` or those
needing a quick lookup for various commands.

Key areas covered in the cheatsheet include:

* A comparison of `systemd` commands with their traditional SysVinit counterparts
* Mappings of system runlevels to `systemd` targets
* A collection of useful commands for managing systemd services, units, and targets
* Information on viewing and managing logs using `journalctl`
* Equivalent `systemd` commands for common power management operations

## Build

Requires Docker; builds the files in the current directory:

```bash
./run.sh     # pdf, png, jpg, svg
./run.sh pdf # PDF only
```
