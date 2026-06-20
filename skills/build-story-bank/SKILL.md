---
name: build-story-bank
description: "Build and maintain a STAR+R interview story bank in the user's long-lived notes store (outside this repo). Use when the user says '/build-story-bank', 'work on the story bank', 'I have an interview at <Co> next week', or when invoked as Phase E of /tailor-cover-letter to extract STAR-shaped material into the bank as a side-effect of cover-letter drafting. Source-of-truth lives in the notes store — application-prep stays in the repo, but interview-prep is store-native because it's cross-application and long-lived. The bank backs every behavioral interview answer with a stable Story ID that traces to specific cv-master.md bullets."
---

# Build Story Bank

Interview-prep companion to the application-prep pipeline. Every behavioral interview question the user gets ("tell me about a time when…") should resolve to a numbered Story ID in this bank — tagged, scoped, and traceable back to a CV bullet. The bank is the single artifact that survives across applications.

---

## Where this lives

The story bank is **cross-application and long-lived**, so it lives in the user's personal **notes store** — a markdown folder outside this repo (e.g. an Obsidian/Logseq/plain-markdown vault). The repo holds per-application, pool-based material (CVs, cover letters); interview-prep lasts the whole job-search arc and sits next to the rest of the user's career notes.

**Store path (configurable):** resolve the notes-store root from the user's own config — **never hard-code a folder name.** The bank lives at `<store_root>/story-bank.md`. If the store root isn't configured or the path can't be resolved, **stop and ask the user** — do not guess paths.

**Writes:** always via tempfile-rename (see `lib/write-discipline.md`) — a notes app may be holding the file open. The bank is read with a normal file read; only writes need the atomic swap.

---

## Schema (locked)

The bank is a markdown file with one Markdown table. Schema:

```
| Story ID | Tags | Situation | Task | Action | Result | Reflection | Source bullets | Used for |
|---|---|---|---|---|---|---|---|---|
| S-001 | leadership · scaling · community-program | <2-3 sentence backdrop> | <what was specifically on the user> | <what the user did, in numbered steps if multi-phase> | <metric or named outcome — falsifiable> | <one-line lesson or "what I'd do differently"> | B-1.4, B-2.1 | Lumen AI (2026-05); Meridian (2026-06) |
```

Field rules:

- **Story ID:** `S-NNN` zero-padded. New stories take the next free ID. Never reuse retired IDs (just leave a gap and append `(retired YYYY-MM-DD)` to the Reflection cell of the retired row).
- **Tags:** dot-separated from the closed taxonomy below. 1–4 tags per story. Don't invent new tags inline — promote new tags via a separate edit pass at the bottom of this file.
  - **Closed tag taxonomy:** `leadership · conflict · scaling · ai-adoption · community-program · creator-ops · events · pm · ambiguity · stakeholder`
- **Situation:** 2–3 sentences. Backdrop only — no user-action yet.
- **Task:** what was on the user specifically. Avoid the team-we; this is the part interviewers probe most.
- **Action:** numbered steps if multi-phase. The richest field. Should be specific enough that it cannot have been done by anyone else on the team.
- **Result:** falsifiable metric, named outcome, or stated qualitative shift. Reuse the language from `cv-master.md` where possible — same number on the CV and in the bank means consistency under cross-check.
- **Reflection:** one-liner. What the user learned, what didn't work, what they'd do differently. The follow-up interviewers ask is "what would you change?" — pre-answering it is a free win.
- **Source bullets:** comma-separated `B-*` IDs from `templates/cv-master.md`. This is the trace — every story must back at least one CV bullet, every CV bullet should back at least one story (gap surfacing is part of the maintenance pass below).
- **Used for:** comma-separated `<Company> (YYYY-MM)` pairs. Append-only; never overwrite. Pattern over time = which stories get pulled most + which sit unused.

## Triggers

### Trigger A — standalone, pre-interview

**Signal:** the user says any of:
- `/build-story-bank` (slash command, full pass)
- `"work on the story bank"`, `"add to the story bank"`, `"I have an interview at <Co> next week"`
- A JD URL or interview-prep context where they haven't asked for cover-letter or CV work

**Flow:**

1. **Open the bank.** Read the current `story-bank.md` from the notes store. Show the user the current Story IDs and tags as a one-screen index.
2. **Surface gaps.** Cross-reference current stories against the cv-master.md `B-*` bullet IDs. Output a one-line list of CV bullets that have no story backing them (`B-2.3 — no story; covers community-program scaling`). Same for over-tagged stories (>4 tags) and under-tagged stories (0–1 tags) if any.
3. **Pick a target.** Two modes:
   - **Mode I — interview-driven.** If the user named a company + role, pull the JD top requirements and ask: "Which of these requirements has the weakest story coverage right now?" Recommend filling those gaps first.
   - **Mode II — gap-driven.** If no company named, pick the highest-impact gap from step 2 and ask the user to confirm before drafting.
4. **Interview the user to expand a STAR+R.** One question at a time:
   - "Walk me through the situation. What was the team / org / context?" — capture 2–3 sentences for **Situation**.
   - "What was specifically on you, vs. on the rest of the team?" — capture **Task**.
   - "Walk me through what you did. Step by step." — capture **Action** as a numbered list.
   - "What was the falsifiable outcome? Number, named result, or stated shift." — capture **Result**.
   - "If you had to do it again, what would you change?" — capture **Reflection**.
5. **Defensibility cross-check.** Before writing the row to the bank:
   - **Stated-facts gate** (only-stated-facts rule): every claim must trace to this conversation, the `templates/` pool files, or `cv-master.md`. If the user drifted into a fabricated detail, surface and ask before writing.
   - **CV consistency check.** If the Result field's metric differs from the metric on the linked B-* bullet in cv-master.md, halt and ask which is correct. Same number must show up in both places.
   - **Tag-taxonomy gate.** If a tag isn't in the closed taxonomy, halt and ask whether to (a) drop the tag, (b) substitute a closed-taxonomy tag, or (c) promote the new tag (rare — requires a manual edit to the taxonomy line in this skill).
6. **Append the row** via tempfile-rename to `<store_root>/story-bank.md`. Don't overwrite existing rows. New row gets the next free Story ID.
7. **Offer the next gap.** "Story S-NNN saved. Next biggest gap is `<gap>` — keep going or pause?"

### Trigger B — Phase E of /tailor-cover-letter (side-effect extraction)

**Signal:** `/tailor-cover-letter` finished Phase D (post-ship reflection) and the cover letter has been shipped. The cover letter pulled on STAR-shaped material from `cv-master.md`.

**Flow (lighter than Trigger A — extraction not interview):**

1. **Identify cover-letter-shaped stories.** From the shipped cover letter `Cover_Letter_<Co>.html`, identify any block (lede, gap row, three-things, closer) that told a STAR-shaped mini-story. The CRED-* / BRICK-* / GAP-* block IDs in `cover-letter-pool.md` map back to specific CV bullets via the pool's metadata.
2. **For each cover-letter mini-story, check the bank.** Is there already a Story ID covering this material? Match by `Source bullets` overlap.
   - **If yes:** append `<Company> (YYYY-MM)` to the existing story's `Used for` cell. Tell the user: "S-NNN reused for <Co>." No new row.
   - **If no:** propose a new Story ID with the cover letter's prose as a STAR+R *draft*. Ask the user: "The <Co> cover letter pulled on `B-X.Y` — that's not in the story bank yet. Want to expand it into a full STAR+R now (~2 min interview), defer to next standalone pass, or skip?"
3. If the user says expand: jump into Trigger A's step 4 interview flow, but pre-fill Situation/Task/Action from the cover-letter prose and only interview to fill Reflection + Result-falsifiability gaps.
4. If the user says defer: write a placeholder row to `story-bank.md` with `Status: stub` in the Reflection cell and the cover-letter prose pasted into Action. Tagged `STUB` in the tags cell. The next standalone Trigger A pass will surface stubs first.
5. If the user says skip: do nothing. Don't pollute the bank with empty rows.

**Phase E timing in /tailor-cover-letter:** runs after Phase D's "Reflection complete" message, before the skill exits. The /tailor-cover-letter skill should call this skill (or its logic inline) — not require a separate `/build-story-bank` invocation. *Open implementation question: whether Phase E is auto-triggered or prompts "Run /build-story-bank now? (yes / skip)" — default to prompt-not-auto until calibration shows it's never skipped.*

## Maintenance pass (every 4-6 weeks, or when interview cadence picks up)

When invoked with `/build-story-bank --maintenance` or "audit the story bank":

1. **Stub sweep.** Find rows tagged `STUB` and prompt to expand them.
2. **Stale Used-for sweep.** For stories with `Used for` entries older than 12 months and no recent additions, prompt: "S-NNN last used 2025-04 — still relevant? (keep / retire)".
3. **Gap sweep.** Re-run the cv-master.md cross-reference. Surface CV bullets without stories.
4. **Tag balance.** Count stories per tag. Surface tags with 0–1 stories (the user may be under-prepared for that question type) and tags with 8+ stories (likely over-prepared, room to consolidate).
5. **Result-consistency sweep.** Spot-check 3 random Result cells against the linked CV bullet's metric. Halt and ask if any drift.

## What this skill does NOT do

- **Does not write to the repo's `templates/`.** Application-prep stays repo-side; interview-prep is store-side. Hard separation.
- **Does not draft full interview answers.** The bank is raw material. Polishing into a 90-second spoken answer happens in the moment, not pre-baked, to keep delivery natural.
- **Does not rank stories by "quality".** Every story is an asset; rank-ordering creates anxiety about "the best ones." The taxonomy + tag balance is the lens, not a leaderboard.
- **Does not auto-invoke research** when a story references unfamiliar company context. Halt and instruct only.
- **Does not modify cv-master.md.** If a story surfaces a CV bullet that needs revising, surface the suggestion and route to `/cv-promote-learnings`. Story bank is read-only against cv-master.
- **Does not produce LinkedIn-ready content.** STAR+R is for spoken interviews — written-format extraction (LinkedIn posts, blog drafts) is a separate task.

## Defensibility one-liner

Every Story ID in the bank must trace to at least one `B-*` bullet in `cv-master.md` and must obey the closed tag taxonomy. Every Result cell must match the metric on the linked CV bullet. If a row violates either rule, the bank is in a broken state and the next maintenance pass must reconcile it before adding new rows. No exceptions.
