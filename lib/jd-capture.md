# jd-capture.md — shared application-folder + JD.md routine

Consumers: tailor-cv (JD-source step), tailor-cover-letter, and any lead/application logging skill.

Read on demand (detail-class — pure pointer from each citer). Do not auto-load.

Purpose: guarantee that by the time `/tailor-cv` or `/tailor-cover-letter` run, the
application folder already exists **and** contains a verbatim `JD.md` — so they never
have to ask the user to paste the JD. Whichever skill runs first (a lead-capture step at
lead time, or an application-logging step at submit time) populates it; later skills find
it and reuse.

Applications root: `applications/` (the working-output root, kept out of version control).

---

## Routine A — find-or-create the application folder (by slug, not date)

The folder name is `YYYY-MM-DD-<company-slug>`, but the **slug is the identity**, not the
date. A lead saved on the 10th and applied on the 14th must resolve to the *same* folder.
So always glob by slug first; only mint a new dated folder when none exists.

```bash
SLUG="<company-slug>"          # company lowercased, hyphenated (e.g. lumen-ai)
ROOT="applications"
# find (not a glob) so an unmatched slug doesn't error under zsh
EXISTING=$(find "$ROOT" -maxdepth 1 -type d -name "*-$SLUG" 2>/dev/null | head -1)
if [ -n "$EXISTING" ]; then FOLDER="$EXISTING"; else FOLDER="$ROOT/$(date +%F)-$SLUG"; mkdir -p "$FOLDER"; fi
echo "$FOLDER"
```

- **Match found** → reuse it silently (even if the date prefix differs from today).
- **No match** → create `<today>-<slug>`.
- Ambiguous slug (two different companies would collide, or a fuzzy match) → ask the user which folder.

`/tailor-cv` still owns copying `templates/cv-template.html` + `templates/portrait.png` into
the folder — this routine only guarantees the directory and `JD.md`, not CV scaffolding.

---

## Routine B — capture JD.md (tiered, LinkedIn-capable)

If `<folder>/JD.md` (or `JD.txt`) already exists and is non-trivial (> 200 chars), **stop —
it's already captured.** Never overwrite a captured JD.

Otherwise, with a JD URL in hand, capture in this order. Stop at the first that succeeds:

**Tier 1 — `fetch-jd.py` (default, no browser).** A small helper that handles LinkedIn via the
no-auth guest job API and everything else via a readability extractor:

```bash
python3 fetch-jd.py "<jd-url>" --out "<folder>/JD.md"
```

Exit 0 = `JD.md` written. Exit 2 = fetch failed (JS/auth wall, closed posting) → Tier 2.

**Tier 2 — a browser-automation CLI (LinkedIn fallback).** Only when Tier 1 exits 2 on a
LinkedIn URL. Drive a headless browser to load the guest-API fragment and read the rendered
description, then write `JD.md` yourself with the standard header (see format below):

1. Open the guest-API fragment (extract `<jobId>` from `/jobs/view/<id>` or a
   `currentJobId=<id>` query param):
   `https://www.linkedin.com/jobs-guest/jobs/api/jobPosting/<jobId>`
2. Read the rendered description, and grab the title / company from the page's
   `.show-more-less-html__markup`, `.topcard__title`, and `.topcard__org-name-link` nodes.
3. If still empty, open the full `/jobs/view/<id>` page, dismiss the login modal (press
   Escape), then re-read the markup node.
4. Write `<folder>/JD.md` with the captured text, then close the browser.

**Tier 3 — paste fallback (last resort).** If both tiers fail, ask the user:
*"Couldn't fetch the JD for `<company>` automatically (LinkedIn guest API + browser both
failed — posting may be closed or auth-walled). Paste the JD text and I'll write it to
`<folder>/JD.md`."* Write their verbatim paste, no edits.

**Never infer the JD from memory or summarize a URL.** Verbatim text only — the defensibility
gate in `/tailor-cv` depends on `JD.md` being the real posting.

### JD.md header format (when writing by hand — Tier 2/3)

```
# {Company} — {Role Title}

Source: {url}
Fetched verbatim {YYYY-MM-DD}.

{verbatim JD body}
```

`fetch-jd.py` already emits this shape, including a `Location:` line for LinkedIn.
