---
name: cv-checkpoint
description: Save mid-tailoring CV state to a checkpoint file under the active applications/YYYY-MM-DD-<co>/ folder, so a fresh context can resume tailoring after /clear. Use when the user says "/cv-checkpoint", "checkpoint the cv", "pause cv tailoring", or signals they want to /clear with CV work in progress. Scoped to the CV pipeline only — not cover letters, not general work pauses.
triggers:
  - /cv-checkpoint
  - checkpoint the cv
  - pause cv tailoring
  - save cv state
  - clearing with cv in progress
---

# CV Checkpoint Skill

Snapshots in-flight state from the `/tailor-cv` workflow to a markdown file in the active per-application folder, so a fresh post-`/clear` session can pick up tailoring without rebuilding context.

**Sibling to `/tailor-cv`.** Both operate on the same `applications/YYYY-MM-DD-<co>/` folder convention. `/tailor-cv` produces state; `/cv-checkpoint` snapshots it; the next `/tailor-cv` resumes from it.

**Scope:** CV tailoring only. Not cover letters, not general work pauses, not the broader job-search workflow.

## Paths

```
Applications root: applications/
Master pool      : templates/cv-master.md
Tailoring rules  : templates/cv-tailoring-rules.md
Learnings sidecar: templates/cv-learnings.md
```

(`applications/` is the working-output root, kept out of version control.)

## Flow

### 1. Detect active application folder

Follow the shared tailoring routine from `lib/tailoring-common.md` (folder-detect block + confirm gate).

Resolve the picked folder to an absolute path before proceeding. All checkpoint fields use absolute paths.

### 2. Capture state from working memory

The skill prompts the conversation context to dump current tailoring state into the schema below. **Do not reconstruct from the partially-written HTML** — work from what's in the active context. The skill is invoked at the moment the user wants to pause, so the state is fresh in conversation.

If a field is genuinely unknown (e.g., the JD strand wasn't picked yet), write `unknown` rather than guessing. Better to mark unknown than to write a wrong fact that the resume session will trust.

### 3. Read prior checkpoint timestamp (breadcrumb)

If `<folder>/_cv-checkpoint.md` already exists, read the existing `checkpoint_timestamp` value. Carry it forward as `previous_checkpoint_at` in the new file. This gives the resume session a one-step breadcrumb without keeping a full history.

If the file doesn't exist yet, set `previous_checkpoint_at: none`.

### 4. Write `<folder>/_cv-checkpoint.md`

Overwrite-on-write. One file per application folder; each `/cv-checkpoint` call replaces the prior contents (the `previous_checkpoint_at` line is the only memory of the prior call).

Schema — stable `## Field: <name>` headers, parseable by future skill updates:

```markdown
# CV Checkpoint — <Company>

> Mid-tailoring state snapshot. Written by /cv-checkpoint. Consumed by /tailor-cv on resume.

## Field: application_folder
<absolute path to applications/YYYY-MM-DD-<slug>/>

## Field: company
<Company name as it appears in your tracker>

## Field: role_title
<Role title from the JD>

## Field: jd_source
<absolute path to JD file (typically <folder>/JD.md), or URL if not yet captured to disk>

## Field: jd_strand_picked
<one of: #community | #events | #pm | #ai-ops | #social | unknown>

## Field: role_sub
<one of: RS-PM | RS-COM | RS-SOC | unknown>

## Field: profile_lead_noun
<one of: L-PM | L-COM | L-SOC | unknown>

## Field: picks
bullets_pm_2025: [<comma-separated B-PM-* IDs>]
bullets_scm_2023_2024: [<comma-separated B-SCM-* IDs>]
bullets_cm_2018_2022: [<comma-separated B-CM-* IDs>]
bullets_early: [<comma-separated B-EARLY-* IDs>, or empty if dropped]
projects: [<comma-separated P-* IDs>]
highlights: [<comma-separated H-* IDs>]
certs: [<comma-separated C-* IDs>]
tools: [<comma-separated T-* IDs>]
skills: [<comma-separated S-* IDs>]

## Field: sections_locked
- <list of pool names already finalised, e.g. "highlights", "certs">
- (or "none yet")

## Field: sections_draft
- <list of pool names with provisional picks>
- (or "all")

## Field: defensibility_check_status
<one of: not_run | passed | failed_with_notes>

## Field: defensibility_notes
<free text — only populated if status = failed_with_notes>

## Field: open_decisions
- <free-text "still need to decide" items>
- (or "none")

## Field: pending_sections
- <pools or steps not yet tailored>
- (or "none — gate-ready")

## Field: checkpoint_timestamp
<ISO-8601, e.g. 2026-05-02T15:42:00+02:00>

## Field: previous_checkpoint_at
<ISO-8601 of prior checkpoint, or "none">
```

**No HTML snapshot.** The in-progress `Resume_<co>.html` in the same folder is the artifact. Resume reads it directly.

### 5. Print confirmation

```
Checkpoint saved to <absolute path>.
Safe to /clear. Resume with: Resume CV tailoring from <absolute path>
```

That's the whole skill.

## Resume protocol (handled by /tailor-cv, documented here for reference)

When `/tailor-cv` runs and detects an existing `_cv-checkpoint.md` in the target folder, it:

1. Reads `cv-master.md`, `cv-tailoring-rules.md`, and the checkpoint file.
2. Warns if `checkpoint_timestamp` is more than 24h old: *"Checkpoint is N hours old. JD context may have shifted; continue?"*
3. Prints a resume summary:
   ```
   Resuming <Company> tailoring.
   Locked: <sections_locked>
   Pending: <pending_sections>
   Open decisions: <open_decisions>
   Defensibility: <defensibility_check_status>
   Continue?
   ```
4. Requires explicit confirmation before any further writes.

This protocol lives inside `/tailor-cv` Step 4. `/cv-checkpoint`'s job ends at writing the file.

## What this skill does NOT do

- Does not snapshot the HTML draft. The HTML is the artifact; resume reads it directly.
- Does not re-render or re-validate the in-progress CV. That's `/tailor-cv`'s job on resume.
- Does not modify `cv-master.md`, `cv-tailoring-rules.md`, or `cv-learnings.md`.
- Does not update any external tracker. Application logging is a separate skill.
- Does not close out the tailoring. Even on a clean checkpoint, the tailoring is still mid-flight.
