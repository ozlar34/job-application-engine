# Phase D — Post-ship reflection (detail)

This is the full detail of the Phase D step referenced by `SKILL.md`. This is the cover-letter reflection loop, sibling to `/cover-letter-promote-learnings` (the manual gate that moves durable patterns into the locked content). Do NOT spawn a separate lifecycle skill — this is run inline at ship time.

Triggered when the user signals ship. Triggers:
- Verbal: "shipped", "logged", "submitted", "applied", "sent the cover letter"
- The application logging skill ran and created the tracker row

When triggered:

1. **Diff `_cl_original_draft.html` vs the current shipped `.html`** at `<folder>/Cover_Letter_<Co>.html` (or the .md if the user edited the .md instead). Use `diff -u` and surface non-trivial diffs (skip whitespace-only and reordering).

2. For each non-trivial diff, surface as a one-line item to the user:
   ```
   Diff #<N>:
     Before: "<original prose excerpt>"
     After:  "<edited prose excerpt>"
     Block source: <inferred LD-* / CRED-* / BRICK-* / GAP-* / CAT-* / CLOSE-* ID, or "out-of-pool"; if "out-of-pool", flag as a stronger signal>
   ```

3. **One-keypress classification per diff**:
   - **typo** → fix in `templates/cover-letter-pool.md` immediately (apply the typo correction at the source line), don't log to learnings.
   - **phrasing-preference** → append to `templates/cover-letter-learnings.md` `## Active (unpromoted)` with the diff, the source block ID, and the user's one-line reason.
   - **variant-rule** → append (variant-selection rule learning).
   - **voice-rule** → append (banned-phrase / tone-rule learning).
   - **pattern-rule** → append (pattern-selection rule learning).
   - **JD-specific** → append to `templates/cover-letter-learnings.md` `## JD-specific log` (never promoted, reference only).

4. After all diffs classified, count active promotable entries (`phrasing-preference` + `variant-rule` + `voice-rule` + `pattern-rule` in `## Active (unpromoted)`). If `count >= THRESHOLD`:

   ```
   📋 cover-letter-learnings.md has N promotable entries (threshold: <THRESHOLD>).
   Run /cover-letter-promote-learnings now to merge into the pool / rules / voice-profile, or skip and I'll prompt at the start of the next cover letter.
   ```

5. Print: *"Reflection complete. Cover letter tailoring closed for `<Company>`."*
