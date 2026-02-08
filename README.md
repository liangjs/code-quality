# Code Quality Review Plugin

A Claude Code plugin that reviews code for long-term maintainability based on principles from _"A Philosophy of Software Design"_ by John Ousterhout.

Focuses on structural and design quality — not bugs, correctness, or style.

## Overview

This plugin provides the `/code-quality:review` skill that helps identify design issues reducing long-term maintainability by detecting complexity patterns such as shallow modules, information leakage, and unclear abstractions. Use it during code reviews or before commits to catch structural problems early—issues that accumulate gradually and become expensive to fix later.

**When to use:**
- Before committing significant changes to understand design impact
- During pull request reviews to evaluate maintainability
- When refactoring to validate that changes improve structure
- To learn design principles through feedback on your code

**Not designed for:**
- Finding bugs or correctness issues (use linters and tests)
- Enforcing code style or formatting (use formatters)
- Performance optimization suggestions
- Security vulnerability detection

## Installation

### Local Development

To test the plugin locally during development:

```bash
claude --plugin-dir /path/to/code-quality
```

### From Git Repository

To install from a marketplace or git repository, use the Claude Code plugin system. (Instructions will be added once the plugin is published.)

## Usage

```
/code-quality:review                  # Review current git changes
/code-quality:review src/auth.py      # Review specific file
/code-quality:review src/             # Review directory
/code-quality:review abc123           # Review commit
/code-quality:review main..feature    # Review branch diff
/code-quality:review #123             # Review PR
```

## Review Dimensions

- **Module Depth** — Deep vs shallow modules, interface simplicity
- **Information Hiding** — Encapsulation, information leakage
- **Abstraction Layers** — Layer separation, pass-through methods
- **Cohesion & Separation** — Together-or-apart decisions, code repetition
- **Error Handling** — Exception proliferation, special cases
- **Naming & Obviousness** — Name precision, code clarity
- **Documentation** — Comment quality, abstraction documentation
- **Strategic Design** — Tactical vs strategic thinking

## Documentation

- [`DESIGN.md`](DESIGN.md) — Complete plugin design and architecture
- [`skills/review/SKILL.md`](skills/review/SKILL.md) — Skill specification and review workflow
