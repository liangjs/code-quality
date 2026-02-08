# Skill Design: `/review-code-quality`

## Overview

A Claude Code skill that reviews code for long-term maintainability concerns. It focuses on structural and design quality — not bugs, correctness, or style.

## Input Resolution

The skill auto-detects what to review based on the argument:

- No argument → staged + unstaged + untracked git changes (default)
- File/directory path → review that file or directory
- Commit hash → review that commit's diff
- Branch range (e.g., `main..feature`) → review the branch diff
- PR number (e.g., `#123`) → review PR diff via `gh`

## Output Format

Ranked issues ordered by severity, with positive feedback when nearly no issues found. Full format spec is in `SKILL.md`; examples are in `examples/output.md`.

## File Structure

```
.claude/skills/review-code-quality/
├── SKILL.md                              # Complexity framing + review workflow + output format
└── principles/
    └─── *.md
```

### SKILL.md

Contains:
- **Complexity framing** (from Ch 2): The overarching introduction explaining what complexity is and why it matters. Complexity manifests as change amplification, cognitive load, and unknown unknowns. Its root causes are dependencies and obscurity. This frames the agent's mindset for the entire review.
- **Input resolution logic**: How to detect and resolve the argument type.
- **Output format spec**: The issue-based format described above.
- **References to all principle files**: So the agent loads the relevant dimensions.

### Principle Files

Each file covers one review dimension with:
- Description of the dimension
- Core principles (derived from the book, not copied verbatim)
- Red flags to look for during review
- Issue tags for the output format

## Review Dimensions

### 1. Module Depth (Ch 4, 6)
Deep vs shallow modules, interface simplicity, API generality, over-decomposition (classitis). A deep module provides powerful functionality behind a simple interface.

### 2. Information Hiding (Ch 5)
Encapsulation quality, information leakage between modules, temporal decomposition anti-pattern, exposed internal data structures.

### 3. Abstraction Layers (Ch 7, 8)
Layer separation, pass-through methods/variables, complexity placement. Each layer should provide a different abstraction. Complexity should be pulled downward, not pushed to callers.

### 4. Cohesion & Separation (Ch 9)
Together-or-apart decisions, code repetition, general-special mixing, conjoined methods that can't be understood independently.

### 5. Error Handling (Ch 10)
Exception proliferation, defining errors out of existence, special case design. Reducing the number of places that must handle exceptions.

### 6. Naming & Obviousness (Ch 14, 17, 18)
Name precision, code clarity, consistency across the codebase, violating reader expectations. Code should be readable quickly with correct first guesses about behavior.

### 7. Documentation (Ch 12, 13, 15)
Comment quality, abstraction documentation, missing/vague/redundant comments. Comments should describe things that aren't obvious from code.

### 8. Strategic Design (Ch 3, 16, 19)
Tactical vs strategic thinking, modification quality, over-engineering, forced patterns. Investing in design vs. taking shortcuts that accumulate complexity.

## Excluded Topics

- **Performance optimization** (Ch 20): Not a maintainability concern. The anti-pattern of premature optimization is covered under Strategic Design.
- **Design exploration** (Ch 11, "design it twice"): A process principle that can't be evaluated from code after the fact. Mentioned briefly in Strategic Design.
