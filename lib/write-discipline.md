# Write Discipline — Tempfile-Rename / Append-Only Race Mitigation

Consumers: build-story-bank (and any skill that writes to a notes store an editor app may hold open).

---

## Race-mitigation discipline

These skills write to files that an open editor (e.g. a notes app syncing the same folder) may be holding. Apply the discipline-based mitigation below — no lockfile is needed; the two mitigations are sufficient.

**For complete-note writes (the common case — `Write` to a new or fully-rewritten file):**
1. `Write` the full file body to `<target>.tmp` (adjacent to the target, same directory)
2. `mv -f <target>.tmp <target>` to swap atomically (same-volume rename guarantee on APFS)
3. Never use `Edit` mid-write of a file an open editor might be holding

**For append-only writes (less common — appending a section to an existing note or log file):**
1. Build the FULL modified contents in `<target>.tmp` adjacent to the target (tempfile-rename idiom)
2. `mv -f <target>.tmp <target>` (same atomic rename)
3. Append-only: the operation MUST NOT rewrite, reflow, reorder, or otherwise touch any existing content or frontmatter — only the new content is inserted at the end of the appropriate section
4. Write append-FIRST, THEN any related processed-flag flip (ordering is load-bearing — a flag flipped before the append succeeds strands a "done" record with its content lost)

**Never:**
- Use `Edit` tool mid-write on a file an editor may hold open
- Write directly to the final path without the tempfile-rename intermediate
- Skip the staleness/conflict check just because the write looks idempotent
