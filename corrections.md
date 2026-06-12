# Corrections & Iterations Log

A running record of changes made to the TBL Liquidity Indicator scrollytelling
page after the initial build. New entries go at the bottom. Keep entries short:
*what changed*, *why*, *file touched*.

Companion to `CLAUDE.md` (which captures the *current* state and design system).
This file is the *history* of how we got there.

---

## 2026-05-21 — Initial build

Built `index.html` from the approved plan. Single file, CDN-loaded
Scrollama + D3 v7 + flubber + rough.js + GSAP. Dark institutional palette,
9 scenes + intro + outro, mobile fallback at <820px. Synthetic deterministic
dataset (mulberry32 PRNG, seed 20260521).

Original aesthetic decisions that got reversed in later rounds:
- Display font: Fraunces (variable serif). *Reversed below.*
- Body font: Source Serif 4. *Reversed below.*
- Zoom box: yellow `#F5C518`. *Reversed below.*
- Intro: full-bleed sketched wave + cinematic overlay title. *Reversed below.*

---

## Round 1 — Font swap

**Change:** All Fraunces and Source Serif 4 declarations replaced with Inter
(weights 300–900). JetBrains Mono kept for chart-axis numerics and small UI tags.

**Why:** User preference. Inter reads more institutional / less editorial for a
research firm landing page.

**Files:** `index.html` — `<link>` to Google Fonts, `--font-display` and
`--font-body` CSS variables, all per-class typography overrides (`.intro__h1`,
`.step__h`, `.step__p`, `.interstitial__h`, `.outro__h`, `.outro__sig`).

---

## Round 2 — Intro reworked as scroll-hijacked dive

**Change:** Original intro removed and replaced with a scroll-hijacked
"dive into the chart" pattern modeled on a Next.js `ScrollExpandMedia` reference
the user shared. Wheel / touch / arrow-key / page-down events feed into a
`progress` value (0→1) that resizes the chart container from `38vw × 30vh`
to `100vw × 100vh`. Body scroll is locked until `progress = 1`. Smooth catmull-rom
d3 path replaces the rough.js sketch (clean white line, not sketchy).

**Why:** User wanted readers to feel like they were diving *into* the chart, not
reading copy over it.

**Side note:** First iteration mistakenly removed the title entirely; corrected
in the next round.

**Files:** `index.html` — `.intro` CSS rewrite, intro IIFE rewrite, scroll-lock
handlers (`onWheel`, `onTouchStart/Move/End`, `onKey`).

---

## Round 3 — Local dev workflow

**Change:** Switched from `open file://...` to live-reload local server.
Command: `npx --yes live-server --port=5180 --no-browser --quiet`.

**Why:** Edit-refresh loop was too slow without auto-reload.

**Files:** None — workflow only. Documented in `CLAUDE.md`.

---

## Round 4 — Title restored, fades on scroll

**Change:** "Liquidity moves in cycles." title brought back, but now bound to
scroll progress. Title opacity = `max(0, 1 - progress / 0.55)` so it's fully
faded by 55% of the dive. Also scales down (`scale(1 - progress*0.06)`) and
translates up (`translateY(-progress*30px)`) so it feels like the reader is
moving past the words into the chart. `mix-blend-mode: difference` on the h1
so it inverts cleanly where it overlaps the white line.

**Why:** User clarified — they didn't want the title removed, they wanted it
to fade as the dive progresses.

**Files:** `index.html` — `.intro__title*` CSS, `setSize()` in intro IIFE.

---

## Round 5 — Yellow zoom box added at end of intro

**Change:** Hand-sketched (rough.js) yellow box fades in during the last 40%
of the dive (progress 0.6 → 1.0), positioned at scene 1's zoom-window
coordinates so the handoff between intro and scene 1 is visually continuous.

**Why:** Smooth the transition — the box becomes the "carry over" element.

**Files:** `index.html` — added `boxG` element + opacity update inside `setSize()`.

---

## Round 6 — Re-entry (scroll up to zoom back out)

**Change:** When the intro is done and the user scrolls up while at `scrollY ≤ 4`,
the intro lock re-engages at `progress = 1`. Wheel-up / touch-down / ArrowUp
all trigger this. From there, scrolling further up *decreases* progress —
the chart shrinks back, the title fades back in.

**Why:** User wants reversibility — scrolling back up should let them re-enter
the intro state, matching the React reference's behavior.

**Files:** `index.html` — `reEngageIntro()` function, updated `onWheel` /
`onTouchMove` / `onKey` to handle the `expanded` branch.

---

## Round 7 — 30-day window tightened (size)

**Change:** Zoom window shrunk from 100 data points to 30 data points
(`zoomEnd - zoomStart` is now exactly 30). Box also hugs the line vertically
with a 3.2-unit data padding around the slice's min/max, instead of spanning
the full chart height. Scene 1's "30 days" label moved to *above* the box
since it's too small for a horizontal pointer from outside.

**Why:** Story is about a 30-day window — the box should actually be 30 days.
And it should look like a tight magnifier, not a vertical band.

**Files:** `index.html` — `data.zoomStart`/`data.zoomEnd`/`data.zoomYMin`/
`data.zoomYMax` initialization, intro `boxNode`, scene 1 `boxNode` + label code.

---

## Round 8 — Box color changed to red

**Change:** Stroke color swapped from yellow `#F5C518` to red `#F85149`
(same red used for sell signals). Added a 15%-opacity red fill rectangle behind
the sketchy stroke, in both the intro and scene 1.

**Why:** User drew the target box in red. Wanted a translucent red fill rather
than just an outline.

**Files:** `index.html` — intro IIFE (added `fillRect` element before `boxNode`),
scene 1 (`sketchG.append('rect')` before `boxNode`), arrow + label color shifted
to `#F85149`.

---

## Round 9 — Box position moved to central peak

**Change:** `data.zoomStart` / `data.zoomEnd` shifted from `555 / 585` (far right)
to `335 / 365` (central). This places the box on the chart's most prominent
peak, where the slow + mid cycles align around i ≈ 345.

**Why:** User pointed with an arrow drawing — the right-side position wasn't
where they wanted the focus. One data change ripples through intro, label,
scene 1, and scene 2's camera-zoom target so they all stay in sync.

**Files:** `index.html` — data IIFE only (`zoomStart` / `zoomEnd` constants).

---

## Round 10 — Label scene added between dive and zoom-in

**Change:** New 160vh `<section class="label-scene">` inserted between the
intro and the first scrolly. Sticky container holds: full-bleed white line,
big bold "TBL Liquidity Index" headline top-left, red dashed Y-axis (left
edge) + red dashed zero line (middle), and a faint red box echo at the
zoom-window position.

**Why:** User wanted readers to know what they were looking at before being
zoomed in. A labeling moment.

**Files:** `index.html` — new HTML section, `.label-scene*` CSS, label-scene
IIFE that paints the SVG content.

---

## Round 11 — Label scene refinements

**Change:**
- Vertical Y-axis red dashed line removed.
- Horizontal axis line repositioned from `y = 300` (middle) to `y = 380`
  (lower-middle), styling changed from dashed to **solid** to match the user's drawing.
- Faint red box echo removed entirely from the label scene — it's a clean
  "name card" with no zoom annotations.
- `.label-scene__title` font-family declaration made explicit
  (`'Inter', -apple-system, ...`) instead of relying on the CSS variable, as a
  safety net.

**Why:** User wanted a cleaner label scene with no red square at all and the
red line lower. The vertical axis didn't add anything.

**Files:** `index.html` — label-scene IIFE (removed `yLine` and `fillRect`,
modified `xLine`), `.label-scene__title` CSS.

---

## Round 12 — Intro box ghost fix

**Change:** At the moment the dive completes (`finishIntro()`), the intro
box's inline opacity is hard-set to `0`. Previously, the box relied on the
parent `.intro.is-done` opacity fade (0.4s ease-out) to disappear — which
allowed it to briefly ghost over the label scene during the transition.

**Why:** User reported the red square "stayed as you scroll down." Investigation
showed the parent fade-out was leaking. Hard-hiding the box directly cuts it
instantly. The `reEngageIntro()` path still restores it correctly via
`setSize()` if the user scrolls back up.

**Files:** `index.html` — intro IIFE, `finishIntro()` function.

---

## Round 13 — Documentation

**Change:** Created `CLAUDE.md` (project context for future sessions) and
this `corrections.md` (change log).

**Why:** User wants future sessions to pick up exactly where we left off
without re-deriving the brand or creative choices.

**Files:** `CLAUDE.md` (new), `corrections.md` (new — this file).

---

## Round 14 — Label scene becomes a two-beat moment; red box removed from intro dive

**Change:** Restructured the dive → label scene → zoom-in transition.

- **Intro dive:** red zoom box removed entirely. The dive now shows only the
  expanding white line. The previous box-fade-in over the last 40% of the dive
  is gone (`boxG` element, `setSize()` box update, and `finishIntro()` hard-hide
  all deleted from the intro IIFE).
- **Label scene:** section height grown from 160vh → 220vh. Sticky chart now
  hosts *two beats* on the same canvas:
  - Beat 1: title is **"TBL Liquidity Index"**, no red box, horizontal red line visible.
  - Beat 2: title cross-fades to **"Zoom into a 30-day period"**, red zoom box
    fades in at the same position used everywhere else (`data.zoomStart` /
    `data.zoomEnd` = 335 / 365), horizontal red line stays put.
- An invisible `.label-scene__beat2-trigger` element sits ~110vh into the section.
  An IntersectionObserver flips between beats based on whether the trigger is in
  view (using `rootMargin: '0px 0px -40% 0px'` so the swap fires when the trigger
  is ~40% up the viewport — feels natural mid-scroll).
- Scrolling back up reverses the swap cleanly (titles + box both restore).

**Why:** User specified the exact chronological flow they wanted:
title card → dive → "TBL Liquidity Index" card → scroll → title swaps to
"Zoom into a 30-day period" with red box appearing *on the same screen* (not on
a new framed pane). Previous flow had the box appearing during the dive itself,
which contradicted this.

**Files:**
- `index.html` — intro IIFE (removed `boxG`/`fillRect`/`boxNode`,
  removed `setSize()` box opacity, removed `finishIntro()` box hide).
- `index.html` — label scene HTML now has `.label-scene__titles` wrapper
  with two `<h2>` elements + invisible trigger.
- `index.html` — `.label-scene*` CSS rewrite (taller section, stacked titles
  with opacity transition, `.label-zoom-box.is-on` state, trigger positioning).
- `index.html` — label-scene IIFE re-adds the red box (initially hidden) and
  wires IntersectionObserver to toggle `is-active` titles + `is-on` box.

---

## Round 15 — Label scene gets a third beat: literal camera zoom

**Change:** Added a third beat to the label scene that *quite literally zooms*
the chart into the red box.

- **Zoom window relocated and shrunk.** `data.zoomStart` / `data.zoomEnd` moved
  from `335 / 365` to `310 / 330` — now a 20-point window (was 30) on a clearly
  ascending segment near the chart's horizontal middle.
- **Section grew to 320vh** so the sticky pane can hold three beats. Triggers
  positioned at `top: 100vh` (beat 2) and `top: 210vh` (beat 3).
- **Beat 3 title:** *"Calculate the change within that 30-day period."*
- **Beat 3 mechanic:** SVG `viewBox` animates from `0 0 1600 600` to a tight
  crop centered on the red box (aspect-preserved 8:3, includes the horizontal
  red line at the bottom). 1.3s `d3.easeCubicInOut` transition. Reverses
  cleanly when the user scrolls back up.
- **Non-scaling strokes** added (`vector-effect="non-scaling-stroke"`) to the
  white path, red baseline, box stroke, and rough.js nested paths so lines
  don't appear chunky once the viewBox crops in.
- **Beat dispatch rewritten.** Switched from per-trigger IntersectionObservers
  to a single rAF-throttled scroll listener that checks each trigger's
  `getBoundingClientRect().top` against a 40%-from-top anchor line. Cleaner
  state model — beat=max trigger that has crossed the anchor. Works correctly
  in both directions.

**Why:** User specified the next beat: literal zoom into the red-box area as
the user scrolls, with a synchronized title swap. Also specified the box
should be smaller and on an upward-moving segment near the middle.

**Files:**
- `index.html` — data IIFE (`zoomStart` / `zoomEnd` constants).
- `index.html` — label scene HTML (third `<h2>` title, third `.label-scene__trigger` div).
- `index.html` — `.label-scene*` CSS (section height 220vh → 320vh, two
  trigger positions via `[data-beat]` selectors, title `max-width: 18ch` so the
  longer beat-3 title can wrap).
- `index.html` — label-scene IIFE (compute zoom viewBox from box bounds,
  `animateViewBox()` via d3 transition, scroll-position beat dispatch, vector
  effects on strokes).

---

## Round 16 — Beat 4 annotation + "Roll forward" text + noisy delta-chart scene

**Change:** Extended the story past the zoom moment with three new beats.

- **Beat 4 added to the label scene.** Same title as beat 3 ("Calculate the
  change within that 30-day period."), same zoomed viewBox, but a yellow
  bracket and *"30-day change"* label now appear next to the red box. The
  bracket spans the actual data delta from `data.index[zoomStart]` to
  `data.index[zoomEnd]`. Label scene grew from 320vh → 420vh; beat 4 trigger
  at `top: 320vh`.
- **New section: `.roll-forward`.** 160vh full-bleed sticky typographic
  moment. Inter 800 weight, large display size, text starts at `translateY(40vh)`
  (below center) and animates to centered position when the section reaches
  ~25% intersection. Content: *"Roll that 30-day change throughout the life of
  our index, and you get a cycle."*
- **New section: `.delta-chart`.** 200vh full-bleed sticky with a new SVG
  rendering the noisy `delta30` series — oscillating white catmull-rom line
  around a solid red zero baseline. Title *"30-Day Change"* top-left.
  Italic accent caption *"Noisy."* bottom-right, fades in after the line draws.
  Stroke-dasharray reveal (2.6s) triggers when section enters view.

**Flow now reads:** title card → dive → "TBL Liquidity Index" + baseline →
"Zoom into a 30-day period" + small red box → zoom into box →
"30-day change" annotation appears → chart releases up, "Roll that 30-day
change throughout the life of our index, and you get a cycle." text rises
from below → text releases up, noisy 30-Day Change chart takes over.

**Note:** The current scrolly section's scenes 1, 2, and 3 are now narratively
duplicated by these full-bleed beats (label scene beats 2/3/4 cover scenes 1/2,
and the new delta-chart section covers scene 3's noisy reveal). They still
render below the new content. Awaiting user direction on whether to delete
those scrolly steps to clean up the flow.

**Files:**
- `index.html` — label scene HTML (added 4th trigger), new `.roll-forward`
  and `.delta-chart` sections.
- `index.html` — CSS for `.label-scene` height bump, `.label-annotation`
  state, full new blocks for `.roll-forward*` and `.delta-chart*`.
- `index.html` — label-scene IIFE (added SVG `<g>` for the annotation with
  bracket + text, updated `setBeat()` to keep title 3 active for beats 3 & 4
  and toggle the annotation on beat 4).
- `index.html` — two new IIFEs: `rollForward()` (IntersectionObserver toggles
  `.is-visible`), `deltaChart()` (renders noisy line + zero baseline, dasharray
  reveal on section enter, italic caption fade-in via parent class).

---

## Round 17 — Delta chart becomes a clean rhythmic wave; scenes.md format rewritten

**Change:**

- **Title text:** `"30-Day Change"` → **`"Rolling 30-day change of our index"`**.
- **`"Noisy."` caption removed entirely** (element, CSS, and the `.is-visible`
  toggle that drove its fade-in).
- **Chart data regenerated.** The delta chart no longer reuses `data.delta30`
  (which had high-frequency jitter). It now generates its line directly from
  two sine components:
  - Macro sine: **3 full cycles** across the width, amplitude 175 viewBox units.
  - Mini sine: 18 cycles across the width (≈ 6 ripples per macro cycle),
    amplitude 22 viewBox units (~13% of macro), phase offset 0.7 rad.
  - **No random noise.** The line reads as a rhythmic curvy wave with small
    riding ripples, not jittery static.
- **Title styling:** added `max-width: 60vw` and `line-height: 1.05` so the
  longer title wraps gracefully on narrower screens.
- **`scenes.md` rewritten in the user's preferred format:** a single continuous
  "Scroll → scene → Scroll → scene" flow with plain-language scene paragraphs.
  Tags/class names removed. A **"← This is where we are now."** marker now
  sits at the end of the live flow so the doc always reflects current progress.
- **`CLAUDE.md` updated** with a new "Document conventions" section that names
  the three living docs (this one, `scenes.md`, `corrections.md`), their roles,
  and the rhythm for updating each. Includes the `scenes.md` format spec.

**Why:** User confirmed the visual target (clean macro sine + small riding
ripples, like rolling-window-of-changes look) for the chart that comes after
*"Roll that 30-day change..."*. Also wanted `scenes.md` to use the natural
scroll-flow format they've been using in chat, and asked that the process be
captured in `CLAUDE.md` for future sessions.

**Files:**
- `index.html` — delta-chart HTML (removed `<p class="delta-chart__caption">`,
  retitled `<h2>`), `.delta-chart__title` CSS (max-width + line-height),
  removed `.delta-chart__caption` and `.delta-chart.is-visible` rules,
  `deltaChart()` IIFE rewritten to generate the two-component sine.
- `scenes.md` — full rewrite into the "Scroll → scene" format with marker.
- `CLAUDE.md` — new "Document conventions" section at the top of the body,
  documenting the three-file system and the `scenes.md` format rules.

---

## Round 18 — Delta chart gets a second beat: the smoothing reveal

**Change:**

- **Delta-chart section becomes a two-beat sticky moment.** Section height
  bumped from 200vh → 240vh; sticky pins for ~140vh of scroll.
- **Title swap.** First title *"Rolling 30-day change of our index"* (beat 1)
  cross-fades to **"The raw 30-day change is noisy, so we smoothed it out."**
  on beat 2. Titles are stacked via absolute positioning and crossfade
  through opacity + 8px translateY, same pattern as the label scene.
- **Smoothed line.** A second SVG path is generated using only the macro sine
  component (no mini ripples). It's drawn in accent yellow (`#F5C518`), 2.6px
  stroke, hidden by default. On beat 2 it reveals via stroke-dasharray (1.9s
  ease-out) and the original noisy line dims to 22% opacity behind it.
- **Trigger** sits at `top: 110vh` of the section; the existing scroll-rAF
  threshold check (`anchor = 40vh from top`) fires `setBeat(2)` cleanly.
- Reverses correctly when scrolled back up (titles swap back, noisy line
  un-dims, smoothed line fades out — only the dasharray reveal animation
  doesn't re-play once it's already drawn).

**Why:** User specified the next beat: title swap to "The raw 30-day change
is noisy, so we smoothed it out." plus a new smoothed line tracing the macro
sine through the existing wavy line — making the smoothing visible.

**Files:**
- `index.html` — delta-chart HTML (titles wrapper with two `<h2>`s, trigger
  div), CSS for `.delta-chart__titles`, `.delta-chart__title`,
  `.delta-chart__trigger`, `.delta-noisy.is-dimmed`, `.delta-smoothed.is-on`,
  and updated section height.
- `index.html` — `deltaChart()` IIFE rewritten: generates both noisy and
  smoothed paths from shared sine parameters, keeps noisy line's
  entry-reveal IntersectionObserver, adds beat-dispatch scroll listener with
  `setBeat()` that toggles `is-dimmed` / `is-on` classes and triggers a
  one-time stroke-dasharray reveal on the smoothed line at first beat-2 entry.
- `scenes.md` — appended new "**The raw 30-day change is noisy…**" block,
  moved the "← This is where we are now." marker.

---

## Round 19 — Delta chart gains beats 3 and 4: SPX overlay + peaks/troughs with wind

**Change:**

- **Delta-chart section is now a four-beat sticky moment.** Height bumped
  from 240vh → 460vh; sticky pins for ~360vh of scroll. Three triggers at
  `top: 100vh / 210vh / 330vh`.

- **Beat 3 — SPX overlay.**
  - Title cross-fades to **"This smoothed cycle moves closely with 30-day
    changes in risk assets like the S&P 500."**
  - Noisy line fully hides (new `.is-hidden` class, opacity 0).
  - Yellow smoothed line stays.
  - New green SPX line reveals (color `#4ADE80`, 2.4px). Path is built from
    `sin(macroFreq + 0.32) * macroAmp * 0.86` plus a `sin(macroFreq*2.4) * 14`
    wobble, so it reads as a *correlated-but-not-identical* trace of the
    yellow line — phase lead, slightly smaller amplitude, gentle extra wave.
  - Legend appears in the bottom-right: small swatches + Inter labels for
    *TBL Liquidity Cycle* (yellow) and *S&P 500 30-day change* (green).

- **Beat 4 — peaks & troughs with wind.**
  - Title cross-fades to **"Thus, finding peaks and troughs in this cycle
    identifies liquidity headwinds or tailwinds."**
  - Green line and legend fade out. Yellow smoothed line stays solo.
  - Two red boxes (rough.js sketched outline + 18% red fill) appear: one
    around the macro peak at `i=250` labeled **TAILWIND**, one around the
    trough at `i=350` labeled **HEADWIND**. Labels in Inter 700 with letter
    spacing, positioned above the peak / below the trough.
  - "Wind blowing" background: a `<g class="delta-wind">` with 7 thin
    `rgba(230,225,215,0.10)` curved horizontal paths, each animated via
    `stroke-dasharray: 36 80` + a `windFlow` keyframe shifting offset to
    `-260` over 14s. Alternating rows reverse direction and vary speed
    (`nth-child(2n)` / `(3n)`) so the field feels alive rather than uniform.

- **Beat reversal still works.** Scrolling back up un-toggles all classes
  cleanly — only the dasharray reveals don't replay once already drawn.

**Why:** User specified two new beats: SPX overlay with legend, then a
peak/trough highlight with "headwind / tailwind" framing and a wind-blowing
background.

**Files:**
- `index.html` — delta-chart HTML (titles 3 + 4 added, two more triggers,
  new `.delta-chart__legend` block with two swatch rows).
- `index.html` — CSS: section height to 460vh, trigger positions for beats 3
  and 4, `.delta-noisy.is-hidden`, `.delta-spx`, `.delta-chart__legend`,
  `.delta-extremes`, `.delta-wind` + `@keyframes deltaWindFlow`, mobile
  fallbacks for all of the above.
- `index.html` — `deltaChart()` IIFE: added SPX line generator, peak/trough
  rough.js boxes with HEADWIND/TAILWIND labels, wind background group,
  legend reference, and extended `setBeat()` to handle the new line/box/wind/
  legend toggles plus a one-time stroke-dasharray reveal for the SPX line.
- `scenes.md` — appended the two new beat blocks, moved the
  *"← This is where we are now."* marker.

---

## Round 20 — Regression fix: noisy line invisible + title wraps to one-word-per-line

**Change:**

- **Root cause of the missing wavy line.** When the delta-chart section grew
  to 460vh in Round 19 (four beats), the IntersectionObserver that triggers
  the noisy line's stroke-dasharray reveal still gated on
  `intersectionRatio > 0.3`. But a 460vh section against a 100vh viewport
  can only ever hit a max ratio of `100/460 ≈ 0.217`. That threshold could
  never be crossed, so the callback never fired and the line stayed hidden
  behind its dasharray offset. Bug introduced in Round 19.
- **Fix.** Observe the sticky inner `.delta-chart__sticky` (which is exactly
  100vh, so it can reach `intersectionRatio = 1`) instead of the long section.
  Threshold logic kept at `> 0.5`, which now fires correctly when about half
  the sticky chart is on screen.
- **Title wrap fix.** `.delta-chart__title` had `max-width: 22ch` which at
  Inter 800 weight + 52px was tight enough to force *one word per line* on
  the longer beat-1 title. Switched to `width: max-content` with
  `max-width: min(820px, 55vw)` so each title sizes naturally to its content,
  capped at a sensible width for wrapping the longest beats (3–4) to 2–3 lines.
  Mobile rule simplified to `max-width: 85vw`.

**Why:** User reported the white wavy line was fully gone from the "Rolling
30-day change of our index" beat. Screenshot also showed the title wrapping
to one word per line. Both are Round-19 regressions.

**Files:**
- `index.html` — `deltaChart()` IIFE: observer target swapped from `section`
  to the inner `.delta-chart__sticky` element.
- `index.html` — `.delta-chart__title` CSS: `max-width: 22ch` →
  `width: max-content` + `max-width: min(820px, 55vw)`. Mobile override
  switched to `max-width: 85vw`.

---

## Round 21 — Swap HEADWIND / TAILWIND labels

**Change:** In the delta-chart beat 4 (peaks/troughs scene), the peak label
now reads **HEADWIND** and the trough label reads **TAILWIND** (previously
the opposite). The boxes themselves are unchanged; only the label strings
swap.

**Why:** User asked for the swap. Framing is forward-looking: at a liquidity
peak the next move is down (a headwind is coming); at a trough the next
move is up (a tailwind is coming).

**Files:**
- `index.html` — `deltaChart()` IIFE, the two `drawExtremeBox(...)` calls.
- `scenes.md` — beat 4 description, label strings swapped.

---

## Round 22 — Delta chart beat 5: title swap + box labels become "FLAT SLOPE"

**Change:**

- **Delta-chart section becomes a five-beat sticky moment.** Height bumped
  from 460vh → 570vh; new beat-5 trigger at `top: 450vh`. Mobile section
  height grew from 380vh → 470vh with trigger 5 at `top: 370vh`.
- **Beat 5 title:** **"These peaks and troughs are found where the slope of
  the cycle is flat."** Cross-fades in via the existing title-stacking
  mechanic.
- **Box labels cross-fade.** Each red box now has TWO stacked `<text>`
  elements: a *primary* label (HEADWIND for the peak, TAILWIND for the
  trough) and a *secondary* label that always reads **FLAT SLOPE**. CSS
  controls visibility via `.delta-extremes.is-flat`:
  - default (beats 4): primary visible, secondary hidden
  - is-flat (beat 5+): primary hidden, secondary visible
  - 0.55s opacity transition both ways so the swap reads as a cross-fade
    in place, not a hard cut.
- All other beat-4 elements stay (boxes, wind background, baseline, smoothed
  yellow line). Beat 5 is purely a label + title swap on the same canvas.

**Why:** User specified the next beat: same chart, same boxes, title swaps
to the "flat slope" framing and both box labels become "FLAT SLOPE."

**Files:**
- `index.html` — delta-chart HTML (5th `<h2>` and 5th trigger added).
- `index.html` — CSS: section height to 570vh + mobile 470vh, trigger 5
  position rules, `.extreme-label--primary/--secondary` opacity rules under
  `.delta-extremes` + `.delta-extremes.is-flat`.
- `index.html` — `deltaChart()` IIFE: `drawExtremeBox()` refactored to
  render both labels via a small `makeLabel()` helper. `setBeat()` adds
  `extremesG.classList.toggle('is-flat', n >= 5)`.
- `scenes.md` — appended new "These peaks and troughs…" block; marker moved.

---

## Round 23 — New section: slope chart with zero-crossing marks

**Change:**

- **New `.slope-chart` section** inserted after `.delta-chart`. 200vh tall,
  sticky 100vh inner, full-bleed dark canvas — same design language as the
  earlier full-bleed scenes.
- **Title:** **"We thus chart the slope of the cycle to find the spots where
  the slope is zero."** Top-left, Inter 800, same sizing pattern as the
  delta-chart titles.
- **Chart contents:**
  - Solid red horizontal zero baseline (same `#F85149`, same opacity).
  - Yellow slope line (`#F5C518`, 2.6px). Mathematically it's the derivative
    of the smoothed cycle from the previous chart: `cos(i * macroFreq) * 160`.
    Same `macroFreq = 6π / 600` so peaks/troughs of the smoothed cycle become
    zero crossings on this line.
  - Six red filled circles (`#F85149` with a soft `rgba` halo stroke) placed
    at the analytic zero crossings `i = 50, 150, 250, 350, 450, 550`.
- **Reveal sequence:** when the sticky inner is 50%+ in view, the line
  draws itself via stroke-dasharray (2.4s ease-out). After 1.7s the circles
  pop in one-by-one with a 100ms stagger using GSAP `back.out(2.2)` easing.
- **Sticky transition into the new section:** standard CSS sticky behavior.
  When the delta-chart section ends, its sticky inner releases naturally and
  the slope chart's sticky takes over — reads as "previous chart shifts up,
  new chart enters from below."

**Why:** User specified the next beat: previous chart shifts up, new chart
appears with the title quoted above, marks at every zero crossing, modeled
on storyboard image 8 (the "Slope / Indicator" sketch).

**Files:**
- `index.html` — new `<section class="slope-chart">` block right after the
  delta-chart `</section>`.
- `index.html` — `.slope-chart*` CSS (section height, sticky, SVG sizing,
  title typography + mobile overrides).
- `index.html` — new `slopeChart()` IIFE that renders the cosine slope,
  computes analytic zero crossings, adds dots, wires the IntersectionObserver
  reveal + GSAP dot pop-in.
- `scenes.md` — appended new "We thus chart the slope of the cycle…" block,
  moved the "← This is where we are now." marker.

---

## Round 24 — Slope chart: line goes purple, dots go white, legend added

**Change:**

- **Slope line color** swapped from `#F5C518` (yellow) to **`#A78BFA`**
  (violet-400). The slope is its own concept now and shouldn't be confused
  with the smoothed cycle, so a distinct color makes sense.
- **Zero-crossing dot color** swapped from `#F85149` (red) to **`#FFFFFF`**
  (pure white). Halo stroke updated to `rgba(255,255,255,0.45)` so the soft
  glow around each dot reads on the dark background.
- **Legend added** in the bottom-right corner of the slope-chart sticky, same
  positioning + typography pattern as the delta-chart legend. One row: a
  purple swatch labeled **"Slope of Cycle."** No transition needed since
  there's only one beat in this section.

**Why:** User wanted the line purple, the dots white, and an explicit legend
identifying the purple line as the "Slope of Cycle."

**Files:**
- `index.html` — slope-chart HTML: `<div class="slope-chart__legend">` block
  inside the sticky.
- `index.html` — `.slope-chart__legend*` + `.slope-chart__swatch` CSS rules,
  mobile fallback.
- `index.html` — `slopeChart()` IIFE: line `stroke` from `#F5C518` →
  `#A78BFA`; circle `fill` from `#F85149` → `#FFFFFF`; halo stroke updated
  to white rgba.
- `scenes.md` — beat description updated to call out purple line, white dots,
  and the legend.

---

## Round 25 — Slope chart beat 2: dot color/label reveal at zero crossings

**Change:**

- **Slope-chart section becomes two beats.** Height bumped from 200vh →
  320vh; sticky pins for ~220vh of scroll. Beat-2 trigger at `top: 120vh`
  (mobile section to 260vh, trigger to `top: 100vh`).
- **Beat 2 title:** **"When the slope crosses below zero, the cycle has
  turned negative, and when the slope crosses above zero, the cycle has
  turned positive."** Cross-fades in via the same stacked-title pattern used
  in the delta-chart.
- **Per-dot direction tagging.** Each zero crossing is now classified at
  creation time: even-indexed crossings (analytic `i = 50, 250, 450`) are
  *downward* crossings of `cos(i*macroFreq)` and tagged `slope-dot--peak`;
  odd-indexed (`i = 150, 350, 550`) are *upward* and tagged
  `slope-dot--trough`. The direction is derived analytically (sign of the
  derivative of cos at each crossing), no runtime computation needed.
- **Color swap on beat 2.** CSS rule `.slope-dots.is-colored .slope-dot--peak`
  paints the dot `#F85149` (red) with a soft red halo; `.slope-dot--trough`
  paints `#3FB950` (green) with green halo. Transitions on `fill` + `stroke`
  for 0.6s ease-out so the change reads as a recolor, not a re-render.
- **Labels.** Every dot now has a sibling `<text>` element created at the
  same time (initial opacity 0). Peak labels read **LIQUIDITY PEAK** in red,
  positioned 26 viewBox units above the dot. Trough labels read **LIQUIDITY
  TROUGH** in green, 38 viewBox units below. All labels fade in via
  `.slope-dots.is-colored .slope-label { opacity: 1 }`.
- Beat dispatch follows the established pattern: stacked title cross-fade
  via `.is-active` class, rAF-throttled scroll listener computes
  `setBeat()` based on whether any trigger has crossed the 40%-from-top
  anchor. Reverses cleanly.

**Why:** User specified the next beat: title swap to the "positive /
negative crossing" framing, white dots recolor based on the direction of the
crossing (red for downward = peak, green for upward = trough), and each dot
gets a small per-color label.

**Files:**
- `index.html` — slope-chart HTML: wrapped both titles in
  `.slope-chart__titles`, added `data-beat` attrs, added beat-2 trigger
  element.
- `index.html` — CSS: section height bumped, stacked-title cross-fade rules
  (`.slope-chart__title.is-active`), trigger position
  `.slope-chart__trigger[data-beat="2"]`, dot color states
  (`.slope-dot--peak/--trough` + `.slope-dots.is-colored`), label visibility
  rules, mobile overrides.
- `index.html` — `slopeChart()` IIFE: `crossings` array now stores
  `{ ix, isPeak }`, dot creation assigns `slope-dot--peak/--trough` class
  and creates a sibling `slope-label` text element with the right copy/color/
  y-offset, added `setBeat()` + scroll-anchor dispatch identical in shape to
  the delta-chart implementation.
- `scenes.md` — appended new "When the slope crosses below zero…" block,
  moved the "← This is where we are now." marker.

---

## Round 26 — Closing scenes + legacy purge + new title + logo

**Change:** Wrapped the page up and threw away the original framed-pane
scrolly content that the new full-bleed flow had already replaced.

- **New section: `.pulse-cta`.** 160vh sticky moment. Small lead-in line
  *"And that's how we built our TBL Liquidity Indicator, available on,"*
  followed by a massive **TBL PULSE →** link in accent yellow (Inter 900,
  font-size `clamp(80px, 16vw, 240px)`, letter-spacing `-0.05em`). Whole
  brand line wraps an `<a>` to `https://research.thebitcoinlayer.com/`
  (target `_blank`, `rel="noopener"`). Both lead and brand animate in from
  below with a 0.15s stagger when the section reaches 25%+ intersection.
  Arrow nudges right 0.15em on hover.
- **New section: `.closing-note`.** 140vh sticky moment. Single paragraph
  in `--ink-dim`: *"This model simply lets us see what liquidity is doing,
  and whether it is helping or harming risk assets. It is a tool that must
  be used alongside other indicators for informed decisions."* Same
  rise-from-below animation pattern.
- **Intro headline rewritten** from *"Liquidity moves in cycles."* to
  **"How we built our TBL Liquidity Indicator."** Subtitle updated to match
  ("A short walk through how the indicator was constructed, from the raw
  index to the buy and sell signals.").
- **TBL logo added** to the intro title block. File copied from
  `tbl_creatives/assets/logos/tbl_logo_white.png` into
  `indicator_story/assets/tbl_logo_white.png` and referenced as
  `<img class="intro__logo" src="assets/tbl_logo_white.png">`. Sized
  `clamp(90px, 11vw, 150px)`, placed above the kicker, fades together with
  the title group on scroll.
- **Legacy purge.** Deleted from `index.html`:
  - HTML: both `.scrolly` divs (scenes 1–5 and 6–7), the `.interstitial`
    section, the `.outro` block, and the `<footer>`. Page now ends at the
    closing note.
  - CSS: `.scrolly`, `.narrative`, `.step*`, `.viz*`, `.interstitial*`,
    `.outro*`, `footer`, the entire "SVG line styles" block
    (`.axis`, `.zero-line`, `.primary-line`, `.smoothed-line`, `.spx-line`,
    `.slope-marker`, `.buy-dot`, `.sell-dot`, `.sketch-layer`, `.label*`),
    and the legacy mobile rules that stacked the sticky scrolly. Mobile
    overrides for the sections that survive (intro, label, delta, slope, CTA,
    closing) live inside each section's own media block.
  - JS: the entire `chart` IIFE (Chart 1), the entire `chart2` IIFE
    (Chart 2), the helpers (`clearSketch`, `clearLabels`, `findPeaksTroughs`),
    every scene function (`scene1` through `scene8`), `renderScene`,
    `setupScrollama`, and both `window.addEventListener('load' / 'intro:done')`
    handlers that paint scene1.
- **Docs updated:** `scenes.md` reflects the new intro copy + the two new
  closing scenes + marker moved to the new end. `CLAUDE.md` "Story arc"
  table rewritten to match the actual sections (intro / label / roll-forward
  / delta / slope / pulse-cta / closing-note) with section names and beat
  counts. The "outro" reference in the brand-voice block was replaced with
  a closing-note reference.

**Why:** User said the page ends at the closing-note scene and everything
following should be removed. Also asked for the TBL logo + a new intro
headline.

**Files:**
- `index.html` — intro HTML (logo + new title + new subtitle),
  `.intro__logo` CSS, two new sections (`.pulse-cta` + `.closing-note`)
  with full CSS + entrance JS, sed-deleted lines 1525–2072 (legacy JS),
  manual edit removed the legacy CSS blocks.
- `indicator_story/assets/tbl_logo_white.png` — copied from
  `tbl_creatives/assets/logos/`.
- `scenes.md` — intro paragraph rewritten, two new closing-scene blocks
  appended, marker moved.
- `CLAUDE.md` — story-arc table rewritten to match current sections;
  brand-voice closing reference updated.

---

## Round 27 — Intro: kicker text removed, logo shrunk to brand-mark size

**Change:**

- Removed the `<div class="intro__kicker">The Bitcoin Layer · Research</div>`
  element from the intro title block. The TBL logo now sits directly above
  the headline with no text label between them.
- Shrank `.intro__logo` from `width: clamp(90px, 11vw, 150px)` (hero size)
  to `width: clamp(56px, 6vw, 84px)` (brand-mark size) so it visually
  replaces the kicker rather than dominating the title block.

**Why:** User asked for the kicker sign to be removed and the logo moved
into its position. With the logo at hero size it floated high above the
headline and didn't read as a brand mark; shrinking it makes it sit in
the slot the kicker used to occupy.

**Files:**
- `index.html` — intro HTML (`<div class="intro__kicker">` deleted),
  `.intro__logo` CSS (width clamp tightened).
- `scenes.md` — intro paragraph updated to drop the kicker description.

---

## Round 28 — Label-scene titles wrapping one-word-per-line (same bug as Round 20)

**Change:** `.label-scene__title` rewritten to use the same width strategy
the delta-chart and slope-chart already use:

- `max-width: 18ch` → `width: max-content` + `max-width: min(820px, 55vw)`
- Mobile rule `max-width: 14ch` → `max-width: 85vw`

**Why:** User reported the label-scene titles ("TBL Liquidity Index,"
"Zoom into a 30-day period," "Calculate the change within that 30-day
period.") were jamming each word onto its own narrow column instead of
wrapping to natural multi-line breaks. Same root cause as Round 20: at
Inter 800 weight + 52px, `18ch` is much narrower than the rendered text
needs, so it forces a vertical column. `width: max-content` lets each
title size to its content, capped by a sensible max so the longest beat
(beat 3) still wraps to 2–3 lines.

**Files:**
- `index.html` — `.label-scene__title` desktop + mobile rules.

---

## Round 29 — Intro logo 2.5× bigger

**Change:** `.intro__logo` width clamp scaled up by 2.5×:
`clamp(56px, 6vw, 84px)` → `clamp(140px, 15vw, 210px)`.

**Why:** After seeing the brand-mark-sized version, user wanted it
noticeably larger.

**Files:**
- `index.html` — `.intro__logo` CSS.

---

## Round 30 — Jank reduction batch (11 fixes from agent audits)

**Change:** Saved `index.stable.html` as a backup snapshot, then applied 11
smoothness/performance fixes to `index.html` based on three parallel agent audits.

1. **Intro stays `position: fixed` permanently.** `.intro.is-done` no longer
   changes `position` / `inset` / `height` — only toggles `opacity: 0` and
   `pointer-events: none`. Eliminates the layout reflow at the intro→label
   handoff (Safari was visibly flashing the scrollbar).

2. **Section heights shortened to remove dead zones.** `.label-scene`
   420vh → 380vh, `.delta-chart` 570vh → 510vh, `.slope-chart` 320vh → 200vh.
   Each section's last beat now fires ~60–80vh before release instead of
   100–200vh. Mobile heights scaled to match.

3. **Intro dive uses `transform: scale()` instead of width/height.** The
   `.intro__media` is now a fixed `100vw × 100vh` box and `setSize()`
   animates `transform: scale(s)` with `transform-origin: center`. GPU-
   composited, no layout per wheel tick. SVG content stays correctly
   centered via `preserveAspectRatio` — the visible chart line is
   indistinguishable from the old approach but each frame is materially cheaper.

4. **Label-scene viewBox resets when section exits.** A new IntersectionObserver
   on `#labelScene` calls `svg.setAttribute('viewBox', fullViewBox)` when
   `!isIntersecting`. Prevents the chart from sitting half-zoomed under the
   next section when the user scrolls back later.

5. **Reveal latches clean up.** New `ensureDrawn(el)` helper snaps a line to
   "fully drawn, no transition" state. Called after the dasharray reveal
   completes (so opacity is the only visibility control thereafter) AND in
   `setBeat()` reverse-cases (`n < 2 && smoothedRevealed → ensureDrawn` etc).
   Slope-chart dots: on `!isIntersecting`, `gsap.killTweensOf(dotEls)` + snap
   each dot to its final `r`. Both directions are now idempotent — no more
   stuck-mid-animation states.

6. **Entrance animations snap to start on exit.** `.roll-forward__text`,
   `.pulse-cta__lead`, `.pulse-cta__brand`, and `.closing-note__text` now
   have their `transition` rules ONLY on the `.is-visible` state. When the
   class is removed, properties snap back to the bottom-staged position
   instantly. Re-entrance always animates from the bottom. No more "weak
   second rise" on rapid up/down.

7. **HEADWIND/FLAT SLOPE cross-fade staggered.** Primary and secondary
   labels now have 0.3s opacity transitions with a 0.3s delay on the
   *incoming* one, so the outgoing label fades out first, then the incoming
   fades in. No more 50/50 overlap moment showing both texts at once.

8. **Wind animation pauses when off-screen.** `.delta-wind path` now has
   `animation-play-state: paused` by default; `.delta-wind.is-on path`
   sets it to `running`. 7 SVG paths no longer repaint continuously while
   off-screen.

9. **Single shared scroll dispatcher.** New `scrollDispatch` IIFE at the
   top of the script — one `window.addEventListener('scroll', …, {passive: true})`,
   one rAF tick per frame, fans out to registered callbacks. Three per-section
   scroll listeners (label-scene, delta-chart, slope-chart) replaced with
   `scrollDispatch.add(checkBeats)`. Confirmed via grep: file went from 3
   scroll listeners + 3 rAF gates to 1 of each.

10. **IntersectionObserver thresholds trimmed.** Roll-forward and closing-scenes
    observers used `[0, 0.15, 0.25, 0.5, 0.75, 1]` — replaced with `[0.25]`
    since the callback only checks a single cutoff. Noisy-line + slope-line
    reveal observers trimmed to `[0, 0.5, 1]`.

11. **Removed `transform-origin 0.2s ease` no-op** from `.pulse-cta__brand`
    transition list. (`transform-origin` was never animated.)

**Why:** User asked for a "less janky" experience after seeing several visible
hitches on scroll. Three parallel agents audited the file for reverse-scroll
state integrity, sticky-handoff transitions, and animation performance; this
batch implements every issue they flagged.

**Files:**
- `index.html` — all 11 fixes.
- `index.stable.html` — saved beforehand as a known-good snapshot. Untouched
  for the jank-reduction batch.

---

## Round 31 — TBL Pulse text replaced with logo image (both files)

**Change:**

- The giant yellow **TBL PULSE** display text in the CTA scene is now an
  `<img class="pulse-cta__logo" src="assets/TBL_Pulse_Logo.png">` element
  wrapped in the same `<a>` (still `href="https://research.thebitcoinlayer.com/"`,
  `target="_blank"`, `rel="noopener"`, plus an `aria-label="TBL Pulse"` for
  screen readers).
- `.pulse-cta__brand` CSS rewritten: dropped the huge `font-size: clamp(80px, 16vw, 240px)`
  (only used for the text), kept the layout as `inline-flex` with the logo +
  arrow side by side. Arrow font-size is now what drives its scale.
- New `.pulse-cta__logo` rule sizes the image at `clamp(280px, 50vw, 820px)`
  width, height auto. Hover dims the logo to 0.85 opacity. Arrow keeps its
  existing nudge-right hover.
- Applied to both `index.html` AND `index.stable.html` (per user's request to
  update both copies).

**Why:** User added the TBL Pulse logo to `assets/TBL_Pulse_Logo.png` and
asked for it to replace the typographic TBL PULSE in the semifinal scene.

**Files:**
- `indicator_story/assets/TBL_Pulse_Logo.png` — user-supplied logo (pre-existing).
- `index.html` — pulse-cta HTML + CSS updated.
- `index.stable.html` — pulse-cta HTML + CSS updated (same changes).

---

## Round 32 — Pushed to GitHub + enabled GitHub Pages on the stable build

**Change:**

- `git init` in `indicator_story/`, default branch `main`.
- Added remote `origin` → `https://github.com/AugustineCarB/indicator-story.git`
  (public repo, was empty before this push).
- Created `docs/` folder containing the served artifact:
  - `docs/index.html` — copy of `index.stable.html` (NOT the experimental
    `index.html`)
  - `docs/assets/TBL_Pulse_Logo.png` + `docs/assets/tbl_logo_white.png`
    so the relative `assets/…` paths in the served HTML resolve.
- Added `.gitignore` (skips `.DS_Store`, `node_modules/`, `.vscode/`,
  `.idea/`, `*.log`) and `README.md` (explains the file layout, local dev
  command, and the "promote experimental to stable" workflow).
- Initial commit, pushed to `origin/main`.
- Enabled GitHub Pages via `gh api -X POST /repos/.../pages` with
  `source[branch]=main` and `source[path]=/docs`. Pages serves the stable
  build from `docs/` while the experimental `index.html` stays untouched
  at repo root for ongoing iteration.

**Public URL:** https://augustinecarb.github.io/indicator-story/

**Why:** User wanted a public-facing hosted version of the page to share
with others, but explicitly asked that the *stable* index drive the public
site (not the experimental copy with the new smoothness fixes).

**Files:**
- New: `.gitignore`, `README.md`, `docs/index.html`, `docs/assets/*`
- Existing files unchanged (local `index.html` still serves as the
  experimental copy; `index.stable.html` remains the source of truth).
- GitHub Pages config: source = `main` branch, path = `/docs`.

---

## Round 33 — index.stable2.html: full smoothness + state-machine pass (Fable 5)

**Change:** Created `index.stable2.html` as a copy of `index.html` and applied
a complete smoothness/bug-fix pass to the copy. `index.html` and
`index.stable.html` are untouched; the deployed site is unaffected.

1. **Intro dive is lerped.** Wheel/touch/keys now move a *target* progress and
   a rAF loop eases the rendered progress toward it (factor 0.13, sleeps when
   settled). Chunky mouse-wheel notches and trackpad bursts produce the same
   glide; mid-dive reversals ease instead of snapping. After the dive
   completes, a 450ms cooldown swallows the same gesture's leftover momentum
   so the page no longer lurches straight into the label scene. Re-engaging at
   the top re-enters at full expansion and eases back out. The `.intro` fade
   transition moved to the base class so re-engage fades in instead of popping.
2. **Label-scene zoom-state bug fixed.** The exit observer used to snap the
   viewBox to full behind the state machine's back; scrolling back up into
   beat 3/4 then hit setBeat's `n === currentBeat` early-return and the zoom
   never re-applied (title + annotation showed, zoom missing). The exit
   handler now interrupts the in-flight d3 tween and resets `currentBeat = 0`
   so any re-entry fully re-applies its beat.
3. **Final-beat dwell fixed.** The last trigger in the label scene (320vh of
   380vh) and delta chart (450vh of 510vh) fired at *exactly* the scroll
   position where the sticky pane releases, so the annotation beat and the
   FLAT SLOPE crossfade were never seen while pinned. Section heights raised:
   label 380→440vh, delta 510→570vh, slope 200→260vh (mobile: 310→380,
   430→500, 180→230; pulse-cta 110→150vh and closing-note 110→140vh on
   mobile, which previously pinned for only 10vh). Every final beat now gets
   ≥50vh of pinned dwell.
4. **Stroke reveals are deterministic.** Shared `drawStroke`/`settleStroke`
   helpers commit the dasharray start state with a forced reflow before
   setting the end state (the old single-rAF pattern could coalesce both
   writes into one style recalc and skip the draw animation entirely).
5. **Slope chart hardened.** Dots force-complete (`gsap.killTweensOf` + snap
   to full radius) if the reader reaches beat 2 while the pop-in stagger is
   still pending, and the reveal skips the stagger when the reader arrives
   already past beat 1. The "Slope of Cycle" legend now fades in as the line
   draw approaches the right edge (it was always-on before; scenes.md said it
   appears).
6. **30-day box is 30 points.** Zoom window widened from 20 points (310–330)
   to 30 (305–335), same ascending segment, same center. The copy says
   "30-day period"; the box now spans exactly 30 daily points. (Note: this
   repo's CLAUDE.md said 335–365 while the shipped code was 310–330 — the
   doc had drifted; both now corrected.)
7. **Mobile canvas.** Phones (≤820px) draw on a 1600×1000 viewBox with all
   vertical geometry scaled by KY = 1000/600; the chart band at 390px is
   ~67% taller instead of a thin letterboxed strip. SVG text scales up
   (annotation 8→22, extreme labels 15→38, slope labels 13→40 user units),
   and only the central LIQUIDITY PEAK/TROUGH pair keeps labels on phones
   (six labeled dots cannot fit side by side; all six dots still color).
   Desktop output is pixel-identical (KY = 1).
8. **Platform.** Sticky panes use `100svh` with a `100vh` fallback (iOS
   URL-bar); full-bleed SVGs use `100%` instead of `100vw` (scrollbar-width
   overflow); `prefers-reduced-motion` honored (instant lerp, settled strokes,
   no cue bob, wind paused, instant viewBox); scrollama + flubber script tags
   removed (loaded but never called — beat dispatch is hand-rolled and
   nothing morphs); OG meta tags, theme-color, and an inline SVG favicon
   added; `history.scrollRestoration = 'manual'`; dead `intro:done` event
   removed; `window.__tblStory.skipIntro()` added as a deterministic hook for
   automated tests.

**Why:** User asked for a "perfected" copy of the page — smoother, every scene
displaying as intended, bugs fixed — without touching the existing files.

**Verification:** Headless system-Chrome (playwright-core) tours at 1440×900
and 390×844 plus a `prefers-reduced-motion` pass: screenshot + DOM-state dump
at all 16 beats, plus stress regressions (re-entry into the label zoom,
instant blast to slope beat 2, back-scroll into delta beat 3, intro re-engage
→ shrink → re-dive). All states matched scenes.md; zero page errors.

**Files:**
- New: `index.stable2.html` (the only page file touched).
- Docs: `scenes.md` (30-point box wording, mobile label note), `CLAUDE.md`
  (stack, file layout, zoom window, decisions log, TODOs), `README.md`
  (file table + promotion note), this file.

---

## How to add new entries

Each round gets a section with this shape:

```md
## Round N — <short title>

**Change:** <what changed in 1–3 sentences>

**Why:** <user's stated rationale or the problem it solved>

**Files:** <which file(s), and which functions/selectors if useful>
```

If a change reverses an earlier decision, note that explicitly so the
reasoning chain stays readable.
