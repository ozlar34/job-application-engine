# Cover Letter Tailoring Rules

> **Synthetic showcase data.** All persona facts, numbers, titles, and company names below belong to the fictional persona **Alex Rivera** (see `cv-master.md` / repo README). This file demonstrates the *selection and defensibility logic* the `/tailor-cover-letter` skill runs — not a real workflow against real data.

> Consumed by the `/tailor-cover-letter` skill. Mechanical rules for selecting blocks from `cover-letter-pool.md`, projecting them into `cover-letter-template.html` via per-application drafting, and verifying defensibility through a 7-check gate. Tailoring is **per-block selection + sign-off + light JD-grafting**, never wholesale rewriting.

---

## Variant selection table

> Pick ONE variant per application. The skill proposes a variant with rationale at Phase A Step 9; the user signs off or overrides.

| Target role shape | Variant | Lede | Credibility | SPL mention |
|---|---|---|---|---|
| AI-forward, creator platforms, community-lead roles (AI-native and creator-platform companies) | **A — Broad** | LD-PRODUCT-RITUAL / LD-RECENT-POST / LD-CONCRETE-WHY | CRED-A | Once in subordinate clause; OMIT for AI/SaaS/no-code/dev-tooling targets |
| Esports, gaming-native, live-ops-heavy roles (gaming studios, pure-esports seats) | **B — SPL-forward** | LD-CATEGORY-FRAME / LD-PRODUCT-RITUAL | CRED-B | Leads the credibility paragraph |
| Ops / PM / program-manager roles where process-at-scale beats community-at-scale (generic PM seats at any target) | **C — Ops/PM-forward** | LD-CONCRETE-WHY / LD-RECENT-POST | CRED-C | Named once at end of paragraph |
| Highly-specific JDs naming 2-3 candidate strands by program name, plus a category Alex speaks insider on (no-code / AI-builder platforms and seats) | **D — Structured** | LD-VARIANT-D-JD-VOCAB | CRED-D | OMITTED by default |

### Variant-fit signals

- **Variant A** signals: AI-native company, creator-economy positioning, "community-as-product" framing, JD names creator/community/events but not by program-specific vocabulary.
- **Variant B** signals: JD explicitly names esports tournaments, broadcast production, live-ops at large audience, championship circuits.
- **Variant C** signals: JD leads with project/program coordination, requirements gathering, stakeholder management, SDLC understanding, cross-functional facilitation. Title is PM/Program Manager.
- **Variant D** signals: JD names 2-3 of Alex's strands by their actual program names ("Ambassador and Expert program", "voice-of-customer pipeline to product", "no-code stack ownership"). The lede *quotes* the JD's vocabulary verbatim.

> **Variant misalignment is a hard-gate failure** (Check C4). Picking Variant B for an AI/SaaS role, or Variant A for a pure esports seat, blocks render until corrected.

---

## SPL inclusion / exclusion rule

The Skybound Pro League (SPL) is one strand of Alex's scope. It is **not the whole story** and must not be defaulted into every letter.

### Include SPL when

- The JD touches **gaming, esports, live ops, or large-broadcast event production**.
- The target company has gaming/esports overlap (gaming studios, esports orgs, etc.).
- A specific JD strand maps directly to live-broadcast scope (e.g., "production of large-audience live events").

### Omit SPL when

- The target is **AI-native / SaaS / no-code / dev-tooling** with no gaming overlap.
- Variant D is selected (omits SPL by default per the variant table above).
- The JD does not name any gaming, esports, live-ops, or broadcast vocabulary.

### When SPL is named, naming convention

- **First mention:** "global esports league" or "Skybound Pro League".
- **Second mention onwards:** "SPL".
- **Spelling: SPL, never SLP.** Mechanical name check in C3.

---

## JD-strand → brick mapping

> When Pattern 1 (three-things JD-map) is in use, the skill picks 3 BRICK-* blocks based on the JD's named priorities. The map below is advisory — JDs may name strands in their own vocabulary that map to the same bricks.

| JD priority | Primary BRICK pick | Backup BRICK pick |
|---|---|---|
| Multi-platform community ownership at scale | BRICK-COM-MULTIPLATFORM | BRICK-COM-INTEL |
| Voice-of-customer pipeline to product | BRICK-COM-INTEL | — |
| Ambassador / Expert / Creator partnership program | BRICK-AMB-LOYALTY | — |
| Event production / on-site / VIP / conference presence | BRICK-EVT-VIPCREATOR | BRICK-EVT-IPCROSS |
| AI tooling / no-code / automation ownership | BRICK-AI-CENTRAL | BRICK-COM-INTEL |
| Project / program management at small team | BRICK-PM-PROMOTION | BRICK-AI-CENTRAL |
| IP / brand / partnership crossover programs | BRICK-EVT-IPCROSS | BRICK-EVT-VIPCREATOR |
| Esports / live-ops / large-broadcast (Variant B only) | BRICK-EVT-SPL | BRICK-EVT-VIPCREATOR |
| Live-ops / fast-paced / real-time / high-growth operating rhythm (non-gaming) | BRICK-LIVEOPS-SCALE | — |

> **BRICK-LIVEOPS-SCALE routing (non-gaming live-ops competency).** Expresses the live-global-championship operating rhythm label-stripped, so the Tier-1 competency (live, no-retry, multi-region coordination at scale) survives the SPL-omit rule on non-gaming targets — where it would otherwise vanish entirely when SPL is dropped. Fire it as:
> - **A full three-things row** when the JD is Track-B (PM / program / ops) OR its vocabulary names *fast-paced, live, real-time, high-growth, 0-to-1, ambiguity, scrappy, cross-functional-under-deadline*.
> - **Prose texture only** (one grafted clause in CRED-* or CLOSE-*, no slot consumed) for community/social-led JDs, where the three slots are better spent on BRICK-COM-MULTIPLATFORM / BRICK-AMB-LOYALTY / BRICK-COM-INTEL.
> - **Never for gaming/esports/live-ops targets** — those route to BRICK-EVT-SPL (Variant B), which carries the real viewer numbers. Using both double-counts the same event.
>
> Defensibility: numbers trace to `cv-master.md` (Locked numbers registry + B-SCM-04); the de-gaming vocabulary ("global championship", never "esports/broadcast") keeps it consistent with the de-gamed CV (C5/C7) and it passes C3 as SPL-absent. Its "4 regions / 12 languages" cross-checks against the CV's retained live-ops scope anchor (see `cv-tailoring-rules.md` §de-gaming) so C7 does not flag an orphan metric.

### Three-things composition rules

- **Exactly three rows.** Two looks light; four blurs the message. If the JD only gives two strong matches, use Pattern 2 (honest-gap) for the third slot or fall back to prose-only.
- **Headings echo the JD's vocabulary verbatim.** If the JD says "Ambassador and Expert programs", the heading is "Ambassador and Expert programs" — not "Creator program".
- **Each body has at least one number, named tool, named program, or named property.** Adjective-only sentences fail C5.
- **Each body ≤60 words.** Stretching past 60 means the match is forced — drop the row.

---

## Pattern selection (1 / 2 / 3)

| Pattern | When to use | Default? | Hard rules |
|---|---|---|---|
| **Pattern 1 — Numbered three-things JD-map** | JD names ≥2 strands that map distinctly to Alex's scope | YES (default for all variants when JD supports it) | Exactly 3 rows; JD-vocabulary headings; ≥1 number/tool/program per body; ≤60 words/body |
| **Pattern 2 — Honest-gap callout** | JD/company exposes a real, visible gap on the defensible-claims ladder | Optional; use 0 or 1 per letter | One gap; one paragraph; three sentences max; concrete remediation paired |
| **Pattern 3 — Category-fluency paragraph** | Target lives in a market category Alex speaks insider on (no-code, creator-economy, AI-tooling) | Optional; use 0 or 1 per letter | 3-5 named reference points; live tension acknowledged; products personally used on each side |

### Composition order in the rendered letter

```
LD-*  →  CRED-*  →  [Pattern 1: BRICK-* × 3]  →  [Pattern 3: CAT-*]  →  [Pattern 2: GAP-*]  →  CLOSE-*
```

> Pattern 3 placed before Pattern 2 deliberately — category fluency is forward-looking; honest-gap is a confessional and lands better just before the closer.

### Pattern misuse flags (C5 scan)

- Pattern 1 with only 2 rows → fail (must be exactly 3).
- Pattern 2 with two gaps in one letter → fail (one gap per letter, max).
- Pattern 2 used for logistics (salary, location, notice, visa) → fail (logistics belong in CLOSE-*).
- Pattern 3 in a category Alex is not an insider in → fail (faked fluency reads worse than no paragraph).

---

## Title framing rule (per-application)

The title used in the cover letter body and on the contact-block subtitle is **picked per-application at Phase A**. There is no default and no pool entry for title framing.

### Examples of valid title framings

- **"Community Builder"** — preferred for the body when the cover letter is leading with the human-side scope (Community / Creator / Events).
- **"Senior Community Manager"** — application-specific subtitle override (e.g., a community-lead application used this in the role-sub line).
- **"Project Manager (Community Operations)"** — formal contract title; use when the application is for a PM/Program Manager seat and the JD expects formal title alignment.
- **"Senior Community & Operations Manager"** — current default subtitle in `cover-letter-template.html` (line 319).

### How the skill captures the pick

`/tailor-cover-letter` Phase A Step 8:
1. Read `_cv-checkpoint.md` (if present) and surface the CV's role-sub pick (e.g. RS-PM "Project Manager (Community Operations)") as a **reference, not a default**.
2. Ask the user: *"Which title for the cover letter body? Which subtitle for the contact block? (CV used: <RS string>)"*
3. Capture both into `_cl-checkpoint.md`.
4. The body title is grafted into the lede or credibility paragraph if the variant references it; the subtitle replaces line 319 of the rendered HTML.

> **Never default to the CV's role-sub.** The CV is per-application formal; the cover letter body title is per-application narrative. Different decisions.

---

## Personality-hooks gate (Step 0)

> Hard gate at Phase A Step 7 of `/tailor-cover-letter`. Halt-and-instruct behavior; no auto-routing.

### The check

For the target company, fetch the company research note (external tracker) and verify the note body contains a `## Personality Hooks` section. The section must list ≥1 hook with an authenticity rating.

### If present

Continue to Step 8. The skill reads the hooks and surfaces the top 1-2 (highest authenticity rating) for use in the lede (Step 10) and optionally the closer (Step 15).

### If missing

**Halt.** Print:

```
Step 0 personality-hooks gate failed: no `## Personality Hooks` section in the research note for <Company>.

Run `/deep-research <Company> for job-search` first. The explicit `for job-search` suffix triggers the Personality Brief regardless of company tier; the skill auto-synthesizes ranked hooks (with authenticity ratings + recommended insertion points) and appends them to the research note body.

Re-invoke `/tailor-cover-letter` after research is done.
```

Exit cleanly. Do not auto-invoke `/deep-research` — `/deep-research` is a 5-15 min run that should not silently spawn from inside another skill.

### Why this gate exists

Codified after an early letter shipped without personality research; the gap was caught and patched mid-revision. A founder's competitive-gaming background and a manifesto-as-community-thesis hook only emerged when a fresh research run fired mid-revision. Hard gate prevents repeat.

> **Applies to every company.** Tier-agnostic. The freshly-discovered companies are exactly where personality research delivers the biggest delta.

---

## CV-precondition gate (Phase A Step 4)

> Hard gate. Verifies that a tailored CV exists in the target application folder before any cover letter drafting begins. Workflow is **CV → cover letter, never the reverse**.

### The check

For the target application folder (`applications/YYYY-MM-DD-<slug>/`), verify the presence of:

- `Alex_Rivera_Resume_<Co>.html` — the rendered CV
- (Optionally, `_cv-checkpoint.md` — the CV's pick state, used by C7 for cross-checks)

### If present

Continue. Read `_cv-checkpoint.md` (if present) and capture the CV's:
- `jd_strand_picked` — primary JD strand
- `role_sub` — RS-PM / RS-COM / RS-SOC
- `profile_lead_noun` — L-PM / L-COM / L-SOC
- `picks` — selected bullets, projects, highlights, certs, tools, skills

These feed Phase A Step 8 (title framing reference), Step 9 (variant proposal), Step 10 (JD-strand alignment), and Phase C C7 (CV-CL consistency check).

### If absent

Halt. Print:

```
CV-precondition gate failed: no Alex_Rivera_Resume_<Co>.html found in <folder>.

The workflow is CV → cover letter, never the reverse. Run `/tailor-cv <Company>` first; re-invoke `/tailor-cover-letter` after the CV is shipped.
```

Exit cleanly.

---

## Length bands (C6)

| Format | Target band | Hard cap |
|---|---|---|
| Prose-only fallback | 250-350 words | 350 |
| Structured (Variant D, or any letter using Pattern 1/2/3) | 450-550 words | 600 |

> **Word count below the lower bound is also a fail.** A 200-word prose-only letter is too thin; a 380-word structured letter is wasting the format.

Reading-time check (advisory): structured letters target ≤60 seconds reading time. Word count is the mechanical proxy.

---

## 7-check defensibility gate (Phase C)

> Run as a hard gate before render. **All 7 must pass.** Failures listed with specific line references; render withheld until all pass.

### C1 — Claim sourcing (mechanical grep)

Every load-bearing claim in the letter must trace to one of:
- A block in `cover-letter-pool.md` (verbatim or with light JD-grafting only)
- An entry in `cv-master.md` (numbers, programs, tools, named properties, Locked numbers registry)
- An entry in `positioning-statement.md` (positioning claims)
- A fact stated explicitly by the user in the current session

Mechanical grep: for each `<strong>`-wrapped number, named program, named tool, or named property in the draft, verify presence in one of the source files. Orphan claims fail.

### C2 — No em dashes in body prose

Mechanical regex scan over body content (`p.lede`, `p.body`, `.callout-intro`, `.three-body`, `.gap-row`). Any em-dash (`—` U+2014, also `–` U+2013 en-dash) in body prose fails.

> Em dashes ARE allowed in template chrome (`.eyebrow`: `Cover Letter — [YEAR]`, `footer.foot`: `[Company] — [Role Title]`). The scan excludes those CSS classes.

### C3 — SPL rule compliance

- Letter contains "SPL" or "Skybound Pro League" or "global esports league" or "90k" / "90,000" → SPL is present.
- Variant is **D** AND SPL is present → fail (Variant D omits SPL by default).
- Variant is **A** AND target is AI/SaaS/no-code/dev-tooling AND SPL is present → fail (SPL must be omitted per the SPL rule).
- Letter contains "SLP" anywhere → fail (canonical name is SPL, never SLP).
- First SPL mention is "SPL" without "global esports league" / "Skybound Pro League" → fail (first mention requires full phrasing).

### C4 — Variant alignment

Variant matches role shape per the variant table:
- Variant B selected for an AI/SaaS/no-code/dev-tooling target → fail.
- Variant A selected for a pure-esports seat → fail.
- Variant D selected without 2-3 strands named in JD vocabulary → fail.
- Variant C selected for a community-lead role with no PM/process emphasis → fail.

### C5 — AI-tells scan (cheap-fast-model sub-agent)

Spawn a cheap-fast-model sub-agent with the prompt:

> Read `<absolute path to draft .md>` and `<absolute path to cover-letter-voice-profile.md>`. Scan the draft body prose for any banned phrase, generic opener, performative-enthusiasm verb, vague-category framing, em-dash, "responsibility-list" opener, packed-paragraph multi-job sentence, or "this doesn't connect" gap between paragraphs (transition implicit when it should be explicit). Return JSON: `{"verdict": "pass" | "fail", "flagged": [{"excerpt": "<line>", "category": "<banned-phrase | performative-enthusiasm | vague-category | responsibility-list | em-dash | multi-job-paragraph | implicit-bridge>", "voice_profile_section": "<which section of voice-profile.md it violates>"}]}`.

Hard block on any flagged item (verdict = fail).

### C6 — Length band

Word count over body prose only (excludes contact block, meta strip, salutation, signoff, footer):
- Prose-only fallback: 250-350 words. Outside band → fail.
- Structured (Variant D or any letter with Pattern 1/2/3): 450-550 words. Outside band → fail.

Reading time advisory only — not a hard check.

### C7 — CV-CL consistency check (cheap-fast-model sub-agent)

Spawn a cheap-fast-model sub-agent with the prompt:

> Read `<absolute path to Alex_Rivera_Resume_<Co>.html>`, `<absolute path to _cv-checkpoint.md>` (if present), and `<absolute path to cover-letter draft .md>`. Cross-check the cover letter against the CV along these axes:
>
> 1. **Numbers:** every metric in the cover letter must appear or be derivable from numbers in the CV. Flag any CL number that does not appear in the CV (e.g. CL says 56 creators, CV says 55). Period-bounded numbers in the CL must carry the same period brackets used in the CV.
> 2. **Titles & dates:** employer name (Northwind Interactive), role titles (Project Manager (Community Operations) / Senior Community & Social Manager / Community & Social Manager), date ranges (2024–present, 2022-2023, 2019-2021, etc.) must match the CV exactly.
> 3. **Strand emphasis:** the CL's primary JD-strand should match the CV's `jd_strand_picked` from the checkpoint. If different, flag for confirmation (intentional complement is fine; accidental drift is not).
> 4. **Title framing in body:** the title used in the CL body or contact subtitle (e.g. "Community Builder") must not contradict the CV's `role_sub` (e.g. "Project Manager (Community Operations)") in a way that reads as inconsistency. "Community Builder" alongside RS-PM is a deliberate narrative choice; "Senior PM" alongside RS-COM is a contradiction.
> 5. **Tooling claims:** any tool/program named in the CL must be named or implied in the CV. Orphan tools (CL mentions a tool the CV does not) flag.
> 6. **Gap claims:** if the CL acknowledges a gap (e.g. "no B2B SaaS community"), the CV must not over-claim that exact area. Cross-check the relevant CV sections (e.g. CV bullet about the Creator Ambassador Program does not need to claim B2B SaaS scope).
>
> For each finding, return JSON: `{"verdict": "pass" | "fail", "findings": [{"axis": "numbers | titles | strand | title-framing | tooling | gap", "cl_excerpt": "<line>", "cv_excerpt": "<line or 'absent'>", "issue": "<one sentence>", "fix_suggestion": "<which side to revise: cl | cv | either | confirm-intentional>"}]}`.

Hard block on any finding with verdict = fail. The skill surfaces each finding with the suggested side to revise; the user picks per-finding.

---

## Output format

After all 7 checks pass:

1. **Write `cover-letter.md`** with frontmatter:
   ```yaml
   ---
   tags: [job-search, cover-letter, application, <slug>]
   type: application
   company: <Company>
   role: <Role Title>
   date_drafted: <YYYY-MM-DD>
   variant: <A | B | C | D>
   pattern_set: [<1>, <2>, <3>]  # which patterns were used
   blocks_used:
     lede: <LD-* ID>
     credibility: <CRED-* ID>
     bricks: [<BRICK-* IDs>]
     gap: <GAP-* ID, if used>
     category: <CAT-* ID, if used>
     close: <CLOSE-* ID>
   title_framing:
     body: "<title used in body>"
     subtitle: "<subtitle in contact block>"
   status: draft
   ---
   ```
   Followed by the rendered prose (lede, credibility, three-things, fluency, gap, close).

2. **Render `Alex_Rivera_Cover_Letter_<Co>.html`** by substituting the prose blocks into `cover-letter-template.html`.

3. **Save `_original_draft.html`** snapshot for post-ship diff.

4. **Note in output:** "Edit `cover-letter.md` and re-render — do not hand-edit the .html. The .md is the source of truth."

---

## Learnings threshold

```
THRESHOLD = 8
```

Active promotable entries in `cover-letter-learnings.md` (entries classified as `phrasing-preference`, `variant-rule`, `voice-rule`, or `pattern-rule` in the `## Active (unpromoted)` section). Typo and JD-specific entries don't count.

When `count >= THRESHOLD`:
- At end of `/tailor-cover-letter` reflection step: `📋 cover-letter-learnings.md has N promotable entries (threshold: 8). Run /cover-letter-promote-learnings now to merge into the pool / rules / voice-profile, or skip and I'll prompt at the start of the next cover letter.`
- At start of next `/tailor-cover-letter` (Step 2 threshold pre-check): same prompt, soft escalation, not a hard block.

**Calibrate after 3-4 applications:** if 8 is too chatty, raise to 10. If it never trips, lower to 5.

---

## Things this file does NOT govern

- **Block prose itself.** That's `cover-letter-pool.md`. This file governs selection rules over the pool.
- **Voice / tone / AI-tells lookup table.** That's `cover-letter-voice-profile.md`. C5 reads from there.
- **Doctrinal narrative reference.** That's `cover-letter-base.md`. Kept for human reading; the skill consumes pool + rules + voice-profile mechanically.
- **HTML rendering chrome.** That's `cover-letter-template.html`. Section ordering and CSS are locked.
- **Pre-application research depth.** That's `/deep-research` and the personality-hooks gate.
- **Application logging.** That's `/log-application`. Cover letter ship is signaled separately and triggers `/cover-letter-promote-learnings`.
- **Already-shipped letters.** The Lumen AI worked examples stay frozen as reference. This system applies to future tailorings only.
</content>
