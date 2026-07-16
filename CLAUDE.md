# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A static site hosted via GitHub Pages (repo name `pawanpal-branch.github.com` — pages served from the repo root, no build step). Each `.html` file is a standalone, self-contained test/demo page for exercising **Branch.io** deep-linking behavior (deepviews, `window.open` link tests, app-link/intent redirects, deferred deep linking). There is no shared layout, bundler, or JS module system — every page inlines its own `<style>` and `<script>`.

## Commands

There is no build, lint, or test tooling in this repo (no `package.json`). To preview a page, just open the `.html` file directly in a browser, or serve the directory locally, e.g.:

```
python3 -m http.server 8000
```

then visit `http://localhost:8000/<file>.html`.

## Architecture / conventions

- **Each HTML file is an independent experiment**, not part of a shared app — expect duplicated boilerplate (styles, Branch SDK init snippet) across files rather than shared includes.
- **Branch SDK loading pattern**: pages that integrate Branch.io use the standard inlined Branch snippet in `<head>` that async-loads `branch-latest.min.js` from `cdn.branch.io` and calls `branch.init('key_live_...', ...)`. When adding a new test page, copy this pattern rather than inventing a new one.
- **`deepview-service` pages** (e.g. `flipkart.html`, `kejriwal.html`, `kejriwal2.html`): these carry a `<meta name="deepview-service" content="deepview-service" />` tag and mimic a product/App-store-style landing page (product image, title, "Open in app" style CTA) — used to test Branch deepview rendering for specific link/journey configs. `kejriwal.html` / `kejriwal2.html` are copies with different background images/content, used for A/B-style comparisons.
- **`index.html`** is the home/entry page; it currently has most of its interactive buttons commented out (previous test links to Cleartrip, Flipkart intent URIs, market:// links, BranchBeta links) — treat the commented block as retained reference/history, not dead code to prune, unless asked.
- **`index1.html` / `index2.html`** are minimal placeholder pages linked from `index.html` for simple navigation testing.
- **`deepview.html`** tests `window.open` behavior against a live Branch link (`branchURL` constant near the top) — when pointing this at a different Branch link, update that one constant.
- **`tooltip-demo.html`** is unrelated to Branch — a standalone Bootstrap 5 tooltip/popover styling demo (loads Bootstrap CSS/JS from jsDelivr CDN).
- No routing, no backend — all links between pages are plain `window.location.href` navigations or `<a href>`/`onclick` handlers.
