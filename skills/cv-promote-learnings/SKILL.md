---
name: cv-promote-learnings
description: Review and merge unpromoted entries from cv-learnings.md into cv-master.md (new bullet variants in the pool) or cv-tailoring-rules.md (new selection rules), and drain the pool-polish queue (in-place prose-sharpening of flat-but-relevant master bullets). Use when the user says "/cv-promote-learnings", "review cv learnings", "promote cv learnings", "drain the pool-polish queue", or after /tailor-cv prompts that the learnings backlog is over threshold.
triggers:
  - /cv-promote-learnings
  - review cv learnings
  - promote cv learnings
  - merge cv learnings
  - cv learnings backlog
  - drain pool-polish queue
  - pool-polish candidates
---

# CV Promote Learnings — thin wrapper over the shared engine

The manual gate that moves durable patterns from `cv-learnings.md` into the locked content
(`cv-master.md` and `cv-tailoring-rules.md`). Every edit requires per-entry approval.

This skill runs **two independent passes** in sequence:

1. **Active-queue pass** — the shared engine drains `## Active (unpromoted)` by classification (`phrasing-preference`, `strand-rule`), running a corroboration scan that labels each candidate `strong`/`tentative` before sign-off (engine Step 3) so a single-instance learning is flagged, never promoted as a pattern silently.
2. **Pool-polish pass** — drains `## Pool-polish candidates` (flat-but-relevant master bullets flagged by `/tailor-cv` Step 10a), via in-place prose-sharpening. See `## Pool-polish draining mode` below.

The passes are independent: **pass 1 halting with nothing to promote does NOT skip pass 2.** Always run pass 2 when `## Pool-polish candidates` is non-empty, then print a combined receipt.

Run the promote-learnings engine (pass 1): `lib/promote-learnings-engine.md`

## Parameters

| Parameter | Value |
|---|---|
| `queue_path` | `templates/cv-learnings.md` |
| `target_files` | `cv-master.md` (phrasing-preference) · `cv-tailoring-rules.md` (strand-rule) |
| `classification_map` | `phrasing-preference`, `strand-rule` (2 types) |
| `archive_path` | `templates/cv-learnings.md` `## Archived` section |
| `per_entry_apply_action` | See below |
| `rules_file` (calibration) | `templates/cv-tailoring-rules.md` |

## Apply actions (per classification type)

**phrasing-preference:** Add a new bullet variant to `cv-master.md` with a fresh stable ID (never edit or delete the original — never-renumber rule). Show proposed diff before writing: `About to add to cv-master.md under <role section>: +<NEW-ID> <tags> +<new prose>. Confirm? (yes / cancel)`. On confirm, append to `## Promoted (audit trail)` in `cv-learnings.md`.

**strand-rule:** Append a selection rule to `cv-tailoring-rules.md` under the relevant section. Show proposed diff before writing. On confirm, append to `## Promoted (audit trail)` in `cv-learnings.md`.

## Pool-polish draining mode (pass 2 — additive)

Drains the `## Pool-polish candidates` section of `cv-learnings.md`. These lines are written by `/tailor-cv` Step 10a (a cheap fast model running as a critic): *picked* master bullets that are highly JD-relevant but read flat/generic in their current master prose — bullets the critic cannot reword in the CV itself (the defensibility fence forbids it), so they route here for a one-time pool fix.

**The fix is an in-place prose-sharpening rewrite of the bullet AT ITS STABLE ID in `cv-master.md`.** This is deliberately different from the `phrasing-preference` apply action (pass 1), which *adds a new variant*:

> **Sanctioned exception to "never edit the original prose."** Pool-polish rewrites the bullet in place because the flat prose carries no signal worth preserving — the claim is identical and the rewrite is strictly better-worded. The **stable ID never changes** (never-renumber rule holds), so every `picks:` reference, checkpoint, and learnings diff still resolves. Only the prose body is sharpened. This sits alongside the de-gaming neutralization (`cv-tailoring-rules.md` § non-gaming) as the second sanctioned exception to "selection + ordering, never rewriting" — conditional on a flat-but-relevant flag from the critic.

**Pool-polish entries never count toward the promotion threshold** — they live outside `## Active (unpromoted)`, which is the only section the threshold counts. Pass 2 is offered whenever `## Pool-polish candidates` is non-empty, regardless of threshold state.

### Step P1 — load the queue

Read `## Pool-polish candidates` from `cv-learnings.md`. Each entry is one line:

```
- <master_id> — read flat vs JD "<jd_line>" (<date>): <why>
```

If the section is absent or has zero entries: print `No pool-polish candidates.` and finish pass 2. **Do not error** (a never-flagged queue is the normal steady state).

### Step P2 — per-bullet sign-off walk (never bulk)

N = count of pool-polish entries. Walk each in order. For each:

1. Locate the bullet / project / highlight by `<master_id>` in `cv-master.md` and read its **current prose verbatim**.
2. Draft **one** sharper rewrite obeying every fence rule below. Prose-sharpening only — verb, structure, emphasis.
3. Present (and get explicit `rewrite` before any write):

   ```
   Pool-polish <i> of <N> — <master_id>
   Current:  "<current prose>"
   Proposed: "<sharper prose>"
   JD line:  "<jd_line>"   (the relevance the flat prose underdelivers on)
   Fence:    claim identical · numbers verbatim · no new scope/title/tool/% · no em dashes
   Decision: (rewrite / skip / drop)
   ```

4. Decision semantics:
   - **rewrite** → in-place `Edit` of the bullet at `<master_id>` in `cv-master.md` (replace the prose body; keep the ID heading, strand tags, and any parenthetical annotation). Then remove the entry from `## Pool-polish candidates` (tempfile-rename) and append a one-line note to `## Promoted (audit trail)` in `cv-learnings.md` (`### <date> — pool-polish: <master_id> sharpened in place; claim + numbers unchanged`).
   - **skip** → leave the entry in the queue. Re-appears next run.
   - **drop** → remove the entry from the queue WITHOUT editing `cv-master.md` (the flat read was a false positive / not worth a rewrite). Note the reason in `## Archived (not promoted)` if useful.

   Apply each decision **immediately** before the next entry. Never apply more than one at a time.

### The fence (hard — never relax)

Every proposed rewrite MUST:

- Keep the **underlying claim identical**. Sharpen prose only.
- Keep **every number from the Locked numbers registry verbatim** (and any period bracket attached to it).
- Add **zero new claims**: no scope, %, title, tool, team size, relationship, or duration not already in the entry.
- Carry **no em dashes** (`cv-master.md` is outward-facing). Use commas, periods, parens, line breaks.
- If a sharper phrasing would need a fact **not already in the entry**, STOP and ask the user — never invent to satisfy an "improvement" (only-stated-facts rule; a past fabricated-detail incident is the precedent).

Preserve the stable ID, its strand tags, and any parenthetical annotation — only the prose body changes.

### Step P3 — combined receipt

After pass 2, print one combined receipt covering both passes:

```
/cv-promote-learnings complete.
  Active-queue   — promoted: <a> (strong <s>, tentative <t>)  archived: <b>  skipped: <c>   (backlog: <n>, threshold: <THRESHOLD>)
  Pool-polish    — rewritten: <x>  skipped: <y>  dropped: <z>   (queue: <m>)
```

## What this skill does NOT do

- Does not auto-promote or auto-rewrite. Every edit (variant add, rule append, **or pool-polish in-place rewrite**) requires per-entry confirmation.
- Does not promote a single-instance learning as if it were a pattern. The corroboration scan (engine Step 3) labels it `tentative` and recommends skip; promoting anyway is the user's explicit call. Pool-polish (pass 2) is exempt — it's a one-time prose-quality fix, not a pattern promotion, so no confidence label applies.
- Does not delete original master entries in pass 1. New variants added with new stable IDs.
- In pass 2, rewrites prose **in place at the existing ID** — never renumbers, never changes the claim or any locked number.
- Does not edit `## JD-specific log` or `## Archived` sections (except to move a dropped pool-polish entry into `## Archived`).
- Does not run `/tailor-cv`.
