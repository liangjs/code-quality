# Code Quality Review Skill

Reviews code for long-term maintainability based on principles from _"A Philosophy of Software Design"_ by John Ousterhout.

Focuses on structural and design quality — not bugs, correctness, or style.

## Overview

This skill helps identify design issues that reduce long-term maintainability by detecting complexity patterns such as shallow modules, information leakage, and unclear abstractions. Use it during code reviews or before commits to catch structural problems early—issues that accumulate gradually and become expensive to fix later.

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

## Usage

```
/review-code-quality                  # Review current git changes
/review-code-quality src/auth.py      # Review specific file
/review-code-quality src/             # Review directory
/review-code-quality abc123           # Review commit
/review-code-quality main..feature    # Review branch diff
/review-code-quality #123             # Review PR
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

- [`SKILL.md`](SKILL.md) — Skill specification and review workflow
- [`DESIGN.md`](DESIGN.md) — Architecture and design decisions
- [`principles/`](principles/) — Detailed criteria for each dimension
- [`examples/output.md`](examples/output.md) — Sample outputs
