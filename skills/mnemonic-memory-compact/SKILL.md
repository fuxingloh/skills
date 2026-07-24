---
name: mnemonic-memory-compact
description: |
  Compact the mnemonic/ directory: merge duplicate entries, mark superseded decisions,
  and flag outdated findings so memory doesn't go stale. NEVER load this skill unless
  the user explicitly asks to compact mnemonic memory — "/mnemonic-memory-compact",
  "compact memory", or "clean up mnemonic". Do not load it proactively, as part of
  other mnemonic work, or because stale entries were noticed.
---

# Mnemonic Compact

## Overview

Memory goes stale: topics get duplicated across entries, decisions get overridden, bugs get fixed. Compacting cleans up the `mnemonic/` directory so future sessions don't act on dead information.

Entries are referenced with the `♫/NNN` shorthand (`♫/000` is the index); files on disk live at `mnemonic/NNN-short-slug.md`. See the `mnemonic-memory` skill for how entries are created and structured.

## What Gets Compacted — Confusion, Not Clutter

The test for every entry is: **would a future session act wrongly because of it?** Compact only entries that mislead:

- A decision that was later reversed or overridden
- An approach that was tried and disapproved, still reading as the current plan
- A finding the code has since invalidated
- Two entries that contradict each other with no marker saying which one won

Everything else stays — compacting is not tidying. Do NOT compact:

- **Experiments and explorations**, even failed or dormant ones — they record what was tried and are not confusing
- Entries that are merely old, verbose, or loosely overlapping but do not contradict anything
- Open questions and undecided directions — nothing has been decided, so there is nothing to supersede

When in doubt, leave the entry alone. A redundant note costs a few seconds of reading; a wrongly-superseded experiment erases history.

## When to Use

ONLY when the user explicitly asks: `/mnemonic-memory-compact`, "compact memory", or "clean up mnemonic". Never run a compact on your own initiative. If you notice entries contradicting or duplicating each other during normal work, mention it and suggest the user run `/mnemonic-memory-compact` — do not start compacting.

## Process

1. **Survey.** List `mnemonic/` and skim titles. Grep the entries for overlap signals: repeated topics in slugs, `Superseded`, `## Update` markers, pairs of entries that reference each other.

2. **Classify** each suspect — and first apply the confusion test: if the entry doesn't mislead, it is not a suspect, whatever its age or overlap:
   - **Duplicate** — two entries cover the same topic and a reader can't tell which is authoritative. Fold the unique content into the more complete one (the canonical entry), then supersede the other. Same-topic entries that complement each other are not duplicates.
   - **Superseded / disapproved** — a later entry or decision reversed it, but it still reads as the current plan. The old entry stays but gets marked.
   - **Outdated** — contradicted by the current code. Verify against the code first (search, don't assume), then mark it or append a dated update.

3. **Repoint references first.** Before marking anything, `grep -rn "♫/NNN"` across the repo to find every inbound reference — code comments, other entries, the index — and repoint them at the canonical entry.

4. **Mark, don't delete.** Numbers are stable; never renumber. Prepend a banner to the superseded entry and keep the original body below it — the old reasoning is still history:

   ```markdown
   > **Superseded by ♫/NNN** (YYYY-MM-DD) — one line on what changed.
   ```

   Delete a file outright only when it is a pure duplicate with zero unique content and zero inbound references (both verified by search).

5. **Clean the index.** Remove or update stale lines in `♫/000`; check off or drop completed items in "Open work".

6. **Report.** Summarize what was folded, superseded, or deleted, with the old → canonical `♫/NNN` mapping.

## Rules

- Compact confusion, not clutter — only entries that would make a future session act wrongly. Experiments, explorations, and open questions are never compacted.
- The filesystem is the source of truth — survey by listing and grepping `mnemonic/`, not by trusting `♫/000`.
- Never rewrite the original reasoning in a superseded entry; the banner marks it, the body stays.
- Every mark or deletion must be preceded by an inbound-reference search. A dangling `♫/NNN` in code is worse than a stale entry.
- When in doubt, leave it alone.
