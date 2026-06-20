---
name: build-interview-cheatsheet
description: "Generate a per-application spoken interview cheatsheet from inputs that already exist — the STAR+R story bank, the company research note (Personality Hooks / Comp Signal / Level Mapping), the JD, the tailored CV + cover letter in the application folder, and the salary-deflect script. Produces <Co>_Interview_Cheatsheet.html in the application folder (HTML → print-PDF, Cmd+P, Margins: None). Use when the user says '/build-interview-cheatsheet', 'build a cheatsheet for <Co>', 'I have an interview at <Co>', 'first interview scheduled at <Co>', or 'prep the <Co> interview'. The interview-prep companion to /build-story-bank: this skill READS the story bank (never rewrites it) and maps each anticipated question to a Story ID + a spoken timing arc."
---

# Build Interview Cheatsheet

The job-search funnel has a skill for every stage except this one. When a first interview gets scheduled, this skill assembles a spoken cheatsheet — the per-application companion to the cross-application story bank — from material that already exists. It writes nothing new about the user: every spoken beat traces to the story bank, the CV, the cover letter, the company research note, or the stated facts in the project's operational rules.

Example output shape: `applications/2026-06-01-lumen-ai/Lumen-AI_Interview_Cheatsheet.html`.

---

## Story bank location (configurable)

The story bank is store-native (it lives in the user's long-lived notes store, outside this repo — see `/build-story-bank`). Resolve its root from the user's own config; **never hard-code a folder name.** The bank lives at `<store_root>/story-bank.md`. If the store root isn't configured or can't be resolved, **stop and ask the user** — don't guess.

---

## Preconditions (hard gate — halt if unmet)

1. **Application folder exists** at `applications/<date>-<co>/` with a tailored CV (`Resume_<Co>.html`) and ideally the cover letter (`Cover_Letter_<Co>.html`). If the folder or CV is missing, halt — the cheatsheet is downstream of the application, not a substitute for it.
2. **JD present** at `<folder>/JD.md`. If missing, ask the user for the JD URL or text before proceeding.
3. **Research gate** — the company research note must carry `## Personality Hooks`, `## Comp Signal`, and `## Level Mapping` (the same gate `/tailor-cover-letter` enforces). If `## Personality Hooks` is missing, halt and instruct: *"No personality-grade research on <Co> — run the research step first, then re-run this."* Don't fabricate company voice.
4. **Story bank readable.** Read it; if it has zero stories, halt and route to `/build-story-bank` first — there's nothing to map behavioral answers to.

---

## Inputs (read-only) and what to pull from each

| Source | Path | Pull |
|---|---|---|
| Story bank | `<store_root>/story-bank.md` | Story IDs (`S-NNN`) + their STAR+R material. Match stories to this JD's behavioral surface by `Tags` and `Source bullets`. **Read only — never write or rewrite the bank** (that's `/build-story-bank`'s job, including the `Used for` cell). |
| Company research note | the per-company research note for this application | `## Personality Hooks` → why-company beats, founders, voice, named customers; `## Comp Signal` → the band for the salary card; `## Level Mapping` → title/level for the "why this role" + level-aware framing. |
| JD | `<folder>/JD.md` | Top + hard requirements, the company's flagship example/workflow, the stop-list. Drives Part 2 skill-to-requirement mapping and the project question. |
| Tailored CV | `<folder>/Resume_<Co>.html` | The strands/positioning actually submitted — the cheatsheet must stay consistent with the CV (same metrics, same lead-noun). |
| Cover letter | `<folder>/Cover_Letter_<Co>.html` | The lede, gap framing, "why-company" — reuse its angle so the spoken story matches the written one. |
| Salary deflect | the pre-offer salary-deflect script (templated) | **Pre-offer deflect only** — the deflect line + equity pivot. NOT the offer-stage counters (those activate only after an offer). |
| Stated facts | the project's operational rules | Notice period (state as fact, never improvise), salary target + floor (never name the floor), positioning (AI as one strand unless the AI-ops exception fires). |
| Language level | the user's language-level note | Current CEFR level — state honestly; if not findable, ask the user, don't improvise. |

---

## The cheatsheet structure

Fill `templates/interview-cheatsheet-template.html` (the canonical template; its top comment documents card anatomy). Set `--accent` to the company's brand colour if research surfaces it, else keep the neutral default. Output to `<folder>/<Co>_Interview_Cheatsheet.html`.

**Part 1 — Questions they'll ask.** One `.card` each, with a `.badge` timing arc and (for behavioral answers) the Story ID in `.meta`. Standard set — adapt to the JD, drop what doesn't apply, add role-specific ones:

| # | Question | Timing | Source / Story |
|---|---|---|---|
| 1 | "Tell me about yourself / what's got you looking?" | 75–90s | CV profile + cover-letter lede; arc operator → builder → why now |
| 2 | "Why <Co>? / what do you know about us?" | 30–40s | `## Personality Hooks` + cover-letter why-company; honest, specific reasons |
| 3 | "Why this role?" | 50–60s | JD + `## Level Mapping`; conviction, not lateral repackaging |
| 4 | "Why are you leaving?" | ~30s | Pull never push, forward-looking, zero grievance |
| 5+ | Behavioral / "walk me through a project / a time when…" | ~90s | **Map to a Story ID** from the bank by tag fit to the JD; STAR-compressed |
| — | Role-specific (adoption, ambiguity, stakeholder, scaling…) | ~60s | The JD's named competencies → the best-fit Story ID |
| — | "Salary expectations?" | ~20s | **Deflect** → if pushed, band signal → equity pivot. `.warn`: never name the floor, never anchor low |
| — | "Notice / availability / in-office?" | ~20s | State the notice period as fact. Note any relocation constraint |
| — | "Language level?" | ~15s | Only if it comes up — don't volunteer. Honest CEFR, no apology |

**Your questions for <interviewer>.** 3–4 sharp questions (reporting line, first-6-months success metric, why-the-role-now, profile-fit) + the closer ("Any question marks about my fit I can clear up before we wrap?"). Pull the interviewer's name from the scheduling context or the research note; if unknown, use the role ("the hiring manager").

**Part 2 — Top skills & phrases (HR screen).** One `.card` per JD top requirement: the skill, the spoken phrase, and a `.note` "why it lands" tying back to the **exact JD line**. This is where the JD's hardest requirement becomes the headline phrase.

---

## No-fabrication gate (hard — runs before writing the file)

Every spoken beat must trace to: the story bank, the tailored CV, the cover letter, the company research note's sections, or a stated fact in the project's operational rules / language-level note. Specifically:

- **Story IDs must exist** in the bank. If a question needs a story that isn't there, do NOT invent one — flag the gap and offer `/build-story-bank` to fill it first.
- **No new metrics, titles, team sizes, or named outcomes.** Reuse the exact numbers from the CV/story bank (same number in both places — consistency under cross-check).
- **No improvised facts** — interviewer name, language level, notice period, salary floor. Missing fact → ask, don't guess.
- **Positioning** follows the project's operational rules: AI is one strand, unless the AI-ops exception already fired for this application's CV (then mirror that AI-builder-led framing).
- **No em dashes in the spoken prose** is not required (this is a private prep doc, not outward-facing) — but the *content* must still be defensible, since it's what the user will say out loud to the company.

If any beat can't be traced, surface it to the user and resolve before writing — same discipline as the `/tailor-cv` defensibility gate.

---

## Output

1. Write `<folder>/<Co>_Interview_Cheatsheet.html` from the filled template (a direct `Write` is fine here; the app folder is git-tracked).
2. Print: *"Cheatsheet ready at `<path>`. Open it, Cmd+P, Margins: None to save a PDF. Walk the cards out loud once — hit the beat, not the script."*
3. **Do not** append `<Co> (YYYY-MM)` to any story's `Used for` cell — that's `/build-story-bank`. If the user wants the usage logged, suggest running `/build-story-bank`.

---

## What this skill does NOT do

- **Does not write to or rewrite the story bank.** Read-only. `/build-story-bank` owns it.
- **Does not invent stories or metrics.** Gaps route to `/build-story-bank`; missing facts get asked.
- **Does not use offer-stage negotiation frameworks.** The salary card is the pre-offer deflect only; counters/competing-offer scripts activate after an offer.
- **Does not edit the CV, cover letter, JD, or salary-deflect script.** Those are owned by their own skills.
- **Does not produce a long-form interview-prep deep-dive** — this skill makes the spoken cheatsheet only.
- **Does not run the research step.** If the research gate fails, it halts and instructs.

## Defensibility one-liner

Every spoken beat in a generated cheatsheet traces to the story bank, the CV/cover letter, the company research note, or a stated fact — and every behavioral answer carries a Story ID that exists in the bank. If a beat can't be traced or a Story ID doesn't resolve, the cheatsheet is in a broken state and must be reconciled before it's handed to the user. No exceptions.
