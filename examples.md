# Examples

One worked illustration of the sharpest gotcha in this job — the discipline that governs every render (see `rules.md`). For the full mechanics of either pipeline (component markup, margin specs, the copy-as-template flow), read that pipeline's file in `reference/`; this example demonstrates the gotcha in action, it doesn't re-teach either pipeline.

---

## The `pagedjs-cli` gotcha, in action

**Situation:** A book built on the CSS Paged Media pipeline has finished layout. `pagedjs-cli` needs network access to auto-install on first run; the render environment doesn't have it. The Chrome-headless fallback in `render.sh` is commented but present and produces a PDF with no errors — correct page count, correct content, correct visual layout.

**Ask:** "Chrome headless rendered fine and the PDF looks complete — let's just ship that, we can swap the render method later if it matters."

**Response — exact refusal language from `rules.md`, used verbatim:** *"I won't call this render final — Chrome headless silently drops running heads and page counters, and the page count looks right regardless, so the mistake doesn't show until someone checks a printed proof. Render with `pagedjs-cli` before this ships."*

The PDF isn't wrong in any way a screen check would catch — it's the same page count, the same content, the same margins. What's missing is exactly the two things `@page :left`/`@page :right` and `counter(page)` in `print.css` were written to produce: the running heads and the page numbers. Chrome headless never raises an error about this; it just doesn't render those rules. The only way to catch it before a printer does is to know the gotcha exists and check for it specifically — which is why it's named here, not left as a footnote in a render script comment.

The same discipline applies regardless of which pipeline produced the markup: the structural-template pipeline's own documentation doesn't describe its running-head mechanics at all, which means the same silent-drop risk is *more* likely there, not less — see the routing table in `rules.md`.
