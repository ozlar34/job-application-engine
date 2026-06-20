# Phase C — 7-check defensibility gate (detail)

This is the full mechanical detail of the Phase C Step 17 gate referenced by `SKILL.md`. Run all 7 checks. **All 7 must pass.** Failures listed with specific line references; render withheld until all pass.

**The skill self-checks C1, C2, C3, C4, C6 mechanically. C5 spawns a cheap fast model sub-agent. C7 spawns a separate cheap fast model sub-agent.**

### C1 - Claim sourcing (mechanical grep)

For each `<strong>`-wrapped number, named program, named tool, or named property in the assembled draft:
- Grep `templates/cover-letter-pool.md`: does the claim appear in the picked block (verbatim or with light JD-grafting)?
- Grep `templates/cv-master.md`: does the claim appear?
- Grep `templates/positioning-statement.md`: does the claim appear?
- Was the claim explicitly stated by the user in the current session? (Captured to a session-fact list during Phase B.)

Any orphan claim fails.

### C2 - No em dashes in body prose

Mechanical regex scan over body prose only (`p.lede`, `p.body`, `.callout-intro`, `.three-body`, `.gap-row` content, NOT `.eyebrow` / `footer.foot` chrome).

Any em-dash (U+2014) or en-dash (U+2013) in body prose fails.

### C3 - Esports credential rule compliance

Run all sub-checks per `templates/cover-letter-tailoring-rules.md` Section "Esports credential inclusion / exclusion rule":
- Letter contains a bare short-form abbreviation of the esports credential without disambiguation → fail.
- Variant D AND esports credential mentioned → fail.
- Variant A AND target is AI/SaaS/no-code/dev-tooling AND esports credential mentioned → fail.
- First esports credential mention lacks full disambiguation (league name / event full name) → fail.

### C4 - Variant alignment

Re-check the variant pick against the variant table. Any of:
- Variant B selected for non-esports target → fail.
- Variant A selected for pure-esports seat → fail.
- Variant D selected without 2-3 strands named in JD vocabulary → fail.
- Variant C selected for community-lead role with no PM/process emphasis → fail.

### C5 - AI-tells scan (cheap fast model sub-agent)

Spawn:

```
Read <absolute path to draft .md> and <absolute path to cover-letter-voice-profile.md>.

Scan the draft body prose (lede + credibility + three-things + category-fluency + honest-gap + closer) for any:
- Banned phrase (see voice-profile.md "Banned phrases" section)
- Missing company anchor (apply the "paste test" from voice-profile.md "The company-anchor test"): read the lede's opening (first 1-2 sentences). Flag `missing-company-anchor` if it contains NO verifiable company-specific reference — a named product/feature/launch, a quoted JD-or-site line, the company's stated thesis/positioning, a named program/ritual, or a founder/product-history detail. Generic anaphora ("this role", "your mission", "your team", "the position") is NOT an anchor. If the opening could be pasted unchanged into a different company's letter, it fails. (This catches outward-sounding value/philosophy openers that the banned-phrase list misses — e.g. "I add the most value through system design…".)
- Generic opener (inward/performative patterns from the banned list)
- Performative-enthusiasm verb ("thrilled", "excited", "passionate")
- Vague-category framing ("small set of teams treating X as a category", "at the forefront of")
- Em dash in body prose (already C2-checked but re-flag anyway)
- "Responsibility-list" opener ("Responsible for managing", "Tasked with", "In charge of")
- Packed-paragraph multi-job sentence (paragraph doing >1 of: lede, credibility, proof, gap, fluency, close)
- "This doesn't connect" gap between paragraphs (transition implicit when it should be explicit)

Return JSON:
{"verdict": "pass" | "fail",
 "flagged": [{"excerpt": "<line>", "category": "<banned-phrase | missing-company-anchor | performative-enthusiasm | vague-category | responsibility-list | em-dash | multi-job-paragraph | implicit-bridge>", "voice_profile_section": "<which section of voice-profile.md it violates>"}]}
```

Use a cheap fast model for the Agent call. Hard block on `verdict: fail`.

### C6 - Length band

Word count over body prose only:
- Prose-only fallback: **250-350 words.** Outside band → fail.
- Structured (Variant D or any letter with Pattern 1/2/3): **450-550 words.** Outside band → fail.

Words below the lower bound is also a fail (too thin).

### C7 - CV-CL consistency check (cheap fast model sub-agent)

Spawn:

```
Read <absolute path to Resume_<Co>.html>, <absolute path to _cv-checkpoint.md> if present, and <absolute path to cover-letter draft .md>.

Cross-check the cover letter against the CV along these axes:

1. Numbers: every metric in the cover letter must appear or be derivable from numbers in the CV. Flag mismatches.
2. Titles & dates: employer names, role titles, date ranges must match the CV exactly.
3. Strand emphasis: CL primary JD-strand should match CV jd_strand_picked. If different, flag for confirm.
4. Title framing in body: body/subtitle title must not contradict the CV's role_sub.
5. Tooling claims: every tool/program named in the CL must be named or implied in the CV.
6. Gap claims: if CL acknowledges a gap, the CV must not over-claim that exact area.

Return JSON:
{"verdict": "pass" | "fail",
 "findings": [{"axis": "numbers | titles | strand | title-framing | tooling | gap",
               "cl_excerpt": "<line>",
               "cv_excerpt": "<line or 'absent'>",
               "issue": "<one sentence>",
               "fix_suggestion": "<which side to revise: cl | cv | either | confirm-intentional>"}]}
```

Use a cheap fast model for the Agent call. Hard block on `verdict: fail`.

For each finding, surface to the user:

```
C7 finding <i> of <N> - <axis>
  CL: "<excerpt>"
  CV: "<excerpt or absent>"
  Issue: <one sentence>
  Fix on: <cl | cv | either | confirm-intentional>

Resolve? (revise cl / revise cv / confirm intentional / pause)
```

### Gate result

- **All 7 pass:** continue to Step 18.
- **Any check fails:** continue to Step 19.
