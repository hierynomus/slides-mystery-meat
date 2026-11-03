# Stop Running Mystery Meat in Production

Slides and speaker notes for the talk by **Jeroen van Erp** (SUSE), presented at:

- 2026-02-04 Cloud Native London (in the meatloaf theme)
- 2026-09-23 WeAreDevelopers NA, San Jose (in the hamburger theme)
- 2026-11-12 KubeCon NA, Salt Lake City (in the hamburger theme)

Base images from random registries are the mystery meat of production: you don't know who built them, what's in them, or which build you pulled. This talk walks through provenance, attestation and SBOMs (the anatomy of trust), shows why scanning alone can't save you, and ends with three things to try on Monday.

## Run the slides locally

The deck is built with [Slidev](https://sli.dev). You need [Bun](https://bun.sh) (or Node.js).

```
bun install
bun run dev      # live preview at http://localhost:3030
bun run build    # static site in dist/
bun run export   # PDF export (needs playwright-chromium)
```

Keyboard: arrows or space to move, `f` fullscreen, `o` overview, `p` presenter mode with speaker notes.

The slides and speaker notes are in `slides/slides.md`. Every push to `main` publishes the deck to GitHub Pages.

## Image themes

The full-bleed background photos live in `slides/public/themes/<name>/`, and the slides refer to them by file name only. The shipped theme is `burgers`.

To make a new theme, copy `slides/public/themes/burgers/` to `slides/public/themes/<name>/` and replace the images, keeping the file names and roughly the same crops. Then pick it with:

```shell
VITE_DECK_THEME=<name> bun run dev      # or build / export
```

or set `imageTheme: <name>` in the first slide's frontmatter in `slides/slides.md` to change the default. A theme must contain every file; there is no fallback to `burgers`.
