# Pipeline: Structural Template

**Source:** `GabeYoga-HQ/My-Cover-Designs/BOOK-LAYOUT-Reference.md` — the documented visual system built for the *Joint Dialogue Method* book and reused, per that file's own header, as "the template for all future GabeYoga books." Used across JDM plus three other books. Read directly from this file, not reconstructed from memory.

**When to use this pipeline:** an existing finished book's HTML/CSS in the same family exists to copy as a structural template. See `rules.md`'s routing table.

---

## The copy-as-template flow

This pipeline's whole mechanism, stated in its own source material:

1. Copy the finished book's HTML file as the structural template for the new book.
2. Reference the shared stylesheet (or copy and rename it for the new book).
3. Replace content, **keeping all class names intact** — the shared stylesheet depends on them.
4. Run the regeneration script to render the PDF.

A renamed class breaks the shared stylesheet silently — the whole reason the copy-as-template flow works is that structure and styling stay decoupled from content. See `rules.md`'s "Always" list.

## Trim and margins

- **Trim size:** 6in × 9in — the same trim size the CSS Paged Media pipeline also uses (see `pipeline-css-paged-media.md`), independently arrived at.
- **Margins:** outer 1.1in, inner (gutter) 0.85in, top/bottom 1in. Different numbers from the CSS Paged Media pipeline's 0.95in/1in — a real, independent margin spec, not a shared constant. Don't silently converge the two.
- **Rendering:** Chrome print-to-PDF, with `@media print` rules handling layout.

**Running heads and page numbers — an open gap in the source material, not an assumption to paper over:** unlike the CSS Paged Media pipeline, this pipeline's own documentation doesn't describe how (or whether) it produces running heads or page-number counters. Don't assume Chrome print-to-PDF handles this correctly just because it's the documented render method here — the CSS Paged Media pipeline's own `render.sh` comment confirms Chrome headless drops `@page` running-head and counter rules regardless of which pipeline produced the markup (see `pipeline-css-paged-media.md`'s named gotcha). If a book on this pipeline needs guaranteed running heads or page numbers, check the actual rendered PDF for them directly rather than trusting the render method's reputation, and consider applying the CSS Paged Media pipeline's `pagedjs-cli` render step to this pipeline's own markup if they're missing.

## Color tokens

```css
--teal-deep:  #1a3a3a   /* dark section headers, box backgrounds */
--teal-mid:   #2d5a5a   /* key concept box labels */
--teal-light: #4a7a6a   /* accent */
--gold:       #b8922a   /* insight borders, principle labels */
--gold-light: #d4a84b   /* secondary gold */
--cream:      #f5f0e8   /* page background */
--cream-dark: #ede6d8   /* darker cream panels */
--rust:       #a0522d   /* try-this boxes, warning states */
--ink:        #1a1a1a   /* body text */
--muted:      #6b6b6b   /* captions, citations */
--rule:       #ddd5c8   /* divider lines */
--white:      #faf7f2   /* warm white (not stark) */
```

## Typography

- Serif (`DM Serif Display`) — chapter titles, pull quotes, epigraphs.
- Sans (`DM Sans`) — body text, labels, captions.
- Mono (`DM Mono`) — code or technical sequences.
- Body: 11.5pt / line-height 1.85 / weight 300 — deliberately open, "breathing" layout. **Don't compress these margins or this leading when adapting for a new book** — the source material states this directly: "the open layout is the brand."

## Component library

Eleven named components, each with a fixed class name the copy-as-template flow depends on:

| Component | Class | Notes |
| - | --- | --- |
| Epigraph | `.epigraph` | Opening quote at chapter start — serif italic, no border, `<cite>` for attribution |
| Pull Quote | `.pullquote` | Centered statement pulled from prose, ~2rem serif italic, `margin: 5rem auto` |
| Insight | `.insight` | Supporting observation, italic, gold left-rule border — lighter than a full box |
| Principle Box | `.principle-box` | A named principle, bordered top/bottom, no fill, gold small-caps label |
| Key Concept box | `.jdm-box.box-key-concept` | Teal-mid label bar — defining a new concept |
| Remember box | `.jdm-box.box-remember` | Gold/amber label bar — things the reader must retain |
| Try-This box | `.jdm-box.box-try-this` | Rust/red label bar — practical exercises or prompts |
| In-Practice box | `.jdm-box.box-in-practice` | Teal-deep label bar — clinical/real-world application |
| Callout | `.callout` | Horizontal rule top/bottom, lighter weight than a JDM box — a quoted or standalone passage |
| Observation Box | `.observation-box` | Shaded, icon + bold statement + detail text |
| Lineage Box | `.lineage-box` | Credits a source teacher/tradition, with a sub-label |
| Goal Box | `.goal-box` | Standalone objective statement, used at chapter openers or section transitions |
| Traffic Light states | `.traffic-green` / `.traffic-amber` / `.traffic-red` | Left-border only, color-coded three-state decision framework |

All boxes carry `page-break-inside: avoid` — they never split across a page. Don't compress the stated margins (pull quotes `5rem auto`, principle boxes `3rem 0`, JDM boxes `2.5rem 0`) when adapting for a new book.

## Cover placement

```html
<div class="cover-page"><img src="cover-front-final.png"></div>
<!-- book content -->
<div class="cover-page cover-back"><img src="cover-back-final.png"></div>
```

Covers are separate PNG files on dedicated `.cover-page` divs, not embedded design elements. The back-cover image gets `filter: brightness(1.75) saturate(0.6) sepia(0.2)` to soften and warm it for print. See `reference/output-packaging.md` for where these PNGs land in the final delivery set.
