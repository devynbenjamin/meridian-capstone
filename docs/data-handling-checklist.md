# Data Handling Checklist — Meridian Markets Capstone

For the student team and for Dana Okafor's team at Meridian Markets. Covers
how we receive, store, work with, share, and dispose of Meridian's data for
the duration of the engagement.

Source: `client-brief.md` (Dana Okafor, August 2026) and the NDA it references.

---

## 1. Receiving & Access

- [ ] NDA signed and countersigned before any data request goes to IT
      *(brief: "IT can pull an extract for you once the NDA is signed. Ask
      for Marcus.")*
- [ ] Confirm who on the student team needs access to the raw extract —
      keep this list as short as the work allows
- [ ] Log the date the extract is received and what it contains (POS
      transactions, loyalty data, labor data, store attributes)

## 2. Storage

- [ ] Raw extract stored in a single team-controlled location — not
      forwarded to personal email, personal cloud drives, or personal
      devices outside that location
- [ ] Access to the raw extract limited to the team members identified in
      Section 1
- [ ] Derived/working files (cleaned data, analysis outputs) kept separate
      from the raw extract so it's always clear which files carry
      customer/employee data and which don't

## 3. AI Tool Usage

**This is the section to check before pasting anything into Claude,
ChatGPT, Copilot, or any other AI tool.**

*(brief, Terms of engagement: "Customer records and employee data do not go
into ChatGPT, Claude, Copilot, or any other AI tool. That includes the
loyalty program data, the labor schedules, and any excerpts of them. Our
counsel is firm on this, and it is not negotiable. Sales totals by store and
week, and the store attributes, are fine to use with those tools.")*

| Data | AI tools |
|---|---|
| Loyalty program membership & purchase history | ❌ Never — no excerpts, no summaries, no "just a few rows" |
| Labor scheduling & hours | ❌ Never — no excerpts, no summaries |
| POS transactions, if they contain customer-identifying detail | ❌ Never |
| Sales totals by store and week | ✅ Fine |
| Store attributes (square footage, opening date, lease terms) | ✅ Fine |

- [ ] Before pasting or uploading anything into an AI tool, check it
      against the table above
- [ ] If a file mixes allowed and restricted data (e.g., a POS extract with
      customer IDs attached to totals), treat the whole file as restricted
      until it's been separated
- [ ] When in doubt, don't paste it in — ask a teammate or, if needed, ask
      Dana

## 4. Working With the Data

- [ ] Aggregate or de-identify customer/employee data locally, outside any
      AI tool, before it's used to inform AI-assisted analysis
- [ ] Keep a record of what's been aggregated/de-identified so it's
      reviewable later
- [ ] Store-level and category-level outputs (the dashboard, summary
      stats) are built from the raw data but don't need to reproduce
      individual customer or employee records

## 5. Sharing

- [ ] Anything shared back to Meridian (including the board preview in
      three weeks) contains only aggregated/store-level data — no raw
      loyalty or labor records
- [ ] Anything shared within the student team follows the same storage and
      AI-tool rules as the original extract
- [ ] Questions about what's shareable go to Dana by email — her
      assistant can schedule time but "can't answer questions about the
      analytics" *(brief)*

## 6. Disposal

- [ ] Confirm end-of-project disposal requirements once the signed NDA is
      in hand — **placeholder**, brief states an NDA "will follow this
      brief and covers everything below" but its text isn't available yet
- [ ] Until then, default to deleting the raw extract and any derived
      files containing customer/employee data at project close, unless
      the NDA specifies otherwise
