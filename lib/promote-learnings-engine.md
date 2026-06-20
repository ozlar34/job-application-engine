# promote-learnings-engine.md — shared promote-learnings mechanics

Consumers: cv-promote-learnings, cover-letter-promote-learnings.

Read on demand — never auto-loaded. Each consumer skill invokes this engine with its own
`## Parameters` block.

---

## Parameters

Each wrapper skill must supply the following parameters before invoking the engine:

| Parameter | Description |
|---|---|
| `queue_path` | Path to the learnings queue file — a literal path (cv + cover-letter both pass literal paths). |
| `target_files` | File(s) the approved learnings are spliced into (cv-master.md + tailoring-rules; cover-letter-pool.md + tailoring-rules + voice-profile). |
| `classification_map` | Ordered list of promotable classification types for this skill. cv: `phrasing-preference`, `strand-rule` (2 types). cover-letter: `phrasing-preference`, `variant-rule`, `voice-rule`, `pattern-rule` (4 types). |
| `archive_path` | Destination for archived entries (static for cv/cover-letter). |
| `per_entry_apply_action` | Skill-specific apply action — what to write to `target_files` on `promote`, as documented in the wrapper's own `## Apply actions` section. |

---

## Step 1 — Load the queue

`queue_path` is a literal path. Read the queue file in full. Parse all entries (each starts
with a `## ` heading).

If the file doesn't exist or has zero `## ` entries:

```
Queue empty (or file missing): <queue_path>
Nothing to promote. Done.
```

Stop here.

---

## Step 2 — Filter to promotable entries

From `## Active (unpromoted)`, surface only entries whose `classification` field matches one of the `classification_map` types supplied by the wrapper.

Typo and JD-specific entries are never in the promotable set — typos are fixed at capture; JD-specific entries go to the JD-specific log. If any are found misclassified in `## Active`, ask the user where to move them.

If zero promotable entries:

```
No promotable learnings to review.
Active backlog: <N> total (typo/JD-specific entries skipped).
```

Halt here.

---

## Step 3 — Confidence scan (corroboration before promotion)

The target files are **locked content read on every future tailoring run** — `cv-master.md`, the pool, the rules files, the voice profile. A rule promoted from a single one-off reaction doesn't sit quietly; it fires on every subsequent application. So a learning seen **once** is a *hypothesis*, not a pattern, and promoting it spends durable surface on thin evidence.

The corroboration evidence is the learnings file itself. Step 1 already loaded it in full, so this step is reasoning over what's already in context, not new reads.

For each promotable entry, look for other instances of the **same underlying pattern** — the same *preference*, not the same wording (e.g. "lead community over AI", "drop the open-line on targeted applications", "prefer active verb X over passive Y"). Check three places:

- other `## Active (unpromoted)` entries — repetition here is direct corroboration;
- `## Promoted (audit trail)` — a prior promotion of the same pattern is strong corroboration, and may mean **the rule already exists**; flag the entry as redundant rather than promoting a near-duplicate;
- `## Archived (not promoted)` — a prior *archive* of the same pattern is **contradicting** evidence: the user already decided once it wasn't worth locking. Surface that, don't bury it.

Label each candidate:

- **strong** — the pattern recurs across ≥2 independent entries/applications, or a prior promotion corroborates it.
- **tentative** — single instance (this entry only), or contradicted by a prior archive.

Confidence never *blocks* promotion — the user is the authority on their own materials. What it changes is how a candidate is **presented**: a tentative one is surfaced honestly ("one data point — promoting this locks a rule from a single reaction; recommend skip unless you're confident it generalizes"), never dressed up as a pattern. Never manufacture corroboration to upgrade a candidate to `strong` (the only-stated-facts / no-fabrication rule applies — a past fabricated-detail incident is the precedent).

---

## Step 4 — Per-entry walk

N = count of promotable entries. Walk each in order.

For each entry, surface:

```
Entry <i> of <N> — <date> <Company> [<jd-strand-tags or title>]

Diff:
  Source line `<STABLE-ID or target>`: "<original prose excerpt>"
  → final:                              "<edited prose excerpt>"

Classification: <value from classification_map>
Reason:         <user's one-line reason>
Confidence:     <strong | tentative>   (from the Step 3 scan)
Evidence:       <corroborating instances by date+Company, or "single instance — this entry only"; note any contradicting archive, or "rule already exists" if a prior promotion covers it>

Proposal:
  <per_entry_apply_action from wrapper — what will be written to target_files>
  <if tentative: append one honest line — "single data point; recommend skip unless you're confident this generalizes">

Decision: (promote / archive / skip)
```

**Decision semantics:**

- **promote** → apply the `per_entry_apply_action`; move the entry from `## Active` to `## Promoted (audit trail)` with today's date, a one-line summary of what changed, **and the confidence label** (`[strong]` / `[tentative]`). The label persists so a later run — or an investigation into a rule that's misfiring — can find which rules were locked on thin evidence.
- **archive** → move the entry from `## Active` to `## Archived (not promoted)` with today's date and (optionally) a one-line reason. (A `tentative` candidate the user isn't confident in lands here — archiving it leaves a contradicting-evidence breadcrumb if the same pattern resurfaces later.)
- **skip** → leave in `## Active`. Will appear again next run.

After the user decides, apply the action immediately before moving to the next entry.

---

## Step 5 — Tempfile-rename splice (queue mutation)

On every `promote` or `archive` decision, remove the entry from the queue file via tempfile-rename:

1. Read `$QUEUE_PATH`.
2. Splice out the entry block (the `## ` heading line through the line before the next `## ` heading or EOF).
3. Write to `${QUEUE_PATH}.tmp`.
4. `mv -f "${QUEUE_PATH}.tmp" "$QUEUE_PATH"`.

Never mutate the queue file in-place — the file may be open in an editor.

If `$QUEUE_PATH` has frontmatter, preserve it when splicing. If splicing leaves an empty body (zero `## ` entries remain), keep the frontmatter intact.

---

## Step 6 — Loop or finish

After each entry, continue to the next entry without asking "continue?" — the user invoked this skill to drain the queue, walking all N entries is the contract.

**Exception:** if the user says "stop" or "pause" inline at any point, stop processing further entries. Print the partial receipt for entries already handled.

After the last entry, print the summary receipt:

```
<skill-name> promote run complete.
  Promoted: <N>  (strong: <s>, tentative: <t>)
  Archived: <M>
  Skipped:  <K>
Active promotable backlog: <new count> (threshold: <THRESHOLD>)
```

The `(strong / tentative)` split makes thin-evidence promotions visible at a glance — if a run is mostly `tentative` promotes, that's a signal the queue is accumulating one-offs rather than patterns, worth a second look.

Where `<THRESHOLD>` is re-read from the wrapper's rules-file parameter (calibration may have changed it).

If `new count < THRESHOLD`, no further alert. If still over threshold, mention it but don't block: *"Still over threshold. Re-run when ready, or the tailoring skill will re-prompt at the start of the next run."*

---

## Step 7 — Calibration check

After 3-4 promote runs, if the user mentions the threshold feels wrong, suggest editing the THRESHOLD constant in the caller-supplied rules-file:

- *"Threshold too chatty (alerts before there's a real pattern)? → raise to 10."*
- *"Threshold never trips? → lower to 5."*

Edit the `THRESHOLD = 8` line in the rules-file on the user's call.

---

## Hard rules

- **Never** apply a promote action without showing the diff and getting explicit `promote` confirm.
- **Never** promote a one-off as a pattern. Every entry carries a confidence label from the Step 3 corroboration scan; a single-instance (`tentative`) candidate is surfaced as such with an honest skip-recommendation. Never manufacture corroboration to upgrade a candidate to `strong`.
- **Never** mutate the queue file silently. Every entry removal happens via tempfile-rename AFTER its decision is processed.
- **Never** apply more than one entry's action at a time — each entry is walked, confirmed, and committed independently.
- **Never** treat the queue file as a free-form scratchpad.
- **Always** preserve queue frontmatter when splicing.
- **Always** record the confidence label in the `## Promoted (audit trail)` entry on promote — it's the only durable record of how strong the evidence was.
