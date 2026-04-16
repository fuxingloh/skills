---
name: mnemonic-memory
description: |
  Create, update, and maintain numbered design notes in the mnemonic/ directory.
  Use when a design decision, tradeoff, bug finding, or architecture insight comes up
  in conversation that future sessions need to know about. Also use when the user says
  "log this", "remember this", or "document this decision".
---

# Mnemonic Memory

## Overview

`mnemonic/` is a numbered directory of design notes, decisions, tradeoffs, bug writeups, and reference checkout summaries. It is the project's long-term memory — load-bearing context that survives across conversations. `mnemonic/000-help.md` is the index.

This is NOT documentation. It is internal working memory: why decisions were made, what was tried, what broke, what patterns were borrowed from where. A future session reading `mnemonic/042` should be able to reconstruct the reasoning without the original conversation.

## When to Use

**Proactively create an entry when:**

- A design direction or tradeoff is stated or agreed on in conversation
- A bug is diagnosed and the root cause is non-obvious
- An architecture constraint is discovered (e.g., "Rocket ship rejects fuel needed")
- A reference checkout is studied and patterns are extracted
- The user says "log this", "remember this", or "write this down"

**Do NOT create an entry for:**

- Routine implementation (the code is the record)
- Things already in CLAUDE.md (that's the public-facing guide)
- Ephemeral task details (use tasks or plans instead)
- Git history (use `git log` / `git blame`)

## Creating an Entry

### Step 1: Find the next number

Read `mnemonic/000-help.md` or list the directory to find the highest existing number. The new entry gets the next sequential number.

### Step 2: Write the file

Filename: `mnemonic/NNN-short-slug.md` (e.g., `067-rocket-ship-fuel-missing.md`).

Structure — adapt to the content, but most entries follow this shape:

```markdown
# Title (what happened / what was decided)

## Symptom / Context
What triggered this entry. Error messages, user reports, or the question that came up.

## Root cause / Decision
The actual finding, tradeoff, or architectural choice. Be specific — file paths, line numbers, code snippets, error strings.

## Fix / Implementation
What changed (if anything). Link to the relevant code.

## Why this matters / Rule of thumb
The generalizable lesson. What should a future session do differently because this entry exists?

## Related
Cross-references to other mnemonic entries, CLAUDE.md sections, or external docs.
```

For **reference checkout summaries** (studying a sibling project):

```markdown
# Project name — what it is

## What's worth borrowing
Patterns, architecture ideas, naming conventions.

## What's not transferable
Language gaps, different stacks, things we tried and rejected.

## Key files to read
Specific paths in the checkout that are most instructive.
```

For **bug writeups**:

```markdown
# Bug title

## Symptom
What the user saw. Include error output verbatim.

## Root cause
Why it happened. Trace through the code.

## Fix
What changed.

## Downstream symptoms
Secondary errors caused by the primary failure (red herrings).

## Next steps
If the fix is partial or there are follow-up items.
```

### Step 3: Update the index

Edit `mnemonic/000-help.md`:

1. **Add to the appropriate section** in "Working docs quick reference" — find the right category (Core architecture, Decisions and direction, Reference checkouts, Patterns to learn from, Env and config, Historical context).

2. **Add to "Open work"** if the entry describes something that still needs to be done. Use `- [ ]` checkbox format with a brief description and `(mnemonic/NNN)` reference.

3. **Update "Key design decisions"** table if the entry records a numbered decision.

Format for the quick reference entry:
```
- **NNN** — One-line summary (the key insight — not just a title, but what the reader learns)
```

## Updating an Existing Entry

Append new information with a clear marker. Do not rewrite history — the original reasoning is load-bearing even if the approach changed later.

```markdown
## Update (2026-04-16)
We later discovered X, which changes the recommendation. See mnemonic/NNN.
```

## Cross-referencing

- From code: inline `// See mnemonic/NNN` comments where the reasoning isn't visible at the call site.
- From CLAUDE.md: reference by number in the relevant section.
- From other mnemonic entries: `See mnemonic/NNN` in the Related section.
- From agent frontmatter or TODOs: `(mnemonic/NNN)` parenthetical.

## Reading Mnemonic (Before Work)

Before writing a new feature or bugfix, scan `mnemonic/000-help.md` for related entries. The open work list shows what's planned. The key decisions index shows what's settled. The working docs quick reference tells you where to look for deep context on any topic.

When uncertain about a prior decision, re-read the relevant file rather than reconstructing from memory or guessing.
