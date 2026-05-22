# Scenes — Narrative Spec

The canonical scene-by-scene description of what the reader sees as they
scroll the TBL Liquidity Indicator page. **This file always reflects the
build up to the point we have currently reached** — when you read the last
"Scroll → …" entry, that's where we left off.

Format below: a single continuous flow. Each beat is a scene block with the
on-screen copy and a short note on what changes.

Companion files:
- `CLAUDE.md` — current design system + decisions
- `corrections.md` — chronological change log
- This file — what the reader sees, beat by beat

---

## Current build (in scroll order)

**Title card.** The reader lands on a dark canvas with a small smooth white
line centered on screen. Above the line sits the small white TBL logo,
followed directly by the headline **"How we built our TBL Liquidity
Indicator."** Subtitle below: *"A short walk through how the indicator was
constructed, from the raw index to the buy and sell signals."* A small
`SCROLL TO DIVE IN` cue fades in.

**→ Scroll →**

**Dive into the chart.** Scroll is hijacked. The chart container grows toward
the edges of the screen; the title fades, scales down slightly, and recedes
upward. By the end of the dive, the white line fills the entire viewport. No
red box has appeared yet.

**→ Scroll →**

**TBL Liquidity Index title card.** Full-bleed white chart line on dark, with
*"TBL Liquidity Index"* in bold Inter top-left. A solid red horizontal
baseline runs low across the chart.

**→ Scroll →**

**Zoom into a 30-day period.** Same chart, same baseline. Title cross-fades
to *"Zoom into a 30-day period"*. A small red box (20 data points wide, on
an ascending segment near the middle of the chart) fades in over the line
with translucent red fill + sketched outline.

**→ Scroll →**

**Calculate the change within that 30-day period.** The chart literally
zooms into the red box. The viewBox animates from full chart to a tight crop
centered on the box. The red baseline remains visible at the bottom of the
zoomed view. Title cross-fades to *"Calculate the change within that 30-day
period."*

**→ Scroll →**

**30-day change annotation.** Same zoomed view, same title. A yellow bracket
appears immediately to the right of the red box, spanning the actual vertical
delta of the line within the 30-day window. A yellow label *"30-day change"*
sits to the right of the bracket.

**→ Scroll →**

**Roll that 30-day change throughout the life of our index, and you get a
cycle.** The zoomed chart releases and scrolls up off screen. A large bold
sentence rises from the bottom of the viewport and pins centered:
**"Roll that 30-day change throughout the life of our index, and you get a
cycle."**

**→ Scroll →**

**Rolling 30-day change of our index.** Text releases up. New full-bleed
chart takes over. Title *"Rolling 30-day change of our index"* top-left. The
line itself is a clean macro sine wave (three full cycles across the width)
with smaller sinusoidal ripples riding on top — no random noise; it reads as
a rhythmic cycle. Solid red zero baseline runs through the middle. Line
draws itself via stroke-dasharray reveal when the section enters view.

**→ Scroll →**

**The raw 30-day change is noisy, so we smoothed it out.** Same chart, same
baseline. Title cross-fades from *"Rolling 30-day change of our index"* to
**"The raw 30-day change is noisy, so we smoothed it out."** The original
wavy line dims down to a faint background trace. A second line — yellow,
slightly thicker, perfectly smooth — reveals itself across the chart via
stroke-dasharray, tracing exactly the macro sine shape of the original but
without the little riding ripples. The contrast makes the smoothing visible.

**→ Scroll →**

**This smoothed cycle moves closely with 30-day changes in risk assets like
the S&P 500.** Title cross-fades to **"This smoothed cycle moves closely
with 30-day changes in risk assets like the S&P 500."** The noisy background
line fades out entirely. The yellow smoothed line stays put. A new green
line reveals across the chart, tracing the same broad cycle as the yellow
line but with a slight phase lead, a slightly smaller amplitude, and a gentle
extra wobble — so the two read as correlated but not identical. A small
legend appears in the bottom-right: a yellow swatch labeled *"TBL Liquidity
Cycle"* and a green swatch labeled *"S&P 500 30-day change."*

**→ Scroll →**

**Thus, finding peaks and troughs in this cycle identifies liquidity
headwinds or tailwinds.** Title cross-fades to **"Thus, finding peaks and
troughs in this cycle identifies liquidity headwinds or tailwinds."** The
green S&P line and the legend fade away; only the yellow smoothed line
remains. Behind the chart, faint horizontal flowing curves drift across the
screen, suggesting wind. Two small red boxes with translucent fill and
sketched outlines appear: one at a clear peak of the yellow line with the
label *"HEADWIND"* above it, one at a trough below the baseline with the
label *"TAILWIND"* underneath.

**→ Scroll →**

**These peaks and troughs are found where the slope of the cycle is flat.**
Nothing else changes about the chart. Same yellow line, same red baseline,
same two red boxes at the same peak and trough, same wind drifting in the
background. The title cross-fades to **"These peaks and troughs are found
where the slope of the cycle is flat."** And the labels on the boxes
cross-fade in place: the top box swaps from *"HEADWIND"* to **"FLAT SLOPE"**
and the bottom box swaps from *"TAILWIND"* to **"FLAT SLOPE"** as well.

**→ Scroll →**

**We thus chart the slope of the cycle to find the spots where the slope is
zero.** The previous chart releases up and scrolls off the top. A new
full-bleed chart takes over. Title **"We thus chart the slope of the cycle
to find the spots where the slope is zero."** sits top-left. The chart shows
a single purple line that oscillates up and down, crossing through a solid
red horizontal baseline several times across the width. The line draws
itself in from left to right. Once the line settles, six small white dots
pop in one after another, each landing exactly where the line crosses the
baseline. In the bottom-right, a small legend appears with a purple swatch
labeled *"Slope of Cycle."*

**→ Scroll →**

**When the slope crosses below zero, the cycle has turned negative, and
when the slope crosses above zero, the cycle has turned positive.** Same
chart, same purple line, same baseline, same legend. Title cross-fades to
**"When the slope crosses below zero, the cycle has turned negative, and
when the slope crosses above zero, the cycle has turned positive."** The
six white dots change color in place: every dot where the purple line is
crossing downward through the baseline turns red, every dot where the line
is crossing upward turns green. Each red dot picks up a small label above
it that reads *"LIQUIDITY PEAK,"* and each green dot picks up a label
below it that reads *"LIQUIDITY TROUGH."*

**→ Scroll →**

**And that's how we built our TBL Liquidity Indicator.** The slope chart
releases up and scrolls off. A new full-bleed scene rises in. A small
lead-in line at the top reads *"And that's how we built our TBL Liquidity
Indicator, available on,"* and directly underneath sits the two biggest
words on the screen: **TBL PULSE**, set in massive bold yellow letters with
a small arrow next to them. The whole thing is a clickable link to
research.thebitcoinlayer.com.

**→ Scroll →**

**This model simply lets us see what liquidity is doing.** The CTA scene
releases up. A final full-bleed text moment takes over with a single
restrained paragraph in muted ink: *"This model simply lets us see what
liquidity is doing, and whether it is helping or harming risk assets. It is
a tool that must be used alongside other indicators for informed decisions."*
This is the end of the page.

**← This is where we are now.**

---

## How to extend this file

1. After each round of work, append the next beat at the bottom using the
   same `**→ Scroll →**` + `**Scene name.** description` pattern.
2. Always move the **"← This is where we are now."** marker so it sits below
   the latest built beat — that line is the anchor for "what's live."
3. When the reader sees text on screen, render it as *italic* (subtitle / soft)
   or **bold** (headline / display) so the doc reads at a glance.
4. Don't list mechanics or class names here — those belong in `CLAUDE.md` and
   `corrections.md`. This file is a *what the reader sees* spec, plain language.
5. If a previously built beat changes, edit its block in place rather than
   appending a new one. The list stays linear; older flow gets rewritten.
6. When a beat is deleted from the live flow, remove it from the list. The
   chronological record of *why* it was deleted lives in `corrections.md`.
