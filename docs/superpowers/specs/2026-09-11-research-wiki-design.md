# Research Wiki Design — Meridian Stakeholder Interview Prep

## Purpose

A research wiki to build understanding of Meridian Markets and its market
(specialty grocery competitive landscape, expansion strategy, the Pasadena
market) ahead of the stakeholder interview with Dana Okafor's team. Follows
the LLM-wiki pattern described at
https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f: raw
sources are curated by a human, an LLM turns them into a cross-referenced
set of wiki pages, and a schema file governs how that happens.

This is a knowledge-building tool for the student, not a Meridian
deliverable — it isn't shared with Dana's team.

## Folder Layout

```
wiki/
  SCHEMA.md          — governs structure, conventions, and the three workflows below
  index.md           — catalog of everything in the wiki, one-line summaries, by category
  log.md             — chronological record of ingests, queries, and lints
  overview.md        — synthesis of the whole knowledge base so far
  entities/          — one page per person/org/competitor (e.g. Dana Okafor, named competitors)
  concepts/          — one page per theory/pattern (e.g. "specialty grocery positioning")
  sources/           — one summary page per ingested source, e.g. sources/client-brief.md
wiki/sources-raw/    — the raw documents themselves (pasted text, links, converted PDFs),
                        referenced by the corresponding page in wiki/sources/
```

`wiki/sources/` (LLM-written summaries) and `wiki/sources-raw/` (the
original material) are kept separate so it's always clear which files are
generated and which are the curated input.

This is unrelated to `raw/` at the repo root, which holds Meridian's actual
data extract (POS, loyalty, labor, store attributes) and is excluded from
both git and Claude — see Data Handling below.

## Page Types

- **Source summary** (`wiki/sources/<slug>.md`) — key takeaways, date
  ingested, link back to the file in `wiki/sources-raw/`, open questions
  it raises.
- **Entity page** (`wiki/entities/<slug>.md`) — who/what they are, why
  they matter to this engagement, what's known, open questions.
- **Concept page** (`wiki/concepts/<slug>.md`) — the idea, why it's
  relevant, which sources/entities connect to it.
- **index.md** — flat catalog of every page in the wiki, grouped by type
  (sources / entities / concepts), one-line summary each.
- **log.md** — reverse-chronological entries: date, action (ingest /
  query / lint), what happened, which pages were touched.
- **overview.md** — a running synthesis of what the wiki collectively
  says about Meridian and its market. Rewritten (not appended to) as
  understanding evolves.

## Workflows

### Ingest

1. Student provides a source: pasted text, a link, or a file.
2. Claude reads it and discusses the key takeaways with the student.
3. Claude writes or updates the source summary page.
4. Claude updates `index.md` and appends to `log.md`.
5. Claude revises existing entity/concept pages the source touches
   (typically a handful of related pages, not the whole wiki).
6. Student approves before anything is written to disk.

### Query

1. Student asks a question.
2. Claude searches relevant wiki pages and synthesizes an answer with
   citations back to source pages.
3. If the answer constitutes a substantial new analysis worth keeping,
   Claude and the student decide together whether to file it back into
   the wiki as a new concept/analysis page.

### Lint

1. Student triggers a lint pass (e.g., "lint the wiki").
2. Claude scans for: contradictions between pages, stale claims,
   orphan pages (nothing links to them), and missing cross-references.
3. Claude reports findings; the student decides what to act on.
4. Claude appends the lint pass and its findings to `log.md`.

All three workflows are conversational — there is no slash-command
tooling. `SCHEMA.md` documents these procedures in full so they're
followed consistently across sessions; Claude reads it before performing
any of the three workflows.

## Data Handling

Per `docs/data-handling-checklist.md` (Section 3, AI Tool Usage), this
wiki is entirely LLM-generated, so anything that becomes a raw source is,
by definition, being fed into an AI tool. `SCHEMA.md` states explicitly:

- Only sources the student personally curates and brings in may enter
  the wiki: the client brief, public/outside research (industry
  articles, market reports, competitor information), and any
  Meridian-provided data that the checklist already classifies as
  AI-safe (sales totals by store/week, store attributes).
- Loyalty program data, labor schedules, and any customer- or
  employee-level POS detail must never be used as a wiki source, quoted
  in a wiki page, or pasted into conversation to build one — no
  exceptions, no excerpts.
- If it's unclear whether a piece of data is safe to use as a source,
  default to not ingesting it and check the checklist (or ask) first.

## Seeding

The first ingest, once this spec is approved and the implementation plan
executed, is `client-brief.md` itself.

## Out of Scope

- Slash-command or skill-based tooling for the workflows (rejected in
  favor of the simpler schema-driven conversational approach — this is a
  solo-user tool with no consistency problem to solve yet).
- Sharing or exporting the wiki to Meridian/Dana's team.
- Ingesting any of Meridian's raw extract data (see Data Handling).
