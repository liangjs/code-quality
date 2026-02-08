# Code Quality Review Plugin

## Project Goal

Build a Claude Code plugin (`code-quality`) providing the `/code-quality:review` skill that evaluates code quality for long-term maintainability.

## Book Reference

The book is available at `book/`. Chapters are in `book/chapters/`.

## Plugin Design

See @DESIGN.md for the complete finalized design. The plugin structure follows Claude Code's plugin conventions with manifest in `.claude-plugin/` and skill files in `skills/review/`.

## Implementation Notes

- Read book chapters selectively as needed during implementation, do NOT read the full book upfront.
- Bump the plugin version in `.claude-plugin/plugin.json` when making changes that affect users (skill behavior, output format, new features).
