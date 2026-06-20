# CV Learnings

> Sidecar to `cv-master.md` and `cv-tailoring-rules.md`. Captures manual edits the user makes to skill-tailored drafts after `/tailor-cv` produced them, classified at capture time, with a manual gate (`/cv-promote-learnings`) for merging durable patterns back into the locked content.
>
> **Synthetic showcase data.** The persona (Alex Rivera), the example employer ("Lumen AI"), and every entry below are invented to demonstrate the learnings-log mechanics — not a real edit history.
>
> **Active count rule:** entries in `## Active (unpromoted)` classified as `phrasing-preference` or `strand-rule` count toward the threshold. Typo and JD-specific entries do not.
>
> **Threshold:** see `THRESHOLD = 8` in `cv-tailoring-rules.md`. Calibrate after 3-4 applications.

---

## Active (unpromoted)

<!--
Entry shape (one block per learning):

### YYYY-MM-DD — <Company> [<jd-strand-tags>]
- Diff: master line `<STABLE-ID>` "<original prose excerpt>"
       → final "<edited prose excerpt>"
- Classification: phrasing-preference | strand-rule
- Reason: <one-line why the edit was made>
-->

### 2026-05-12 — Lumen AI [#ai-ops #pm]
- Diff: master line `B-PM-03b` "redeploying that capacity to higher-value work"
       → final "redeploying that capacity to roadmap-facing analysis"
- Classification: phrasing-preference
- Reason: the generic close read flat against an AI-ops JD that rewarded a concrete redeploy target; claim and ≈1.5 FTE figure unchanged.

### 2026-05-12 — Lumen AI [#community]
- Diff: master line `B-PM-06` "turning community signals into structured insights"
       → final "translating community signals into structured insights"
- Classification: phrasing-preference
- Reason: verb swap reads cleaner alongside the JD's "translate community feedback" phrasing; 180K+ span and claim unchanged.

---

## Pool-polish candidates

<!--
Written by /tailor-cv Step 10a (cheap-fast-model critic): *picked* master bullets that are highly JD-relevant
but read flat/generic in their current master prose. Drained by /cv-promote-learnings pass 2 (in-place
prose-sharpening at the stable ID — never a new variant, claim + numbers unchanged). Reference-only;
never counts toward the promotion threshold.

Entry shape (one line per flag):
- <master_id> — read flat vs JD "<jd_line>" (<date>): <why>
-->

- B-PM-08 — read flat vs JD "define processes, roles, and tooling so we ship fast without chaos" (2026-05-12, Lumen AI): opens inward with the tool-migration ("Owned the team's project-management workspace migration: designed…") — double-verbed and led by the tool-swap. The JD rewards the systems-design + rollout + on-schedule-retirement already inside the bullet; lead with that.

---

## Promoted (audit trail)

<!--
Entry shape (one block per promotion):

### YYYY-MM-DD — promoted from <Company>
- <STABLE-ID> phrasing replaced; original entry was YYYY-MM-DD <Company>
- (or: new selection rule appended to cv-tailoring-rules.md; original entry was ...)
-->

### 2026-04-30 — promoted from Lumen AI (de-gaming / off-domain neutralization)
- Added the de-gaming subsection (vocab swap table + proper-name generalization) to cv-tailoring-rules.md (§ Tailoring for non-gaming / off-domain employers). Codified as the one sanctioned exception to "selection + ordering, never rewriting" — conditional on a non-gaming employer; gaming/esports applications use the pool verbatim. Original entry: 2026-04-30 Lumen AI [#social #community].

### 2026-05-05 — promoted from Lumen AI (AI-ops positioning track) [#ai-ops #pm]
- The Lumen AI final inverted the auto-draft's community-first profile into an AI-builder-led one for a genuine AI-builder JD. Codified as the one documented exception to community-first positioning. Added `RS-PM-AIOPS` + `L-PM-AI` (AI-builder profile) to cv-master.md; added two `#ai-ops` rows to the mirror table + a full "AI-ops positioning exception" section to cv-tailoring-rules.md (3 firing conditions). Confirmed by the user at promote time.

### 2026-05-05 — promoted from Lumen AI (AI-building experience bullets) [#ai-ops #pm]
- Added `B-PM-11` (agentic coding assistant as a build platform: tool integrations + reusable CLI skills), `B-PM-12` (extended AI work to the HQ Globalization team, 30+ people), `B-PM-02b` (intelligence-pipeline traceability variant), `B-PM-03b` (≈1.5 FTE reclaim bullet — bullet form of the H-08b/registry figure). Registry row added: HQ Globalization team **30+**.

---

## Archived (not promoted, kept for reference)

<!--
Entry shape: same as Active, plus a one-line "Archived: <reason>" footer.
-->

### 2026-05-05 — archived from Lumen AI [#events] — B-PM-05
- Diff: master line `B-PM-05` "including welcome and aftershow parties"
       → "including welcome and aftershow events"
- Classification: phrasing-preference
- Archived: JD-specific tone preference; "parties" is accurate and on-brand for the Finals VIP program. Not generalizable — left in the master.

---

## JD-specific log (never promoted, reference only)

<!--
Entry shape (one block per JD-specific edit, captured for post-mortem only):

### YYYY-MM-DD — <Company>
- Edit: <one-line summary of what was changed for this specific JD>
- Why JD-specific: <why this won't generalise to other tailorings>
-->

### 2026-05-05 — Lumen AI
- Edit: B-PM-11 named the specific agentic coding assistant and its tool integrations explicitly (master `B-PM-11` stays tool-agnostic).
- Why JD-specific: naming the exact assistant is high-signal for an employer that uses it; kept generic in the master so the pool isn't tool-locked. Apply the name swap at tailoring time for employers that name the tool.
