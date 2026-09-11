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
