---
name: python-project-bootstrap
description: Use ONLY when creating a new Python project, initializing pyproject.toml, or explicitly modernizing an existing project's complete Python toolchain. Provides the user's uv, Ruff, ty, pytest, prek, build, and CI defaults. Do not use for ordinary changes to an established project.
---

# Python Project Bootstrap

Use this workflow for new projects or explicit toolchain modernization. For established projects, preserve their supported Python versions, dependency manager, build backend, layout, and tools unless the user asks to replace them.

## Toolchain

- Use the latest stable Python, currently Python 3.14, unless deployment or dependency compatibility requires an older version.
- Use machine-level `uv>=0.12.9` with a project-local `.venv` and `uv.lock`.
- If `uv` is unavailable, ask the user to choose a package-management strategy before installing anything.
- Prefer Ruff for linting and formatting, ty for type checking, pytest for tests, and prek for Git hooks.
- Use modern typing (`str | None`, `list[int]`), Google-style docstrings, `pathlib`, and marimo rather than Jupyter.

## Dependencies

- Add runtime dependencies with `uv add <package>`.
- Add local development dependencies with `uv add --dev <package>` and store them in `[dependency-groups]`.
- Use `[project.optional-dependencies]` only for published, user-installable package features such as `package[postgres]`; extras are not development dependency groups.
- Use current stable dependency versions. Add compatibility constraints only when there is a concrete support requirement.

## Project Configuration

- Start from `~/.pi/agent/templates/pyproject.toml`, then adapt the project name, package layout, dependencies, Python compatibility, build backend, and checks to the actual project.
- Keep Ruff's target version and ty's Python version aligned with `project.requires-python`.
- Prefer a modern build backend such as hatchling, flit, pdm-backend, or uv-build when the project produces a package. Do not add a build system merely to make a scripts-only project look like a package.
- Put contextual lint exceptions in narrow per-file rules. Add global ignores only for frequent, intentional project-wide tradeoffs, with the rationale next to the rule.

## Hooks And CI

- For new projects that need Git hooks, adapt `~/.pi/agent/templates/prek.toml`, then install with `uv tool install prek` and `prek install`.
- For GitHub Actions, adapt `~/.pi/agent/templates/github-actions.yml` to the repository layout and actual checks. Run push CI only on the default branch when pull requests already run the same checks.
- Treat templates as starting points, not files to copy blindly. Remove irrelevant sections and verify action and dependency versions against current official documentation.

## Validation

After bootstrapping or modernizing, run the applicable checks through `uv`:

```text
uv sync
uv run ruff check .
uv run ruff format --check .
uv run ty check
uv run pytest
```

Skip checks that the project does not use. If a check cannot run, report why and give the next best verification.
