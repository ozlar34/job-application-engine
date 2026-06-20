---
name: tailor-cover-letter
description: Tailor a cover letter for a job application via per-block sign-off drafting from cover-letter-pool.md, with a personality-hooks pre-flight gate, a CV-precondition gate, and a 7-check defensibility gate before output. Use when the user says "/tailor-cover-letter", "tailor a cover letter for <co>", "draft the cover letter for <co>", or signals they are ready to draft a cover letter for an application that already has a tailored CV in the application folder.
triggers:
  - /tailor-cover-letter
  - tailor a cover letter
  - draft the cover letter
  - cover letter for <company>
  - start the cover letter for
---

# Tailor Cover Letter Skill

The cover-letter tailoring workflow as a mechanical skill, not a documented procedure. `cover-letter-pool.md`, `cover-letter-tailoring-rules.md`, and `cover-letter-voice-profile.md` stay as data files; this skill is the workflow that consumes them.

**Why a skill, not docs:** the core failure mode this skill addresses is documentation-execution drift — rules existing in `cover-letter-base.md` but the initial draft being generated without personality research and with packed-paragraph multi-job framing. This skill turns the workflow into mechanical gates that can't be skipped:

- Forced load of `cover-letter-pool.md` + `cover-letter-tailoring-rules.md` + `cover-letter-voice-profile.md` + `cover-letter-learnings.md` before any draft
- Hard-gate **CV-precondition** (no cover letter without a tailored CV in the same folder)
- Hard-gate **personality-hooks pre-flight** (no cover letter without a `## Personality Hooks` section in the company research notes)
- Strict **per-block sign-off** drafting: the skill never produces a full letter and asks "ok?"; it produces ONE block per turn and requires explicit sign-off before moving on
- Hard-gate **7-check defensibility**: no "tailoring complete" message until all 7 checks pass, including a CV-CL consistency cross-check

**Sibling to `/cover-letter-checkpoint`.** Both operate on the same `applications/YYYY-MM-DD-<co>/` folder convention as `/tailor-cv`. `/tailor-cover-letter` produces state; `/cover-letter-checkpoint` snapshots it.

**Sibling to `/cover-letter-promote-learnings`.** This skill reads `cover-letter-learnings.md` as advisory hints (never authoritative); the post-ship reflection step writes new entries to `cover-letter-learnings.md`; `/cover-letter-promote-learnings` is the manual gate that moves durable patterns into the locked content (pool / rules / voice-profile).

**Sibling to `/tailor-cv`.** The workflow is **CV → cover letter**, never the reverse. `/tailor-cover-letter` halts at Phase A Step 4 if no `Resume_<Co>.html` exists in the target application folder.

## Paths

```
Pool              : templates/cover-letter-pool.md
Tailoring rules   : templates/cover-letter-tailoring-rules.md
Voice profile     : templates/cover-letter-voice-profile.md
Learnings sidecar : templates/cover-letter-learnings.md
Doctrine reference: templates/cover-letter-base.md
Template          : templates/cover-letter-template.html
CV claim source   : templates/cv-master.md
Positioning       : templates/positioning-statement.md
Applications root : applications/
```

---

## Phase A - Pre-flight (mechanical, no drafting yet)

### Step 1 - Force-load the doctrine files

Read in this exact order, before any drafting:

1. `cover-letter-pool.md`: full file. The locked pool of selectable blocks (LD-*, CRED-*, BRICK-*, GAP-*, CAT-*, CLOSE-*).
2. `cover-letter-tailoring-rules.md`: full file. Variant table, esports credential rule, JD-strand→brick mapping, 7-check gate, threshold, hooks gate, CV-precondition gate, length bands.
3. `cover-letter-voice-profile.md`: full file. Voice/tone, banned phrases, defensible claims ladder, career-direction framing, feedback-pattern lookup.
4. `cover-letter-learnings.md`: advisory hints from past tailorings. **Read but treat as advisory only**, never authoritative; the locked content is the pool + rules + voice-profile.

If any of the four files is missing, halt with: *"`<filename>` missing, required for cover letter tailoring. Cannot proceed."*

### Step 2 - Threshold pre-check + Folder gate

Follow the shared tailoring routine from `lib/tailoring-common.md` (threshold pre-check and folder gate — tailor-cover-letter branch).

For the threshold pre-check: learnings-file = `cover-letter-learnings.md`; rules-file = `cover-letter-tailoring-rules.md`; promotable types = `phrasing-preference`, `variant-rule`, `voice-rule`, `pattern-rule`.

For the folder gate (cover-letter branch): reuse the existing CV folder — do not fork a new directory. List subdirectories under `applications/` matching `*-<slug>/` and pick the one with a `Resume_<slug>.html` inside. If multiple match, ask which one.

### Step 4 - CV-precondition gate (hard block)

Verify `<folder>/Resume_<Co>.html` exists.

**If absent:** halt with:

```
CV-precondition gate failed: no Resume_<Co>.html found in <folder>.

The workflow is CV → cover letter, never the reverse. Run `/tailor-cv <Company>` first; re-invoke `/tailor-cover-letter` after the CV is shipped.
```

Exit cleanly. Do not auto-invoke `/tailor-cv`.

**If present:** read `<folder>/_cv-checkpoint.md` if it exists and capture:
- `jd_strand_picked` (primary JD strand)
- `role_sub` (RS-PM / RS-COM / RS-SOC)
- `profile_lead_noun` (L-PM / L-COM / L-SOC)
- `picks` (selected bullets, projects, highlights, certs, tools, skills)

These feed Steps 8, 9, 10, and Phase C C7. If the checkpoint is absent, capture the strand and role-sub by reading the CV HTML and inferring (skill self-check, no user prompt).

### Step 5 - Resume gate

Check for an existing `<folder>/_cl-checkpoint.md`.

**If present:** prompt *"Found cover letter checkpoint from `<previous_checkpoint_at>`. Resume from it or start fresh? (resume / fresh)"*. If the user picks `resume`:

1. Read the checkpoint file.
2. If `checkpoint_timestamp` is more than 24h old, warn: *"Checkpoint is `<N>` hours old. JD or company state may have shifted; continue?"*
3. Print the resume summary:
   ```
   Resuming <Company> cover letter.
   Variant: <variant>
   Pattern set: <patterns>
   Locked blocks: <list>
   Pending blocks: <list>
   Open decisions: <list>
   Defensibility: <gate status>
   Continue?
   ```
4. On confirmation, restore picks/locked-blocks/pending-blocks into working memory and skip ahead to the first unlocked drafting step.

**If absent or the user picks `fresh`:** continue to Step 6.

### Step 6 - JD source

Check for an existing `<folder>/JD.md` or `<folder>/JD.txt`. (The CV step usually writes this.)

**If present:** read it. That's the JD. (Normal path — the CV step or a logging skill wrote it.)

**If absent:** prefer auto-capture over a manual paste. Ask: *"No JD.md yet — paste the JD
URL and I'll fetch it (LinkedIn supported), or paste the JD text directly."*
- **Given a URL** → run Routine B (capture JD.md) from
  `lib/jd-capture.md` (fetch script → browser fallback),
  then read the written `<folder>/JD.md`.
- **Fetch fails, or the user pastes text** → write `<folder>/JD.md` with the verbatim text,
  no edits.

Do not infer the JD from memory or summarize a URL; verbatim text only.

### Step 7 - Personality-hooks pre-flight (auto-fallback on missing)

Fetch the company research notes for `<Company>`. Verify the notes contain a `## Personality Hooks` section with ≥1 hook.

**If present:** continue to Step 8. Capture the top 1-2 hooks (highest authenticity rating) for use in Step 11 (lede via the lede-drafter sub-agent) and optionally Step 12 (closer).

**If missing:** spawn an inline 60-second light-hook sub-agent. Do NOT halt.

The sub-agent runs a non-blocking auto-fallback so a fresh application does not stall mid-tailor. The canonical full-depth path is `/deep-research <Company>`, which remains explicitly available as on-demand escalation; the light-hook is the auto fallback when nothing exists yet.

Sub-agent contract:

- Type: `general-purpose`
- Budget: ≤60s
- Inputs:
  - Company: `<name>`
  - company_notes_path: `<path to company research notes>`
- Steps:
  1. WebSearch query="{Company} engineering blog OR culture"
  2. defuddle parse <top 1-2 result URLs> --md
  3. Extract 1-2 hook candidates with verbatim citations (citation = quoted line + source URL)
- Output shape:
  - hooks: list of `{text, citation, source_url}`
- Append-target:
  - destination: append to `company_notes_path` under `## Personality Hooks (auto-light)`
  - body format: ISO date, optional gloss noting this is the light auto-fallback (not full deep-research output), then numbered hooks each with quoted citation and source URL

Spawn one `general-purpose` sub-agent with the brief above. Wait for return. On return, the sub-agent has already appended `## Personality Hooks (auto-light)` to the company notes; the caller does not re-write it.

Continue Phase A flow to Step 8 with the auto-light hooks captured for use in Step 11 (lede via the lede-drafter sub-agent) and optionally Step 12 (closer). Treat them with the same authenticity ranking the full Personality Brief uses, but flag in working memory that the source is the light-fallback so Phase D reflection can note any drift between the auto-light hooks and any later full deep-research output.

The personality research requirement already gates against shipping a cover letter without hooks; this auto-fallback closes the missing-hooks halt path so the workflow does not stall. The Phase C 7-check defensibility gate (C1 claim sourcing, C7 CV-CL consistency) remains the load-bearing enforcement, not Step 7.

### Step 8 - Title framing pick

Show the CV's `role_sub` from `_cv-checkpoint.md` as a reference (not a default), then ask:

```
Title framing for the cover letter:
  - Body title (used in lede or credibility paragraph if the variant references it):
  - Contact-block subtitle (replaces the default "Senior Community & Operations Manager" in the template):

CV used: <RS-string from checkpoint>

Common framings:
  - "Community Builder" (preferred for body when leading with the human-side scope)
  - "Senior Community Manager" (application-specific subtitle, e.g. for community-focused roles)
  - "Project Manager (Community Operations)" (formal contract title, for PM/Program Manager seats)
  - "Senior Community & Operations Manager" (template default)

Pick:
```

Capture both into working memory (will write to `_cl-checkpoint.md` on first checkpoint).

### Step 9 - Variant + pattern proposal

Read the JD. Read `cover-letter-tailoring-rules.md` variant table. Propose a variant + pattern set with one-paragraph rationale:

```
Proposal for <Company>:
  Variant: <A | B | C | D>
  Pattern set: <[1] | [1, 2] | [1, 2, 3] | [1, 3] | prose-only>

Rationale: <one paragraph, what JD signals matched the variant table; which patterns the JD supports>

Sign off? (ok / override <variant>+<patterns> / explain more)
```

On `ok`: lock the variant + pattern set.
On override: take the user's call.
On explain more: walk through the variant table reasoning in detail.

### Step 10 - JD-strand selection (if Pattern 1 in use)

If Pattern 1 (three-things JD-map) is selected, pick the top 3 JD priorities the three-things block will map. Cross-check against the CV's `jd_strand_picked` (from Step 4):

```
Top 3 JD priorities for three-things block:
  01. <JD-vocabulary heading 1> → <BRICK-* candidate from rules.md mapping>
  02. <JD-vocabulary heading 2> → <BRICK-* candidate>
  03. <JD-vocabulary heading 3> → <BRICK-* candidate>

CV primary strand: <strand from _cv-checkpoint.md>
CL primary strand alignment: <match | complement | drift, flag if drift>

Sign off? (ok / adjust / reorder)
```

If alignment is `drift`, the C7 consistency check at Phase C will flag this. Get explicit user sign-off that the drift is intentional.

---

## Phase B - Section-by-section drafting (strict per-block sign-off)

> **Rule of this phase:** drafts are PARALLELIZED, sign-off is SEQUENTIAL. Step 11 fans out 5 sub-agents in a single message to draft the independent blocks (lede + credibility + Pattern 1 + Pattern 2 + Pattern 3); Step 12 drafts the closer in-orchestrator (sequential, post-fan-out, because the closer's variant pick depends on what was drafted in parallel); Step 13 runs the per-block sign-off pass interactively in canonical order. The "ONE block per turn" UI semantics are preserved at sign-off time, but they no longer block drafting.
>
> Sign-off semantics (Step 13):
> - `ok` → block is locked; advance to next block in canonical order.
> - `revise <note>` → regenerate that one block per the note (orchestrator-side); show again; await re-sign-off. Three-things rows are revised per-row.
> - `pause` → halt and recommend `/cover-letter-checkpoint` so the user can `/clear` and resume later.
>
> Drift mitigation: each of the 5 fan-out sub-agents receives identical voice-profile constraints (banned phrases, defensible-claims ladder, performative-enthusiasm bans, body-prose em-dash rule) and identical Personality Hooks payload. Phase C C5 (AI-tells scan) and C7 (CV-CL consistency) catch any cross-block drift pre-output and remain the load-bearing enforcement.

### Step 11 - Parallel block drafting (5-way fan-out)

Spawn the following 5 sub-agents in parallel via a single message. Each returns a JSON object with the drafted block; the orchestrator merges all 5 into working memory before Step 12.

Shared inputs for all 5 sub-agents (drift mitigation: identical across agents):

- variant: locked at Phase A Step 9 (A | B | C | D)
- pattern_set: locked at Phase A Step 9 ([1] | [1, 2] | [1, 2, 3] | [1, 3] | prose-only)
- pool_block_candidates: the 1-2 block IDs from cover-letter-pool.md the variant supports for this agent's block type, with verbatim block text
- jd_strands: primary + supporting strands from Phase A (Step 4 cv-checkpoint reuse and Step 10 if Pattern 1)
- voice_profile_constraints: banned phrases, defensible-claims ladder, performative-enthusiasm bans, no-em-dash-in-body-prose rule (read from cover-letter-voice-profile.md)
- personality_hooks: top 1-2 hooks from company research notes (captured at Phase A Step 7), each with text + citation + source_url
- claim_sources: paths to templates/cover-letter-pool.md, templates/cv-master.md, templates/positioning-statement.md, plus the orchestrator's session_facts list

Shared output schema (each sub-agent returns this JSON):

```json
{
  "block_id": "<picked block ID, e.g. LD-A, CRED-B, BRICK-PIPELINE-EU, GAP-B2B-SAAS, CAT-AI-COMM>",
  "block_type": "lede | credibility | three-things | category-fluency | honest-gap",
  "applicable": true,
  "prose": "<drafted prose, no em-dashes in body, ready for orchestrator embed>",
  "word_count": 0,
  "claim_sources": [
    {"claim": "<text>", "source": "pool | cv-master | positioning-statement | session-fact", "source_excerpt": "<verbatim line from source>"}
  ],
  "rationale": "<one paragraph: why this block_id + JD-strand mapping fits, what voice-profile constraint was the trickiest>"
}
```

If the agent's pattern is NOT in pattern_set (e.g., three-things-drafter when Pattern 1 is not selected), the agent returns `{"applicable": false, "prose": "", ...}` and the orchestrator skips it during Step 13 sign-off.

The 5 sub-agents:

1. **lede-drafter** (general-purpose, cheap fast model)
   - Block type: lede. Always applicable.
   - Candidate pool: LD-* IDs the variant supports per the variant table in cover-letter-tailoring-rules.md.
   - Target word count: 50-65 words.
   - Drift focus: **Company anchor is mandatory.** The opening (first 1-2 sentences) MUST contain a verifiable company-specific reference and MUST pass the paste test (see voice-profile.md "The company-anchor test"). Forbidden: opening with a portable value/philosophy statement (e.g. "I add the most value through system design…") or leaning on generic anaphora ("this role", "your mission"). Anchor fallback ladder, in order: (1) a Personality Hook from the company research notes; (2) a quoted JD line or the company's stated thesis/positioning; (3) `LD-CONCRETE-WHY`'s "one concrete reason" slot, which still names the company. Never fall below the ladder to a portable opener. Also honor voice-profile bans on generic openers and performative-enthusiasm verbs.

2. **credibility-drafter** (general-purpose, cheap fast model)
   - Block type: credibility. Always applicable.
   - Candidate pool: CRED-A, CRED-B, CRED-C, or CRED-D matching the variant.
   - Target word count: 85-100 words.
   - Drift focus: esports credential handling per variant rule (named once with full disambiguation, or omitted entirely per the esports credential inclusion/exclusion rules in cover-letter-tailoring-rules.md); JD-specific proof points grafted into the block's [bracketed slots].

3. **three-things-drafter** (general-purpose, cheap fast model)
   - Block type: three-things. Applicable if Pattern 1 in pattern_set; otherwise returns applicable=false with empty prose.
   - Candidate pool: BRICK-* IDs picked at Phase A Step 10 for the 3 JD-strand mappings.
   - Target word count: ≤60 words per row x 3 rows.
   - Returns ALL 3 rows in a single response. Per-row sign-off happens in Step 13 (sign-off pass), NOT here.
   - Drift focus: JD-vocabulary verbatim headings (do not paraphrase the JD's exact terms); ≥1 number, tool, or program reference per row body; rows are independent so no cross-row narrative.

4. **honest-gap-drafter** (general-purpose, cheap fast model)
   - Block type: honest-gap. Applicable if Pattern 2 in pattern_set; otherwise returns applicable=false with empty prose.
   - Candidate pool: GAP-B2B-SAAS, GAP-NOCODE-SHIPPED, GAP-DEV-COMM, GAP-PIPELINE matching the deficit named in the JD.
   - Target word count: ≤3 sentences (roughly 50-80 words).
   - Drift focus: Defensible-claims ladder from cover-letter-voice-profile.md (B2B SaaS, pipeline-revenue, dev-community framed as real gaps, NOT over-claimed); JD-specific platform name or company name grafted into the [bracketed slots].

5. **category-fluency-drafter** (general-purpose, cheap fast model)
   - Block type: category-fluency. Applicable if Pattern 3 in pattern_set; otherwise returns applicable=false with empty prose.
   - Candidate pool: CAT-NOCODE, CAT-AI-COMM, or CAT-CREATOR-ECON matching the target's category.
   - Target word count: ~110 words.
   - Drift focus: Specific reference points (named products, named events, named figures); live tension (a real ongoing debate in the category); personally-used products (the user's actual stack from templates/positioning-statement.md).

Orchestrator merge: once all 5 return, validate each return (applicable==false implies empty prose; applicable==true implies word count within band and claim_sources non-empty). Store returns in working memory keyed by block_type. Proceed to Step 12 with full visibility into what each pattern drafted (the closer's variant pick depends on this).

The interactive per-block sign-off DOES NOT run here. Step 13 handles it.

### Step 12 - Closer drafting (sequential, post-fan-out)

The closer is drafted AFTER the 5 parallel returns from Step 11 because the variant pick (CLOSE-LOGISTICS-WINK / CLOSE-LOGISTICS-PLAIN / CLOSE-PROSE-TRANSITION / CLOSE-LOGISTICS-FOLDED) sometimes depends on what was drafted in parallel. For example, CLOSE-LOGISTICS-FOLDED folds the gap into the closer when there is no separate GAP-* block (i.e., honest_gap.applicable == false in the Step 11 returns). This is in-orchestrator drafting, not a sub-agent invocation.

Inputs (from working memory, no new fetch):

- variant: from Phase A Step 9
- pattern_set: from Phase A Step 9
- fan_out_returns: all 5 from Step 11 (lede, credibility, three_things, honest_gap, category_fluency)
- voice_profile_constraints: same as Step 11 shared
- personality_hooks: same as Step 11 shared (closer optionally references the top hook for symmetry with the lede)
- claim_sources: same as Step 11 shared
- jd_signals: extracted at Phase A (any JD line worth winking at, any logistics signal)

Variant pick logic:

- `CLOSE-LOGISTICS-WINK` if a JD detail worth winking at exists
- `CLOSE-LOGISTICS-PLAIN` if no detail worth winking at
- `CLOSE-PROSE-TRANSITION` for prose-only fallback letters
- `CLOSE-LOGISTICS-FOLDED` if the gap is folded into the closer (no separate GAP-* block, i.e., honest_gap.applicable == false)

Draft the closer prose. Target: 30-50 words.

Working-memory output:

- block_id: <CLOSE-* picked>
- prose: <drafted closer, no em-dashes>
- word_count: <integer>
- wink: <one-line if used, else null>

Proceed to Step 13 with all 6 drafted blocks (5 fan-out + 1 closer) loaded.

### Step 13 - Per-block sign-off pass (interactive, sequential)

Walk each drafted block in canonical order. For each block:

1. Show the draft (prose + word count + block_id + a one-line claim-source summary for traceability).
2. Ask `ok / revise <note> / pause`.
3. On `ok`: lock the block; advance to the next block.
4. On `revise <note>`: regenerate that one block per the note (orchestrator-side, no sub-agent re-spawn for single-block tweaks); show again; await re-sign-off.
5. On `pause`: halt and recommend `/cover-letter-checkpoint` so the user can `/clear` and resume later.

Canonical sign-off order:

- Lede (from lede-drafter)
- Credibility (from credibility-drafter)
- Three-things rows 01, 02, 03 (per-row sign-off; the three-things-drafter returned all 3 rows, walk them sequentially), only if Pattern 1 in pattern_set
- Honest-gap (from honest-gap-drafter), only if Pattern 2 in pattern_set
- Category-fluency (from category-fluency-drafter), only if Pattern 3 in pattern_set
- Closer (from Step 12)

For each block, the show-and-ask format is:

```
<BLOCK_TYPE> (<block_id>, <target word count>):

<draft prose>

Word count: <N>
Claim sources: <one-line summary>

Sign off? (ok / revise <note> / pause)
```

For the three-things block, repeat the show-and-ask three times (rows 01, 02, 03) using the row-level format:

```
THREE-THINGS ROW <01 | 02 | 03>:

Heading (JD-vocabulary verbatim): <heading>
Body (≤60 words, ≥1 number/tool/program):

<draft body>

Word count: <N>
BRICK source: <BRICK-* ID>

Sign off? (ok / revise <note> / pause)
```

Once all blocks are locked, advance to Step 13a (flat-block critic annotation pass), then Phase C (Step 17, the 7-check defensibility gate).

### Step 13a - Flat-block critic annotation pass (cheap fast model, annotation-only)

After all blocks are locked (Step 13) and before the defensibility gate, spawn **one** cheap-fast-model critic sub-agent for a light pass over the *picked* pool blocks vs the JD. This is the cover-letter analog of `/tailor-cv` Step 10a's `flat_flags` routing: it surfaces picked blocks whose **canonical pool prose** is highly JD-relevant but reads flat/generic — blocks that cannot be reworded in *this letter* (the Phase C fence forbids it: C1 claim-sourcing, C7 consistency), so they route to a pool-polish queue for a one-time pool fix gated behind `/cover-letter-promote-learnings`.

**Annotation-only — the critic does NOT edit the pool or the draft.** It writes one reference line per flag to `cover-letter-learnings.md`. `_cl_original_draft.html` does not exist yet (it is written at Step 18), so this changes nothing about the Phase D diff.

**Scope (critical):** the critic flags the **canonical block prose in `cover-letter-pool.md`** (the template), NOT the JD-grafted draft instance. Flatness that exists only because a `[bracketed slot]` is unfilled is by design, not a defect — never flag a block for having brackets. Drafting-time graft into brackets is out of scope here.

Spawn a `general-purpose` sub-agent using a cheap fast model and this brief:

> Read `templates/cover-letter-pool.md`, `<folder>/JD.md`, and `templates/cover-letter-voice-profile.md`. You are a cover-letter pool critic. For each of these PICKED block IDs — `<comma-separated list of the locked block_ids from Step 13 + Step 12>` — read its **canonical prose** in `cover-letter-pool.md` (the block body, ignoring `[bracketed slots]` and the `>` annotation). Flag a block ONLY if its fixed canonical prose is **highly relevant to a specific JD line yet reads flat/generic** — opens inward, leads with a tool-swap or title instead of the outcome, double-verbs, or buries the JD-relevant proof. The fix must be possible by **prose-sharpening alone**, keeping the claim identical and every number/fact verbatim, adding zero new claims; if a block only looks flat because it is *missing* a fact, do NOT flag it. Gaps (GAP-*) must stay framed as real gaps — only flag if sharpenable without over-claiming. Ignore em dashes and banned phrases (a different gate handles those). Return JSON:
> ```json
> {"flat_flags": [{"block_id": "<LD-*/CRED-*/BRICK-*/GAP-*/CAT-*/CLOSE-*>", "jd_line": "<verbatim JD line>", "why": "<what reads flat + what stronger thing already in the block should lead>"}]}
> ```

On return:

1. **Pool-polish queue (flat-block routing):** if `flat_flags` is non-empty, append each to `cover-letter-learnings.md` under a `## Pool-polish candidates` section (create the section if absent, placed after `## Active (unpromoted)`), one line per flag:
   ```
   - <block_id> — read flat vs JD "<jd_line>" (<today>): <why>
   ```
   These are **reference-only** for `/cover-letter-promote-learnings` pass 2 (the real fix is a `cover-letter-pool.md` in-place prose-sharpening rewrite, out of scope here). They never count toward the learnings promotion threshold and are never auto-applied.
2. If the critic returns nothing, note it and continue. This step never blocks — proceed to Phase C regardless.

The `## Pool-polish candidates` section is drained interactively by `/cover-letter-promote-learnings` pass 2, never by this skill.

---

## Phase C - Defensibility gate (hard block before render)

### Step 17 - Run the 7-check gate

Run all 7 checks. **All 7 must pass.** Failures listed with specific line references; render withheld until all pass. C1, C2, C3, C4, C6 are mechanical self-checks; C5 and C7 each spawn a cheap fast model sub-agent.

Full per-check detail (C1 claim sourcing, C2 em-dash scan, C3 esports credential rule, C4 variant alignment, C5 AI-tells sub-agent prompt, C6 length band, C7 CV-CL consistency sub-agent prompt) lives in `references/defensibility-gate.md` — read it before running the gate.

### Gate result

- **All 7 pass:** continue to Step 18.
- **Any check fails:** continue to Step 19.

### Step 18 - On gate pass: write output and finish

1. **Write `cover-letter.md`** to `<folder>/cover-letter.md` with frontmatter + assembled prose. (Schema in `templates/cover-letter-tailoring-rules.md` Section "Output format".)

2. **Render `Cover_Letter_<Company-Slug-CapsCase>.html`** by substituting prose blocks into `templates/cover-letter-template.html`:
   - Replace `[YEAR]` in `.eyebrow` with the current year.
   - Replace the default subtitle with the Step 8 subtitle pick.
   - Replace `[DD MONTH YYYY]` in the meta-strip Date block with today's date.
   - Replace `[Hiring Team / Hiring Manager Name], [Company]` with the addressed-to value.
   - Replace `[Role Title]` with the role title from JD.
   - Replace the Salutation `[Hiring Team at Company / Hiring Manager Name]` with the actual addressee.
   - Substitute the lede `<p class="lede">…</p>` content.
   - Substitute the credibility `<p class="body">…</p>` content (the first `.body` paragraph after the lede).
   - If Pattern 1 in use: substitute the `.callout` block's three rows. If not: delete the entire `<div class="callout">…</div>` block.
   - If Pattern 3 in use: substitute the second `<p class="body">…</p>` (the category-fluency one). If not: delete that paragraph.
   - If Pattern 2 in use: substitute the `.gap-row` content. If not: delete the entire `<div class="gap-row">…</div>` block.
   - Substitute the closer `<p class="body">…</p>` (the last body paragraph before signoff).
   - Replace `[Company]` and `[Role Title]` in the footer with the actual values.

3. **Save `_cl_original_draft.html`** snapshot (CL-specific name — `_original_draft.html` is owned by `/tailor-cv` in this same folder):
   ```bash
   cp <folder>/Cover_Letter_<Co>.html <folder>/_cl_original_draft.html
   ```

4. Print:

```
Cover letter draft complete at <absolute path to .html>.

Source-of-truth markdown: <absolute path to .md>.
Edit the .md and re-render. Do NOT hand-edit the .html.

Recommend /clear and run this in a fresh context: Read the cover letter at <absolute path>. Scan it thoroughly for grammar errors and potential changes that would increase readability. Only focus on grammar and sentence structure, not on context. Do not suggest any changes unless you think it is necessary.
```

The active session ends. The user may `/clear` and run the grammar pass in a fresh context, then return for ship + reflection.

### Step 19 - Gate-fail behavior

The draft `.md` and `.html` stay written (don't delete or revert). But **withhold the "tailoring complete" message and the grammar-pass prompt**.

Print:

```
Drafts written to <absolute paths>.
Defensibility gate failed:
  - C<N>: <reason>
  - C<M>: <reason>

Resolve and re-run the gate, or /cover-letter-checkpoint and pause.
```

Wait for the user's call. If they resolve, re-run from Step 17. If they checkpoint, hand off to `/cover-letter-checkpoint`.

---

## Phase D - Post-ship reflection

Triggered when the user signals ship (verbal "shipped"/"logged"/"submitted"/"applied"/"sent the cover letter", or when the application logging skill created the tracker row). The full reflection procedure — diff `_cl_original_draft.html` vs shipped, per-diff one-keypress classification, the promotable-entry threshold prompt, and the closing message — lives in `references/post-ship-reflection.md`. Read it when ship is signaled.

---

## What this skill does NOT do

- Does not draft CVs. That's `/tailor-cv`.
- Does not log to an external tracker. That's a separate skill.
- Does not edit `cover-letter-pool.md`, `cover-letter-tailoring-rules.md`, or `cover-letter-voice-profile.md` content (except for typo fixes in Phase D). That's `/cover-letter-promote-learnings`. Step 13a only *annotates* — it writes flat-block flags to `cover-letter-learnings.md`'s `## Pool-polish candidates` queue, never to the pool or the draft.
- Does not auto-invoke `/deep-research` when the personality-hooks gate fails. Halt and instruct only.
- Does not auto-invoke `/tailor-cv` when the CV-precondition gate fails. Halt and instruct only.
- Does not render PDF. PDF is browser print → Cmd+P → Save as PDF, Margins: None. The skill ends at `.md` + `.html`.
- Does not modify the shipped artifacts after Step 18. Phase D's diff is read-only against both files.
- Does not produce a full draft and ask "ok?". Per-block sign-off only; no exceptions.

## Defensibility one-liner

Every load-bearing claim, structural pattern, and voice choice in any cover letter produced by this skill must trace to a stable ID in `templates/cover-letter-pool.md`, a rule in `templates/cover-letter-tailoring-rules.md`, a tone choice in `templates/cover-letter-voice-profile.md`, or a fact in `templates/cv-master.md` / `templates/positioning-statement.md`. If it doesn't, the 7-check gate fails and the "tailoring complete" message is withheld. No exceptions.
