# kaziarman.github.io

Personal academic website for Kazi Arman Ahmed. One `index.html` with a fixed left rail, a scrolling right column, scroll-spy on the navigation, a News block with a live year timeline, and citation badges. No build step, no Jekyll, no dependencies.

## Folder structure

```
kaziarman.github.io/
├── index.html            the whole website
├── .nojekyll             tells GitHub Pages to serve files as-is
├── README.md             this file
└── assets/
    ├── cv.pdf            your CV (the CV icon opens this)
    ├── profile.png       square photo (optional; a "KA" circle shows if missing)
    └── citations.json    optional, overrides citation counts (see below)
```

Create the `assets/` folder yourself and drop in `cv.pdf` and `profile.png`. Both are optional; the site works without them.

## Put it live (about five minutes)

1. Create a GitHub repository named exactly `KaziArman.github.io` (must match your username).
2. Upload `index.html`, `.nojekyll`, `README.md`, and your `assets/` folder.
3. Settings > Pages. Source: Deploy from a branch, branch `main`, folder `/ (root)`. Save.
4. Wait a minute, then open `https://kaziarman.github.io`.

To update later, edit `index.html`, commit, and it refreshes in under a minute.

## How it is organized

Open `index.html`. Two parts matter.

The HTML sections in `<main>` (About, Research, Projects, Experience, Education, Honors, Acknowledgments, Contact) are plain HTML. Edit the text directly. Each `<section>` has an `id`; the left-nav link `href="#id"` points to it, and the scroll-spy highlights the matching link as you scroll.

News and Publications are built from two JavaScript arrays near the bottom of the file, because the News year timeline and the publication grouping are computed from them.

## Editing News

Find `var NEWS = [ ... ]`. Each entry:

```js
{ date: "2026-08-01", text: "What happened." },
```

Keep them newest first. The date format is `YYYY-MM-DD`. The "N entries" label, the year timeline, and the per-year counts are all generated from these dates. Delete or add lines freely.

## Editing Publications

Find `var PUBS = [ ... ]`. Each entry:

```js
{ group: "2025", key: "breast", doi: "10.1371/...", url: "https://...",
  title: "Paper title",
  authors: 'Author A, <span class="me">Kazi Arman Ahmed</span>, Author B',
  venue: "Journal, volume(issue), pages" },
```

- `group` is either a year (for example `"2025"`) or `"Preprints and manuscripts"`. Entries with the same group are listed together, in the order they appear. Only the last four years show by default; older years move behind a "View all publications" button automatically.
- Wrap your own name in `<span class="me">...</span>` so it is emphasized.
- `flag` is optional; use `"Preprint"` or `"Under review"` to show a small tag.
- `url` is optional; the title links to it.
- `key` is a short unique id used for the citation badge.
- `doi` is optional; if present, the citation count is fetched live.

## Citation counts

Each publication can show a "Cited by N" badge. Counts resolve in this order:

1. A number in the `CITATIONS` object, for example `var CITATIONS = { stick: 5, breast: 14 };`, keyed by the publication `key`.
2. A number in `assets/citations.json` with the same keys.
3. If neither is set and the entry has a `doi`, the count is fetched live from Semantic Scholar when the site runs on GitHub Pages.

Google Scholar and ResearchGate cannot be read live from a static page (no public API, no cross-origin access). If you want Scholar-accurate numbers that refresh on their own, the usual approach is a scheduled GitHub Action that writes `assets/citations.json`. Ask and I will add it.

## Editing Research

The Research section has three numbered themes, each an `<article class="theme">` block. Inside a theme: the number (`theme__num`), the title, a one-sentence problem statement (`theme__lede`), optional steps (`<ol class="steps">`, each `<li>` has a short label in `step__k` and one sentence), and method tags (`<span class="tag">`). Add `<span class="status status--wip">In progress</span>` next to a title for unfinished work. Copy a whole theme block to add a new direction, and update the number.

The "Also working on" box below the themes is a single `<article class="rmini">`.

## Other things to change

- Name, role, email, social links: the `<aside class="rail">` block near the top.
- Photo: add `assets/profile.png` (square). No file is fine; the initials circle shows.
- CV: add `assets/cv.pdf`, or repoint the CV icon to any URL.
- Acknowledgments: replace the italic funding placeholder with your real funding.
- Colors and fonts: the `:root` block at the top of `<style>`.

## Note on fonts

The serif (Newsreader) and the monospace dates (IBM Plex Mono) load from Google Fonts. The small sans-serif labels use Switzer from Fontshare. All three load on your live GitHub Pages site. In a sandboxed preview the Switzer request may be blocked and the labels fall back to a system sans, which looks nearly identical.
