# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Agent Skills is an open format specification for giving AI agents new capabilities through structured skill packages. This repository contains both the format specification/documentation website and a Python reference library for parsing and validating skills.

## Repository Structure

- **`docs/`** — Mintlify-based documentation site (agentskills.io). See `docs/CLAUDE.md` for docs-specific guidance.
- **`skills-ref/`** — Python reference library for parsing, validating, and prompting with skills. See `skills-ref/CLAUDE.md` for development guidance.

## Commands

### Documentation Site (docs/)

```bash
cd docs && npx mint dev    # Local dev server at http://localhost:3000
```

### Reference Library (skills-ref/)

```bash
uv run pytest                  # Run all tests
uv run pytest tests/test_parser.py  # Run a single test file
uv run pytest -k test_name     # Run a single test by name
uv run ruff format .           # Format code
uv run ruff check --fix .      # Lint and autofix
```

All `uv run` commands should be run from the `skills-ref/` directory.

### CLI (after `uv sync` in skills-ref/)

```bash
skills-ref validate path/to/skill          # Validate a skill directory
skills-ref read-properties path/to/skill   # Parse frontmatter to JSON
skills-ref to-prompt path/to/skill-a path/to/skill-b  # Generate XML for agent prompts
```

## Architecture

### Skill Format

A skill is a directory containing a `SKILL.md` file with YAML frontmatter (metadata) and a markdown body (instructions), plus optional `scripts/`, `references/`, and `assets/` subdirectories.

The format uses **progressive disclosure** — three levels of context to minimize token usage:
1. **Metadata** (~100 tokens): name + description, loaded at agent startup for matching
2. **Instructions** (<5000 tokens): full SKILL.md body, loaded on activation
3. **Resources** (as needed): files from scripts/, references/, assets/, loaded on demand

### Reference Library Modules (skills-ref/src/skills_ref/)

- **`parser.py`** — YAML frontmatter parsing and `SkillProperties` extraction
- **`validator.py`** — Validation logic (name format, field constraints, directory name matching, NFKC normalization)
- **`prompt.py`** — Generates `<available_skills>` XML blocks for agent system prompts
- **`models.py`** — `SkillProperties` dataclass
- **`errors.py`** — Exception hierarchy: `SkillError` → `ParseError`, `ValidationError`
- **`cli.py`** — Click-based CLI wrapping the above modules

### Key Validation Rules

- Skill names: 1-64 chars, lowercase Unicode letters/digits/hyphens, no leading/trailing/consecutive hyphens, must match parent directory name
- Descriptions: 1-1024 chars
- All text fields use NFKC Unicode normalization
