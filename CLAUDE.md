# PAI Brain Alignment

**Read this before doing anything else in this repo.**

This repo (pai-opportunity-rater) is one product on the PAI Brain platform. The platform's canonical strategy, architecture, and roadmap live in the pai-wiki repo at `wiki/projects/pai-brain/`. Before working on any task in this repo, read these five files in order:

1. `pai-wiki/wiki/projects/pai-brain/README.md` — what the PAI Brain is and what products live on it
2. `pai-wiki/wiki/projects/pai-brain/architecture.md` — the system diagram and how the pieces connect
3. `pai-wiki/wiki/projects/pai-brain/roadmap.md` — the master roadmap across all PAI Brain repos
4. `pai-wiki/wiki/projects/pai-brain/glossary.md` — names and definitions (Ozzy, Company Radar, PAI Signals, etc.)
5. `pai-wiki/wiki/projects/pai-brain/conventions.md` — shared rules every Claude session must follow

If you can't access pai-wiki in your current session, ask Keith to either provide the file contents directly or run the task in a session that has pai-wiki access.

**Mental model:** pai-wiki is the brain. pai-aeronews, pai-flight-deck, pai-tools, amanuensis, pai-data-pipeline, pai-opportunity-rater, and future products are organs that connect to it. The five canonical files are the nervous system that keeps every Claude conversation across every organ aligned with the same vision.

---

# pai-opportunity-rater — Claude Operating Guide

## What this repo is

A **mobile-first Progressive Web App** for rating PAI AeroNews Opportunity Spotter ("Ozzy") outputs via Tinder-style swipe cards. Requested by the COO as the human-feedback layer for Ozzy: as PAI staff rate the opportunities Ozzy surfaces, the accumulated ratings teach the company what kinds of opportunities are worth acting on and what kinds aren't.

Without this feedback loop, Ozzy produces drafts the company never trains. With it, Ozzy can be tuned over time based on what real PAI raters say "act on this," "more like this," or "less like this" to.

## How it works

- **Swipe right** = "more like this"
- **Swipe left** = "less like this"
- **Swipe up** = "act on this" (top-tier signal)
- Each rater has a configured role and weight for scoring.
- Ratings persist in localStorage across sessions.
- All historical ratings can be exported as JSON for analysis.

## Architecture

- **Single 886-line `index.html`** — entire app: HTML + inline CSS + inline vanilla JS. No framework, no bundler, no tests.
- **`manifest.json`** — PWA manifest with PAI navy/cyan branding.
- **`sw.js`** — Service worker, cache-first for offline use.
- **`assets/pai-logo.png`** — PAI logo.
- **Zero build step.** Serve the directory with any static file server.

## How to run locally
```
npx serve .
# or
python3 -m http.server 8000
```

Open the served URL on a phone or desktop browser. "Add to Home Screen" installs as a PWA.

## How to work in this repo

- **Edit `index.html` directly.** All app logic, styles, and markup live there. Changes are immediate — refresh the browser to see them.
- **Don't introduce a build step or framework** without explicit Keith approval. The simplicity of "one file, no tooling" is a feature, not a limitation.
- **Test changes on actual phones**, not just desktop. This is a mobile-first PWA — the primary use case is swiping cards on a phone.
- **Don't modify the export JSON schema** without coordination — downstream analysis depends on a stable format.

## Connection to the rest of the PAI Brain

- **Input source:** Ozzy outputs (currently posted to Teams; future: also written to a SharePoint list — see master roadmap). The rater pulls opportunities to display from this source.
- **Output destination:** rating data accumulates in localStorage and can be exported as JSON. Eventually this output should feed back into Ozzy's tuning (see master roadmap).
- **Counterpart:** Ozzy generates, raters rate, the company learns. Both halves of the loop need to work.
