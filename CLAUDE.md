# indicator_story — TBL Liquidity Indicator Scrollytelling Page

This folder contains a single-page scroll-driven HTML explainer that walks the
reader through how The Bitcoin Layer's proprietary Liquidity Indicator is
constructed — Index → 30-day change → smoothed cycle → slope → buy/sell signal.

## What this is

- **Purpose:** Top-of-funnel marketing page for thebitcoinlayer.com. Free /
  public. Turns a proprietary framework into a single-page "aha moment" the way
  Ray Dalio's *How the Economic Machine Works* did for the long-term debt cycle.
- **Audience:** Macro-curious bitcoin investors, financial professionals,
  students of monetary theory. Drives subscriptions to TBL Pro ($49/mo, Substack).
- **One author:** Nik Bhatia, first-person voice ("I"). All copy is scaffolded
  for Nik to finish — see "Copy rules" below.

## Critical guardrail

**Signal protection.** This page is public-facing. Per the root CLAUDE.md, never
expose the real TBL Liquidity green/red dot, specific dates, entry/exit prices,
or live signal values. The final buy/sell dot scene uses synthetic illustrative
data placed at deterministic x-positions on a synthetic series — **never real
TBL signals**. Copy reads "the shape of a signal," not "buy here." If anyone
asks to wire up live Supabase data, push back: keep this page illustrative.

## File layout

```
indicator_story/
├── index.html              ← single file: HTML + inline CSS + inline JS
├── CLAUDE.md               ← this file
├── storyline_idea/         ← user's hand-drawn 9-frame storyboard (HEIC)
└── artwork_inspiration/    ← Dalio cycle-sketch reference (PNG)
```

Everything is in `index.html`. No build step, no npm, no bundler. Edit and refresh.

## Document conventions — keep these three in sync

This folder maintains three living documents. After any change that the
reader would see on screen, update all three:

| File | What it holds | Update rhythm |
|---|---|---|
| `CLAUDE.md` (this file) | Current design system, libraries, decisions, guardrails. The *what the project is now* doc. | When a design token, library, or guardrail changes. |
| `scenes.md` | The narrative spec — what the reader sees, beat by beat, in scroll order. Plain language only, no code or class names. | After every round that affects the reader's flow. |
| `corrections.md` | Chronological change log — what changed, why, which files touched. | After every round, append a new `## Round N` block at the bottom. |

### `scenes.md` format (important)

Use a single continuous **"Scroll → scene → Scroll → scene"** flow. Each beat
is one paragraph: bold name + plain-language description of what the reader
sees, with on-screen text rendered as **bold** (display copy) or *italic*
(secondary copy). No mechanics, no class names, no library references.

Always keep a **"← This is where we are now."** marker on its own line at the
end of the live flow so the reader of the doc knows exactly where the build
has reached. Move the marker forward each round as new beats land.

When a beat changes, edit its paragraph in place — don't append a duplicate.
When a beat is removed from the live flow, delete its paragraph from
`scenes.md` and document the removal in `corrections.md`.

### `corrections.md` format

Each round gets a `## Round N — <short title>` heading with three fields:
**Change**, **Why**, **Files**. Append at the bottom; never rewrite earlier
rounds. If a later round reverses an earlier decision, call that out
explicitly in the new round so the reasoning chain stays readable.

---

## Local development

A live-reload server is the workflow.

```bash
cd /Users/agustincarrasco/Desktop/tbl_ai_ops/indicator_story
npx --yes live-server --port=5180 --no-browser --quiet
```

Then open http://127.0.0.1:5180/index.html. The browser auto-refreshes on save.

The user runs Antigravity IDE (VS Code fork) with Jupyter extension — no special
tooling assumptions needed; this is plain HTML.

## Stack (CDN, no install)

| Library | Version | Role |
|---|---|---|
| Scrollama | 3.2.0 | Scene triggers via IntersectionObserver |
| D3 | 7 | Scales, line generators, path interpolation, axes |
| flubber | 0.4.2 | Path-to-path morphing for chart-state transitions |
| rough.js | 4.6.6 | Hand-sketched outlines on annotations and the zoom box |
| GSAP | 3.12.5 | Numeric tweens for stroke-dasharray reveals + dot pop-in |

No React, no Svelte, no Three.js, no Lenis. Native scroll is correct for data stories.

## Design system

### Color tokens (CSS custom properties)

| Token | Hex | Use |
|---|---|---|
| `--bg` | `#0D1117` | Page background |
| `--bg-card` | `#161B22` | Sticky chart pane background |
| `--ink` | `#E6E1D7` | Primary chart line, body text |
| `--ink-dim` | `#7A8290` | Secondary text, axis labels |
| `--accent` | `#F5C518` | Highlights, key callouts (used sparingly) |
| `--buy` | `#3FB950` | Stylized buy dot (final scene) |
| `--sell` | `#F85149` | Stylized sell dot + **zoom box** |
| `--sketch` | `rgba(230,225,215,0.85)` | Rough.js stroke default |

**Important:** the zoom-window box uses red (`#F85149`) with a 15% red fill in
the intro/scene-1, and a fainter ~8% red fill in the label scene. This was a
deliberate user choice — not yellow (the original plan had it yellow).

### Typography

- **Display + body:** Inter (Google Fonts, 300–900 weights).
- **Mono:** JetBrains Mono with `font-feature-settings: "tnum"` for chart-axis
  numerics and small UI tags.
- The original plan called for Fraunces + Source Serif 4. User overrode to Inter
  on the second iteration. Don't revert.

### Motion

- Chart morphs: `d3.easeCubicInOut`, 800–1200ms.
- Stroke-dasharray line reveals: 1.6–2.4s, `power2.out`.
- Intro line draw: 2.6s, `cubic-bezier(.22,.61,.36,1)`.
- "The Boat" pacing rule: leave breathing room between beats. Slow and editorial,
  not snappy.

## Aesthetic inspirations

1. **Ray Dalio — *How the Economic Machine Works*** (full video).
   `artwork_inspiration/*.png`. Drives the *chart and annotation* look.
2. **SBS — *The Boat* (Matt Huynh, 2015)** — https://www.sbs.com.au/theboat/.
   Drives *pacing and atmosphere*. Slow scroll, full-bleed panels, weighty silence.
3. The user's own 9-frame hand storyboard in `storyline_idea/*.HEIC` is the
   single source of truth for what happens scene-by-scene.

## Story arc (current implementation)

**See `scenes.md` for the full reader-facing flow.** This table is a
high-level scan only. Every entry below corresponds to a section in
`index.html`; the scenes.md doc is the source of truth for what the reader
sees, beat by beat.

| Section | Type | Beats | What happens |
|---|---|---|---|
| **Intro** (`.intro`) | Full-bleed, scroll-hijacked | 1 | Logo + headline "How we built our TBL Liquidity Indicator." Wheel/touch drives `progress` 0→1; chart container expands from 38vw×30vh to 100vw×100vh. Title fades+recedes by progress 0.55. **Scroll-up at scrollY≤4 re-engages the lock.** |
| **Label scene** (`.label-scene`) | Full-bleed sticky | 4 | (1) "TBL Liquidity Index" name card with red baseline. (2) Title swaps to "Zoom into a 30-day period," red box appears. (3) Title swaps to "Calculate the change within that 30-day period," viewBox zooms into the box. (4) Same view, yellow bracket + "30-day change" annotation appear next to the box. |
| **Roll-forward** (`.roll-forward`) | Full-bleed sticky | 1 | Typographic moment: "Roll that 30-day change throughout the life of our index, and you get a cycle." |
| **Delta chart** (`.delta-chart`) | Full-bleed sticky | 5 | (1) "Rolling 30-day change of our index" — clean macro-sine + ripples. (2) Smoothed yellow line reveals over dimmed noisy line. (3) Green SPX line + legend overlay. (4) Boxes at one peak + one trough with HEADWIND/TAILWIND labels and wind-blowing background. (5) Title swaps to flat-slope framing, both box labels cross-fade to "FLAT SLOPE." |
| **Slope chart** (`.slope-chart`) | Full-bleed sticky | 2 | (1) Purple slope line draws, white dots pop in at six zero crossings; legend "Slope of Cycle." (2) Title swaps to crossing-direction framing; downward-crossing dots turn red with "LIQUIDITY PEAK" labels above, upward-crossing dots turn green with "LIQUIDITY TROUGH" labels below. |
| **TBL Pulse CTA** (`.pulse-cta`) | Full-bleed sticky | 1 | Small lead-in copy plus massive "TBL PULSE →" link to research.thebitcoinlayer.com. |
| **Closing note** (`.closing-note`) | Full-bleed sticky | 1 | Final restrained paragraph: "This model simply lets us see what liquidity is doing…" End of page. |

## Data

- **Pure illustrative.** No Supabase, no real history, no API calls.
- Deterministic synthetic series generated at page load via a `mulberry32(seed)`
  PRNG with seed `20260521` — same shape every visit.
- 600 data points. The "Index" oscillates around 50 with three sin waves (slow
  T=320, mid T=110 phased, fast T=38) + linear drift + small noise.
- Derived series: `delta30`, `smoothed` (double EMA approximating HP filter),
  `spxDelta`, `slope`.
- 30-day zoom window: **data points 335–365**, centered on a clean central peak
  where slow + mid cycles align (i≈345). This is the position the user
  approved after iteration. Don't move it without asking.
- `data.zoomYMin` / `data.zoomYMax` are precomputed from the slice — used to
  draw the zoom box tightly around the line with `PAD=3.2` data-unit padding.

## Decisions log (chronological — read this before changing things)

| Date | Decision | Rationale |
|---|---|---|
| 2026-05-21 | **Single HTML file, no build** | User picked over multi-file structure. |
| 2026-05-21 | **Free public marketing page** (vs Pulse-internal) | Top-of-funnel role. Signal-protection rule applies. |
| 2026-05-21 | **Pure illustrative data** | No live signals on a public page. |
| 2026-05-21 | **Inter** replaces Fraunces + Source Serif | User override. |
| 2026-05-21 | Intro switched from cinematic-overlay to **scroll-hijack expand** | User shared a Next.js `ScrollExpandMedia` reference. |
| 2026-05-21 | Title **stays visible**, fades with `progress` | User clarified after initial implementation removed it entirely. |
| 2026-05-21 | **Box color = red `#F85149`** with ~15% fill | User drew the target in red. |
| 2026-05-21 | **30-day window = 30 data points** (was 100) | The story is about 30 days; the box should actually be 30 days. |
| 2026-05-21 | **Box centered on data points 335–365** (central peak) | User pointed with an arrow to the central peak after seeing it on the right. |
| 2026-05-21 | **Label scene added** between dive and zoom-in | User wanted readers to know what the line is before zooming. |

## Brand voice (inherited from root CLAUDE.md)

- First-person ("I" — Nik), never "we."
- **Avoid** em dashes, mid-sentence colons, "let's break this down," "it's
  worth noting," "money printer," "fiat" used loosely, "store of value."
- **Use** when they fit naturally: first-layer money, savings technology,
  freedom of currency denomination, the Bitcoin Age.
- No ghostwriting. Drafts under Nik's name must be visibly scaffolded — copy in
  this page has `[NIK: ...]` prompts where his voice needs to land. Nik finishes
  the last 15–25%.
- Letter framing ("Dear Readers" / "Until next time, Nik") works on Substack
  letters; the page ends on a restrained closing-note paragraph instead.

## Things still TODO / open

- [ ] Nik fills in the `[NIK: ...]` prompts in scenes 1, 2, 4, 5, 6.
- [ ] Decide whether the final CTA goes to `thebitcoinlayer.substack.com` or a
  custom landing.
- [ ] Decide whether to add an audio cue à la "The Boat" (currently silent).
- [ ] Mobile QA at 375px — the sticky pattern is dropped via media query but
  hasn't been tested on a real phone.
- [ ] Possibly: a "dark-mode toggle" or a cream-paper variant (the original
  plan had cream as an alternative palette).

## Don't do this

- Don't switch the box back to yellow.
- Don't replace Inter with serifs.
- Don't wire this page to Supabase.
- Don't reveal real signal logic, dates, or prices anywhere.
- Don't add em dashes in user-facing copy.
- Don't introduce a build step for a "small" reason — the no-build constraint
  is a feature, not an oversight.
