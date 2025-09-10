# Repository Guidelines

## Project Structure & Module Organization
- `src/specify_cli/` — Typer + Rich CLI (entrypoint `specify`). Core logic in `__init__.py`.
- `scripts/` — Bash helpers for Spec‑Driven Development (e.g., `create-new-feature.sh`, `setup-plan.sh`, `check-task-prerequisites.sh`, `update-agent-context.sh`).
- `templates/` — Authoring templates and agent command sources (`templates/commands/*.md`).
- `memory/` — Project constitution and update checklist.
- `specs/` — Created per feature branch (e.g., `specs/001-create-taskify/`).
- `docs/` — DocFX site; deployed via GitHub Pages workflow.
- `.github/workflows/` — CI for releases and docs.

## Build, Test, and Development Commands
- Setup (editable install): `uv venv && uv pip install -e .`
- Run CLI locally: `uv run specify --help`, `uv run specify check`, `uv run specify init <name> --ai claude`
- Feature bootstrap: `bash scripts/create-new-feature.sh "Add Kanban board"`
- Plan setup: `bash scripts/setup-plan.sh`  | Validate: `bash scripts/check-task-prerequisites.sh --json`
- Docs (requires DocFX): `docfx docs/docfx.json`
- Note: Release artifacts are produced by CI; no local release step is required.

## Coding Style & Naming Conventions
- Python (3.11+): PEP 8, 4‑space indent, type hints where useful. Keep CLI code under `src/specify_cli/`; prefer small, composable functions; use Rich for UX and Typer for commands.
- Bash: `#!/usr/bin/env bash`, `set -euo pipefail`; share helpers via `scripts/common.sh`.
- Branches: `NNN-short-slug` (e.g., `001-create-taskify`). Scripts assume `^[0-9]{3}-`.
- Specs: `specs/<branch>/{spec.md, plan.md, tasks.md, research.md, data-model.md, quickstart.md}`.

## Testing Guidelines
- No formal unit tests yet. Validate behavior via:
  - CLI: `uv run specify check` and a dry `init --here` in a temp directory.
  - Scripts: `bash -x` for tracing; optional lint with `shellcheck scripts/*.sh`.
- Add targeted tests if introducing complex logic; keep them under `tests/` if added.

## Commit & Pull Request Guidelines
- Commits: imperative, concise; Conventional Commits welcome (`feat:`, `fix:`, `docs:`, `ci:`, `chore:`).
- PRs: clear description, linked issues, reproduction/usage commands, and sample output when touching CLI. For docs/templates, include before/after snippets or screenshots.
- CI & Releases: Changes to `memory/`, `scripts/`, or `templates/` trigger automated template releases. Do not manually bump versions; workflow updates release versions as needed.

## Agent & Security Notes
- Keep agent command sources in sync across `templates/commands/*.md`.
- Do not commit secrets. Workflows use `GITHUB_TOKEN`; CLI calls GitHub API anonymously.
