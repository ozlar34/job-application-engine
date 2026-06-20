# Architecture

This document describes the **full** job-application pipeline — including the
sourcing/scoring and tracking stages that are *not* reproduced as runnable code in
this repo. The crafting stages (TAILOR, PREP) ship here as Claude Code skills; the
rest are described so the system reads as a coherent whole.

> All names, companies, and numbers below are synthetic (persona: Alex Rivera). See
> [`../templates/README.md`](../templates/README.md).

---

## Design principles

1. **Markdown is the database.** Fact pools, rules, learnings, and the story bank are
   plain markdown — diffable in git, editable by hand or by an approval pass. No opaque
   store sits between you and your own history.
2. **Stable IDs, locked numbers.** Every CV bullet and cover-letter block has a
   permanent ID. Metrics live in a registry and read identically everywhere. Tailoring
   *selects and orders*; it never mints new facts.
3. **Gates over trust.** A drafting step's output is not trusted until an independent
   gate (often a separate sub-agent) clears it. The drafter never grades its own work.
4. **Describe-don't-implement at the infra boundary.** Anything bound to private
   infrastructure (trackers, webhooks, databases) is described here rather than shipped,
   so the public repo stays free of real IDs and credentials.

---

## Stage map

```
  LEAD ──▶ SCORE ──▶ RESEARCH ──▶ TAILOR ──▶ APPLY ──▶ TRACK ──▶ PREP
  └─────── job-match-radar ──────┘  └──── this repo ────┘ └─ infra ─┘ └ this repo ┘
```

| Stage | Where it lives | Status |
|---|---|---|
| LEAD — watchlist + job digest | `job-match-radar` | external repo |
| SCORE — fit scoring + ranking | `job-match-radar` | external repo |
| RESEARCH — company + JD capture | this repo (`lib/jd-capture.md`) + described below | partial |
| TAILOR — CV + cover letter | this repo (`skills/`) | **shipped** |
| APPLY / TRACK — submit + log | infra-bound | described only |
| PREP — story bank + cheatsheet | this repo (`skills/`) | **shipped** |

---

## The crafting core (shipped)

### Fact pools (`templates/`)

- **`cv-master.md`** — the CV fact pool: every bullet the user can truthfully claim,
  each with a stable ID (`RS-*` / `L-*` selectable pairs, `B-*` source bullets) and any
  metric locked to a registry value.
- **`cover-letter-pool.md`** — reusable cover-letter blocks (lede, credibility,
  three-things, gap, category) with stable IDs and `[bracketed slots]` for per-company
  fill.
- **`cv-tailoring-rules.md` / `cover-letter-tailoring-rules.md`** — JD-strand mapping,
  relevance ranking, and the defensibility gate definitions.
- **`*-voice-profile.md`, `positioning-statement.md`** — voice constraints and
  positioning variants.
- **`*-learnings.md`** — append-only logs of per-application observations.
- **`*.html`** — render shells with `{{TOKEN}}` slots; PDFs are print output.

### Selection → review → gate (`tailor-cv`, `tailor-cover-letter`)

1. **Capture the JD** verbatim into a per-application folder (`lib/jd-capture.md`:
   fetch → browser fallback → paste).
2. **Decompose the JD into strands**; rank each pool item by strand relevance.
3. **Critic pre-pass** (cheap model) flags weak/generic picks per section.
4. **Collaborative review** — for the CV, section-by-section; for the cover letter,
   5-way parallel block drafting then per-block sign-off (one block per interaction).
5. **Defensibility gate** (hard block):
   - *CV:* trace check · number-registry check · RS↔L mirror check · strand alignment ·
     out-of-pool prose scan (sub-agent) · 2-page fit · JD-coverage tally.
   - *Cover letter:* claim sourcing · em-dash scan · credential rules · variant
     alignment · AI-tells scan (sub-agent) · length band · CV↔CL consistency (sub-agent).
6. **Emit** markdown + HTML into the application folder. A failed gate blocks output.

A **CV-precondition gate** blocks any cover letter whose sibling tailored CV doesn't
exist — the letter is always built against a known CV.

### Checkpoint / resume (`cv-checkpoint`, `cover-letter-checkpoint`)

Mid-tailoring, state (picks, locks, pending sections, gate status) snapshots to a
checkpoint file so a fresh context resumes precisely. CV and CL checkpoints are
independent. A `previous_checkpoint_at` breadcrumb carries one-step memory; resumes
older than 24h warn before skipping ahead.

### Learnings loop (`cv-promote-learnings`, `cover-letter-promote-learnings`)

Per-application observations append to `*-learnings.md`. A separate, **human-approved**
promotion pass (shared engine: `lib/promote-learnings-engine.md`) labels each entry
`strong`/`tentative` by confidence, then walks it for approval. Approved entries become
new pool variants (fresh IDs — never renumber) or new selection rules. A pool-polish
pass may sharpen prose *in place* at an existing ID, with claim and numbers unchanged.

### Interview prep (`build-story-bank`, `build-interview-cheatsheet`)

- **`build-story-bank`** maintains a cross-application STAR+R story bank (markdown table,
  closed tag taxonomy) in the user's long-lived notes store — outside the repo. Gates:
  stated-facts-only, CV-consistency (Result metric must match the pool), tag-taxonomy.
  Can run standalone or extract STAR shapes from shipped cover letters.
- **`build-interview-cheatsheet`** generates a per-application spoken cheatsheet: Part 1
  behavioral cards map to Story IDs by tag fit and structural beats trace to the CV/CL;
  Part 2 maps each JD requirement to a skill + spoken phrase + JD citation. Reads the
  story bank read-only. No-fabrication gate: every beat traces to story bank, CV, cover
  letter, company research, or stated facts.

### Shared library (`lib/`)

| File | Role |
|---|---|
| `tailoring-common.md` | Folder detection + gate, threshold pre-check, force-load discipline, resume protocol, post-ship diff/classify. |
| `promote-learnings-engine.md` | Queue load, confidence scan, per-entry walk, atomic splice, summary receipt. |
| `write-discipline.md` | Atomic write patterns (tempfile-rename, append-only) for files an editor may hold open. |
| `jd-capture.md` | Application-folder creation (by slug) + verbatim JD capture (fetch → browser → paste tiers). |

---

## The sourcing + scoring layer (described — see job-match-radar)

The front half of the pipeline lives in
[job-match-radar](https://github.com/ozlar34/job-match-radar) and is summarized here for
completeness:

- **LEAD** — a watchlist of target companies plus a recurring **job digest** that pulls
  new postings. New roles surface as leads.
- **SCORE** — each lead is scored for fit against the user's profile and ranked, so
  effort goes to the applications most worth tailoring. The scoring prompt and its own
  learnings/promotion loop live in that repo.

In the working system these stages hand a ranked, captured JD to the **TAILOR** stage in
this repo.

## The tracking layer (described — infra-bound, not shipped)

These stages exist in the working system but are bound to private infrastructure
(external tracker, webhooks, a database), so they are described rather than reproduced:

- **`add-watchlist-company`** — add a company to the sourcing watchlist.
- **`run-job-digest` / `job-digest-review`** — run the recurring digest and triage its
  output into leads.
- **`log-job-lead` / `log-application`** — record a lead, then a submitted application,
  with status and outcome.
- **`score-promote-learnings`** — the scoring-side analogue of the crafting promotion
  passes: merge durable scoring observations back into the scoring rules.

Their selection/promotion *patterns* are the same ones shipped here (stable IDs,
confidence-labelled human-approved promotion, atomic writes); only the storage target
differs.

---

## Data flow summary

```
  watchlist ─▶ digest ─▶ ranked leads ─▶ JD capture ─▶ cv-master + pools
                                              │              │
                                              ▼              ▼
                                      tailor-cv ──▶ tailor-cover-letter
                                              │              │
                                       defensibility gates (block on fail)
                                              │              │
                                              ▼              ▼
                                     application folder (md + HTML → PDF)
                                              │
                          ┌───────────────────┼───────────────────┐
                          ▼                   ▼                    ▼
                    log application      learnings log      story bank ─▶ cheatsheet
                    (infra)              │
                                   promote-learnings (human-approved)
                                         │
                                   back into pools + rules
```
