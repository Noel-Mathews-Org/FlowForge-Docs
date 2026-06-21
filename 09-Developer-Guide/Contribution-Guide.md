# Contribution Guide

[← Back to Main Index](../README.md)

## Branching Strategy
FlowForge utilizes a simplified Git flow:
- `Cloud-Track-dev`: The primary development branch. All feature branches merge here via Pull Request.
- `main`: The production branch. `Cloud-Track-dev` is merged into `main` for releases.

## Commit Standards
Follow conventional commits:
- `feat: added new task approval workflow`
- `fix: resolved race condition in stream consumer`
- `docs: updated api catalog`

## Pre-Commit Hooks
Run `pre-commit install` in your local repository. This ensures:
- Python code is formatted with `black`.
- Frontend code is linted with `eslint`.
- No secrets or large binaries are accidentally committed.
