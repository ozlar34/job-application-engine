---
name: cover-letter-checkpoint
description: Save mid-tailoring cover letter state to a checkpoint file under the active applications/YYYY-MM-DD-<co>/ folder, so a fresh context can resume the per-block sign-off drafting after /clear. Use when the user says "/cover-letter-checkpoint", "checkpoint the cover letter", "pause cover letter tailoring", or signals they want to /clear with a cover letter in progress. Scoped to the cover letter pipeline only — not CVs, not general work pauses.
triggers:
  - /cover-letter-checkpoint
  - checkpoint the cover letter
  - pause cover letter tailoring
  - save cover letter state
  - clearing with cover letter in progress
---

# Cover Letter Checkpoint Skill

Snapshots in-flight state from the `/tailor-cover-letter` workflow to a markdown file in the active per-application folder, so a fresh post-`/clear` session can pick up tailoring without rebuilding context.

**Sibling to `/tailor-cover-letter`.** Both operate on the same `applications/YYYY-MM-DD-<co>/` folder convention. `/tailor-cover-letter` produces state; `/cover-letter-checkpoint` snapshots it; the next `/tailor-cover-letter` resumes from it.

**Sibling to `/cv-checkpoint`.** Same architectural pattern, different artifact. CVs and cover letters can be in flight simultaneously and have separate checkpoints in the same folder.

**Scope:** Cover letter tailoring only. Not CVs, not general work pauses, not the broader job-search workflow.

## Paths

```
Applications root : applications/
Pool              : templates/cover-letter-pool.md
Tailoring rules   : templates/cover-letter-tailoring-rules.md
Voice profile     : templates/cover-letter-voice-profile.md
Learnings sidecar : templates/cover-letter-learnings.md
```

(`applications/` is the working-output root, kept out of version control.)

## Flow

### 1. Detect active application folder

Follow the shared tailoring routine from `lib/tailoring-common.md` (folder-detect block + confirm gate).

Resolve the picked folder to an absolute path. All checkpoint fields use absolute paths.

### 2. Capture state from working memory

The skill prompts the conversation context to dump the current cover letter tailoring state into the schema below. **Do not reconstruct from a partially-written .md or .html** — work from what's in the active context. The skill is invoked at the moment the user wants to pause, so the state is fresh.

If a field is genuinely unknown (e.g., variant has not been picked yet), write `unknown` rather than guessing. Better to mark unknown than to write a wrong fact that the resume session will trust.

### 3. Read prior checkpoint timestamp (breadcrumb)

If `<folder>/_cl-checkpoint.md` already exists, read the existing `checkpoint_timestamp` value. Carry it forward as `previous_checkpoint_at` in the new file. This gives the resume session a one-step breadcrumb without keeping a full history.

If the file doesn't exist yet, set `previous_checkpoint_at: none`.

### 4. Write `<folder>/_cl-checkpoint.md`

Overwrite-on-write. One file per application folder; each `/cover-letter-checkpoint` call replaces the prior contents (the `previous_checkpoint_at` line is the only memory of the prior call).

Schema — stable `## Field: <name>` headers, parseable by future skill updates:

```markdown
# Cover Letter Checkpoint — <Company>

> Mid-tailoring state snapshot. Written by /cover-letter-checkpoint. Consumed by /tailor-cover-letter on resume.

## Field: application_folder
<absolute path to applications/YYYY-MM-DD-<slug>/>

## Field: company
<Company name as it appears in your tracker>

## Field: role_title
<Role title from the JD>

## Field: jd_source
<absolute path to JD file (typically <folder>/JD.md), or URL if not yet captured to disk>

## Field: cv_jd_strand_picked
<the CV's primary JD strand, captured from <folder>/_cv-checkpoint.md at Phase A Step 4 of /tailor-cover-letter>
<one of: #community | #events | #pm | #ai-ops | #social | unknown | absent (no _cv-checkpoint.md)>

## Field: cv_role_sub
<the CV's role-sub line, e.g. "Project Manager (Community Operations)" — captured from _cv-checkpoint.md>
<or "absent" / "unknown">

## Field: title_framing_body
<title used in the cover letter body, picked at Phase A Step 8 — e.g. "Community Builder">
<or "unknown">

## Field: title_framing_subtitle
<subtitle used in contact block, picked at Phase A Step 8 — e.g. "Senior Community Manager">
<or "unknown">

## Field: variant
<one of: A | B | C | D | unknown>

## Field: pattern_set
[<list from {1, 2, 3} or "prose-only">]

## Field: jd_strand_priorities
- 01: <heading text from JD-vocabulary>
- 02: <heading text from JD-vocabulary>
- 03: <heading text from JD-vocabulary>
(or "not yet picked")

## Field: blocks_locked
- lede: <LD-* ID, or "unlocked">
- credibility: <CRED-* ID, or "unlocked">
- bricks: [<BRICK-* IDs by row order>, or "unlocked"]
- category: <CAT-* ID, "n/a", or "unlocked">
- gap: <GAP-* ID, "n/a", or "unlocked">
- close: <CLOSE-* ID, or "unlocked">

## Field: blocks_draft
- <list of block names with provisional drafts, or "all unlocked">

## Field: defensibility_check_status
<one of: not_run | passed | failed_with_notes>

## Field: defensibility_notes
<free text — only populated if status = failed_with_notes; one line per failed check>

## Field: open_decisions
- <free-text "still need to decide" items>
- (or "none")

## Field: pending_blocks
- <list of blocks not yet drafted>
- (or "none — gate-ready")

## Field: personality_hooks_used
- <one-line summary of which hooks from the research note's `## Personality Hooks` section were applied to the lede / closer>
- (or "none yet")

## Field: checkpoint_timestamp
<ISO-8601, e.g. 2026-05-03T22:14:00+02:00>

## Field: previous_checkpoint_at
<ISO-8601 of prior checkpoint, or "none">
```

**No HTML snapshot.** The in-progress `cover-letter.md` and `Cover_Letter_<Co>.html` in the same folder are the artifacts. Resume reads them directly.

### 5. Print confirmation

```
Cover letter checkpoint saved to <absolute path>.
Safe to /clear. Resume with: Resume cover letter tailoring from <absolute path>
```

That's the whole skill.

## Resume protocol (handled by /tailor-cover-letter, documented here for reference)

When `/tailor-cover-letter` runs and detects an existing `_cl-checkpoint.md` in the target folder, it (Phase A Step 5):

1. Reads pool + rules + voice-profile + learnings + checkpoint.
2. Warns if `checkpoint_timestamp` is more than 24h old: *"Checkpoint is N hours old. JD or company state may have shifted; continue?"*
3. Prints a resume summary:
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
4. Requires explicit confirmation before any further writes.

This protocol lives inside `/tailor-cover-letter` Phase A Step 5. `/cover-letter-checkpoint`'s job ends at writing the file.

## What this skill does NOT do

- Does not snapshot the `.html` or `.md` draft. Those are the artifacts; resume reads them directly.
- Does not re-render or re-validate the in-progress letter. That's `/tailor-cover-letter`'s job on resume.
- Does not modify `cover-letter-pool.md`, `cover-letter-tailoring-rules.md`, `cover-letter-voice-profile.md`, or `cover-letter-learnings.md`.
- Does not update any external tracker. Application logging is a separate skill.
- Does not close out the tailoring. Even on a clean checkpoint, the tailoring is still mid-flight.
- Does not interact with `/cv-checkpoint`. CV and cover letter checkpoints are independent files in the same folder.
