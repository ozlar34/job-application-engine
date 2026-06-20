---
name: tailor-cv
description: Tailor a CV from cv-master.md to a per-application folder, enforcing the JD-strand selection workflow, placeholder substitution against cv-template.html, a collaborative section-by-section review, and the 7-check defensibility gate before output. Use when the user says "/tailor-cv", "tailor a cv for <co>", "start the cv for <co>", or shares a JD with intent to apply.
triggers:
  - /tailor-cv
  - tailor a cv
  - tailor the cv
  - start the cv for
  - draft a cv for
  - cv for <company>
---

# Tailor CV Skill

The CV tailoring workflow as a mechanical skill, not a documented procedure. `cv-master.md` and `cv-tailoring-rules.md` stay as data files; this skill is the workflow that consumes them.

**Why a skill, not docs:** the failure that triggered this design was a documentation-execution drift problem — the rules existed as prose, but the assistant didn't apply them all. This skill turns the workflow into mechanical gates that can't be skipped:

- Forced load of `cv-master.md` + `cv-tailoring-rules.md` + `cv-learnings.md` before any draft
- Ordered strand → role-sub → Profile lead-noun selection (mirror rule enforced)
- **Relevance-ranked pick** from each pool with JD-line citations — not tag-match-then-arbitrary-count — so JD-relevant material is picked and front-loaded at draft time
- **Critic pre-pass** (a cheap fast model) warm-starts the section review with grounded per-section flags; flat-but-relevant bullets route to a pool-polish queue
- Section-by-section collaborative review of the rendered draft before the gate — the user reviews each section alongside the assistant (default-on norm)
- Hard-gate defensibility checklist — no "tailoring complete" message until all 7 checks pass; a **bounded in-pool auto-resolve** closes coverage gaps before escalating to the user
- Auto-templated grammar-pass prompt with the absolute path baked in

**Sibling to `/cv-checkpoint`.** Both operate on the same `applications/YYYY-MM-DD-<co>/` folder convention. `/tailor-cv` produces state; `/cv-checkpoint` snapshots it.

**Sibling to `/cv-promote-learnings`.** This skill reads `cv-learnings.md` as advisory hints (never authoritative); the post-finalization reflection step inside this skill writes new entries to `cv-learnings.md`; `/cv-promote-learnings` is the manual gate that moves durable patterns into `cv-master.md` and shrinks the active backlog.

## Paths

These resolve against the repo's `templates/` and `lib/` directories. Adapt the `Applications root` to wherever you keep working application folders locally (this directory holds real personal output and is typically kept out of version control).

```
Master pool       : templates/cv-master.md
Tailoring rules   : templates/cv-tailoring-rules.md
Learnings sidecar : templates/cv-learnings.md
Template          : templates/cv-template.html
Portrait          : templates/portrait.png        (your headshot; supply your own)
Applications root : applications/                  (local working dir, untracked)
```

---

## Flow

### Step 1 — Force-load the three data files

Read in this exact order, before doing anything else:

1. `cv-master.md` — full file. The pool of every defensible bullet, project, highlight, cert, tool, skill.
2. `cv-tailoring-rules.md` — full file. Substitution table, mirror rule, pick counts, defensibility checklist, grammar prompt, threshold.
3. `cv-learnings.md` — advisory hints from past tailorings. **Read but treat as advisory only**, never authoritative; the locked content is `cv-master.md`.

If any of the three files is missing, halt with: *"`<filename>` missing — required for tailoring. Cannot proceed."*

### Step 2 — Threshold pre-check + Folder gate

Follow the shared tailoring routine from `lib/tailoring-common.md` (threshold pre-check and folder gate — tailor-cv branch).

For the threshold pre-check: learnings-file = `cv-learnings.md`; rules-file = `cv-tailoring-rules.md`; promotable types = `phrasing-preference`, `strand-rule`.

For the folder gate: identify company from the user's invocation; propose `applications/<today>-<company-slug>/`; confirm before creating; on approval: `mkdir -p` the folder, copy `cv-template.html` → `<folder>/Resume_<Company-Slug-CapsCase>.html`, copy `portrait.png` → `<folder>/portrait.png`.

### Step 4 — Resume gate

Check for an existing `<folder>/_cv-checkpoint.md`.

**If present:** prompt *"Found checkpoint from `<previous_checkpoint_at>`. Resume from it or start fresh? (resume / fresh)"*. If the user picks `resume`:

1. Read the checkpoint file.
2. If `checkpoint_timestamp` is more than 24h old, warn: *"Checkpoint is `<N>` hours old. JD context may have shifted; continue?"*
3. Print the resume summary:
   ```
   Resuming <Company> tailoring.
   Locked: <sections_locked>
   Pending: <pending_sections>
   Open decisions: <open_decisions>
   Defensibility: <defensibility_check_status>
   Continue?
   ```
4. On confirmation, restore `picks` / `sections_locked` / `pending_sections` into working memory and skip ahead to the first unlocked step.

**If absent or the user picks `fresh`:** continue to Step 5.

### Step 5 — JD source

Check for an existing `<folder>/JD.md` or `<folder>/JD.txt`.

**If present:** read it. That's the JD. (This is the common path — a lead-capture step should have already saved it.)

**If absent:** prefer auto-capture over a manual paste. Ask: *"No JD.md yet — paste the JD
URL and I'll fetch it, or paste the JD text directly."*
- **Given a URL** → fetch the page and write the extracted JD text to `<folder>/JD.md`.
- **Fetch fails, or the user pastes text** → write `<folder>/JD.md` with the verbatim text,
  no edits.

**Do not infer the JD from memory or summarize a URL** — verbatim text only.

### Step 6 — JD-strand selection → role-sub → Profile lead-noun

Walk these three picks in order, no skipping. Each is a separate decision; do not collapse them.

#### 6a. JD primary strand

Read the JD. Pick **one** primary strand from the canonical 5 (per `cv-tailoring-rules.md`):
- `#community` — community ownership, creator programs, multi-platform engagement, sentiment, voice-of-customer
- `#events` — on-site event production, VIP / creator hosting, IP crossovers, esports / live ops
- `#pm` — project / program management, cross-functional coordination, rollouts, stakeholder management, SOPs
- `#ai-ops` — automation, AI workflows, AI assistants, intelligence pipelines, human-in-the-loop
- `#social` — social-media strategy and execution, channel-level growth, content calendar, paid + organic mix

Identify 0–2 secondary strands (supporting tags).

State the picks back to the user: *"Primary: `<strand>`. Secondary: `<list>`. Confirm?"*. One-key approve. If the user overrides, take their pick.

#### 6b. Role-sub pick (RS-*)

Apply the mirror table from `cv-tailoring-rules.md`:

| JD primary strand | RS pick |
|---|---|
| `#pm` (PM/Program Manager, AI-adjacent) | RS-PM |
| `#ai-ops` (AI Operations PM, AI-tooling) | RS-PM |
| `#community` | RS-COM |
| `#events` (on-strand events) | RS-COM |
| `#social` | RS-SOC |

State the pick back: *"Role-sub: `<RS-id>` → `<role-sub string>`. Confirm?"*

#### 6c. Profile lead-noun pick (L-*)

Mirror to RS pick. The mirror is non-negotiable — defensibility check 3 fails on mismatch.

| RS | L |
|---|---|
| RS-PM | L-PM |
| RS-COM | L-COM |
| RS-SOC | L-SOC |

**Track B note:** for PM/Program Manager roles where the JD is heavily PM-led but the company is *not* AI-native or AI-forward, use L-PM and trim the AI clause in the second sentence rather than picking a new variant. The PM lead noun must hold.

State the pick back: *"Profile lead-noun: `<L-id>` (mirrors `<RS-id>`). Confirm?"*

### Step 7 — Ranked pick from each pool

For each pool, **rank** the master entries carrying the picked primary (then secondary) strand per the §"Relevance ranking (pick · order · cut)" routine in `cv-tailoring-rules.md`, then take the **top-N by rank** — not tag-match-then-arbitrary-count. Resolve mutually-exclusive variants (e.g. `B-PM-01`/`01b`/`01c`) by JD fit *before* ranking, and let **directness of JD match outrank mere on-strand membership** (both per that routine). Order items within each section by rank (highest first) so JD-relevant signal leads. The ranking is ordinal (a relative ordering of candidates), never a fabricated point value. Pick counts per `cv-tailoring-rules.md`:

| Pool | Pick count | Notes |
|---|---|---|
| `B-PM-*` (most recent role) | 4-6 | Spine; cap by 2-page fit |
| `B-SCM-*` (prior role) | 4-6 | Spine; cap by 2-page fit |
| `B-CM-*` (earlier role) | 2-3 | Period-bounded, less load-bearing |
| `B-EARLY-*` (earliest role / side project) | 1-2 by default | Drop only when 2-page A4 fit forces it |
| `P-*` (Selected Projects) | 2-4 | Order by JD strand priority |
| `H-*` (Highlights) | 4 | Match JD strand |
| `C-*` (Certifications) | 3 default, 4-5 if JD aligns | C-AI-LIT + C-AI-FUND + C-AI-AUTO is the default trio |
| `T-*` (Tools groups) | 3 (all groups) | Reorder by JD strand priority; drop individual tools irrelevant to strand |
| `S-*` (Skills groups) | All 4 | Reorder by JD strand priority |

Record picks **by stable ID, not by re-pasting prose**. Show the full picks block to the user before rendering:

```
Picks:
  RS: <RS-id>
  L: <L-id>
  bullets_pm: [<comma-separated B-PM-* IDs>]
  bullets_scm: [<comma-separated B-SCM-* IDs>]
  bullets_cm: [<comma-separated B-CM-* IDs>]
  bullets_early: [<comma-separated B-EARLY-* IDs>]
  certs: [<comma-separated C-* IDs>]
  highlights: [<comma-separated H-* IDs>]
  skills: [<comma-separated S-* IDs>]
  tools: [<comma-separated T-* IDs>]
  tools_heading: <Tools | Tech Stack — derived from RS track>
  projects: [<comma-separated P-* IDs>]
```

**Rank rationale (audit trail for the ranked pick).** Alongside the picks block, show for each picked bullet, project, and highlight a one-line rationale: the JD line(s) it matched (the relevance citation that ranked it). This lets the user see *why* each entry beat the ones left out, and surfaces a thin-pool case (a slot filled by a low-rank entry with no real JD match) before render. Keep it terse — one line per pick, grouped by pool:

```
Rank rationale:
  B-PM-04  ← JD: "own the EMEA creator program across markets"
  B-PM-05  ← JD: "scale community ops"
  ...
  (left out: B-PM-07 — no JD-strand match this role)
```

Ask: *"Picks look right? (yes / adjust)"*. On adjust, take the user's edits and re-show.

**Special case — SOC-led tailorings:** if there's no `#social`-tagged H-* in the highlights pool, fill the 4 highlight slots from `#community` highlights and scale-stat highlights instead.

### Step 8 — 2-page overflow handling

After picks but before rendering: estimate whether the picks will fit a 2-page A4. The locked CSS in `cv-template.html` produces a 2-page layout when total bullet count is roughly:

- Most-recent-role bullets: ≤ 6
- Prior-role bullets: ≤ 6
- Earlier-role bullets: ≤ 3
- Earliest-role bullets: 1–2 (default included)
- Total bullets across all roles: **~15–17 max**

Plus 2-4 projects, 4 highlights, 3-5 certs, 3 tool groups, 4 skill groups.

If the picks legitimately exceed this, ask:

```
Picks would overflow 2-page A4. Drop options (one or more):
  - Drop earliest-role block entirely (B-EARLY-*)
  - Drop an earlier-role bullet: <list>
  - Drop a prior-role bullet: <list>
  - Drop a most-recent-role bullet: <list>
  - Drop a project: <list>
Which?
```

**No silent auto-dropping.** Always ask. Suggest the earliest-role block first as the soft drop (per cv-master.md "Drop only when the 2-page A4 fit gate forces it").

If `DROP_EARLY` is the chosen drop, set the flag — the renderer must drop both the `{{BULLETS_EARLY}}` content AND the surrounding `<article class="job">` wrapper for the earliest-role block.

### Step 9 — Render the tailored HTML

Read `cv-template.html`. Apply the substitution table from `cv-tailoring-rules.md`:

| Placeholder | Source | Render template |
|---|---|---|
| `{{DATE}}` | Today's date as `Month Year` (e.g., `May 2026`) | Plain text |
| `{{ROLE_SUB}}` | Picked `RS-*` string | Plain text |
| `{{PROFILE_PARAGRAPH}}` | Picked `L-*` → `**Profile:**` paragraph (strip the `**Profile:**` label) | Plain text into `<p class="profile">` |
| `{{OPEN_LINE}}` | Picked `L-*` → `**Open:**` line (strip the `**Open:**` label) | Plain text into `<p class="open">` |
| `{{BULLETS_PM}}` | Picked `B-PM-*` IDs | Concat `<li>{prose with <strong>numbers</strong>}</li>` |
| `{{BULLETS_SCM}}` | Picked `B-SCM-*` IDs | Same `<li>` template |
| `{{BULLETS_CM}}` | Picked `B-CM-*` IDs | Same `<li>` template |
| `{{BULLETS_EARLY}}` | Picked `B-EARLY-*` IDs | Same `<li>` template; if `DROP_EARLY`, substitute empty string AND drop wrapping `<article class="job">` |
| `{{CERTS}}` | Picked `C-*` IDs | Concat `<div class="cert">{cert label}</div>` |
| `{{HIGHLIGHTS}}` | Picked `H-*` IDs | Concat `<div class="hl-row"><span class="hl-val">{Val}</span><span class="hl-lbl">{Lbl}</span></div>` |
| `{{SKILLS}}` | Picked `S-*` IDs | Concat `<div class="skill-item"><h4>{heading}</h4><p>{desc}</p></div>` |
| `{{TOOLS_HEADING}}` | Derived from picked `RS-*` track: `RS-PM` → `Tech Stack`; `RS-COM`/`RS-SOC` → `Tools` (see cv-tailoring-rules.md § Tools heading rule) | Plain text |
| `{{TOOLS}}` | Picked `T-*` IDs | Concat `<div class="tool-item"><div class="lbl">{heading}</div><div class="val">{tools}</div></div>` |
| `{{SELECTED_PROJECTS}}` | Picked `P-*` IDs | Concat `<div class="skill-item"><h4>{project name}</h4><p>{description}</p></div>` |

**Bullet prose rendering:** the master entry is markdown with `**number**` for bold. Convert `**...**` to `<strong>...</strong>`. Keep all other prose verbatim — no edits.

**Number wrapping:** when converting bullets, ensure every number that appears in the Locked numbers registry is wrapped in `<strong>` tags. If a number from the registry appears bare in the master prose (e.g., "2022 →" in a project), wrap it.

**Bullet IDs:** do not include the stable ID (`B-PM-01`, etc.) in the rendered HTML. IDs live only in `cv-master.md` and `_cv-checkpoint.md`.

Write the substituted HTML to `<folder>/Resume_<Company-Slug-CapsCase>.html`.

### Step 10 — Save `_original_draft.html`

Immediately copy the just-written HTML to `<folder>/_original_draft.html`. This is the frozen "raw mechanical render" snapshot, used by the post-finalization reflection step (Step 14) to detect everything the user changed afterward. It is deliberately frozen **before** the Step 10b section-by-section review, so the review's collaborative edits also surface as learning signal in Step 14.

```bash
cp <folder>/Resume_<Company-Slug-CapsCase>.html <folder>/_original_draft.html
```

### Step 10a — Critic annotation pass (cheap fast model, pre-review)

Before the collaborative review, spawn **one** critic sub-agent (a cheap, fast model) to pre-compute the Step 10b per-section flags, so the review becomes *reviewing an annotated draft* rather than generating flags live and cold. **Annotation-only — the critic does NOT edit the draft.** `_original_draft.html` is already frozen (Step 10), so this changes nothing about the Step 14 diff.

Spawn a `general-purpose` sub-agent on a fast/cheap model with this brief:

> Read `<folder>/Resume_<Company-Slug-CapsCase>.html`, `<folder>/JD.md`, and `cv-master.md`. You are a CV critic. For each section (Header, Profile, each Experience block, Education & Certifications, Highlights, Tools, Selected Projects), flag issues and opportunities — every flag grounded in a specific JD line. **In-pool only: never propose new prose; opportunities must cite an existing master ID.** Also flag any *picked* bullet that is highly JD-relevant but reads flat/generic in its current master prose (these cannot be reworded in this CV — the defensibility fence forbids it — so they route to a pool-polish queue). Return JSON:
> ```json
> {"sections": [{"section": "<name>", "issues": [{"text": "...", "jd_line": "..."}], "opportunities": [{"text": "...", "master_id": "<B-*/P-*/H-*/...>", "jd_line": "..."}]}], "flat_flags": [{"master_id": "...", "jd_line": "...", "why": "..."}]}
> ```

On return:

1. Hold the per-section `issues` + `opportunities` in working memory, keyed by section, for Step 10b to lead with.
2. **Pool-polish queue (flat-bullet routing):** if `flat_flags` is non-empty, append each to `cv-learnings.md` under a `## Pool-polish candidates` section (create the section if absent), one line per flag:
   ```
   - <master_id> — read flat vs JD "<jd_line>" (<today>): <why>
   ```
   These are **reference-only** for `/cv-promote-learnings` (the real fix is a `cv-master.md` prose rewrite, out of scope here). They never count toward the learnings promotion threshold and are never auto-applied.

If the critic returns nothing actionable, note that and continue — Step 10b still runs (it is the load-bearing human gate; the critic only warm-starts it).

### Step 10b — Section-by-section review (collaborative gate)

Before the defensibility gate, walk the rendered CV with the user **one section at a time**. This is a content / structure / opportunity review done *together* — distinct from the Step 13 grammar pass (which is mechanical, solo, fresh-context). This review is a norm on every tailoring: eyes on each section alongside the assistant to catch issues and surface improvements before anything locks.

**Default-on.** Offer it every time. The user may decline for a given application (*"skip the review"*) — then continue to Step 11. Do not skip silently, and do not collapse it into the grammar pass.

**Section order** (matches the rendered layout — walk in this order, one at a time, never batch):

1. **Header** — name, role-sub title, contact block
2. **Profile (01)** — profile paragraph + open line
3. **Experience (02)** — walk each job block separately, most-recent → earliest
4. **Education & Certifications (03)**
5. **Sidebar** — Highlights → Tools → Languages → Selected Projects (each separately)

**Per section:**

1. Show the section's current rendered content (the actual prose, not the stable IDs).
2. **Lead with the Step 10a critic's pre-computed flags for this section** (from working memory), then add anything the critic missed. Present as one short list:
   - **Issues** — anything weak, redundant, buried, off-strand for the JD's primary strand, or inconsistent with another section.
   - **Opportunities** — a stronger pool entry available for this slot, a reorder that front-loads JD-relevant signal, a tighter phrasing, a better-matched highlight / tool / project for the JD.
   - Ground every flag in `<folder>/JD.md` and the picked primary strand. If JD research surfaced a specific weighting (e.g. "this company weights people-leadership"), use it. (The critic's flags already carry their `jd_line`; keep it attached so the user sees the grounding.)
3. Invite the user's own edits / suggestions for that section.
4. Apply agreed changes **with the only-stated-facts rule intact** — every edit must still trace to `cv-master.md`, the JD, or this session. If a change needs a fact outside that set (a new title, %, scope, duration, team size, tool, or relationship), **ASK before writing** — never invent to satisfy an "improvement."
5. Respect deliberate choices: if the user has already locked something against your suggestion, flag it once, then drop it — do not re-litigate.
6. Move to the next section.

**On completion (or decline):** continue to Step 11 (defensibility gate). The gate re-validates the *reviewed* draft — so any content the review changed is re-checked for traceability (Check 5), 2-page fit (Check 6), and JD coverage (Check 7) before the grammar prompt prints.

### Step 11 — Run the 7-line defensibility checklist as a hard gate

Run all 7 checks. **The skill self-checks 1–4, 6, and 7 mechanically. Check 5 spawns a cheap fast-model sub-agent for independent verification.**

#### Check 1 — Trace check
Every `<li>` body in the rendered HTML must trace to one entry in `cv-master.md` (a bullet, project, highlight, cert, tool, or registry entry). For each picked stable ID, verify the rendered prose is recognisably the same as the master entry (modulo `**` → `<strong>` and grammar-only edits). Mechanical string match.

#### Check 2 — Number check
Every `<strong>`-wrapped number in the rendered HTML must appear verbatim in the Locked numbers registry at the top of `cv-master.md`. Period-bounded numbers (e.g., "50+ live streams (2018-2022)") must carry their period in the rendered bullet.

#### Check 3 — Mirror check
The picked `RS-*` mirrors the picked `L-*` per the mirror table:
- RS-PM ↔ L-PM
- RS-COM ↔ L-COM
- RS-SOC ↔ L-SOC

Mismatched pairs fail. No mixing.

#### Check 4 — Strand alignment check
The picked Selected Projects, Highlights, and role-sub all point at the same primary JD strand. (Secondary strands among projects/highlights are fine; the *primary* strand must be consistent.)

For each picked `P-*`, `H-*`, `RS-*`: at least one of the entry's tags must be the JD primary strand.

#### Check 5 — Out-of-pool prose check (fast-model sub-agent)

Spawn a sub-agent on a cheap fast model with the prompt:

> Read `<absolute path to draft.html>` and `<absolute path to cv-master.md>`. Flag any prose in the draft `<li>` bodies, project descriptions, or highlight rows that doesn't appear verbatim (or as a clear grammar-only edit — e.g., a comma added, "and" replaced with "plus") in the master pool. Return JSON: `{"verdict": "pass" | "fail", "flagged": [{"line": "<excerpt>", "reason": "<why>"}]}`.

This is belt-and-suspenders — Check 1's mechanical match catches direct misses; the sub-agent catches paraphrases that slip past string match.

#### Check 6 — 2-page A4 fit check
Cannot be measured automatically without a browser render. **Defer to the user**:

> Open `<folder>/Resume_<co>.html` in a browser, Cmd+P → check the print preview shows exactly 2 pages. (pass / fail)

If the user says fail, ask which pool to trim and loop back to Step 8.

#### Check 7 — JD coverage tally

Quantify how much of the JD this CV actually addresses. Mechanical, no LLM judgment. Produces a number that sits next to the qualitative gate.

**Procedure:**

1. Re-read `<folder>/JD.md`.
2. Extract the **distinct requirements** from the JD. A "requirement" = one of:
   - A bullet under "Requirements" / "What you'll do" / "What we're looking for" / "Must have" / "Nice to have"
   - A skill named in a "skills" or "tools" line ("strong familiarity with X", "experience with Y")
   - A scope/responsibility named in the role summary ("you will own the EMEA community program", "you will scale creator-ops across N markets")

   Skip boilerplate (DEI statement, benefits, "about us", office location).
3. Count `R = total distinct requirements`.
4. For each requirement, classify it against the rendered CV (`<folder>/Resume_<Company-Slug-CapsCase>.html`):
   - **Direct** — a `<li>` body, project description, highlight row, cert, tool, or skill in the rendered HTML names the requirement near-verbatim or with the canonical synonym (e.g. JD says "creator operations", CV says "creator-ops" or "creator operations").
   - **Reframed** — a rendered entry covers the requirement substantively but uses a different framing (e.g. JD says "stakeholder management across product/engineering/marketing", CV says "cross-functional rollouts coordinating product, engineering, and marketing teams"). Document the reframe pair.
   - **Gap** — no rendered entry addresses the requirement.
5. Count `D = direct`, `F = reframed`, `G = gaps`. (`D + F + G == R`.)

**Output the tally to the user in this exact shape:**

```
JD coverage:
  Reqs in JD     : <R>
  Direct hits    : <D>  (<D/R*100, rounded>%)
  Reframed       : <F>
  Gaps           : <G>  → <one-line list of the gap requirements>

  Coverage score : <(D + F)/R * 100, rounded>%
```

**Fail thresholds (any one trips the gate):**
- `G > 2` (more than two unaddressed requirements)
- `Coverage score < 75%` (the CV addresses less than three-quarters of the JD)
- A gap requirement is on the **stop-list**: anything in the JD's *must-have* section, the JD's headline title strand (e.g. for a Senior PM role, "program management" or "project management" cannot be a gap), or any skill/tool the JD repeats more than twice.

**On fail — bounded in-pool auto-resolve first (max 3 iterations):** before asking the user, attempt in-pool gap closure. For each gap requirement, find the highest-ranked `B-*` / `P-*` / `H-*` in `cv-master.md` (per §"Relevance ranking") that covers it and is not already picked; swap it in for the lowest-ranked picked entry the 2-page fit allows displacing (per the §"2-page fit — drop order" line-cut ranking). Re-run all 7 checks. Repeat up to 3 times.

- **Converges (gate passes):** present the resolved draft with a one-line summary of each swap — `closed gap "<req>" by swapping in <new-id> for <dropped-id>` — then continue to Step 13.
- **Cannot converge** (no in-pool gap-closer exists for a gap, or 2-page won't allow the swap): fall through to the user-ask below.

**Constraint:** auto-resolve only ever swaps *real* pool entries. It never fabricates, never rewords, and never drops the coverage bar itself — if a gap has no in-pool closer, that gap survives to the user-ask. (The "only stated facts" rule overrides this loop, same as the rest of the gate.)

**On fail after auto-resolve exhausts:** report each still-tripped threshold. Do NOT silently lower the bar — ask the user: *"Coverage tripped \<reason\> (auto-resolve closed \<N\> gap(s), \<M\> remain). Pick: (1) trim a low-value bullet to make room for a gap-closer from the pool, (2) accept the gap and proceed (state the strategic reason), (3) walk this JD."*

**On pass:** print the tally inline with the other check results so the user has the audit trail.

**Constraint — never fabricate to close a gap.** Closing a gap means swapping in a real `B-*` / `P-*` / `H-*` from `cv-master.md` that addresses the requirement, OR consciously accepting the gap. Never paraphrase a requirement into a bullet that wasn't in the master pool. The "only stated facts" rule overrides this check.

#### Gate result
- **All 7 pass:** continue to Step 13.
- **Any check fails:** continue to Step 12.

### Step 12 — Gate-fail behavior

The draft HTML stays written (don't delete or revert). But **withhold the "tailoring complete" message and the grammar-pass prompt**.

Print:

```
Draft written to <absolute path>.
Defensibility gate failed:
  - Check <N>: <reason>
  - Check <M>: <reason>

Resolve and re-run the gate, or /cv-checkpoint and pause.
```

Wait for the user's call. If they resolve, re-run from Step 11. If they checkpoint, hand off to `/cv-checkpoint`.

### Step 13 — On gate pass: print grammar-pass prompt

Print exactly this string, with the absolute path templated in:

```
CV draft complete at <absolute path>. Recommend /clear and run this in a fresh context: Read the CV at <absolute path>. Scan it thoroughly for grammar errors and potential changes that would increase the readability of the CV. Only focus on the grammar and sentence structure, not on context. Do not suggest any changes unless you think it is necessary.
```

The grammar pass is a separate-context check by design. Same-context grammar review tends to skip past errors the writing-context assistant already justified.

After printing, the active session ends. The user may `/clear` and run the grammar pass in a fresh context, then return for finalization + reflection.

### Step 14 — Post-finalization reflection

Triggered when the user **finalizes the CV** — not at application submit. Triggers (whichever comes first):
- Verbal finalization signal after the Step 13 grammar pass: "CV is final", "done with the CV", "looks good", "finalize the CV"
- The CV PDF is exported / saved

Do **not** run before the defensibility gate passes (Step 13), and do **not** run mid-edit. The reflection is decoupled from submit. Fallback: if the user says "shipped/submitted/applied" and the reflection has not yet run for this CV, run it then so the learning loop is never silently skipped.

When triggered:

1. **Diff `_original_draft.html` vs the current finalized HTML** at `<folder>/Resume_<co>.html`. Use `diff -u` and surface non-trivial diffs (skip whitespace-only and reordering diffs). Because `_original_draft.html` is frozen at the raw mechanical render (Step 10, pre-review), this diff captures everything the user changed afterward — Step 10b section-by-section review edits, grammar-pass edits, and any manual tweaks alike. The review edits are the richest learning signal; classify them like any other diff.

2. For each non-trivial diff, surface as a one-line item to the user:
   ```
   Diff #<N>:
     Before: "<original prose excerpt>"
     After:  "<edited prose excerpt>"
     Master line: <inferred B-* / P-* / H-* ID, or "out-of-pool"; if "out-of-pool", flag as a stronger signal>
   ```

3. **One-keypress classification per diff**:
   - **typo** → fix in `cv-master.md` immediately (apply the typo correction at the source line), don't log to learnings
   - **phrasing-preference** → append to `cv-learnings.md` `## Active (unpromoted)` with the diff, the master-line stable ID, and the user's one-line reason
   - **strand-rule** → append to `cv-learnings.md` `## Active (unpromoted)` with the diff, the JD strand it relates to, and the user's reason
   - **JD-specific** → append to `cv-learnings.md` `## JD-specific log` (never promoted, reference only)

4. After all diffs classified, count active promotable entries (`phrasing-preference` + `strand-rule` in `## Active (unpromoted)`). If `count >= THRESHOLD`:

   ```
   📋 cv-learnings.md has N promotable entries (threshold: <THRESHOLD>).
   Run /cv-promote-learnings now to merge into cv-master, or skip and I'll prompt at the start of the next CV.
   ```

5. Print:
   ```
   Reflection complete. Tailoring closed for <Company>.
   When you export the PDF (Cmd+P, Margins: None), optionally sanity-check ATS parseability with any PDF-to-text linearizer — a two-column layout that scrambles into the wrong reading order is the failure to watch for.
   ```

---

## What this skill does NOT do

- Does not draft cover letters. That's the cover-letter pipeline (`/tailor-cover-letter`).
- Does not log applications to any tracker.
- Does not edit `cv-master.md` content (except for typo fixes in Step 14). That's `/cv-promote-learnings`.
- Does not edit `cv-tailoring-rules.md`. That's `/cv-promote-learnings` for promoted strand rules.
- Does not render PDF. PDF is browser print → Cmd+P → Save as PDF, Margins: None. The skill ends at HTML.
- Does not modify the finalized HTML after Step 13. Step 14's diff is read-only against both files.

## Defensibility one-liner

Every `<li>` body, project description, and highlight row in any tailored CV produced by this skill must trace to a stable ID in `cv-master.md`. If it doesn't, the gate fails and the "tailoring complete" message is withheld. No exceptions.

## Coverage one-liner

Every CV that ships through this skill carries a JD-coverage tally (D / F / G). If `G > 2` or coverage `< 75%` or a stop-list requirement is a gap, the gate fails. The tally is the audit trail — when a future role calibrates against this one, the gap list is what tells you what was deliberately accepted vs. what slipped through.
