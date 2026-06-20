# Cover Letter Pool

> **Synthetic showcase data.** Every fact, number, company, and program below is invented for the fictional persona **Alex Rivera** (see `cv-master.md` / repo README). It demonstrates the *structure* of the cover-letter block-pool the `/tailor-cover-letter` skill consumes — not a real letter. Replace the pool entries with your own defensible blocks to use the system.

> Locked pool of selectable cover-letter blocks with stable IDs. Consumed by `/tailor-cover-letter` via per-block selection (no re-pasting prose). Mirrors the role of `cv-master.md` in the CV pipeline.
>
> **Tailoring is selection + light JD-grafting, never wholesale rewriting.** Each block in this file is defensible — every claim ties back to `cv-master.md` or `positioning-statement.md`. JD-specific edits to a block (e.g., grafting in a JD's program name) are fine and expected; out-of-pool composition is not.
>
> **Stable ID rule:** every block carries an ID. IDs are never renumbered. New variants get fresh sequential IDs (`-02`, `-03`, …). Originals are preserved unless explicitly approved for deprecation via `/cover-letter-promote-learnings`.
>
> **JD-strand tags:** blocks are tagged with the JD strands they serve (`#community`, `#events`, `#pm`, `#ai-ops`, `#social`). The tags drive selection in `/tailor-cover-letter` Phase A.

---

## Block ID prefixes

| Prefix | Block class | Used at |
|---|---|---|
| `LD-*` | Lede / hook frames | Step 10 (lede draft) |
| `CRED-*` | Credibility paragraph variants (A/B/C/D) | Step 11 (credibility draft) |
| `BRICK-*` | Three-things proof bricks (one per JD-strand) | Step 12 (Pattern 1) |
| `GAP-*` | Honest-gap callouts with paired remediation | Step 13 (Pattern 2) |
| `CAT-*` | Category-fluency paragraphs | Step 14 (Pattern 3) |
| `CLOSE-*` | Closing logistics blocks | Step 15 (closer) |

> **Title framing is NOT a pool entry.** Per `cover-letter-tailoring-rules.md`, the title used in the body and contact subtitle is picked per-application at Phase A, not selected from this pool.

---

## Lede frames (LD-*)

> Pick ONE per application. Lede is 50-65 words. Must reference a real reaction or recognition moment, not packaged-research framing. Personality Hooks from the company research note (Step 0) override these frames when available.

### LD-VARIANT-D-JD-VOCAB `#pm` `#community` `#events` `#ai-ops`

> The Variant D structural lede. Use when the JD names 2-3 candidate strands by program name or by their actual vocabulary. Quote the JD's own language verbatim in the heading.

The [Company role-name] role is the first I have seen where [JD strand 1], [JD strand 2], and [JD strand 3] are named as one mandate. [Optional one-line beat — e.g. "I read it twice to make sure."] That is the seat I have been building toward at Northwind Interactive since 2024.

*~55 words.*

### LD-RECOGNITION-FOUNDER `#community` `#events`

> Personal-recognition lede when there is a real founder-background or product-history connection. Kept low-key (no name-drop, no flex). Source pattern: Lumen AI lede (a founder's stated background → Alex's six years on the broadcast/VIP side).

[One sentence anchored on the company's stated thesis or framing — e.g. "Lumen AI's framing that 'what others say about your product matters more than what you say about it' describes the thesis I have been operating on the human side for the last six years as a Community Builder."] [One bridge sentence connecting that thesis to Alex's actual work shape — ambassador programs, creator advocacy, community-led brand work — all resting on the same idea: earned voice scales further than owned voice.] [One closing sentence naming the JD's specific scope: e.g. "The [role] mandate scopes [JD strand 1], [JD strand 2], and [JD strand 3] into one seat that I usually see split across three roles."]

*~95 words. Slightly longer than other ledes; absorbs the credibility-paragraph thesis statement.*

### LD-PRODUCT-RITUAL `#community` `#ai-ops`

> Lede that anchors on a specific product, ritual, post, launch, or detail that genuinely caught attention. Source pattern: Lumen AI lede (Lumen AI's positioning as an AI-native no-code platform for business apps).

[Company]'s [specific product / positioning / launch / public artifact] is what got me into this email. The reason: [one sentence connecting that artifact to a single concrete strand of Alex's actual work — e.g. "the line about 'passion for no-code and automation, used in a meaningful way' is the most accurate single line for what my actual job has been for the last year, centralizing Northwind Interactive's community team toolchain off legacy SaaS and onto a self-hosted automation + AI-assistant stack with role-specific assistants behind a Human-in-the-Loop review gate."]

*~75 words.*

### LD-RECENT-POST `#community` `#ai-ops`

> Anchor on something the company published recently (changelog, blog post, launch). Use when there is a concrete artifact to reference. Source: `cover-letter-base.md` common hook starters.

I read [specific post / changelog / launch / release thread / public statement] last week and it lines up with what I have been running at Northwind Interactive for [X] years. [One bridge sentence naming the connection — what shape of work the post describes, and which strand of Alex's scope matches.]

*~50-65 words.*

### LD-CATEGORY-FRAME `#community` `#events`

> Position the company as one of a small set of teams treating a specific thread the way Alex wants to. Source: `cover-letter-base.md`. **Caution:** the "small set of teams treating X as a category" lexicon is on the C5 banned-phrase list when it slips into AI-generated boilerplate. Use this frame ONLY when the named thread is unusually concrete (a specific product surface, a named program, a named ritual) — never with abstract "community-as-product" language.

[Company] is one of a handful of teams where [specific thread — forum contributors / changelog discussions / community Spaces / changelog rituals / Ambassador Programs] is treated as product work, not content support. [One sentence naming why that frame matters, with a specific reference to a product surface or program.] That is the shape of work I have been running at Northwind Interactive for the last 6 years.

*~65 words.*

### LD-CONCRETE-WHY `#community` `#events`

> Direct, no-preamble lede when the JD is solid but no obvious personality hook surfaced. Source: `cover-letter-base.md`. Lower-risk fallback.

I applied because [one concrete reason — a specific program, a named tool, a particular customer set], and because the [community / creator / events] side of [Company] looks like the natural next surface for the program I have been running at Northwind Interactive for [X] years.

*~50 words.*

---

## Credibility paragraph variants (CRED-A / B / C / D)

> Pick ONE per application based on role shape (see `cover-letter-tailoring-rules.md` variant table). Each is ~85-100 words. These are the locked, defensible scope-statements — JD-specific grafting (e.g., naming a different proof-point in the second sentence) is fine; full rewrites are not.

### CRED-A — Broad (default for non-esports targets) `#community` `#ai-ops` `#pm`

> Current PM scope leads. Community intelligence pipeline + AI centralization are the concrete proof points. SPL is named once, in a subordinate clause — or omitted entirely for AI/SaaS/no-code/dev-tooling targets per the SPL rule in `cover-letter-tailoring-rules.md`.

I own community and social operations for Northwind Interactive's flagship mobile RPG franchise: a 180K+ multi-platform player community across 120K Reddit, 45K Instagram, and 15K Discord, a weekly community intelligence pipeline feeding HQ, and the AI workflow centralization that standardized the 6-person EU team's toolchain (shared knowledge base, Human-in-the-Loop review on every output). I was promoted to Project Manager (Community Operations) in 2024 after 6 years scaling the program across 7 countries, building the Creator Ambassador Program with agency partnerships and running the franchise's global esports league at peak 90,000 concurrent viewers.

*~95 words. Drop the "global esports league at peak 90,000 concurrent viewers" clause for AI/SaaS/no-code/dev-tooling targets.*

### CRED-B — SPL-forward (esports / gaming-native roles only) `#events` `#community`

> Lead with the league. Use when the role is actually about esports / live ops / large-audience event production. Do not use for general community or AI-forward roles.

I run community and social operations for Northwind Interactive's global esports league, the Skybound Pro League, which peaked at 90,000 concurrent viewers across 4 regions and 12 broadcast languages. The scope covers a 180K+ multi-platform community (120K Reddit, 45K Instagram, 15K Discord), on-site event production at world finals, VIP creator care, and the Creator Ambassador Program across agency partnerships. I was promoted to Project Manager (Community Operations) in 2024 after running the program for 6+ years across 7 countries.

*~90 words.*

### CRED-C — Ops / PM-forward (process-heavy seats) `#pm` `#ai-ops`

> Lead with the promotion and the process work (tool migration, SOPs, AI toolchain). Community scope is the foundation, not the headline. Use for ops-heavy PM roles or any PM seat.

I led Northwind Interactive's project-management migration to a single workspace (two-tier board architecture, SOP authored, legacy tooling retired on schedule) and centralized the team's AI toolchain: shared knowledge base, 15+ role-specific assistants, Human-in-the-Loop review protocols, and a weekly community intelligence pipeline that runs itself. On the back of that work I was promoted to Project Manager (Community Operations) on the 6-person community and social team in 2024. That operational scope sits on top of 6 years running community and social for Northwind Interactive's flagship mobile RPG franchise, including its 180K+ multi-platform community and the global esports league at peak 90,000 concurrent viewers across 4 regions and 12 broadcast languages.

*~95 words.*

### CRED-D — Structured (highly-specific JDs, no SPL) `#community` `#ai-ops` `#pm`

> Use when the JD names 2-3 of the candidate's strands by their actual program names. Pairs with LD-VARIANT-D-JD-VOCAB. The credibility paragraph IS this block (no separate hook beyond the lede); the structural patterns (three-things, gap, fluency) follow.

I run community and social operations for Northwind Interactive's flagship mobile RPG: a 180K+ multi-platform community across Reddit (120K), Instagram (45K), and Discord (15K), plus the Creator Ambassador Program. In 2024 I was promoted to Project Manager (Community Operations) after six years on the program, with a mandate covering [no-code / AI tooling / project management systems — pick what fits the JD] for our 6-person distributed EU team. That has meant [one or two specific proof-points: self-hosted automation stack, 15+ role-specific assistants behind a human-in-the-loop review gate, community intelligence pipeline running six months without manual intervention feeding HQ directly].

*~95 words. SPL, "global esports league", "90,000 concurrent viewers" are OMITTED in this variant unless the JD itself touches gaming/esports/live ops.*

---

## Three-things proof bricks (BRICK-*)

> Selected when Pattern 1 (numbered three-things JD-map) is in use. Pick exactly 3 per letter. Headings echo JD vocabulary verbatim — bricks below carry **proof bodies only**; the heading is constructed at draft time from the JD. Bodies are ≤60 words and contain at least one number, named tool, named program, or named property.

### BRICK-COM-MULTIPLATFORM `#community` `#events`

> Multi-platform community ownership at scale, with events run as one operation alongside community. Source pattern: Lumen AI three-things row 01.

I own EU community and events for the franchise: a 180K+ player community across Reddit, Instagram, and Discord, and 15+ offline events delivered across 7 countries (player meetups, regional finals, partner activations, IP crossover events). The community calendar and the events calendar are run as one operation, which is what kept the load manageable on a small team. Crossovers with two major entertainment franchises ran through the same pipeline under strict licensor approval.

*~65 words.*

### BRICK-AMB-LOYALTY `#community` `#events` `#pm`

> Creator Ambassador Program — the canonical ambassador-program-built-from-scratch proof. Source pattern: Lumen AI three-things row 02.

The Creator Ambassador Program I built in 2021 and run today is a tiered ambassador program covering 55 partnered creators, since adapted by Northwind teams in Asia and the Americas. Commitments to a recurring Twitch cadence are matched with paid campaigns, in-game rewards, and full-expense invitations to offline events, with up to 40% engagement lift during event windows. Monthly KPIs flow through a Twitch and YouTube API tool I shipped.

*~70 words.*

### BRICK-EVT-VIPCREATOR `#events`

> Most recent VIP creator event proof point. Source pattern: Lumen AI three-things row 03.

Most recent reference: SPL 2023 World Finals in Lisbon. 5-day VIP programme for 80+ players and creators from 12+ countries: welcome party, day-prior team-building, and aftershow, with 250+ Instagram posts and 600+ stories from 30 partnered creators and 30+ pre-event Twitch promo streams. The broadcast side (4 regions, 12 languages, 90,000 peak concurrent) sits behind the same operation.

*~70 words. Drop the "broadcast side" sentence for AI/SaaS/no-code/dev-tooling targets per the SPL rule.*

### BRICK-COM-INTEL `#community` `#ai-ops`

> Community intelligence pipeline — the canonical "voice-of-customer feeding product" proof point. Source pattern: Variant A transition paragraph + CRED-A.

The community intelligence pipeline feeds HQ directly and shaped the team's escalation protocol for localisation issues and product feedback. It turns daily Reddit, Discord, and Instagram traffic into a priority-tiered weekly report plus a chat-channel sentiment digest, in production for 6+ months with zero manual intervention.

*~50 words.*

### BRICK-AI-CENTRAL `#ai-ops` `#pm`

> AI workflow centralization — the canonical AI-toolchain ownership proof point. Source pattern: CRED-A, CRED-C, Variant D.

Centralizing the 6-person EU team's AI toolchain reclaimed ~8 hours per week across the team and retired legacy SaaS tooling on schedule. 15+ role-specific assistants, each behind a Human-in-the-Loop review gate, now run as the team's standard daily workflow, built on a self-hosted automation and AI-assistant stack with a shared knowledge base drawn from team SOPs.

*~60 words.*

### BRICK-PM-PROMOTION `#pm`

> 2024 promotion to PM, with the operational scope it unlocked. Source pattern: CRED-C.

I led Northwind Interactive's project-management migration to a single workspace: two-tier board architecture, SOP authored, legacy tooling retired on schedule. On the back of that work I was promoted to Project Manager (Community Operations) on the 6-person community and social team in 2024, formalising cross-functional coordination with HQ on rollouts, localisation escalations, and the weekly intelligence pipeline.

*~55 words.*

### BRICK-EVT-IPCROSS `#events` `#community`

> IP crossover events — the licensor-approved-event proof point. Source pattern: CV master B-PM, B-SCM bullets; Lumen AI row 01 partial.

6 IP crossover events delivered with two major entertainment franchises, all under strict licensor approval with zero compliance escalations. Crossover content drove a 25%+ engagement lift on the affected windows. The same coordination pipeline (HQ + licensor team + EU on-site) runs for every crossover and is the basis for how I would scope a multi-IP partnership program.

*~60 words.*

### BRICK-EVT-SPL `#events`

> SPL scope as a standalone three-things row. Use ONLY for esports / gaming-native / live-ops / large-broadcast roles per the SPL rule.

I have led the Skybound Pro League (SPL) since 2021. Cumulative reach during my ownership: 4 regions, 12 broadcast languages, peak 90,000 concurrent viewers, and 1.5M+ cumulative broadcast viewers across 40+ live streams.

*~45 words.*

### BRICK-LIVEOPS-SCALE `#pm` `#ai-ops` `#community`

> Live-ops operating-rhythm competency, **label-stripped for non-gaming targets**. Expresses the transferable strength built running a live global championship WITHOUT the esports label, so the Tier-1 competency survives the SPL-omit rule (where it would otherwise vanish entirely when SPL is dropped). Vocabulary uses the de-gaming neutralisation ("global championship", never "esports / broadcast / players / concurrent viewers") to stay consistent with the de-gamed CV (C5/C7). Defensibility: the numbers trace to `cv-master.md` (Locked numbers registry: 4 regions, 12 languages) + B-SCM-04; the operational detail (pre-set escalation paths, real-time calls) traces to B-SCM-05 / B-SCM-05b ("resolved localisation issues in near real time"); "one shared source of truth across time zones" is a persona-confirmed fact, so C1 clears it via the "stated by the user this session" source. **Do NOT use for gaming/esports/live-ops targets** — those route to BRICK-EVT-SPL, which carries the real viewer numbers; using both double-counts the same event. Source pattern: design session; promoted same day from defensible-only to richer operational phrasing.

For six years I ran a global championship delivered live across 4 regions and 12 languages, where nothing could be fixed after it aired. That meant pre-set escalation paths, one shared source of truth across time zones, and calls made in the moment, not over a sprint. It's the operating tempo I'd bring to [JD's live / fast-paced / high-growth strand].

*~55 words. Label-stripped by design — contains no "SPL", "esports", "broadcast", "concurrent viewers", or "90k", so it passes C3 cleanly as SPL-absent. The closing sentence is the graft point (name the JD's live/fast-paced strand). For gaming targets, use BRICK-EVT-SPL instead.*

---

## Honest-gap callouts (GAP-*)

> Pattern 2 blocks. Pick ONE if the JD or company shape exposes a gap on the defensible-claims ladder (see `cover-letter-voice-profile.md`). Three sentences max, one gap, paired with concrete remediation.

### GAP-B2B-SAAS `#community` `#pm`

> Use when the target is a B2B SaaS startup and the JD scope is community/PM. Source pattern: Lumen AI honest-gap callout (verbatim).

Caveat worth naming: I have not run community for a B2B SaaS startup before. The transferable mechanics are clear from the scope above; the SaaS-specific layer I would learn on the job.

*~30 words.*

### GAP-NOCODE-SHIPPED `#community` `#ai-ops`

> Use when the target is a no-code platform and Alex has not yet shipped a production app on that specific platform. Source pattern: Lumen AI honest-gap callout.

I have not yet shipped a production app on [Platform] itself. I am running a trial this week to build a small internal tool so we are working from the same reference point if we get to a call.

*~40 words.*

### GAP-DEV-COMM `#community`

> Use when the target is an AI/dev-tooling company and the JD leans technical-developer-community (DevRel-flavoured, integration-heavy). Honest acknowledgement that Alex's community work has been creator/player-side, not engineer-side.

I have not yet run a developer-facing community where the audience is engineers shipping integrations against an SDK. The transferable mechanics — multi-platform ownership, ambassador-program design, intelligence pipelines feeding product — apply directly; the developer-specific tone and rituals I would learn quickly given the team's existing surface.

*~50 words.*

### GAP-PIPELINE `#community` `#pm`

> Use when the JD explicitly names community-to-revenue pipeline ownership or ARR-tied community metrics. Honest acknowledgement that Alex has not run community as a sales-pipeline input.

I have not run community as a direct sales-pipeline input or owned ARR-tied community metrics. The closest analog in my current scope is the priority-tiered intelligence pipeline that already feeds HQ on product and localisation; reframing the same mechanics around revenue signals is a translation, not a category jump. Happy to walk through how I would scope that on the call.

*~60 words.*

---

## Category-fluency paragraphs (CAT-*)

> Pattern 3 blocks. Use ONLY when Alex has genuine insider knowledge of the target's category — the pattern collapses without specifics. See `cover-letter-voice-profile.md` for the "show usage, not just awareness" rule.

### CAT-NOCODE `#ai-ops`

> No-code category fluency. Source pattern: Lumen AI letter.

On no-code specifically: I am a heavy user of self-hosted no-code automation on the no-code side, and agentic coding assistants on the codegen side, so I have a working view of where each tool earns its keep. I follow [category thought leaders] and [category publication / feed], and I can place where [hosted AI-codegen tools] land against guardrailed business-app builders. The live tension in the category is between raw codegen and structured business-app builders. Both have a place; the line I draw is around production reliability and the user the platform is actually for.

*~110 words. The bracketed proper nouns are graft points — fill with the candidate's genuine category reference points; never ship placeholders.*

### CAT-AI-COMM `#community` `#ai-ops`

> AI / dev-tooling community fluency. Less developed than CAT-NOCODE; use cautiously and only when there is a real anchor. Update this block with each application that exercises it.

On the AI-tooling community specifically: [one sentence positioning Alex inside the category, naming a tool he uses, a publication he tracks, or a specific community surface]. [One sentence naming the live tension or debate — closed-source vs open-weights, MLOps vs AI engineering, hosted-platform vs self-hosted — with Alex's actual position]. [One sentence naming what Alex has personally used on each side of the line.]

*PLACEHOLDER. Tighten with a real AI/dev-tooling application. Do not ship as-is.*

### CAT-CREATOR-ECON `#community`

> Creator economy / creator-tooling category fluency. Use for creator-platform companies.

On creator tooling specifically: [one sentence naming the tools Alex follows or uses — capture/streaming tools, creator dashboards, platform extensions, etc.]. [One sentence naming the live tension — agency-mediated vs direct-creator monetization, tier-based vs flat-creator-deal structures, etc. — with Alex's actual position]. [One sentence naming the Creator Ambassador Program as Alex's hands-on reference for designing creator-economy structures.]

*PLACEHOLDER. Tighten with a real creator-economy application. Do not ship as-is.*

---

## Closing logistics blocks (CLOSE-*)

> Pick ONE per application. The closer is 30-50 words. No begging, no "thrilled," no over-deferential framings.

### CLOSE-LOGISTICS-WINK `#community` `#events` `#pm` `#ai-ops`

> Standard logistics close + one optional one-line wink that proves the JD was read end-to-end. Source pattern: Lumen AI close (a regional-office detail typically reserved for another team).

Amsterdam-based, no relocation needed. [Optional one-line wink — name a regional office, a program detail, a customer the company recently signed, or any other detail that shows the JD or careers page was read end-to-end.] Happy to share specifics on any of the above whenever suits you.

*~45 words.*

### CLOSE-LOGISTICS-PLAIN `#community` `#events` `#pm` `#ai-ops`

> No-wink variant for when the JD is sparse and there is no detail worth winking at. Plain logistics close.

Amsterdam-based, no relocation needed. Happy to share specifics on any of the above, or to walk through how the Northwind scope translates to [Company]'s [community / creator / events / ops] surface. Open to a short call whenever suits you.

*~40 words.*

### CLOSE-PROSE-TRANSITION `#community` `#events`

> Closer for prose-only fallback letters (no structural patterns). Pairs with the prose-fallback transition paragraph. Source pattern: prose-fallback letter close.

Happy to share specifics on any of the above, or to walk through how the Northwind scope translates to [Company]'s [community / creator / events] surface. Open to a short call whenever suits you.

*~30 words.*

### CLOSE-LOGISTICS-GAPFOLD `#community` `#pm`

> Closer that folds a one-sentence honest-gap acknowledgement into the logistics line. Use when GAP-* has been used inline earlier; this avoids a separate gap block + a separate close. Source pattern: Lumen AI close.

[One-sentence gap acknowledgement, e.g. "Caveat worth naming: I have not run community for a B2B SaaS startup before. The transferable mechanics are clear from the scope above; the SaaS-specific layer I would learn on the job."] Amsterdam-based, no relocation needed. Happy to share specifics on any of the above whenever suits you.

*~75 words. NOTE: this variant absorbs the gap block; do not use both GAP-B2B-SAAS as a callout AND CLOSE-LOGISTICS-GAPFOLD in the same letter.*

---

## How blocks compose into a letter

Per `cover-letter-tailoring-rules.md` Section "Composition":

**Default structured composition (Variants A/B/C/D with Pattern 1+2):**
```
LD-* (lede, 50-95 words)
CRED-* (credibility paragraph, 85-100 words)
[Pattern 1] BRICK-* × 3 (three-things, ≤60 words each body)
[Pattern 2] GAP-* (honest-gap, 30-60 words)
CLOSE-* (closer, 30-50 words)
```

**Default structured composition with Pattern 3 (category-fluency added):**
```
LD-* → CRED-* → BRICK-* × 3 → CAT-* → GAP-* → CLOSE-*
```

**Prose-only fallback (Hook → Credibility → Transition → Close, ≤300 words):**
```
LD-* (lede, 50-65 words)
CRED-* (credibility paragraph, 85-100 words)
[Transition paragraph] — drafted fresh from the JD's specific unlock + one or two BRICK-* paraphrased into prose
CLOSE-PROSE-TRANSITION
```

**Maximum block count per letter:** 1 LD + 1 CRED + 3 BRICK + 1 GAP + 1 CAT + 1 CLOSE. No letter uses two ledes or two credibility paragraphs.

---

## How to add a new block

When `/cover-letter-promote-learnings` promotes a `phrasing-preference` learning into this pool:

1. Identify the block class (LD / CRED / BRICK / GAP / CAT / CLOSE).
2. Generate the next sequential ID for the prefix family (e.g. if `BRICK-AMB-LOYALTY` exists alone in the AMB family, the new one is `BRICK-AMB-02`).
3. Tag with the JD strands the block serves.
4. Write the block prose (verbatim from the promoted learning, or tightened on confirm).
5. Add a `> Source pattern: <YYYY-MM-DD> <Company> <ID>` blockquote pointer at the end.
6. **Never edit or delete the original block** — both are valid until a deprecation is explicitly approved.

The `/cover-letter-promote-learnings` skill walks this for each promotable learning per-entry, so the rules above are the contract for how that skill writes into this file.
</content>
