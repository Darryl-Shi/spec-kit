# Repository Guidelines

## Project Structure & Module Organization
- `src/specify_cli/`: Typer + Rich CLI; entrypoint `specify`; core logic in `__init__.py`.
- `scripts/`: Bash helpers for features, plans, and checks (e.g., `create-new-feature.sh`, `setup-plan.sh`).
- `templates/`: Authoring templates; agent command sources in `templates/commands/*.md`.
- `memory/`: Project constitution and update checklist.
- `specs/`: One folder per feature branch (e.g., `specs/001-create-taskify/`).
- `docs/`: DocFX site; published via GitHub Pages workflow.
- `.github/workflows/`: CI for docs and automated template releases.

## Build, Test, and Development Commands
- **Setup (editable install):** `uv venv && uv pip install -e .`
- **CLI usage:** `uv run specify --help`, `uv run specify check`, `uv run specify init <name> --ai claude`.
- **Feature bootstrap:** `bash scripts/create-new-feature.sh "Add Kanban board"`.
- **Plan + validation:** `bash scripts/setup-plan.sh` then `bash scripts/check-task-prerequisites.sh --json`.
- **Docs:** `docfx docs/docfx.json` (requires DocFX). Releases are CI‑driven; do not bump versions locally.

## Coding Style & Naming Conventions
- **Python:** PEP 8, 4‑space indent, type hints where useful. Keep CLI code in `src/specify_cli/`; prefer small, composable functions; use Rich for UX and Typer for commands.
- **Bash:** `#!/usr/bin/env bash` with `set -euo pipefail`; share helpers via `scripts/common.sh`.
- **Branches:** `NNN-short-slug` (e.g., `001-create-taskify`); scripts expect `^[0-9]{3}-`.

## Testing Guidelines
- **Behavior checks:** `uv run specify check` and a dry `init --here` in a temp directory.
- **Scripts:** run with tracing `bash -x scripts/<name>.sh`; optional lint `shellcheck scripts/*.sh`.
- **Optional tests:** add targeted cases only when introducing complex logic; place under `tests/`.

## Commit & Pull Request Guidelines
- **Commits:** imperative, concise; Conventional Commits welcome (`feat:`, `fix:`, `docs:`, `ci:`, `chore:`).
- **PRs:** clear description, linked issues, reproduction/usage commands, and sample output or screenshots for CLI/docs/template changes.
- **CI & releases:** changes to `memory/`, `scripts/`, or `templates/` trigger automated template releases.

## Agent & Security Notes
- **Sync sources:** keep agent command sources aligned across `templates/commands/*.md` and CLI options.
- **Secrets:** never commit credentials; workflows use `GITHUB_TOKEN`. CLI calls GitHub API anonymously.
- **Local testing:** prefer environment variables; do not store secrets in `specs/` or `memory/`.

