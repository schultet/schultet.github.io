# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

"Append-Only" (tagline: *Notes I write before I build.*) — a static personal notes collection (not a regular blog) with **no build step, no generator, and no framework** — hand-written HTML, plain CSS, and a little vanilla JavaScript. It is published via GitHub Pages from the repo root (this repo is `schultet.github.io`), so `index.html` must stay at the top level. Content is **bilingual (German + English, as fits)**; most existing prose is German. **Minimal dependencies is a deliberate constraint**: no CDNs, no frameworks — fonts are self-hosted, and prefer custom CSS + native HTML5 elements over libraries.

Each page may have its own look — that's intentional. The landing page is modern (light/dark); the post/book pages use a warmer "paper" style. Don't try to unify them.

There is nothing to build, lint, or test. To preview, open `index.html` directly in a browser (the post list is embedded in the page, so no local server is needed). To publish, commit and push to the default branch — GitHub Pages serves `/` automatically.

## Architecture

- `index.html` — the landing page. It contains both the notes list **and** the JS that renders/filters it. Styled by `assets/home.css` (modern look — **not** `blog.css`). Uses semantic HTML5 (`<header>`, `<main>`, `<search>`, `<nav>`, `<footer>`, `<time>`).
  - The list of notes lives in a `const posts = [ … ]` array near the top of the inline `<script>`. Each entry is `{ title, date: "YYYY-MM-DD", url, tags: [...], excerpt }`.
  - Everything below the `/* ab hier nichts mehr aendern */` comment is the rendering engine: it sorts newest-first by `date`, collects all tags, and filters by a live search box (matches title + excerpt + tags) plus tag buttons. Tag filtering is **AND** (an entry must carry every active tag) — `matches()` has a comment showing how to switch to OR. User-supplied strings are run through `esc()` before being inserted as HTML.
- `assets/home.css` — stylesheet for the landing page **only**. Modern light/dark design (light tokens in `:root`, dark overrides in `@media (prefers-color-scheme:dark)`). Self-hosts **Inter** (body/UI) + reuses **JetBrains Mono** (small labels) via `@font-face` at the top.
- `posts/` — one self-contained HTML file per note. `posts/_template.html` is the starting point; copy it. Posts link back to `../index.html` and pull in `../assets/blog.css`.
- `assets/blog.css` — the "paper" stylesheet for the post/book pages (warm background, Fraunces/Newsreader serifs). Colors are CSS variables in its `:root { … }` block. **Not** used by the landing page.
- `assets/fonts/` — all fonts are **self-hosted** variable `.woff2` files (`latin` + `latin-ext` subsets); there is no Google Fonts `<link>` anywhere. `@font-face` `src` paths are relative to the CSS file that declares them (`fonts/…`), so they resolve from any page linking that CSS. **Exception:** `posts/designing-data-intensive-applications.html` links no external CSS — it carries a full inline `<style>` with its own `@font-face` copies using `../assets/fonts/…` paths. Swapping fonts there means editing that file too.
- `impressum.html`, `datenschutz.html` — German legal pages at the repo root, styled with `blog.css`, linked from every footer (`.foot-nav`). They are templates full of `.platzhalter` placeholders (German, GDPR/DDG-oriented) meant to be filled in with real details from a lawyer or generator — not legal advice.
- `impressum.html`, `datenschutz.html` — German legal pages at the repo root, linked from every footer (`.foot-nav`). They are templates full of `.platzhalter` placeholders (German, GDPR/DDG-oriented) meant to be filled in with real details from a lawyer or generator — not legal advice.

## Adding a post (two coupled edits)

A post is only complete when **both** of these are done — the post file alone won't appear on the homepage:

1. Copy `posts/_template.html` to `posts/YYYY-MM-DD-kurztitel.html` and fill in the title, date, tags, and body.
2. Add one object to the `posts` array in `index.html` pointing at that file.

Reuse exact tag spellings across posts (e.g. always `"Datenbanken"`, never sometimes `"DB"`) — the filter groups by literal string match, so inconsistent casing/wording fragments the tag list.
