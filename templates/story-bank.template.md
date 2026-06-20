# Story Bank (template / schema showcase)

> **Synthetic showcase data.** The example rows below are invented for the fictional persona **Alex Rivera** (see `cv-master.md` / repo README). This file documents the *schema* of the STAR+R interview story bank — not a real bank. Replace the rows with your own defensible stories to use the system.

> **This is a schema reference, not the live bank.** The real story bank is **cross-application and long-lived**, so it lives in your personal **notes store** (a markdown vault outside this repo), at `<store_root>/story-bank.md` — never inside `templates/`. The `build-story-bank` skill resolves the store root from your own config and writes there via tempfile-rename. This template just shows the structure so the schema is visible in the repo without exposing real interview prep.

---

## What the bank is for

Every behavioral interview question ("tell me about a time when…") should resolve to a numbered **Story ID** in the bank — tagged, scoped, and traceable back to a specific `B-*` bullet in `cv-master.md`. The bank is the single artifact that survives across applications; per-application CVs and cover letters are produced from the repo's pools, but interview prep is store-native because it spans the whole job-search arc.

---

## Schema (locked)

One Markdown table, these columns in this order:

| Story ID | Tags | Situation | Task | Action | Result | Reflection | Source bullets | Used for |
|---|---|---|---|---|---|---|---|---|

### Field rules

- **Story ID:** `S-NNN`, zero-padded. New stories take the next free ID. Never reuse retired IDs — leave a gap and append `(retired YYYY-MM-DD)` to the retired row's Reflection cell.
- **Tags:** dot-separated, drawn only from the closed taxonomy below. 1–4 tags per story. Don't invent tags inline; promote a new tag via a separate edit to the taxonomy line.
- **Situation:** 2–3 sentences. Backdrop only — no self-action yet.
- **Task:** what was specifically on *you*, not the team. Interviewers probe this most — avoid the team-we.
- **Action:** numbered steps if multi-phase. The richest field. Specific enough that no one else on the team could have done it.
- **Result:** falsifiable metric, named outcome, or stated qualitative shift. Reuse the exact language/number from `cv-master.md` — same number on the CV and in the bank means consistency under cross-check.
- **Reflection:** one-liner. What you learned, what didn't work, or what you'd do differently. Pre-answers the "what would you change?" follow-up.
- **Source bullets:** comma-separated `B-*` IDs from `cv-master.md`. Every story backs at least one CV bullet; every CV bullet should back at least one story.
- **Used for:** comma-separated `<Company> (YYYY-MM)` pairs. Append-only; never overwrite. The pattern over time shows which stories get pulled most and which sit unused.

### Closed tag taxonomy

```
leadership · conflict · scaling · ai-adoption · community-program · creator-ops · events · pm · ambiguity · stakeholder
```

### Story ID convention

`S-NNN` zero-padded, assigned in creation order, never reused. A `STUB` tag marks a deferred placeholder row (cover-letter prose pasted into Action, awaiting a full STAR+R interview pass).

---

## Example rows (synthetic — Alex Rivera)

| Story ID | Tags | Situation | Task | Action | Result | Reflection | Source bullets | Used for |
|---|---|---|---|---|---|---|---|---|
| S-001 | events · creator-ops · stakeholder | The Skybound Pro League's 2023 World Finals in Lisbon ran as a 5-day VIP program for 80+ players and partnered creators from 12+ countries, with daily programming and a contracted-creator content framework. As the EU community owner I was on-site lead for the creator track. | Own on-site community execution end to end: VIP communications, daily activity programming, vendor coordination, and creator-specific content briefings — with no second on-the-ground producer to fall back on. | 1) Pre-built a per-creator briefing pack mapping each of 30 contracted creators to their deliverables. 2) Ran welcome party, team-building day, and aftershow logistics against a fixed broadcast schedule. 3) Coordinated vendors and VIP comms in real time across the broadcast week. | 5-day program delivered for 80+ VIPs; the contracted-creator framework produced 250+ Instagram posts, 600+ stories, and 30+ pre-event Twitch promo streams across the event window. | A single shared briefing doc per creator removed almost all day-of questions; next time I'd build it one week earlier to give creators more lead time. | B-PM-05, P-SWC25 | Lumen AI (2026-05) |
| S-002 | ai-adoption · scaling · pm | At Northwind Interactive the 6-person distributed EU community team was spending hours each week on manual reporting, content drafting, and sentiment synthesis, with no shared tooling standard. | Define and ship the team's AI/automation strategy and the guardrails that keep generated output safe before it reaches external audiences. | 1) Defined the no-code/automation toolchain and a shared knowledge base of tools, prompts, and compliance SOPs. 2) Shipped 15+ role-specific assistants, each gated by a human-in-the-loop checkpoint. 3) Authored the SOPs and extended the pattern to the HQ Globalization team. | Reclaimed ~8 hours/week per person (~1.5 FTE across the team); the community intelligence pipeline ran 6+ months in production with zero manual intervention. | Guardrails-first adoption beat speed-first — leading with the human-in-the-loop checkpoint is what got other teams to trust and reuse the assistants. | B-PM-01, B-PM-03b, B-PM-12 | Lumen AI (2026-05) |

---

## Maintenance reminders

- **Stub sweep:** expand any `STUB`-tagged rows before adding new ones.
- **Gap sweep:** cross-reference rows against `cv-master.md` `B-*` IDs; surface bullets with no backing story.
- **Result-consistency sweep:** spot-check Result cells against the linked CV bullet's metric — same number must appear in both places.
- **Tag balance:** surface tags with 0–1 stories (under-prepared for that question type) and tags with 8+ (room to consolidate).

## Defensibility one-liner

Every Story ID must trace to at least one `B-*` bullet in `cv-master.md` and obey the closed tag taxonomy; every Result cell must match the metric on the linked CV bullet. A row that violates either rule puts the bank in a broken state that the next maintenance pass must reconcile before adding new rows.
