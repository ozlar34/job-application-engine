# tailoring-common.md — shared tailoring mechanics

Consumers: tailor-cv, tailor-cover-letter, cv-checkpoint, cover-letter-checkpoint.

Read on demand (detail-class — pure pointer from each citer). Do not auto-load.

---

## Folder-detect block

List the most-recently-modified application folders to identify the active one:

```bash
ls -1dt applications/*/ 2>/dev/null | head -5
```

(`applications/` is the working-output root — wherever you keep per-application folders locally; this typically holds real personal output and is kept out of version control.)

**Confirm gate:**

- **Zero candidates** modified today → ask: *"No application folder modified today. Which folder am I working with? (paste path or company slug)"*
- **One candidate** modified today, clearly the active one → use it silently.
- **Multiple candidates** modified today (mtime within ~2h of each other) → ask: *"Multiple folders touched today: [list]. Which one?"*

Resolve the picked folder to an absolute path before proceeding. All checkpoint and output fields use absolute paths.

---

## Folder gate

The folder gate has two branches depending on the caller:

**tailor-cv branch (new folder creation):**

Propose the folder name:

```
applications/<today>-<company-slug>/
```

Where `<today>` is today's date in `YYYY-MM-DD` format (local time) and `<company-slug>` is the company name lowercased and hyphenated.

Confirm before creating. On approval:
1. `mkdir -p` the folder.
2. Copy `templates/cv-template.html` → `<folder>/Resume_<Company-Slug-CapsCase>.html`.
3. Copy `templates/portrait.png` → `<folder>/portrait.png`.

**tailor-cover-letter branch (reuse existing CV folder):**

Do not fork a new directory. The folder was created by `/tailor-cv`. List subdirectories under `applications/` matching `*-<slug>/` and pick the one with a `Resume_<slug>.html` inside. If multiple match, ask which one.

---

## Threshold pre-check

Count entries in the learnings file under `## Active (unpromoted)` that are classified as the promotable types for this skill (caller-supplied — cv: `phrasing-preference` + `strand-rule`; cover-letter: `phrasing-preference` + `variant-rule` + `voice-rule` + `pattern-rule`). Typo and JD-specific entries do not count.

The threshold lives in the rules-file as `THRESHOLD = 8`. Re-read it from there each run — calibration may have changed it.

If `count >= THRESHOLD`, prompt (soft escalation, not a hard block):

```
📋 Still N promotable learnings unpromoted from last time (threshold: <THRESHOLD>).
Run /<promote-learnings-skill> first? (recommended) / skip and tailor anyway
```

If the user says skip, continue. If they run the promote-learnings skill first and come back, re-count on resume.

The learnings-file, rules-file, and promotable-types are caller-supplied (cv and cover-letter each supply their own paths and type lists).

---

## Force-load discipline

Always force-load the data files before any draft or pick action. Read in the explicit order specified in the skill. If any required file is missing, halt with: *"`<filename>` missing — required. Cannot proceed."*

---

## Resume protocol

On invocation, check for an existing checkpoint file in the target folder.

**If present:** prompt *"Found checkpoint from `<previous_checkpoint_at>`. Resume from it or start fresh? (resume / fresh)"*. If the user picks `resume`:
1. Read the checkpoint file.
2. If `checkpoint_timestamp` is more than 24h old, warn: *"Checkpoint is `<N>` hours old. Context may have shifted; continue?"*
3. Print the resume summary (fields vary by skill — use the checkpoint schema from the skill's own write-step).
4. On confirmation, restore picks and state into working memory and skip ahead to the first unlocked step.

**If absent or the user picks `fresh`:** continue with the skill's normal flow.

---

## Post-ship diff and classify

Triggered when the user signals ship (verbal: "shipped", "logged", "submitted", "applied"; or after the application-logging skill creates the tracker row).

1. **Diff `_original_draft.html` vs the current shipped file** using `diff -u`. Surface non-trivial diffs (skip whitespace-only and reordering diffs).
2. For each non-trivial diff, surface as a one-line item with Before/After/Source.
3. **One-keypress classification per diff** (classification types are caller-specific — cv uses typo/phrasing-preference/strand-rule/JD-specific; cover-letter adds variant-rule/voice-rule/pattern-rule).
4. After all diffs classified, count active promotable entries. If `count >= THRESHOLD`, prompt to run the matching promote-learnings skill.
5. Print: *"Reflection complete. Tailoring closed for `<Company>`."*
