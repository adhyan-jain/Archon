# Archon — agent context

## What is Archon

Engineering intelligence engine for AI coding agents. Extracts unwritten
conventions, detects architectural drift, and serves codebase intelligence
to agents via MCP.

Product 1 of 2. SE-OS (multi-agent system) comes later, only after Archon ships.

## Current State

Stage: 0 — Foundation
Status: Setting up monorepo, tooling, shared contracts

## Structure

packages/core          → shared types (Convention, ConstraintResult, DriftReport)
packages/infra         → LLM router, caching, git operations
packages/graph         → AST engine, dependency graph, index
packages/conventions   → convention mining, drift detection
packages/agent         → constraint engine, MCP server
packages/intel         → temporal memory, health scores

## Dependency Direction (NEVER violate)

core       ← nothing
infra      ← core
graph      ← core
conventions ← core, graph
agent      ← core, graph, conventions
intel      ← core, graph, conventions

## Conventions

- Python 3.12+, all code typed (mypy strict)
- ruff for linting and formatting (line-length 100)
- from __future__ import annotations in every file
- Docstrings on all public classes and functions
- Async by default for I/O operations
- Conventional commits: type(scope): description
- All cross-package communication through core contract types
- No direct LLM calls outside the LLM router
- No direct git operations outside the git wrapper

## Do NOT

- Add SE-OS features. Ship Archon first.
- Skip stages. Build order matters.
- Create circular package dependencies.
- Use Any type or disable mypy.
- Commit API keys or secrets.

## Decisions Log

- Monorepo over multi-repo (shared contracts evolve frequently)
- uv over poetry (faster, native workspaces)
- Pydantic v2 for contracts (validation + serialization)
- mypy strict (typed contracts must be correct)
- hatchling build backend (minimal, fast)
