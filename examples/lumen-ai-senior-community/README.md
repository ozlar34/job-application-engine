# Worked example — Lumen AI · Senior Community Manager

> **All synthetic.** Every file in this folder is generated for the fictional persona **Alex Rivera** applying to the fictional company **Lumen AI**. No real person, company, role, or metric appears. This is a single end-to-end demonstration of what the crafting pipeline produces, so the engineering is visible without exposing real application data.

## What this folder shows

One job posting (`JD.md`) carried through the full crafting half of the system, from a pooled fact base to four submission-ready artifacts:

```
JD.md  ──►  /tailor-cv              ──►  Alex_Rivera_CV.md     +  Alex_Rivera_CV.html
       ──►  /tailor-cover-letter    ──►  Alex_Rivera_Cover_Letter.md  +  .html
       ──►  /build-interview-cheatsheet ──►  Lumen_AI_Interview_Cheatsheet.html
```

| File | Produced by | What to look at |
|---|---|---|
| `JD.md` | (input) | Fictional posting. Names 3 strands the rest of the pipeline maps onto. |
| `Alex_Rivera_CV.md` | `/tailor-cv` | The **tailoring trace** at the top: which pool IDs were picked from `templates/cv-master.md` and why. Nothing is rewritten, only selected and ordered. |
| `Alex_Rivera_CV.html` | `/tailor-cv` | The same picks rendered into `templates/cv-template.html`. Print to PDF: Cmd+P, Margins None. |
| `Alex_Rivera_Cover_Letter.md` | `/tailor-cover-letter` | Variant D + the three-things JD-map. Note the esports/90K facts are **omitted** for this AI/dev-tooling target (a built-in rule). |
| `Alex_Rivera_Cover_Letter.html` | `/tailor-cover-letter` | Rendered cover letter. |
| `Lumen_AI_Interview_Cheatsheet.html` | `/build-interview-cheatsheet` | Spoken cheatsheet. Behavioral cards trace to story-bank IDs **S-001** and **S-002** (see `templates/story-bank.template.md`, both tagged `Lumen AI (2026-05)`). |

## The defensibility thread

The point of the example is the chain, not the prose:

- Every `<strong>` number in the CV traces to the **Locked numbers registry** in `templates/cv-master.md`.
- Every CV bullet traces to a stable pool ID (`B-*`, `P-*`, `H-*`).
- Every spoken claim in the cheatsheet traces back to the CV, the cover letter, or a story-bank row.
- The cover letter's three-things headings are copied verbatim from `JD.md`.

That traceability is what lets the system tailor aggressively without ever fabricating a claim.

## No PDFs here

PDFs are gitignored (they are the print output, not source). Open any `.html` in a browser and Cmd+P → Save as PDF → Margins: None to produce the submission file. Portraits are gitignored too, so the CV's portrait slot renders empty in this checked-in copy.
