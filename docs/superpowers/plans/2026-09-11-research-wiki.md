# Research Wiki Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Scaffold and seed a research wiki at `wiki/` that helps prepare for the Meridian stakeholder interview, following the LLM-wiki pattern (curated raw sources → LLM-generated, cross-referenced pages, governed by a schema file).

**Architecture:** A folder of markdown pages (`index.md`, `log.md`, `overview.md`, `entities/`, `concepts/`, `sources/`) plus a `SCHEMA.md` that documents the ingest/query/lint procedures Claude follows conversationally — no slash-command tooling. Raw source material lives in `wiki/sources-raw/`, gitignored.

**Tech Stack:** Plain markdown files, no code, no dependencies.

**Spec:** `docs/superpowers/specs/2026-09-11-research-wiki-design.md`

## Global Constraints

- Only the student's own curated sources may enter the wiki: the client brief, public/outside research, and any Meridian data the data handling checklist already marks AI-safe (sales totals by store/week, store attributes). **Never** loyalty data, labor schedules, or customer/employee-level POS detail, as a source or quoted in any page — per `docs/data-handling-checklist.md`, Section 3.
- `wiki/sources-raw/` is gitignored; everything else under `wiki/` is committed.
- All three workflows (ingest, query, lint) are conversational — no slash commands, no skill tooling.
- Page filenames are kebab-case slugs, e.g. `wiki/entities/dana-okafor.md`, `wiki/sources/client-brief.md`.

---

### Task 1: Scaffold the wiki folder structure

**Files:**
- Create: `wiki/index.md`
- Create: `wiki/log.md`
- Create: `wiki/overview.md`
- Create directories: `wiki/sources/`, `wiki/entities/`, `wiki/concepts/`, `wiki/sources-raw/`
- Modify: `.gitignore`

**Interfaces:**
- Produces: the four top-level files and four directories that Tasks 2 and 3 write into. `wiki/index.md` uses the section headers `## Sources`, `## Entities`, `## Concepts`. `wiki/log.md` uses one `##` heading per dated entry, newest first. `wiki/overview.md` is free-form prose under a single `# Overview` heading.

- [ ] **Step 1: Create the wiki directories**

```bash
mkdir -p wiki/sources wiki/entities wiki/concepts wiki/sources-raw
```

- [ ] **Step 2: Create `wiki/index.md`**

```markdown
# Wiki Index

Catalog of everything in this wiki. One line per page.

## Sources

_(none yet)_

## Entities

_(none yet)_

## Concepts

_(none yet)_
```

- [ ] **Step 3: Create `wiki/log.md`**

```markdown
# Wiki Log

Reverse-chronological record of ingests, queries, and lint passes.
```

- [ ] **Step 4: Create `wiki/overview.md`**

```markdown
# Overview

_(No sources ingested yet — this will synthesize what the wiki knows
about Meridian and its market once there's something to synthesize.)_
```

- [ ] **Step 5: Add `wiki/sources-raw/` to `.gitignore`**

Add this block to the existing `.gitignore` (append, don't remove existing entries):

```gitignore

# Wiki raw sources (external articles/research) — not version-controlled
wiki/sources-raw/
```

- [ ] **Step 6: Verify — what done looks like**

Run:

```bash
ls wiki/ && echo --- && cat wiki/index.md && echo --- && cat wiki/log.md && echo --- && cat wiki/overview.md && echo --- && cat .gitignore
```

Expected: `wiki/` lists `index.md`, `log.md`, `overview.md`, `concepts/`, `entities/`, `sources/`, `sources-raw/`; the three files print with the content above; `.gitignore` includes `wiki/sources-raw/`.

**How you check it:** run the command above yourself, or open `wiki/` in your editor's file tree and confirm the four files and four folders exist with that starting content.

- [ ] **Step 7: Commit**

```bash
git add wiki/index.md wiki/log.md wiki/overview.md .gitignore
git commit -m "Scaffold research wiki folder structure

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01RBS8WWtHSM4UKmC4ebaQ8d"
```

(`wiki/sources/`, `wiki/entities/`, `wiki/concepts/` are empty and won't appear in `git status` until Task 3 adds files to them — that's expected, git doesn't track empty directories.)

---

### Task 2: Write `wiki/SCHEMA.md`

**Files:**
- Create: `wiki/SCHEMA.md`

**Interfaces:**
- Consumes: the folder layout and file conventions from Task 1 (`wiki/index.md`'s three headings, `wiki/log.md`'s entry format).
- Produces: the procedures Task 3 (and every future ingest/query/lint) follows. Referenced by name (`wiki/SCHEMA.md`) from `docs/superpowers/specs/2026-09-11-research-wiki-design.md`.

- [ ] **Step 1: Write `wiki/SCHEMA.md`**

```markdown
# Wiki Schema

Governs how this wiki is built and maintained. Read this before performing
any ingest, query, or lint.

## Structure

- `index.md` — catalog of every page, grouped under `## Sources`,
  `## Entities`, `## Concepts`, one line each: `- [title](path) — one-line summary`.
- `log.md` — reverse-chronological entries, newest first, one `##` heading
  per entry: `## YYYY-MM-DD — <ingest|query|lint>: <short description>`,
  followed by 1-3 sentences on what happened and which pages were touched.
- `overview.md` — a single running synthesis of what the wiki collectively
  says about Meridian and its market. Rewritten, not appended to, as
  understanding evolves.
- `sources/<slug>.md` — one page per ingested source. Sections: `## Key
  Takeaways`, `## Open Questions`, and a link back to `sources-raw/<slug>.*`
  if the raw material was saved.
- `entities/<slug>.md` — one page per person/org/competitor. Sections:
  `## Who They Are`, `## Why They Matter`, `## What We Know`, `## Open
  Questions`.
- `concepts/<slug>.md` — one page per theory/pattern relevant to the
  engagement. Sections: `## The Idea`, `## Why It's Relevant`, `##
  Connections` (links to related source/entity/concept pages).
- Filenames are kebab-case slugs matching the page title.

## Data Handling — read before every ingest

Per `docs/data-handling-checklist.md`, Section 3 (AI Tool Usage): this
wiki is entirely LLM-generated, so anything used as a source here is, by
definition, being fed into an AI tool.

- Allowed sources: the client brief, public/outside research (industry
  articles, market reports, competitor information), and Meridian data
  the checklist already marks AI-safe (sales totals by store/week, store
  attributes).
- Never a source, never quoted, never excerpted: loyalty program data,
  labor schedules, customer- or employee-level POS detail.
- If it's unclear whether something is safe to use, don't ingest it —
  check the checklist or ask first.

## Workflow: Ingest

1. Student provides a source (pasted text, a link, or a file).
2. Check it against Data Handling above before reading further.
3. Read it and discuss the key takeaways with the student.
4. Write or update `sources/<slug>.md`.
5. If the raw material should be kept, save it to `sources-raw/<slug>.*`
   and link to it from the source page.
6. Update `index.md` (add/update the entry under `## Sources`, and under
   `## Entities`/`## Concepts` for any pages touched in step 7).
7. Revise existing `entities/` and `concepts/` pages this source touches —
   typically a handful of related pages, not the whole wiki. Create new
   entity/concept pages if the source introduces something not yet
   covered.
8. Append an entry to `log.md`.
9. Ask the student to approve before writing anything to disk.

## Workflow: Query

1. Student asks a question.
2. Search `index.md` to find relevant pages, then read those pages.
3. Synthesize an answer with citations back to the specific source/entity/
   concept pages used.
4. If the answer is a substantial new analysis worth keeping, ask the
   student whether to file it back into the wiki as a new concept page
   (following the Ingest steps 6-9 above for that new page).

## Workflow: Lint

1. Student triggers a lint pass.
2. Read every page in `sources/`, `entities/`, and `concepts/`.
3. Check for: contradictions between pages, stale claims (superseded by a
   later source), orphan pages (nothing in `index.md` or another page
   links to them), and missing cross-references (two pages clearly relate
   but don't link to each other).
4. Report findings to the student; they decide what to act on.
5. Append the lint pass and its findings to `log.md`.
```

- [ ] **Step 2: Verify — what done looks like**

Run:

```bash
test -f wiki/SCHEMA.md && grep -c "^## " wiki/SCHEMA.md
```

Expected: file exists; grep prints a count of 5 or more (confirms the Structure/Data Handling/three Workflow sections are all present).

**How you check it:** read `wiki/SCHEMA.md` yourself and confirm it covers page conventions, the data-handling rule, and all three workflows clearly enough that you'd be comfortable with me following it verbatim.

- [ ] **Step 3: Commit**

```bash
git add wiki/SCHEMA.md
git commit -m "Add wiki schema documenting page conventions and workflows

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01RBS8WWtHSM4UKmC4ebaQ8d"
```

---

### Task 3: Seed the wiki — ingest `client-brief.md`

**Files:**
- Create: `wiki/sources/client-brief.md`
- Create: `wiki/entities/meridian-markets.md`
- Create: `wiki/entities/dana-okafor.md`
- Create: at least one file under `wiki/concepts/` (exact slug depends on the brief's content — see Step 3)
- Modify: `wiki/index.md`, `wiki/log.md`, `wiki/overview.md`

**Interfaces:**
- Consumes: the page templates and Ingest workflow from `wiki/SCHEMA.md` (Task 2); `client-brief.md` at the repo root as the source text.
- Produces: the first populated example of every page type, for Task-4-and-beyond ingests to follow as a model.

- [ ] **Step 1: Re-read `client-brief.md` and `wiki/SCHEMA.md`**

`client-brief.md` is at the repo root (already read earlier in this
project). Re-read `wiki/SCHEMA.md`'s Ingest workflow (Task 2) immediately
before this step so the page structure is fresh.

- [ ] **Step 2: Write `wiki/sources/client-brief.md`**

Follow the `sources/<slug>.md` template from `SCHEMA.md` (`## Key
Takeaways`, `## Open Questions`). Key Takeaways should cover: what
Meridian is (14 stores, LA/Orange/Ventura counties, ~$78M revenue, ~620
employees, competes on prepared foods/local sourcing/smaller footprint),
the growth story (6→14 stores in 5 years via leases from chains that
pulled out), the ask (dashboard for store/category performance to decide
on the Pasadena site), the loyalty program (~40,000 members, unused so
far), the data available (POS ~3yrs, loyalty, labor, store attributes),
the AI-tool restriction, and the timeline (8 weeks, board preview in 3).
Open Questions should surface what the brief leaves unclear (e.g., what
specifically makes the Pasadena site "obvious," what "improve customer
experience" means operationally, whether prior expansions have data on
what made stores succeed or struggle).

- [ ] **Step 3: Write the entity and concept pages this source touches**

Create, following `SCHEMA.md`'s templates:
- `wiki/entities/meridian-markets.md` — the company itself: who they are,
  why they matter (this is the engagement's subject), what's known from
  the brief, open questions.
- `wiki/entities/dana-okafor.md` — the stakeholder: role (VP of
  Operations), what she's asked for, how she prefers to communicate
  (email, travels Tue/Wed, slow to reply, assistant can't answer
  analytics questions), open questions to raise in the interview.
- At least one concept page capturing a pattern worth tracking across
  future research, for example `wiki/concepts/specialty-grocery-positioning.md`
  (competing on prepared foods/local sourcing/smaller footprint vs.
  national chains) or `wiki/concepts/lease-driven-expansion.md`
  (growing by taking over leases in neighborhoods chains exited). Pick
  whichever the brief supports better; it's fine to write both if the
  content warrants it.

Each new entity/concept page should link back to `sources/client-brief.md`
as the source of what's known so far.

- [ ] **Step 4: Update `wiki/index.md`**

Add one-line entries under `## Sources`, `## Entities`, and `## Concepts`
for every page created in Steps 2-3, replacing the `_(none yet)_`
placeholders.

- [ ] **Step 5: Append to `wiki/log.md`**

Add an entry at the top (newest first):

```markdown
## 2026-09-11 — ingest: client-brief.md

Ingested the Meridian Markets client brief. Created source summary,
entity pages for Meridian Markets and Dana Okafor, and concept page(s)
for [list what you actually created in Step 3]. Seeds the wiki.
```

- [ ] **Step 6: Rewrite `wiki/overview.md`**

Replace the placeholder with 2-4 short paragraphs synthesizing what the
wiki now knows: who Meridian is, what they need, why now, and what's
still open — pulling from the pages just created, not restating the raw
brief.

- [ ] **Step 7: Verify — what done looks like**

Run:

```bash
ls wiki/sources wiki/entities wiki/concepts && echo --- && cat wiki/index.md && echo --- && head -5 wiki/log.md
```

Expected: `wiki/sources/client-brief.md`, `wiki/entities/meridian-markets.md`,
`wiki/entities/dana-okafor.md`, and at least one file in `wiki/concepts/`
all exist; `wiki/index.md` no longer shows any `_(none yet)_` placeholder;
`wiki/log.md`'s top entry is the 2026-09-11 ingest entry.

**How you check it:** read through `wiki/overview.md` first — if it
reads as an accurate, useful summary of the brief without you having to
cross-check every line against the original, the seeding worked. Spot-check
one entity page and the source page against `client-brief.md` for accuracy.

- [ ] **Step 8: Commit**

```bash
git add wiki/sources wiki/entities wiki/concepts wiki/index.md wiki/log.md wiki/overview.md
git commit -m "Seed wiki with first ingest: client-brief.md

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
Claude-Session: https://claude.ai/code/session_01RBS8WWtHSM4UKmC4ebaQ8d"
```

---

## Done

All three tasks committed. The wiki is scaffolded, `SCHEMA.md` documents
the workflows, and it's seeded with a full example (source + entity +
concept pages) built from `client-brief.md`. Future sources are ingested
by asking Claude conversationally, per `wiki/SCHEMA.md`.
