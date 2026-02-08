---
description: Review code for structural and design quality.
argument-hint: "[file|dir|commit|main..branch|#pr]"
allowed-tools:
  - Bash(git diff:*)
  - Bash(git show:*)
  - Bash(git ls-files:*)
  - Bash(git log:*)
  - Bash(gh pr diff:*)
  - Read
  - Glob
  - Grep
  - LSP
---

Review code for long-term maintainability concerns. Focus on structural and design quality — not bugs, correctness, or style.

## Complexity Framing

Complexity is anything about the structure of a software system that makes it hard to understand and modify. It is the central enemy of maintainability. It shows up in three ways:

- **Change amplification** — A simple change requires edits in many places. Example: a background color is hardcoded in every page template instead of defined once in a shared config. Changing the color means touching every file.

- **Cognitive load** — A developer must hold too much context to work safely. Example: a function allocates memory and returns a pointer, expecting the caller to free it. Every caller must remember this invisible contract or leak memory. Note: more lines of code does not always mean more complexity — a longer but clearer approach can have lower cognitive load than a terse but opaque one.

- **Unknown unknowns** — It is not obvious what must change or what the developer needs to know. This is the worst symptom. Example: a site uses a shared `bannerBg` variable, but a few pages also compute a darker accent from it with a hardcoded formula. Changing `bannerBg` silently breaks those pages, and nothing tells the developer they exist.

These symptoms arise from two root causes:

- **Dependencies** — Code that cannot be understood or modified in isolation. A method signature creates a dependency between its implementation and every caller. Dependencies are unavoidable, but good design minimizes their number and makes the remaining ones simple and obvious.

- **Obscurity** — Important information is not apparent. A variable named `time` with no documented unit, or an error table that must be updated whenever a new status code is added but has no visible link to the status enum. Obscurity often combines with dependencies to create unknown unknowns.

Complexity is incremental — it accumulates from many small decisions, not a single catastrophic error. Review with this in mind: each piece of code either adds a small amount of complexity or reduces it.

## Input Resolution

The argument is `$ARGUMENTS`. Determine what to review:

1. **No argument** (empty) → Review staged + unstaged + untracked git changes. Use `git diff HEAD` and `git ls-files --others --exclude-standard` to gather the changes.
2. **File or directory path** (valid path) → Review that file or recursively review files in that directory.
3. **Commit hash** (matches a git commit) → Review that commit's diff using `git show`.
4. **Branch range** (contains `..`, e.g. `main..feature`) → Review the branch diff using `git diff`.
5. **PR number** (starts with `#`) → Fetch the PR diff using `gh pr diff` (strip the `#`).

Read the resolved code before reviewing. For diffs, focus the review on the changed code but use surrounding context to evaluate design decisions.

## Review Process

1. Resolve the input and read the code to review.
2. Load all principle files from `principles/` to inform the review.
3. Analyze the code through each review dimension, looking for the red flags described in the principle files.
4. Rank findings by severity — how much complexity they introduce or will introduce over time.
5. Select the most impactful issues. Limit output to roughly 5 issues; for very large diffs, focus on the top issues by complexity impact.
6. Format the output following the example in `examples/output.md`.

## Output Format

Start with a header stating what was reviewed, then list issues ranked by severity.

Each issue has:
- **[Tag]** from the relevant principle file's issue tags, followed by a short title summarizing the issue
  - Prefer the predefined tags from principle files when the issue fits
  - Use a descriptive custom tag (e.g., [Hardcoded Dependency], [Inconsistent Abstraction]) for issues that don't fit the standard categories
  - Custom tags should follow the same format: bracketed, clear, and specific
- **Location** as `file:path:SymbolOrLine`
- **1-2 sentence description** of the problem and its impact on complexity
- **Suggestion** that is specific to the project context, not a generic principle restatement

Do NOT include summary statistics, overall scores, or section dividers between issues. Just the header and the ranked issue list.

If there are nearly no issues, give brief positive feedback noting what the code does well.

See `examples/output.md` for the expected format.

## Review Dimensions

Load these principle files for the full set of red flags and issue tags:

- `principles/module-depth.md` — Deep vs shallow modules, interface simplicity, over-decomposition
- `principles/information-hiding.md` — Encapsulation, information leakage, temporal decomposition
- `principles/abstraction-layers.md` — Layer separation, pass-through methods, complexity placement
- `principles/cohesion-separation.md` — Together-or-apart decisions, code repetition, general-special mixing
- `principles/error-handling.md` — Exception proliferation, defining errors out of existence
- `principles/naming-obviousness.md` — Name precision, code clarity, consistency
- `principles/documentation.md` — Comment quality, abstraction documentation
- `principles/strategic-design.md` — Tactical vs strategic thinking, modification quality
