# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

An R Bookdown project: a mathematics bridging course (A-Level to University) for incoming UCL Management Science students. The output is both an interactive HTML gitbook and a PDF (via LuaLaTeX).

## Building the book

From R or the terminal:

```r
# Build HTML only
bookdown::render_book('index.Rmd', 'bookdown::gitbook')

# Build PDF only (requires LuaLaTeX + UCL Sans, Fira Math, Inter, Source Code Pro fonts)
bookdown::render_book('index.Rmd', 'bookdown::pdf_book')

# Build all formats
bookdown::render_book('index.Rmd')

# Live-reload preview server
bookdown::serve_book()
```

In RStudio, the Build panel (Ctrl+Shift+B) builds all formats as configured in `_output.yml`.

Output lands in `_book/`. Intermediate files (`_bookdown_files/`, `*.knit.md`, `*.utf8.md`) are gitignored.

## Architecture

| File | Role |
|------|------|
| `index.Rmd` | Book metadata (YAML) + preface + shared R helpers |
| `01-intro.Rmd` … `10-vectors.Rmd` | Content chapters, numbered to control order |
| `_bookdown.yml` | Book filename, output dir, chapter script (`R/webex.R`) |
| `_output.yml` | Format config for gitbook and pdf_book |
| `template.tex` | LaTeX preamble — sets UCL brand fonts via `fontspec` |
| `styles.css` | Custom CSS for video/Desmos containers |
| `include/webex.css`, `include/webex.js` | Client-side logic for interactive exercises |
| `R/webex.R` | Loads `webexercises` package before each chapter renders |

## Shared helper functions (defined in `index.Rmd`)

These are available in all chapters without re-declaration:

- **`embed_youtube(id, title, playlist, playlist_link)`** — responsive iframe (HTML) or downloaded thumbnail image (PDF). YouTube video IDs are stored in `images/videos/` as `.jpg` thumbnails for PDF builds.
- **`embed_desmos(id, title, height, geometry, crop_top, crop_left)`** — Desmos calculator/geometry iframe (HTML) or static screenshot (PDF) from `images/desmos/`.
- **`static_embed(img, url, title, width)`** — fallback used by the above two: linked image if file exists, otherwise a blockquote link.

## Interactive exercises (webexercises)

Self-check boxes use the `webexercises` R package:

```r
# Fill-in-the-blank (numeric)
`r fitb(8, num = TRUE)`

# Multiple choice — mark correct answer with name = "answer"
`r mcq(c(answer = "A", "B", "C", "D"))`
```

Wrap question blocks in a fenced div:
```
::: {.webex-check .webex-box}
...questions...
:::
```

These render as interactive checkable widgets in HTML. In PDF they appear as static boxes.

## Content conventions

- Chapter headings use `# Chapter Title {#anchor}` — the anchor is used for cross-references (`\@ref(anchor)`).
- Section headings: `## Section {#anchor}`, `### Subsection`.
- Inline math: `$...$`. Display math: `$$...$$`.
- Each chapter typically follows: key idea → worked examples (simple / moderate / challenging) → practice exercises → common mistakes → self-check webexercises box.
- Management Science context is woven throughout — worked examples reference pricing, EOQ, IRR, compound interest, etc.
