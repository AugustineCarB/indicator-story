# indicator-story

A scroll-driven HTML explainer that walks readers through how
**The Bitcoin Layer**'s proprietary Liquidity Indicator is constructed —
from the raw Index to the 30-day change, to the smoothed cycle, to the
slope, to the buy/sell signal.

Single HTML file, no build step. CDN-loaded libraries
(D3 v7 + rough.js + GSAP; the older copies also load Scrollama + flubber).
Illustrative synthetic data only — never real signals.

## Live site

The deployed version is served from `docs/index.html` via GitHub Pages →
**https://augustinecarb.github.io/indicator-story/**

## Also embedded in TBL Pulse

A **copy** of this story is embedded in the TBL Pulse dashboard. On 2026-06-12
`index.stable2.html` was copied (byte-for-byte, synthetic data only) into the
TBL-Pulse repo at `public/indicator-story/index.html` (+ `assets/`), and is now
on `main` — it opens full-screen from the **"How the Indicator is Built"**
button in the TBL Liquidity tab.

**The two copies do not auto-sync.** If you change the story here, re-copy it
into TBL-Pulse too:

```bash
cp index.stable2.html /path/to/TBL-Pulse/public/indicator-story/index.html
```

(This mirrors how `TBL-Pulse/public/books/*.html` are maintained.)

## Files

| File | Role |
|---|---|
| `index.stable.html` | The known-good build. Source of truth for the deployed site (copied into `docs/index.html`). |
| `index.html` | The active local working copy. Iterated on with smoothness / performance fixes; not yet deployed. |
| `index.stable2.html` | Round 33 (2026-06-12): full smoothness + state-machine pass over `index.html`. Lerped intro, fixed zoom re-entry + final-beat dwell, mobile-tall canvas, reduced-motion. Verified headless (desktop, 390px mobile, reduced-motion); candidate for the next promotion. |
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

Then open http://127.0.0.1:5180/index.html (the experimental copy),
http://127.0.0.1:5180/index.stable2.html (the Round 33 perfected candidate),
or http://127.0.0.1:5180/index.stable.html (the deployed stable copy).

## Updating the deployed site

When a copy is ready to promote (e.g. `index.stable2.html` after review),
copy it over the stable + docs copies:

```bash
cp index.stable2.html index.stable.html
cp index.stable2.html docs/index.html
git add index.stable.html docs/index.html
git commit -m "Promote stable2 to stable"
git push
```
