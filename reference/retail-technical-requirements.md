# Retail technical requirements — KDP / IngramSpark

**Checked live 2026-09-22** during this build, per the brief's own instruction: this is the one place the internal pipelines' evidence (both built and documented before this check) could have drifted from external platform reality. **Re-verify against the live pages before using these numbers on an actual title going to print** — platform specs change; this is a dated snapshot, not a standing guarantee.

Sources: [KDP — Set Trim Size, Bleed, and Margins](https://kdp.amazon.com/en_US/help/topic/GVBQ3CMEQW3W2VL6); [IngramSpark — File Requirements for Print Books](https://www.ingramspark.com/blog/file-requirements-for-print-books); [IngramSpark File Creation Guide (PDF)](https://www.ingramspark.com/hubfs/downloads/file-creation-guide.pdf).

---

## KDP (Amazon)

- **6in×9in trim, with bleed:** page size 6.125in × 9.25in.
- **Bleed:** 0.125in (3.2mm) on top, bottom, and outside edge. **No bleed on the inside (gutter) edge.**
- **Outside margin minimum:** 0.25in (6.35mm).
- **Inside (gutter) margin:** varies by page count — 0.375in for books under 150 pages, scaling up to 0.875in for books with 700+ pages.
- **Page count range for 6×9:** 24–828 pages.

## IngramSpark

- **Trim size tolerance:** 1/16in (0.0625in / 2mm) variance is normal in printing — a real, physical cut variance, not a file-spec choice.
- **Interior bleed:** 0.125in (3mm) on the three outer edges. **No bleed on the bind (inside/gutter) side.**
- **Cover bleed:** 0.125in (3mm) on all four sides (case-laminate titles need 0.625in, for wrapping).
- **Barcode:** black only (0/0/0/100 CMYK), placed over a white box or background. Recommended size 1.75in × 1in, vector or high-quality raster. Leave a 1.75in × 1in area free of text and graphics for it.

## The gap found in this build's own pipelines

Neither internal pipeline builds true print bleed. Both handle "full-bleed" pages by removing margin at the trim edge (`margin: 0` in the CSS Paged Media pipeline's `@page bleed`), not by extending content 0.125in *past* the trim edge on the non-gutter sides the way KDP and IngramSpark both require.

**Body-text margins are not at risk.** Both pipelines' standard page margins (0.95in–1in in the CSS Paged Media pipeline; 0.85in–1.1in in the structural-template pipeline) comfortably exceed both platforms' minimums (KDP outside 0.25in / gutter 0.375–0.875in by page count; IngramSpark's same-class minimums), so ordinary chapter pages clear this with room to spare.

**Full-bleed pages are the actual risk** — part-openers, photo spreads, quote-spreads, cover pages. As currently built, these fill exactly to the trim line with no overrun. Combined with IngramSpark's stated 0.0625in trim-cut tolerance, a real physical trim could expose a thin white sliver at the edge of what was meant to be an edge-to-edge image.

**Before calling any build with a full-bleed page retail-ready:** flag this explicitly, and confirm the actual image/background for that page extends at least 0.125in past the 6in×9in trim boundary on the three non-gutter sides (all four for a standalone cover file) — not just that the CSS margin is zero. This is a render-file check, not a CSS-rule check; `margin: 0` alone does not prove it.

## Barcode placement (once an ISBN exists)

Leave a clear 1.75in × 1in area on the back cover for the barcode — black only, vector or high-quality raster, on a white background. See `reference/output-packaging.md`'s ISBN note for what to say in the packaging README when no ISBN is assigned yet.
