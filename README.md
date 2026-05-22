# indicator-story

A scroll-driven HTML explainer that walks readers through how
**The Bitcoin Layer**'s proprietary Liquidity Indicator is constructed —
from the raw Index to the 30-day change, to the smoothed cycle, to the
slope, to the buy/sell signal.

Single HTML file, no build step. CDN-loaded libraries
(Scrollama + D3 v7 + flubber + rough.js + GSAP). Illustrative synthetic data
only — never real signals.

## Live site

The deployed version is served from `docs/index.html` via GitHub Pages →
**https://augustinecarb.github.io/indicator-story/**

## Files

| File | Role |
|---|---|
| `index.stable.html` | The known-good build. Source of truth for the deployed site (copied into `docs/index.html`). |
| `index.html` | The active local working copy. Iterated on with smoothness / performance fixes; not yet deployed. |
| `docs/index.html` | The file GitHub Pages serves. Mirror of `index.stable.html`. |
| `docs/assets/` | Logos referenced by the deployed page. |
| `assets/` | Same logos, for the local working copy. |
| `storyline_idea/` | The 9-frame hand-drawn storyboard the page is built from. |
| `artwork_inspiration/` | Ray Dalio "How the Economic Machine Works" reference art. |
| `CLAUDE.md` | Design system + decisions log for future contributors. |
| `scenes.md` | Plain-language reader spec, beat by beat. |
| `corrections.md` | Chronological change log. |

## Local development

```bash
npx --yes live-server --port=5180 --no-browser --quiet
```

Then open http://127.0.0.1:5180/index.html (the experimental copy) or
http://127.0.0.1:5180/index.stable.html (the stable copy).

## Updating the deployed site

When the experimental copy is ready to promote, copy it over the stable +
docs copies:

```bash
cp index.html index.stable.html
cp index.html docs/index.html
git add index.stable.html docs/index.html
git commit -m "Promote experimental to stable"
git push
```
