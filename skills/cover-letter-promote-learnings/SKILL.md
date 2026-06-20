---
name: cover-letter-promote-learnings
description: Review and merge unpromoted entries from cover-letter-learnings.md into cover-letter-pool.md (new block variants), cover-letter-tailoring-rules.md (new selection rules), or cover-letter-voice-profile.md (new banned phrases / tone rules), and drain the pool-polish queue (in-place prose-sharpening of flat-but-relevant pool blocks). Use when the user says "/cover-letter-promote-learnings", "review cover letter learnings", "promote cover letter learnings", "drain the cover-letter pool-polish queue", or after /tailor-cover-letter prompts that the learnings backlog is over threshold.
triggers:
  - /cover-letter-promote-learnings
  - review cover letter learnings
  - promote cover letter learnings
  - merge cover letter learnings
  - cover letter learnings backlog
  - drain cover-letter pool-polish queue
  - cover-letter pool-polish candidates
---

# Cover Letter Promote Learnings — thin wrapper over the shared engine

The manual gate that moves durable patterns from `cover-letter-learnings.md` into the locked
content (`cover-letter-pool.md`, `cover-letter-tailoring-rules.md`, `cover-letter-voice-profile.md`).
Every edit requires per-entry approval.

**Sibling to `/cv-promote-learnings`.** Same architectural pattern, different artifact set.

This skill runs **two independent passes** in sequence:

1. **Active-queue pass** — the shared engine drains `## Active (unpromoted)` by classification (`phrasing-preference`, `variant-rule`, `voice-rule`, `pattern-rule`), running a corroboration scan that labels each candidate `strong`/`tentative` before sign-off (engine Step 3) so a single-instance learning is flagged, never promoted as a pattern silently.
2. **Pool-polish pass** — drains `## Pool-polish candidates` (flat-but-relevant pool blocks flagged by `/tailor-cover-letter` Step 13a), via in-place prose-sharpening. See `## Pool-polish draining mode` below.

The passes are independent: **pass 1 halting with nothing to promote does NOT skip pass 2.** Always run pass 2 when `## Pool-polish candidates` is non-empty, then print a combined receipt.

Run the promote-learnings engine (pass 1): `lib/promote-learnings-engine.md`

## Parameters

| Parameter | Value |
|---|---|
| `queue_path` | `templates/cover-letter-learnings.md` |
| `target_files` | `cover-letter-pool.md` · `cover-letter-tailoring-rules.md` · `cover-letter-voice-profile.md` |
| `classification_map` | `phrasing-preference`, `variant-rule`, `voice-rule`, `pattern-rule` (4 types) |
| `archive_path` | `templates/cover-letter-learnings.md` `## Archived` section |
| `per_entry_apply_action` | See below |
| `rules_file` (calibration) | `templates/cover-letter-tailoring-rules.md` |

## Apply actions (per classification type)

| Classification | Target file | What to write |
|---|---|---|
| `phrasing-preference` | `cover-letter-pool.md` | New block variant with fresh stable ID (never-renumber rule) |
| `variant-rule` | `cover-letter-tailoring-rules.md` | New selection rule under "Variant selection table" or "Variant-fit signals" |
| `voice-rule` | `cover-letter-voice-profile.md` | New banned phrase, tone rule, or feedback-pattern entry |
| `pattern-rule` | `cover-letter-tailoring-rules.md` | New rule under "Pattern selection (1/2/3)" or "Three-things composition rules" |

Show proposed diff and confirm before writing each. On confirm, append to `## Promoted (audit trail)` in `cover-letter-learnings.md`.

## Pool-polish draining mode (pass 2 — additive)

Drains the `## Pool-polish candidates` section of `cover-letter-learnings.md`. These lines are written by `/tailor-cover-letter` Step 13a (the flat-block critic): *picked* pool blocks that are highly JD-relevant but read flat/generic in their current canonical pool prose — blocks the critic cannot reword in the letter itself (the defensibility gate forbids it: C1 claim-sourcing, C7 CV-CL consistency), so they route here for a one-time pool fix.

**The fix is an in-place prose-sharpening rewrite of the block AT ITS STABLE ID in `cover-letter-pool.md`.** This is deliberately different from the `phrasing-preference` apply action (pass 1), which *adds a new variant block*:

> **Sanctioned exception to "never edit or delete the original block."** Pool-polish rewrites the block in place because the flat prose carries no signal worth preserving — the claim is identical and the rewrite is strictly better-worded. The **stable ID never changes** (never-renumber rule holds), so every variant-table reference, checkpoint, and learnings diff still resolves. Only the prose body is sharpened, never the `[bracketed slots]`. This is the sanctioned exception to the pool's "tailoring is selection + light JD-grafting, never wholesale rewriting" rule (and to "never edit or delete the original block"). It is the cover-letter sibling of `/cv-promote-learnings` pass 2 (in-place sharpening of flat-but-relevant `cv-master.md` bullets). Conditional on a flat-but-relevant flag from the critic; outside this gate the pool is never hand-edited.

**Pool-polish entries never count toward the promotion threshold** — they live outside `## Active (unpromoted)`, which is the only section the threshold counts. Pass 2 is offered whenever `## Pool-polish candidates` is non-empty, regardless of threshold state.

### Step P1 — load the queue

Read `## Pool-polish candidates` from `cover-letter-learnings.md`. Each entry is one line:

```
- <block_id> — read flat vs JD "<jd_line>" (<date>): <why>
```

If the section is absent or has zero entries: print `No pool-polish candidates.` and finish pass 2. **Do not error** (a never-flagged queue is the normal steady state).

### Step P2 — per-block sign-off walk (never bulk)

N = count of pool-polish entries. Walk each in order. For each:

1. Locate the block by `<block_id>` in `cover-letter-pool.md` and read its **current canonical prose verbatim** (the block body — not the `>` annotation blockquote, not the `*~N words.*` footer).
2. Draft **one** sharper rewrite obeying every fence rule below. Prose-sharpening only — verb, structure, emphasis. Leave every `[bracketed slot]` exactly as-is (those are draft-time JD grafts, out of scope).
3. Present (and get explicit `rewrite` before any write):

   ```
   Pool-polish <i> of <N> — <block_id>
   Current:  "<current canonical prose>"
   Proposed: "<sharper prose>"
   JD line:  "<jd_line>"   (the relevance the flat prose underdelivers on)
   Fence:    claim identical · numbers/facts verbatim · no new scope/title/tool/%/relationship · gaps stay real gaps · no em dashes · brackets untouched
   Decision: (rewrite / skip / drop)
   ```

4. Decision semantics:
   - **rewrite** → in-place `Edit` of the block at `<block_id>` in `cover-letter-pool.md` (replace the prose body; keep the `### <block_id> <tags>` heading, the `>` annotation blockquote, every `[bracketed slot]`, and the `*~N words.*` footer). Then remove the entry from `## Pool-polish candidates` (tempfile-rename) and append a one-line note to `## Promoted (audit trail)` in `cover-letter-learnings.md` (`### <date> — pool-polish: <block_id> sharpened in place; claim + numbers/facts unchanged`).
   - **skip** → leave the entry in the queue. Re-appears next run.
   - **drop** → remove the entry from the queue WITHOUT editing `cover-letter-pool.md` (the flat read was a false positive / not worth a rewrite). Note the reason in `## Archived (not promoted)` if useful.

   Apply each decision **immediately** before the next entry. Never apply more than one at a time.

### The fence (hard — never relax)

Every proposed rewrite MUST:

- Keep the **underlying claim identical**. Sharpen prose only.
- Keep **every defensible number/fact verbatim** (concurrent-viewer counts, follower/partner-creator counts, country counts, dates, named tools, named programs, named partners) — and any period bracket attached.
- Add **zero new claims**: no scope, %, title, tool, team size, partner, relationship, or duration not already in the block.
- Keep any **gap framed as a real gap** (GAP-* blocks): the voice-profile defensible-claims ladder binds. Never sharpen a gap into an over-claim, never soften it into a non-gap.
- Carry **no em dashes** (`cover-letter-pool.md` is outward-facing body prose). Use commas, periods, parens, line breaks. (Voice-profile no-em-dash-in-body-prose rule.)
- Leave every **`[bracketed slot]` untouched** — those are filled by the `/tailor-cover-letter` fan-out at draft time, not here.
- If a sharper phrasing would need a fact **not already in the block**, STOP and ask the user — never invent to satisfy an "improvement" (only-stated-facts rule; a past fabricated-detail incident is the precedent).

Preserve the stable ID, its JD-strand tags, the annotation blockquote, and the word-count footer — only the prose body changes.

### Step P3 — combined receipt

After pass 2, print one combined receipt covering both passes:

```
/cover-letter-promote-learnings complete.
  Active-queue   — promoted: <a> (strong <s>, tentative <t>)  archived: <b>  skipped: <c>   (backlog: <n>, threshold: <THRESHOLD>)
  Pool-polish    — rewritten: <x>  skipped: <y>  dropped: <z>   (queue: <m>)
```

## What this skill does NOT do

- Does not auto-promote or auto-rewrite. Every edit (variant add, rule append, voice-rule add, **or pool-polish in-place rewrite**) requires per-entry confirmation.
- Does not promote a single-instance learning as if it were a pattern. The corroboration scan (engine Step 3) labels it `tentative` and recommends skip; promoting anyway is the user's explicit call. Pool-polish (pass 2) is exempt — it's a one-time prose-quality fix, not a pattern promotion, so no confidence label applies.
- Does not delete original pool entries in pass 1. New variants added with new stable IDs.
- In pass 2, rewrites prose **in place at the existing block ID** — never renumbers, never changes the claim or any locked number/fact, never touches a `[bracketed slot]`.
- Does not edit `## JD-specific log` or `## Archived` sections (except to move a dropped pool-polish entry into `## Archived`).
- Does not run `/tailor-cover-letter`.
- Does not interact with `/cv-promote-learnings`. CV and cover letter learnings are independent.
