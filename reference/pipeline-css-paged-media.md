# Pipeline: CSS Paged Media

**Source:** `GabeYoga-HQ/Detox Book/book-build/{print.css,render.sh,build.js}` — the real, shipped production trail for *"Are You Actually Hungry? The Missing Conversation About Detox, Wellness, and Common Sense"* (Gabriel Azoulay). Read directly from these files, not reconstructed from memory.

**When to use this pipeline:** fresh markdown chapter source exists, and there's no existing finished book's HTML/CSS in the family to copy as a structural template. See `rules.md`'s routing table.

---

## Trim and margins

```css
@page {
  size: 6in 9in;
  margin: 0.95in 1in 1in 1in;   /* top / right / bottom / left */
}
```

6in×9in trade trim — the same trim size the structural-template pipeline also uses (see `pipeline-structural-template.md`), independently arrived at. Margins are 0.95in top, 1in on the other three sides. The ~1.0in side margins set a measure of roughly 56 characters.

## Running heads and page numbers

```css
@page {
  @bottom-center { content: counter(page); }
}
@page :left {
  @top-center { /* left-page running head content */ color: #b3aa9a; margin-bottom: 0.2in; }
}
@page :right {
  @top-center { /* right-page running head content */ color: #b3aa9a; margin-bottom: 0.2in; }
}
@page :first {
  margin: 0;
  @bottom-center { content: none; }
  @top-center { content: none; }
}
```

`@page :left` / `@page :right` give the recto/verso running heads their own content; `counter(page)` drives the visible page number. `@page :first` suppresses both on the very first page (the title page).

**The gotcha — named here, not left as a render-script comment:** these rules are CSS Paged Media spec, and **Chrome headless silently ignores them.** A Chrome-headless render produces a PDF with the correct page count and correct visual content, but with no running heads and no page numbers — no error, no warning, nothing that shows on a screen check. Only `pagedjs-cli` (the paged.js polyfill) actually honors `@page` running heads and counters. `render.sh`'s own comment states this directly:

> "paged.js polyfills the CSS Paged Media @page rules in print.css (counter(page), the @top-center running head, clean page breaks) that Chrome headless ignores."

**This is the refusal gate.** See `rules.md` for the exact language and `examples.md` for it in action. Render command: `npx --yes pagedjs-cli book.html -o book.pdf`. The `--yes` flag auto-installs `pagedjs-cli` on first run, which needs network access — the one real constraint on using this pipeline in a fully offline environment.

## Full-bleed pages (part-openers, photo spreads)

```css
@page bleed {
  margin: 0;
  @bottom-center { content: none; }
  @top-center { content: none; }
}
```

A separate `@page bleed` rule removes all margin and running-head/counter chrome for full-page-image content — part-openers, photo spreads, the title page.

**Known gap, confirmed against current KDP/IngramSpark specs (see `reference/retail-technical-requirements.md`):** `margin: 0` fills the page *to the trim edge*, not past it. Real print bleed requires content to extend 0.125in beyond the trim edge on the three non-gutter sides, so trimming variance (IngramSpark alone tolerates up to 0.0625in) never exposes a white sliver. As built, this pipeline's "full-bleed" pages are edge-to-edge at the trim line, not true bleed. Flag this explicitly on any build using `@page bleed` before calling it retail-ready — don't assume it already clears a real print QC check.

## Component library

`build.js` parses markdown chapter source into structured components and assembles the final HTML. Named component kinds, read directly from the parser (`parseBody`, `renderBody`, and the per-kind render functions):

| Component | Purpose |
| - | --- |
| `chapter` | Standard chapter body flow |
| `part-opener` | Full-bleed section-opening page (uses `page: bleed`) |
| `pull-quote` | A centered, floated statement pulled from the prose |
| `callout` | A call-out box — `function callout(line)` wraps a line in `<div class="callout avoid-break">` with a decorative sigil |
| `reflect-box` | A reader-reflection prompt box |
| `timeline` | `function timeline(rows)` renders a dated sequence of rows in an `avoid-break` container |
| `quote-spread` | A full-bleed standalone-quote page |

`break-inside: avoid` (aliased as `.avoid-break`) is applied to callouts and timelines so they never split across a page boundary — mirrors the structural-template pipeline's identical `page-break-inside: avoid` discipline on its own boxes (see `pipeline-structural-template.md`), independently arrived at by both systems.

## Render command

```bash
npx --yes pagedjs-cli book.html -o book.pdf
```

Never the Chrome-headless fallback (`--headless --disable-gpu --print-to-pdf`) for a build that needs running heads or page numbers — which is nearly every book. See the refusal gate above.
