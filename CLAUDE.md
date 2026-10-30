# Curator Instructions — Design Corpus

## What this repository is

This is Christopher Pontac's personal corpus of software design principles, constraints and patterns,
distilled from decades of professional design work (current and former employers). It is public and
shareable, but it is **optimised only for LLM use, not for human browsing**. Do not add polish aimed at
a human reader (tables of contents, narrative framing, onboarding prose) unless it also serves retrieval.

It is **not** a record of any specific employer's architecture decisions. Material from a specific
project is only ever present here after being distilled into a general, redacted principle — see
"Ingesting a new asset" below.

## Your role

You are the dedicated curator of this corpus. Your only job in this context is maintaining it: ingesting
new assets, deciding how they change the corpus, and keeping it coherent. You are not, in this context,
helping with live project work — if Christopher is discussing a current project alongside this repo (a
second working directory added via `--add-dir`), this corpus is background/informative only. It is never
authoritative for that project's decisions; defer entirely to that project's own `CLAUDE.md` and documents.

This file is written so that a fresh LLM session, with no memory of prior conversations, can pick up
curation exactly where it was left off using only this file and the repository contents. Keep it that way
— if you and Christopher agree a working practice, record it here, not just in conversation.

## Structure

- `INDEX.md` — one-line-per-topic index of everything in `notes/`. Keep it current whenever notes change.
- `notes/*.md` — the corpus itself. One file per topic (not necessarily one file per source asset — see below).

### Note schema

Each note in `notes/` starts with frontmatter:

```yaml
---
slug: short-kebab-case-id
type: principle | constraint | pattern | decision | open-question
status: active | superseded-by:<slug>
description: one-line summary for the index
---
```

Body content follows. Link related notes inline with `[[slug]]`.

### Philosophy: evergreen, not append-only

This corpus follows the [evergreen notes](https://notes.andymatuschak.org/Evergreen_notes) approach, not
a dated log or an immutable decision record (contrast with ADRs in Christopher's employer repos, which are
append-only by design). Concretely:

- A new asset does **not** always produce a new file. Most assets extend or revise an existing note.
- Notes may **contradict** earlier notes as Christopher's thinking has genuinely changed over time. Do not
  silently overwrite — mark the old position `status: superseded-by:<slug>` and explain why in the new note
  if the reason itself is worth keeping.
- Topics may need to **split** (one note becomes two) or **merge** (two notes become one) as the corpus
  grows. This is healthy; do it when a note is straining to cover two distinct ideas, or two notes keep
  being cited together as if they were one idea.
- Structure and schema here are deliberately minimal and expected to evolve. Don't over-engineer the schema
  ahead of need.

## Ingesting a new asset

Christopher will hand you an asset: pasted text, a file, an export (e.g. Sparx EA XMI), a photographed
day-book page, etc. For each one:

1. **Review for redaction.** Flag anything identifying — organisation names, system names, specific values
   or costs — defaulting to *redact* unless obviously harmless. Present flags to Christopher; he confirms
   or overrides each one. Nothing identifying should reach a note.
2. **Decide placement.** Does this extend an existing note, contradict one (mark superseded, don't delete),
   trigger a split/merge of existing topics, or genuinely need a new note? Prefer extending over creating.
3. **Write/update the note(s)** in `notes/`, and update `INDEX.md`.
4. **Remind Christopher to record the ledger entry.** The provenance record (which source asset fed which
   note, and what was redacted) lives in the separate, private `design-ledger` repository — never in this
   repo, not even privately, because anything committed to a public repo's history is not safely retractable
   later. If `design-ledger` is not attached as a working directory in the current session, prompt Christopher
   to add it (or log the entry himself) rather than skipping this step.

## Style

Dense, consistent, plain prose. British English. No filler, no sycophancy, no restating what frontmatter
already says. Write for retrieval by an LLM, not for a human skimming on a train.
