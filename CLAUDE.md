# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Agent Skills is an open format specification for giving AI agents new capabilities. A skill is a folder with a SKILL.md file containing YAML frontmatter (metadata) and markdown instructions. This repository contains the specification, a documentation site, and a reference Python library.

## Repository Structure

- **`docs/`** — Mintlify documentation site (agentskills.io). See `docs/CLAUDE.md` for docs-specific guidance.
- **`skills-ref/`** — Reference Python library for parsing, validating, and rendering skills. See `skills-ref/CLAUDE.md` for library-specific guidance.

## Development Commands

### Documentation Site

```bash
npm run dev              # Start Mintlify dev server (localhost:3000)
```

Requires Mintlify CLI: `npm i -g mint`

### Reference Library (skills-ref/)

```bash
cd skills-ref
uv run pytest                                    # Run all tests
uv run pytest tests/test_parser.py               # Run one test file
uv run pytest tests/test_parser.py::test_name    # Run a single test
uv run ruff format .                             # Format code
uv run ruff check --fix .                        # Lint and auto-fix
```

## Contributing Context

- Documentation improvements (in `docs/`) are the primary welcome contribution
- The reference library (`skills-ref/`) is **not accepting code contributions** currently
- AI contributions must be disclosed in PR descriptions
- Proposals go to GitHub Discussions; concrete bugs go to Issues
- Docs deploy automatically on push to `main` via Mintlify
- Logos for the adopter carousel go in `docs/images/logos/` with entries added to `docs/snippets/LogoCarousel.jsx`
