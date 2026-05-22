# Indicator Story — Site Copy

All user-facing text from `index.html`, in scroll order. Edit this file freely; another Claude instance will map edits back into the HTML. Anchors next to each block point to where the string lives in the HTML so re-syncing is mechanical.

---

## Document head

**Tab title** — `<title>` (line 6)
> The TBL Liquidity Indicator — How It's Built

**Meta description** — `<meta name="description">` (line 7)
> A scroll-driven walkthrough of how The Bitcoin Layer's proprietary Liquidity Indicator is constructed.

---

## Intro (scene 0, scroll-hijacked dive-in)

**Kicker** — `.intro__kicker` (line 430)
> The Bitcoin Layer · Research

**H1** — `.intro__h1` (line 431, two lines split by `<br/>`)
> Liquidity moves
> in cycles.

**Subhead** — `.intro__sub` (line 432)
> And cycles can be measured. Here is how I built the indicator that traces them.

**Scroll cue** — `#introCue` (line 434)
> SCROLL TO DIVE IN

---

## Label scene

**Section title** — `.label-scene__title` (line 441)
> TBL Liquidity Index

---

## Scrolly part 1 (scenes 1–5)

### Scene 1 — Zoom in (line 450)

**Step number**
> 01 · ZOOM IN

**Heading**
> Zoom into a 30-day period

---

### Scene 2 — The change (line 458)

**Step number**
> 02 · THE CHANGE

**Heading**
> Measure the move in our TBL Liquidity Index.

---

### Scene 3 — Roll it forward (line 465)

**Step number**
> 03 · ROLL IT FORWARD

**Heading**
> Do that every day.

**Paragraph (primary)**
> Slide the window forward by one day. Recalculate. Repeat for every day in the history of the Index. The result is a new series. The thirty-day change of liquidity, over time.

**Paragraph (dim)**
> It is noisy. It has to be. Daily prints carry every twitch of the rates market, the dollar, balance sheets, and vol.

---

### Scene 4 — Smooth (line 472)

**Step number**
> 04 · SMOOTH

**Heading**
> Strip the noise.

**Paragraph**
> A Hodrick-Prescott filter pulls the trend out from under the daily chop. What remains is the cycle. The shape underneath the static.

**Nik prompt**
> [NIK: optional aside on why HP, not a moving average. Be specific.]

---

### Scene 5 — Cross-check (line 479)

**Step number**
> 05 · CROSS-CHECK

**Heading**
> It tracks risk assets.

**Paragraph**
> Overlay the S&P 500's own thirty-day change. The two cycles trace each other. The Liquidity Cycle is not a curiosity. It is the tide that pulls equities with it.

**Nik prompt**
> [NIK: cite Howell or Alden here if you want, or your own framing on liquidity → risk.]

---

### Viz pane labels (scenes 1–5)

**Caption (default)** — `#vizCaption` (line 490)
> TBL LIQUIDITY · ILLUSTRATIVE

**Readout (default)** — `#vizReadout` (line 491)
> Δ 30D · +14.2

**Caption variants set in JS** *(scenes change these dynamically)*
- Scene 1: `TBL LIQUIDITY · ILLUSTRATIVE`
- Scene 2: `30-DAY WINDOW · ZOOMED`
- Scene 3: `30-DAY Δ · NOISY`
- Scene 4: `30-DAY Δ · SMOOTHED CYCLE`
- Scene 5: `LIQUIDITY CYCLE × SPX 30-DAY Δ`

**In-chart labels (drawn in JS)**
- Scene 1: `30 days` (above zoom box)
- Scene 2: `Δ = {value}` (above bracket)
- Scene 5 legend: `— LIQUIDITY CYCLE` (yellow) and `-- S&P 500 30D Δ` (blue)

---

## Interstitial (between scenes 5 and 6)

**Heading** — `.interstitial__h` (line 500, two lines split by `<br/>`, second word in accent color via `.interstitial__em`)
> Now we look for the
> *turns*.

---

## Scrolly part 2 (scenes 6–7)

### Scene 6 — Peaks & troughs (line 508)

**Step number**
> 06 · PEAKS & TROUGHS

**Heading**
> Where the cycle pauses.

**Paragraph**
> At every peak and every trough, the cycle flattens. Slope goes to zero. That is the inflection. The moment liquidity stops accelerating in one direction and begins to reverse.

**Nik prompt**
> [NIK: tie this to a real regime if it doesn't tip the signal, e.g. "every cycle since 2008 has done this."]

---

### Scene 7 — The slope (line 515)

**Step number**
> 07 · THE SLOPE

**Heading**
> Differentiate.

**Paragraph (primary)**
> Take the first derivative of the smoothed cycle. The peaks and troughs collapse to zero crossings. The slope is the signal. Above zero, liquidity is accelerating. Below zero, it is draining.

**Paragraph (dim)**
> This is the Indicator. Not the cycle itself, but the rate of change of the cycle.

---

### Viz pane labels (scenes 6–7)

**Caption (default)** — `#vizCaption2` (line 526)
> TBL LIQUIDITY INDICATOR · ILLUSTRATIVE

**Caption variants set in JS**
- Scene 6: `LIQUIDITY CYCLE · PEAKS & TROUGHS`
- Scene 7: `INDICATOR · SLOPE OF THE CYCLE`
- Scene 8: `INDICATOR · BUY / SELL`

**In-chart labels (drawn in JS)**
- Scene 6: `Flat slope` (on the peak and the trough callouts)
- Scene 8: `Liquidity trough · Buy` (green) and `Liquidity peak · Sell` (red)

---

## Outro / scene 8 (line 534)

**Kicker** — `.outro__kicker`
> 08 · THE INDICATOR

**Heading** — `.outro__h`
> A trough is a buy. A peak is a sell.

**Paragraph** — `.outro__p`
> When the slope crosses zero from below, liquidity has turned the corner upward. When it crosses zero from above, it has rolled over. The shape of every signal looks like this. The real ones, with confirmation logic and live data, sit inside TBL Pro.

**CTA button** — `.outro__cta` (links to `https://thebitcoinlayer.substack.com`)
> Read TBL Pro →

**Sign-off** — `.outro__sig` (two lines split by `<br/>`)
> Until next time,
> Nik

---

## Footer (line 544)

> THE BITCOIN LAYER · thebitcoinlayer.com · Illustrative; not investment advice.
