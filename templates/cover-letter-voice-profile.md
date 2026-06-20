# Cover Letter Voice Profile

> Voice, tone, and AI-tells reference for `/tailor-cover-letter`. Read by the skill's C5 defensibility check (cheap-fast-model sub-agent scan) and consulted during per-block drafting.
>
> **Why a separate file:** `cover-letter-tailoring-rules.md` governs mechanical selection (variant table, SPL rule, pick counts). This file governs prose quality — what makes a sentence read as Alex Rivera vs. read as AI-generated boilerplate. Kept separate so the rules file stays scannable, and so the C5 scan reads from a single canonical lookup.
>
> **Synthetic showcase.** Every persona detail, company, and number below belongs to the fictional candidate **Alex Rivera** (see `cv-master.md`). Replace the persona block, defensible-claims ladder, and examples with your own to use the system.

---

## Tone — what good sounds like

- **Clear, structured, professional. Never casual.** No conversational filler ("So, basically", "honestly", "to be fair").
- **Calm, analytical, grounded.** Not defensive, not condescending, not over-enthusiastic.
- **European metric units** when measurements appear.
- **Near-zero emojis** (target: zero in body prose). The eyebrow and footer chrome are emoji-free by design.
- **Reads as if shareable internally with stakeholders without modification.** Pass: a hiring manager could forward this to their CEO. Fail: cringe-on-forward.
- **Assumes informed reader.** Depth over simplification. Don't pre-explain industry context the reader already has.

## Length expectations

- **Prose-only letters:** 250-350 words (target band; ≤300 is the sweet spot, 350 is the absolute cap).
- **Structured letters** (Variant D, or any letter using Pattern 1/2/3): 450-550 words. Reading time matters more than word count — target ≤60 seconds.
- A letter under 230 words is too thin. A letter over 600 is the wrong format — split into a tighter prose-only or trim the patterns.

---

## One paragraph = one job

The single most-cited feedback pattern: *"This feels forced / glued together. The paragraph is doing too many jobs at once; split it."*

**Each paragraph or block must do exactly one job.** Examples of valid jobs:
- The lede: name one concrete reason this company caught your attention + bridge to your work.
- The credibility paragraph: ground your current scope in a single coherent narrative.
- A three-things row: prove ONE JD strand with ONE number/tool/program.
- The honest-gap callout: acknowledge ONE deficit and pair it with ONE concrete remediation.
- The category-fluency paragraph: position yourself in ONE market category.
- The closer: logistics + one optional wink.

If a paragraph is doing two jobs, split it. The C5 scan flags multi-job paragraphs as `forced-glue`.

---

## Banned phrases (recruiter-flagged AI tells)

The C5 scan looks for these in any draft. Hard-block on hits in body prose; warn on hits in the lede if there's no better alternative.

### Generic openers
- `"I am writing to apply for"` — every junior cover letter starts this way; recruiters mute it on sight.
- `"I am excited to apply"` / `"I am thrilled to apply"` — see "Performative enthusiasm" below.
- `"My name is Alex Rivera and I"` — name appears in the contact block; redundant.
- `"As a [role title] with X years of experience"` — responsibility-list framing; lead with what was done, not what the title was.
- **Portable opener** — any lede whose opening (first 1-2 sentences) names no verifiable company-specific detail, even when it sounds outward and on-voice. The tell is the *paste test*: if the opening could be dropped, unchanged, into a cover letter for a different company and still read as written-for-them, it fails. This catches the value/philosophy opener that the inward/performative bans above miss. Verbatim cautionary example (shipped to two companies, both flat): *"I add the most value through system design: modeling how a process works, then wiring the AI agents and automations that run it. This role is scoped around that…"* — sentence one is fully portable, and "this role" is generic anaphora, not an anchor. See the company-anchor test under "Personal-hooks doctrine."

### Performative enthusiasm
- `"I am thrilled"` / `"I am excited"` / `"I am passionate about"` — unverifiable, AI-writing tell.
- `"passionate about [no-code / AI / community / category X]"` — same. Use verbs of practice instead: *follow*, *use*, *watch*, *run*, *track*.
- `"deeply passionate"` / `"truly passionate"` — adjective stacking on top of an already-banned word.

### Vague category framing
- `"a small set of teams treating X as a category"` — recurring AI-generated phrasing. Recognised pattern.
- `"one of the few companies that actually [verb] [thing]"` — same shape, different lexicon.
- `"at the forefront of [category]"` / `"leading the [category] revolution"` — hype copy.

### Responsibility-list openers
- `"Responsible for managing"` / `"Tasked with"` / `"In charge of"` — every CV bullet that fails the `/tailor-cv` voice check leads with these. Same rule applies in cover letters when describing scope.
- Lead with **what was done** and the **measurable outcome**, not the title's job description.

### Closings that beg or oversell
- `"I would be thrilled to discuss"` / `"It would be an honor"` — over-deferential.
- `"I believe I would be a great fit"` — beggy. Show fit through evidence; do not assert it.
- `"Looking forward to your response"` — stock close. Replace with `"Open to a short call whenever suits you"` or similar.

### Em dashes
- **No em dashes anywhere in body prose.** Use commas, periods, parentheses, or line breaks. (Em dashes are allowed in template header eyebrow `Cover Letter — [YEAR]` and footer chrome `[Company] — [Role Title]` only.)
- This is a recruiter-flagged AI tell, codified into both the job-search rules and the voice profile.

---

## Defensible identity claims

The "what the candidate can credibly claim" ladder. Use this when sourcing claims for the lede, credibility paragraph, or three-things bricks. Cover-letter-grade claims must hold up under interview scrutiny.

### Can speak to credibly (genuine, ≥3 years)
- **Esports broadcast, VIP, and live-ops at scale** — 6+ years. Skybound Pro League (SPL) project lead since 2021.
- **Ambassador program design from scratch** — built and runs the Creator Ambassador Program (55 partnered creators, three-tier).
- **Vendor coordination, on-site logistics, creator care** — across 7 countries, 15+ offline events.
- **Multi-platform community ownership at 180K+ scale** — Reddit (120K), Instagram (45K), Discord (15K), Twitter/X, TikTok.
- **Community intelligence pipelines** — priority-tiered reports + daily sentiment digest, in production 6+ months without manual intervention.
- **AI workflow centralization for a small team** — citation-grounded knowledge base, 15+ role-specific assistants, human-in-the-loop review on every output, ~8 hours/week reclaimed across a 6-person team.
- **IP crossover events** — 6 crossovers with two major entertainment franchises under licensor approval, zero compliance escalations.

### Cannot speak to credibly (real gaps; acknowledge with honest-gap pattern when relevant)
- **B2B SaaS community at any scale.** No prior experience.
- **Pipeline-driven community-to-revenue motions.** Has never run community as a sales-pipeline input or owned ARR-tied metrics.
- **Technical developer community** — DevRel-flavoured roles where the community is engineers shipping integrations against an SDK. Has not done this.
- **No-code production builds on a target's own platform** — has used many no-code tools as a builder/integrator (self-hosted and no-code automation), but has not shipped a production app on a target's own no-code platform itself. Honest-gap-with-remediation territory.

### Honest-gap doctrine

When the JD or company shape exposes one of the gaps above, name it. Rules:
- **One gap, one paragraph, three sentences max.**
- **The gap must be real and visible** to a sharp reader. No softballs ("I'm new to working remotely" is not a gap).
- **Always pair with a concrete remediation.** Action in motion ("running a trial this week"), transferable proxy ("the same mechanism applied to a different audience"), or call-side commitment ("happy to talk through how I'd close that on the call"). Never just admit and dangle.
- **Never use the gap pattern for logistics.** Salary, location, notice period, visa — these go in the closer or are dropped.
- **Never overcorrect.** Two gaps in one letter reads as overcompensating and erodes credibility.

---

## Career direction framing

The candidate is deliberately moving from gaming into B2B SaaS / AI. Two stated reasons (use these, not invented ones):

1. **"AI/SaaS is where the most interesting community work is happening right now"** — the GEO / earned-voice-measurement category specifically resonates.
2. **"Wants to operate closer to product and business outcome than is possible in the current role at a 180K+ flagship inside a global publisher."**

### Banned framings for the move
- ❌ `"pushing away from gaming"` — even though the element is somewhat real, this framing is defensive and reads as escape rather than direction.
- ❌ `"leaving gaming behind"` / `"transitioning out of gaming"` — same.
- ❌ Apologetic framings (`"I know this is a different category, but"`).

Always frame as a **toward** move (toward AI/SaaS, toward product-proximate work), never an **away-from** move.

---

## Personal-hooks doctrine

The lede must reference a **real** reaction or recognition moment, not a packaged research framing. A founder-backstory recognition for a target company can be a real moment of recognition; "I have spent six years on the broadcast, VIP side of the same world" leverages it in low-key form (no name, no flex).

### Pass — personal hook
- Names a specific product, ritual, post, launch, or detail that genuinely caught attention.
- The "why this caught my attention" line is concrete (not "your mission resonates with me").
- Bridges to the candidate's actual work without flexing — kept low-key.

### Fail — packaged-research framing
- "After researching [Company] extensively, I am impressed by your..."
- "[Company]'s mission to [generic restatement of homepage] aligns with my values."
- Name-dropping the founder's name explicitly when a low-key reference would land better.
- Listing all the company's accolades back at them (they know).

### The company-anchor test (hard requirement on the opening)

The lede's opening (first 1-2 sentences) **must** contain a verifiable company-specific reference. This is not a preference — it is the single load-bearing rule that separates a hooky opener from a flat one, and the C5 gate hard-blocks on it.

**A verifiable company-specific reference is one of:**
- A named product, feature, or launch ("[Company]'s automation strand lists self-hosted automation").
- A quoted line from the JD or the company's own site ("passion for no-code and automation, used in a meaningful way").
- The company's stated thesis or positioning ("[Company]'s framing of GEO…", "[Company]'s framing of community as the heart of your brand").
- A named program, ritual, or recent public artifact (a changelog, a post, an event).
- A founder-background or product-history detail (kept low-key).

**Generic anaphora does NOT count as an anchor:** "this role", "your mission", "your team", "the position", "what you're building" are pointers, not references. An opener that leans on them is portable and fails the test.

**The paste test:** could the opening be pasted, unchanged, into a cover letter for a different company and still read as written-for-them? If yes → fail; sharpen it to lead with one of the references above.

### Engaging vs. flat openings (calibration)

| Flat (fails the paste test) | Engaging (anchored) | Why the anchored one hooks |
|---|---|---|
| "I add the most value through system design: modeling how a process works, then wiring the AI agents and automations that run it." (generic, portable) | "Your automation strand lists self-hosted automation as a plus. I have run a self-hosted automation stack in production for the Northwind Interactive EU team since 2024." | Opens on a detail only *this* JD has, then lands a matching production fact with a date — proof, not philosophy. |
| "I add the most value through system design… This role is scoped around that." (generic anaphora) | "[Company]'s framing of community as the heart of your brand caught my attention. Their loyalty program as a moat is the kind of identity-driven community I've spent 6 years building." | Names a specific brand element, signalling the letter was read against *their* materials, then bridges to matching scale. |

### Common hook starters (fallback when Personality Hooks haven't surfaced one)

Use these only when Step 0 personality research has produced no high-authenticity hook (the external-tracker `## Personality Hooks` section is empty or returned only weak signals).

- *"The [specific product / program / ritual] is what got me into this email."* (concrete, low-risk)
- *"I read [specific post / changelog / launch] last week and it lines up with what I've been running at Northwind Interactive for [X] years."*
- *"[Company] is one of a handful of teams where [specific thread] is treated as product work, not content support."*
- *"I applied because [one concrete reason], and because the [community / creator / events] side of [Company] looks like the natural next surface for the program I've been running."*

---

## Common feedback signals (and what they mean)

When the candidate pushes back on a draft, the feedback usually maps to one of these patterns. Recognising the pattern helps the skill propose a targeted revision rather than a generic rewrite.

| Feedback signal | Meaning | Fix |
|---|---|---|
| *"This sounds AI-generated"* | Banned phrase or vague-category framing slipped in | Identify the offending sentence, replace with concrete claim (number/tool/program/named property) |
| *"This feels forced / glued together"* | Paragraph is doing >1 job | Split into two paragraphs or two blocks |
| *"This is a blanket statement with nothing to back it"* | Adjective-only sentence; reasoning not visible | Add the number, tool, program, or named property; if there isn't one, delete the sentence |
| *"This doesn't connect to me"* | The bridge between two ideas is missing or implicit | Make the bridge explicit in one sentence; do not rely on the reader to infer it |
| *"This is too packed"* | Multiple proof-points crammed into one sentence | Split into separate sentences or move one to a different block |
| *"This is overclaiming"* | A claim that doesn't trace to defensible identity-claims ladder | Soften the framing or remove; never claim something not on the ladder |
| *"The voice isn't right"* | Tone is casual, defensive, or condescending — not calm/analytical/grounded | Re-read the credibility paragraph in `cover-letter-base.md` Variant A as the tone reference |

---

## What this profile does NOT govern

- **Variant selection (A/B/C/D).** That's `cover-letter-tailoring-rules.md`.
- **SPL inclusion/exclusion mechanics.** That's `cover-letter-tailoring-rules.md`.
- **Pattern selection (1/2/3).** That's `cover-letter-tailoring-rules.md`.
- **Pool block IDs.** That's `cover-letter-pool.md`.
- **Per-application title framing.** Asked at Phase A in `/tailor-cover-letter`; never defaulted.
- **HTML rendering.** That's `cover-letter-template.html`.
