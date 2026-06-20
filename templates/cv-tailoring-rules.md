# CV Tailoring Rules

> Consumed by the `/tailor-cv` skill. Mechanical rules for projecting `cv-master.md` (the long-form fact pool) onto a tailored, per-application CV via placeholder substitution against `cv-template.html`. Tailoring is **selection + ordering**, never rewriting.
>
> **Synthetic showcase data.** Every company, number, and example below belongs to the fictional persona **Alex Rivera** (see `cv-master.md`). They illustrate the *engineering* of the tailoring system, not a real résumé.

---

## Placeholder substitution model

`cv-template.html` is a placeholder shell, not a render-able CV. It contains `{{TOKEN}}` placeholders that `/tailor-cv` substitutes with rendered HTML built from picked stable IDs in `cv-master.md`. The full placeholder reference lives in the comment block at the top of `cv-template.html` — that comment is the contract between the master pool and the template, and must stay in sync when either side changes.

### Substitution table

| Placeholder | Source pool | Render template |
|---|---|---|
| `{{DATE}}` | Set by `/tailor-cv` at run time | Plain text (e.g., `May 2026`) |
| `{{ROLE_SUB}}` | Picked `RS-*` from cv-master | Plain text role-sub string |
| `{{PROFILE_PARAGRAPH}}` | Picked `L-*` → `**Profile:**` line | Plain text into `<p class="profile">` |
| `{{OPEN_LINE}}` | Picked `L-*` → `**Open:**` line | Plain text into `<p class="open">` |
| `{{BULLETS_PM_2025}}` | Picked `B-PM-*` IDs | `<li>{prose with <strong>numbers</strong>}</li>` per bullet |
| `{{BULLETS_SCM_2023_2024}}` | Picked `B-SCM-*` IDs | Same `<li>` template |
| `{{BULLETS_CM_2018_2022}}` | Picked `B-CM-*` IDs | Same `<li>` template |
| `{{BULLETS_DREPT}}` | Picked `B-DREPT-*` IDs | Same `<li>` template; drop the wrapping `<article class="job">` if the label entry is cut entirely |
| `{{CERTS}}` | Picked `C-*` IDs | `<div class="cert">{cert label}</div>` per cert |
| `{{HIGHLIGHTS}}` | Picked `H-*` IDs | `<div class="hl-row"><span class="hl-val">{Val}</span><span class="hl-lbl">{Lbl}</span></div>` per highlight |
| `{{SKILLS}}` | Picked `S-*` IDs | `<div class="skill-item"><h4>{heading}</h4><p>{desc}</p></div>` per group |
| `{{TOOLS_HEADING}}` | Derived from picked role-sub track (§ Tools heading rule) | Plain text: `Tech Stack` (Track B) or `Tools` (Track A) |
| `{{TOOLS}}` | Picked `T-*` IDs | `<div class="tool-item"><div class="lbl">{heading}</div><div class="val">{tools}</div></div>` per group |
| `{{SELECTED_PROJECTS}}` | Picked `P-*` IDs | `<div class="skill-item"><h4>{project name}</h4><p>{description}</p></div>` per project |

> The `{{BULLETS_*}}` token names are frozen identifiers (the year suffix is part of the token string, not live data); the rendered job-period labels follow the persona dates in `cv-master.md` (see § Section ordering).

### Picks tracking

Every substitution is recorded in `_cv-checkpoint.md` under the `picks:` field by stable ID — never by re-pasting prose. Example:

```
picks:
  RS: RS-COM
  L: L-COM
  bullets_pm_2025: [B-PM-04, B-PM-05, B-PM-01, B-PM-02]
  bullets_scm: [B-SCM-01, B-SCM-02, B-SCM-04, B-SCM-03, B-SCM-06]
  bullets_cm: [B-CM-01, B-CM-02]
  bullets_drept: [B-DREPT-03]
  certs: [C-AI-LIT, C-AI-FUND, C-AI-AUTO]
  highlights: [H-01, H-02, H-04, H-05]
  skills: [S-COMOPS, S-COMMS, S-PM, S-AIOPS]
  tools: [T-COM, T-PM, T-AI]
  tools_heading: Tools
  projects: [P-LOY, P-SWC25, P-INFTRACK]
```

### The template is skill-only

`cv-template.html` is not openable as a standalone CV — placeholders render as literal text. **All tailored CVs must be produced by `/tailor-cv` substituting against the template.** Hand-editing the template body is a deliberate template revision, not a per-application action.

---

## JD-strand tags (canonical)

Five strands. Every pickable item in `cv-master.md` carries one or more of these. Tag names are frozen.

- `#community` — community ownership, creator programs, multi-platform engagement, sentiment, voice-of-customer
- `#events` — on-site event production, VIP / creator hosting, IP crossovers, esports / live ops
- `#pm` — project / program management, cross-functional coordination, rollouts, stakeholder management, SOPs
- `#ai-ops` — automation, AI workflows, self-hosted / no-code automation, AI assistants, intelligence pipelines, human-in-the-loop
- `#social` — social-media strategy and execution, channel-level growth, content calendar, paid + organic mix

A JD lands on one **primary** strand (the spine of the role) and 0-2 **secondary** strands (supporting tags). The tailored CV picks primarily from the primary strand, fills with secondary, and skips items tagged only in unrelated strands.

---

## Role-sub ↔ Profile lead-noun mirror rule

The `RS-*` (role-sub line under the contact block) **must mirror** the `L-*` (Profile lead-noun) picked from `cv-master.md`. Mismatched pairs are a defensibility check failure.

| JD primary strand | RS pick | L pick |
|---|---|---|
| `#pm` (PM/Program Manager, AI-adjacent) | RS-PM | L-PM |
| `#ai-ops` (AI-forward company, but PM/community-led JD) | RS-PM | L-PM |
| `#ai-ops` (explicit AI-builder / AI-enablement role) | RS-PM-AIOPS | L-PM-AI |
| `#community` (Senior Community Manager, Community Lead) | RS-COM | L-COM |
| `#events` (Senior Events Manager, on-strand events) | RS-COM | L-COM |
| `#social` (Senior Social Media Manager) | RS-SOC | L-SOC |
| `#social` + `#community` (blended Social + Community Lead) | RS-SOCCOM | L-SOCCOM |

> **No mixing.** Picking RS-PM with L-COM (or vice versa) breaks the mirror — defensibility check 3 will fail.

---

## AI-ops positioning exception (community-first override)

The default positioning rule is **community-first**: lead with community / events / creator-ops; AI is one supporting strand, never the spine (see the project's positioning rule). This holds for every CV **except** the narrow case below.

**When the exception fires** — all three must be true:
1. The JD's **primary** strand is `#ai-ops`, **and**
2. The role is explicitly an **AI-building / AI-enablement** role — title or body signals like "AI Operations Associate/PM", "AI Program Manager", "act as a builder", "build and deploy AI solutions", "drive AI adoption across teams", **and**
3. The JD treats coding as optional and AI-tool fluency as the core requirement (i.e., it wants a builder-operator, not an ML engineer).

**What changes when it fires:**
- Pick **RS-PM-AIOPS** ("Project Manager (AI Operations)") + **L-PM-AI** (AI-builder spine; community demoted to the "where I learned where automation pays off" bridge clause). The 2024 Experience job-role heading also renders "Project Manager (AI Operations)" to match the role-sub. This "(AI Operations)" parenthetical is the candidate's owned framing of an AI-operations remit (real title: "Project Manager (Community Operations)"), **not** a fabrication — never flag it on AI-builder roles. Applies to the whole AI-ops track; the plain-title RS-PM-AI is deprecated. (Extended from AI-Operations-titled roles to all AI-builder roles.)
- Lead the Experience bullets with the AI-building set: **B-PM-11** (agentic coding assistant as build platform), **B-PM-12** (cross-team adoption, HQ Globalization), **B-PM-01b** (AI strategy), **B-PM-02b** (intelligence pipeline w/ traceability), **B-PM-03b** (≈1.5 FTE reclaim). Community/events bullets become the *closing* context, not the opener.
- Highlights lead with `#ai-ops`: **H-07** (15+ assistants), **H-08b** (≈1.5 FTE), **H-13** (40+ team AI-enabled).
- Selected Projects lead with the build artifacts: **P-AIWIKI**, **P-MORN**, **P-INFTRACK**, **P-OPSSTACK** (Second Brain) — the personal-system project is high-signal here because it proves native day-to-day fluency.
- Tools lead with **T-AI**; **T-COM** trims hard to the few tools the role actually touches. The Tools sidebar heading renders **"Tech Stack"** (Track B label — see § Tools heading rule).

**When in doubt, do NOT fire it.** A merely AI-forward company with a community/PM-led JD stays community-first (RS-PM / L-PM). First codified after an AI Operations Associate role at an AI-forward company — the `/tailor-cv` auto-draft correctly applied community-first; the AI-builder inversion was a deliberate manual override for a genuine AI-builder JD.

---

## Tools heading rule

The `{{TOOLS_HEADING}}` sidebar label is **strand-conditional**, keyed to the picked role-sub track (one decision, derived — not a separate pick to litigate):

| Picked role-sub | Track | `{{TOOLS_HEADING}}` |
|---|---|---|
| `RS-PM`, `RS-PM-AIOPS` | B — PM / AI-ops | **`Tech Stack`** |
| `RS-COM`, `RS-SOC`, `RS-SOCCOM` | A — community / social | **`Tools`** |

**Why:** on PM / AI-ops CVs the toolchain (self-hosted automation, an agentic coding assistant, Docker, tool integrations) is a differentiator, so "Tech Stack" signals technical depth; on community / social CVs the same block is supporting detail, so the plainer "Tools" fits and avoids over-signalling engineering. The label is derived, never chosen independently — it always follows the role-sub, so it cannot disagree with the section's content ordering. Record the resolved string in `_cv-checkpoint.md` as `tools_heading`.

---

## Open-line rule (drop on targeted applications)

The `{{OPEN_LINE}}` ("Looking for … roles in Amsterdam or EU remote") reads as redundant — even slightly off — on a CV submitted directly to a **specific posted role**: the application itself states the intent. **Drop `{{OPEN_LINE}}` by default on any targeted application.** Keep it only for speculative / networking / open-application CVs where there is no specific JD. The `L-*` entries retain their Open strings for that speculative use; this rule governs tailoring-time inclusion, not the pool.

---

## Pick counts per pool

> Approximate ceilings — the 2-page A4 fit gate (check 6) is the hard cap. If a strand legitimately demands more, `/tailor-cv` asks the user which to drop.

| Pool | Pick count | Notes |
|---|---|---|
| Bullets per role (PM 2024-, SCM 2022-2023) | 4-6 each | These two roles are the spine; cap by 2-page fit |
| Bullets per role (CM 2019-2021) | 2-3 | Period-bounded, less load-bearing |
| Bullets per role (DREPT/label) | 1 (B-DREPT-03, consolidated) by default | Single consolidated bullet is the standard; drop only when 2-page A4 fit forces it |
| Selected Projects | 2-4 | Order by JD strand priority |
| Highlights (sidebar) | 4 | Match JD strand |
| Certifications | 3 default, 4-5 if JD aligns | C-AI-LIT + C-AI-FUND + C-AI-AUTO is the default trio |
| Tools groups | 3 (all groups) | Reorder by JD strand priority; drop individual tools irrelevant to strand |
| Skills groups | All 4 (default-on) | **First block to cut under 2-page pressure** — see drop order below |

---

## Relevance ranking (pick · order · cut)

The **same ordinal ranking** drives three decisions: which pool entries to **pick** (Step 7), how to **order** items within a section, and which to **cut** under 2-page overflow (check 6). It is a *ranking*, not a numeric score — rank the candidates in a pool, then take / order / drop by rank. **Never attach a fabricated point value.**

Rank each candidate within its pool by three factors, in priority order:

1. **JD-strand relevance** — does the entry hit the JD's primary (then secondary) strand and its keywords? Rank by how *directly* it matches: an entry that names a JD requirement near-verbatim outranks one that is merely on-strand. **Cite the JD line(s) each top pick matches** — the relevance claim must trace to JD text, never a vibe. Relevance, not recency, is the spine: a bullet from an *older* role that hits the JD keywords outranks a *recent*-role bullet that doesn't.
2. **Uniqueness** — does a higher-ranked pick already carry this claim or locked number? An entry whose signal is duplicated by something already picked drops in rank; an entry carrying a claim/number that appears nowhere else rises.
3. **Cover-letter dependency** — does the tailored cover letter cite this entry? If yes it rises (cutting it orphans a cover-letter claim). **Inert on the first CV draft** (the cover letter doesn't exist yet — CV precedes cover letter); applies only on a re-cut *after* `/tailor-cover-letter` has run.

**Before ranking — resolve variants.** Several pool entries are mutually-exclusive phrasing variants of one claim (e.g. `B-PM-01`/`01b`/`01c`, `B-PM-03`/`03b`, `H-08`/`H-08b`). Pick the **single** variant whose `cv-master.md` annotation best fits the JD *first*, then rank that one representative. Never rank two variants of the same claim against each other, and never ship both.

**Precedence when ranking the eligible set.** Eligibility is primary-**OR**-secondary strand. Within the eligible set, **directness of JD match is the top factor** — a secondary-strand entry that names a JD requirement near-verbatim outranks a primary-strand entry that only matches loosely. That is the whole point of ranking: sharp beats generically-on-strand. Only when two candidates match the JD with *comparable* directness does the **primary-strand** one rank higher, as the tiebreak.

**Where each use applies:**
- **Pick (Step 7):** within each pool, rank all candidates carrying the JD primary or secondary strand; take the top-N per the pick counts above.
- **Order:** within each section / placeholder, emit items in rank order (highest first) so JD-relevant signal leads. Exact *mid-section* order is approximate — the extremes are stable (the lead item, the clearly-off-strand drop), and the Step 10b section review is where final order locks, so small rank ties are not litigated mechanically.
- **Cut (check 6 overflow):** when the block drop order forces a choice *among bullets within a block*, drop the lowest-ranked first; on a tie, the off-primary-strand line goes. This refines, never overrides, the block drop order.

(Adapted from a relevance-weighted CV-cutting approach; promoted from cut-only to pick + order + cut to fix JD-blind selection at pick time.)

---

## 2-page fit — drop order

When picks overflow 2-page A4 (defensibility check 6 fails), trim in this sequence until it fits. **Never silently auto-drop** — `/tailor-cv` Step 8 still asks the user which to cut; this is the recommended order:

1. **Skills block (`{{SKILLS}}` / all `S-*` groups) — first to cut.** Default-on, but the first sacrifice for space: the sidebar Selected Projects block covers the same competencies with more specificity, so Skills is the most redundant/compressible block. (Locked after a consumer-brand application where Skills was dropped and Selected Projects kept.)
2. DREPT/label bullets (`B-DREPT-*`).
3. Lowest-priority CM 2019-2021 bullet.
4. Lowest-priority SCM / PM bullet that sits off the JD primary strand.
5. A Selected Project (last resort — these carry the most JD-specific signal).

### Line-level cut ranking — defining "lowest-priority"

Steps 3-4 say "lowest-priority bullet" but don't define it: when the drop order forces a choice *among individual bullets within a block*, rank the candidates per §"Relevance ranking (pick · order · cut)" above and cut the **lowest-ranked** line first; on a tie, the off-primary-strand line goes. This *refines, never overrides*, the block drop order — Skills still goes before DREPT before CM bullets; the ranking only decides *which* bullet inside the block currently being trimmed.

---

## Section ordering (locked by template)

Section order is baked into `cv-template.html` and **not configurable per-application**. Tailoring overrides the contents of each section, never their position.

**Body (left column):**

1. Identity + role-sub + contact block (header, locked)
2. Profile (Section 01) — `{{PROFILE_PARAGRAPH}}` + `{{OPEN_LINE}}`
3. Experience (Section 02) — chronological: PM 2024-Present → SCM 2022-2023 → CM 2019-2021 → label
4. Education & Certifications (Section 03) — locked Education row + `{{CERTS}}`

**Sidebar (right column):**

1. Portrait
2. Highlights — `{{HIGHLIGHTS}}`
3. Skills — `{{SKILLS}}`
4. Tools — `{{TOOLS}}`
5. Languages (locked)
6. Selected Projects — `{{SELECTED_PROJECTS}}`

> Reordering requires a deliberate template revision (edit `cv-template.html` body structure), not a per-application choice. Within each section, item order *within* a placeholder follows JD strand priority — that's the configurable surface.

---

## Locked-number-must-match rule

Every `<strong>`-wrapped number in the tailored CV must appear **verbatim** in the Locked numbers registry at the top of `cv-master.md`. If a tailored bullet needs a number not in the registry, the bullet does not get written — escalate to the user instead.

Any metric changed or newly framed during tailoring (rescaled, re-expressed, aggregated, unit-converted) must be added to the Locked numbers registry **in the same pass**, with its derivation noted — never left floating only on the application CV. Codified after a tailored-CV reclaim figure was bumped 8 h/wk → 48 man-hours/wk (≈1.5 FTE) without a registry write-back.

Period-bounded entries in the registry (e.g., "Live streams on Twitch and YouTube (2019-2021)", "Influencer-led campaign Twitch viewership lift (2019-2021, pre-Program)") must carry their period in the bullet. Standing claims (no period bracket in the registry header) can be cited without a period.

---

## Pre-flight defensibility checklist (7 checks)

Run as a hard gate before declaring tailoring complete. Check 5 spawns a **cheap fast model** sub-agent; the rest are skill self-checks.

> **Canonical detail lives in `tailor-cv/SKILL.md` Step 11.** This list is a summary; on any drift, SKILL.md wins.

1. **Trace check** — every claim in the tailored CV traces to one line in `cv-master.md` (a bullet, project, highlight, cert, tool, or registry entry). No orphan claims.
2. **Number check** — every `<strong>`-wrapped number matches the Locked numbers registry verbatim. Period-bounded numbers carry their period.
3. **Mirror check** — the picked role-sub (RS-*) mirrors the picked Profile lead-noun (L-*) per the table above.
4. **Strand alignment check** — the picked Selected Projects, Highlights, and role-sub all point at the same primary JD strand. (Secondary strands among the projects/highlights are fine; the *primary* strand must be consistent.)
5. **Out-of-pool prose check (mechanical)** — every bullet, project description, and highlight in the draft appears in `cv-master.md` (allowing only grammar-pass edits per check 6's grammar prompt). Mechanical string match: tailored prose must be a recognisable derivative of a master entry, not a fresh composition.
6. **2-page A4 fit check** — the rendered HTML at A4 fits in 2 pages with the locked CSS. No content cut off; no awkward orphaned headings.
7. **JD coverage tally** — the rendered CV addresses ≥75% of distinct JD requirements with ≤2 gaps and no stop-list gap. Mechanical tally (D/F/G). On fail, the skill first attempts a **bounded in-pool auto-resolve** (swap the highest-ranked pool gap-closer, re-run, cap 3) before escalating to the user. Full procedure in `tailor-cv/SKILL.md` Step 11 Check 7 (canonical).

> **Check 5 — cheap-fast-model sub-agent verification:** spawn with prompt: *"Read `<draft.html>` and `<cv-master.md>`. Flag any prose in the draft that doesn't appear verbatim (or as a clear grammar-only edit) in the master pool. Return JSON: `{verdict: pass|fail, flagged: [{line: '<excerpt>', reason: '<why>'}]}`."* This catches paraphrases that the mechanical string match would miss.

**Gate-fail behaviour:** the skill writes the draft HTML regardless, but withholds the "tailoring complete" message and the grammar-pass prompt until the gate passes. On fail, output: `Draft written to <path>. Defensibility gate failed: [list]. Resolve and re-run gate, or /cv-checkpoint and pause.`

---

## Grammar-pass prompt (locked string)

After gate pass, the workflow's last instruction is one line printed back to the user. The grammar-pass prompt is a locked string; the path is templated in at print time from the application folder + filename.

```
CV draft complete at <absolute_path>. Recommend /clear and run this in a fresh context: Read the CV at <absolute_path>. Scan it thoroughly for grammar errors and potential changes that would increase the readability of the CV. Only focus on the grammar and sentence structure, not on context. Do not suggest any changes unless you think it is necessary.
```

> **Why locked:** the grammar pass is a separate-context check by design. Same-context grammar review tends to skip past errors the writing-context AI assistant already justified. A fresh-context read with no JD-tailoring memory catches more.

---

## Learnings threshold

```
THRESHOLD = 8
```

Active promotable entries in `cv-learnings.md` (entries classified as `phrasing-preference` or `strand-rule` in the `## Active (unpromoted)` section). Typo and JD-specific entries don't count.

When `count >= THRESHOLD`:
- At end of `/tailor-cv` reflection step: `📋 cv-learnings.md has N promotable entries (threshold: 8). Run /cv-promote-learnings now to merge into cv-master, or skip and I'll prompt at the start of the next CV.`
- At start of next `/tailor-cv` (Step 6.5 threshold pre-check): same prompt, soft escalation, not a hard block.

**Calibrate after 3-4 applications:** if 8 is too chatty, raise to 10. If it never trips, lower to 5.

---

## Tailoring for non-gaming / off-domain employers

When the target employer is outside gaming/esports (consumer brands, generic SaaS, non-software), apply these adjustments on top of normal strand selection. **Gaming/esports employers use the pool as-is — none of this applies.** (First codified after the first consumer-brand application.)

### Vocabulary and proper-name neutralization (de-gaming)

The **one sanctioned exception** to "selection + ordering, never rewriting": 1:1 vocabulary swaps and proper-name generalizations that preserve every locked number and the underlying claim. Never introduce a new claim. The master pool always holds the gaming-native canonical prose; this neutralization happens at tailoring time only.

**Vocabulary substitutions:**

| Gaming-native (master canonical) | Neutralised (non-gaming output) |
|---|---|
| global esports league | global championship |
| players / players and creators | competitors / competitors and creators |
| player sentiment | community sentiment |
| new-player friction and veteran edge cases | new and long-time users |
| patches | releases |
| meta analyses, balance and patch reports | sentiment digests, release-feedback reports |
| in-game rewards | rewards |
| Twitch and YouTube metrics | creator metrics |
| pre-event Twitch promo streams | pre-event promo streams |

**Proper-name generalization:** drop named gaming agencies/vendors → "agency partnerships"; collapse long-form event names to acronym on first use ("Skybound Pro League … World Finals" → "Skybound Pro League … Finals (SPL)").

Keep the numbers; neutralise the vocabulary.

**Live-ops scope anchor (retain for C7).** Keep the neutralised "**4 regions** / **12 languages**" live-delivery scope in the profile/experience prose (B-SCM-04 and the profile variants) even when H-05 (90K concurrent) is dropped per Highlight preference below. These two numbers are the cross-check anchor for the cover letter's `BRICK-LIVEOPS-SCALE` (the label-stripped live-ops competency brick): if the de-gamed CV drops them entirely, the cover letter's live-ops claim becomes an orphan metric and C7 hard-blocks the render. "4 regions / 12 languages" is domain-neutral once "global esports league → global championship" is applied, so it survives de-gaming cleanly — the 90K / concurrent-viewer figures are the only part that must go.

### Highlight preference

Prefer universal community/social highlights over esports-broadcast-specific ones:
- **Lead with:** H-01 (180K+ community), H-02 (30 → 55 Ambassador Program creators), H-11 (15+ offline events in 7 countries), H-12 (25% YoY engagement growth).
- **Drop:** H-05 (90K peak concurrent viewers) and H-06 (6 / 0 IP crossovers / compliance) — esports-broadcast metrics that read as off-domain.

### Certification ordering

Lead with the general/AI-forward certs (C-AI-AUTO, C-AI-FUND, C-GENAI-LENS) and place the gaming-specific C-AI-LIT (AI Literacy in the Games Industry) last. Keep it — still real and AI-relevant — but de-prioritise.

---

## Things this file does *not* govern

- **Cover letter tailoring.** That's `cover-letter-base.md` + the cover-letter section of the project rules. CV-only logic here.
- **JD-strand classification logic.** Out of scope per the plan. Operator decides which strand a JD lands on; the rules here only kick in once the strand is named.
- **CV visual design / CSS / HTML chrome.** That's `cv-template.html`. This file governs content selection and placeholder substitution; the template governs rendering.
- **Already-shipped CVs.** Earlier shipped applications stay frozen. Updates apply only to future tailorings.
