---
name: publishing-preparation
description: Take a finished, revision-passed manuscript and produce the actual retail/delivery artifacts a publishing channel requires — a laid-out, trim-sized, paginated print PDF, an EPUB, a DOCX, and a stripped Markdown master, packaged to a proven convention. Use when the user wants to (1) lay out a finished manuscript into a trim-sized print/PDF edition, e.g. "the manuscript's done, lay it out for print"; (2) generate EPUB/DOCX files for e-retail or Kindle Create, e.g. "make the KDP files"; (3) apply or extend the reusable in-book component library (pull quotes, callout boxes, chapter dividers, part-openers) to a chapter; (4) diagnose why running heads or page numbers disappeared from a rendered PDF; (5) package a finished book into the front-cover/back-cover/PDF/EPUB/DOCX/MD-master convention with a packaging README; or (6) check a laid-out book against current KDP/IngramSpark trim, bleed, and barcode requirements. Also trigger on "typeset my book", "build the print PDF", "package this book for KDP", "why did my page numbers disappear", "copy this book as the template for a new one".
---

# Publishing Preparation

A production-layout specialist. Takes a finished manuscript — never its prose — and produces the retail-ready package using one of two independently-built, already-proven pipelines.

**Read first, every session:** [identity.md](identity.md) (who you serve, what you do/don't, how you sound) → [rules.md](rules.md) (Always/Never, the pipeline-routing table, the exact `pagedjs-cli` refusal-gate language, empty-input handling). Both short; read both before touching any file.

## Then open only what the active situation needs

`rules.md`'s routing table tells you which of these to open — not all of them:

| Situation | Open |
| --- | --- |
| Fresh markdown chapter source, no existing structural template to copy | `reference/pipeline-css-paged-media.md` |
| An existing finished book's HTML/CSS in the family to copy as structural template | `reference/pipeline-structural-template.md` |
| Either pipeline's render is complete — packaging the delivery set | `reference/output-packaging.md` |
| Checking trim/bleed/margin/barcode before calling a build retail-ready | `reference/retail-technical-requirements.md` |
| Image resolution, alt text or captions; formats beyond the six-file set; "do the formats match?" | Not this skill: hand to [book-format-integrity-skill](https://github.com/NFTYoginis/book-format-integrity-skill), which takes this package and its source as input |

`examples.md` holds one worked illustration of the sharpest gotcha in this whole job — the `pagedjs-cli` vs. Chrome-headless render difference. It doesn't substitute for reading the active pipeline file.
