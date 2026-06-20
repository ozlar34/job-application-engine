# templates/

The **fact pools, tailoring rules, and HTML shells** the crafting skills in `skills/` read at runtime. Tailoring is **selection + ordering** out of these pools, never freehand rewriting — that is what makes every line in a generated CV or cover letter trace back to a defensible source entry.

## Synthetic data disclaimer

Every name, number, company, and product in these files is **invented** for a fictional persona, **Alex Rivera** — a generic Community / Operations / AI-adoption lead in Amsterdam. None of it is real. The point of the templates is to show the *structure* of the system (stable IDs, JD-strand tags, defensibility gates, learnings loops), not to publish a résumé. To use the engine for real, replace the persona identity block and pool entries with your own defensible facts and keep the scaffolding.

## What's here

| File | Role |
|---|---|
| `cv-master.md` | Long-form CV fact pool — every defensible claim, with stable IDs, JD-strand tags, and a locked-numbers registry. |
| `cv-tailoring-rules.md` | How `/tailor-cv` projects the pool onto one application: strand mapping, relevance ranking, 2-page fit, 7-check defensibility gate. |
| `cv-template.html` | Placeholder HTML shell (`{{TOKENS}}`) the skill substitutes into; print-to-PDF for the final artifact. |
| `cv-learnings.md` | Append-only log of phrasing/strand learnings; promoted into the pool via `/cv-promote-learnings` at a threshold. |
| `cover-letter-base.md` / `cover-letter-pool.md` / `cover-letter-tailoring-rules.md` | Cover-letter equivalents — reusable "bricks," voice/structure rules, and the cover-letter defensibility gate. |
| `cover-letter-template.html` | Placeholder HTML shell for the cover letter. |
| `cover-letter-voice-profile.md` | Voice/tone contract and banned-phrase list. |
| `cover-letter-learnings.md` | Cover-letter learnings log. |
| `positioning-statement.md` / `positioning-learnings.md` | The two-track positioning variants (Community/Social/Ops vs PM/Program at AI-forward cos) and their learnings. |
| `interview-cheatsheet-template.html` | Placeholder HTML shell for the per-application interview cheatsheet. |
| `story-bank.template.md` | Schema-only sample of the STAR+R interview story bank (the live bank is store-native, outside this repo). |

> `portrait.png` is referenced by the HTML shells but intentionally **not committed** (gitignored) — supply your own.
