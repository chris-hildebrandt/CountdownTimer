# Countdown Bar — Build Spec

Status: **Draft for review** (rev 4, 2026-10-25)
Owner: Chris Hildebrandt
Repo: `chris-hildebrandt/CountdownTimer`

Changes in rev 4:
- Open questions resolved; section 12 now lists none.
- Mission start derived from the end date (18 months earlier) instead of a separate setting (9.2).
- Single-unit modes show whole units remaining, no fractions (9.1).
- Visuals come from existing open-source libraries and icon sets wherever possible, not hand-drawn (7.1, 9.2, 9.3).
- Visitors can switch display styles with a small palette button (9.3).

Changes in rev 3:
- Findings from the saved Google Sites page (section 2.1). **The current widget has hit its view limit and is not showing.**
- Responsive design now targets Google Sites' aspect-ratio embed box, not a fixed height (section 6).
- Device-clock correction added (section 7.2) at the cost of one tiny request after first paint.
- Phase 2 rewritten around your wishlist: measurement modes, progress, display styles, slideshow (section 9).

---

## 1. Purpose

Replace the Common Ninja "Countdown Bar" widget on the family's Google Sites tribute
page for Sister Hildebrandt's mission (Brazil, ~18 months, home April 2028).

| Phase | Scope |
|---|---|
| **1 — Clone** | Same layout, content and behavior as the Common Ninja bar, on solid white, with readability fixes and a clock correction. Hosted on GitHub Pages. |
| **2 — Wishlist** | Measurement modes, progress indicator, display styles (split-flap), and maybe the photo slideshow. Section 9. |

Phase 1 code is structured so phase 2 plugs in without a rewrite: the display reads
from a "remaining time" value, and each measurement mode or style is a small
separate function.

## 2. What exists today

A single horizontal bar, ~125 px tall, with a soft drop shadow underneath.

| Region | Content |
|---|---|
| Background | Photo of the São Paulo Brazil Temple spire against blue sky. |
| Left | Alarm-clock icon, then **"Sister Hildebrandt- CALLED to SERVE!"** in a dark serif. |
| Center | **"Be right back in...."** above digit tiles `528 : 00 : 43 : 55`, one mint tile per digit, labels **DAY HR MIN SEC** underneath. |
| Right | Pill button **"send her LOVE"**, dark green, white serif. |

| Setting | Value |
|---|---|
| Title | Sister Hildebrandt- CALLED to SERVE! |
| Timer Title | Be right back in.... |
| Button Text | send her LOVE |
| Button Link | https://chris-hildebrandt.github.io/missionary-email-redirect/ |
| Hide Button on Done | Off |
| Countdown ends at | April 5, 2028 5:00 PM, America/Boise |

### 2.1 Findings from the saved Google Sites page
1. **The widget is down.** In the saved copy, the embed shows *"Widget has reached its
   monthly views limit."* plus a "Free Website Widget" ribbon instead of the countdown.
   Visitors currently see no countdown.
2. **How it's embedded:** through Google Sites' **Embed → Embed code**, which pasted
   Common Ninja's `<script>` snippet. Google runs pasted code inside its own sandboxed
   frame on `googleusercontent.com`, and that frame sets `<base target="_blank">`, so
   links open in a new tab by default.
3. **The embed box keeps an aspect ratio**, about **2.85 : 1** (width : height). Its
   height grows and shrinks with the page width rather than staying fixed. On a phone
   (~360 px wide) the box is ~126 px tall; on a wide desktop column (~1100 px) it's
   ~390 px tall, which is why there's empty space under the bar in the editor.
   The ratio can be changed by dragging the embed's handles in the Google Sites editor.
4. The live Common Ninja configuration loads from their servers at runtime, so the
   saved files don't contain its exact fonts or colors. The visual spec stays matched
   by eye from your screenshot.

The saved page files stay out of git (see `.gitignore`); they hold a private site.

## 3. Requirements

### 3.1 Functional
- **F1** Count down to a fixed instant, a local date-time plus an IANA timezone
  (`2028-04-05T17:00`, `America/Boise`). Correct for viewers in any timezone and
  across DST changes.
- **F2** Days show at least 2 digits and grow as needed. Hours, minutes and seconds always show 2.
- **F3** Update every second, recomputed from the (corrected) clock each tick so it never drifts.
- **F4** Done state: `00:00:00:00`; hide the button if `hideButtonOnDone` is on.
- **F5** The button opens `buttonLink` in a new tab on desktop and mobile
  (`target="_blank" rel="noopener"`). Any URL scheme; nothing prefixed.
- **F5a** Address pass-through: the email address is kept out of the current files of both
  public repos (the redirect repo's older history still has it; accepted). The Google
  Sites embed URL carries it scrambled (base64url) as `#to=<value>`; the widget appends that
  `#to=<value>` to an http(s) `buttonLink` that has no `#` of its own, and the
  `missionary-email-redirect` page reads it. The `#` part is never sent to a server or in a
  Referer. Without it, the redirect page says to open it from the family site instead of breaking.
  `embed-link.html` makes the embed URL from an address, in the browser only.
  Order: the redirect change goes live only after the new embed is published, because the
  Common Ninja button and old bookmarks have no `#to=` and would get the "missing" message.
- **F6** All settings, colors and fonts live in one config block at the top of `index.html`.
- **F7** Clock correction: if the viewer's device clock is off by more than 2 seconds,
  the countdown uses corrected time instead (section 7.2).

### 3.2 Non-functional
- **N1 Fast:** section 7.3.
- **N2 Responsive:** fills Google Sites' aspect-ratio embed box from 320 px to 1920 px wide (section 6).
- **N3 Easy to edit:** one config block, editable on github.com.
- **N4 Accessible:** text ≥ 4.5:1 contrast; `role="timer"` with a summary updated once a minute; visible focus ring; respects `prefers-reduced-motion`.
- **N5 Private:** no analytics, tracking or third-party scripts.

## 4. Visual spec (phase 1)

| Element | Spec |
|---|---|
| Bar | Background `#ffffff`, `box-shadow: 0 12px 24px -12px rgba(0,0,0,.25)`, square corners. |
| Title | Lora 600, `#1d2a2a`; alarm icon before it. |
| Timer title | Lora 400 italic, `#2b3a3a`, centered above the tiles. |
| Digit tile | Radius 4 px, cream paper fill `#f6efdf`, digit Lora 600 `#1b3a4b`, tabular figures, small drop shadow. Animated flip card (PQINA Flip 1.8.4, MIT) once the library loads after first paint; plain tile before that, with reduced motion, with `flip: false`, or if the CDN fails. |
| Colon | Lora 700, `#1d2a2a`. |
| Unit label | Small caps, letter-spaced, `#1e6c93`. |
| Button | Radius 8 px, fill `#1e6c93` (the site's blue), Lora 600 white; hover darkens 8%; 3 px focus ring. |

**Revised after launch (Oct 2026):** the title and timer title are removed (the site adds
its own text), and the widget is two parts that fill the box: the ticker, and the button
below it (beside it in boxes wider than 5:1). The progress bar (9.2) becomes the third part.
Digit size = min(6.25vw, 23.8vh) stacked, min(4.76vw, 38.4vh) side by side. The title,
timer-title and icon rows above, and section 6's breakpoints, are superseded.

At the desktop size in the screenshot: title ~20 px, tiles ~30×38 px with ~24 px digits,
button text ~17 px. Section 6 covers how sizes scale. All color pairs are checked
for ≥ 4.5:1 contrast in build step 6.

## 5. Configuration

```js
const CONFIG = {
  title:            "Sister Hildebrandt- CALLED to SERVE!",
  timerTitle:       "Be right back in....",
  buttonText:       "send her LOVE",
  buttonLink:       "https://chris-hildebrandt.github.io/missionary-email-redirect/",
  hideButtonOnDone: false,
  endsAt:           "2028-04-05T17:00",   // wall-clock time in timeZone
  timeZone:         "America/Boise",
  labels:           { days: "DAY", hours: "HR", minutes: "MIN", seconds: "SEC" },
  icon:             "alarm",             // Material Symbols icon, inlined as SVG; "none" hides it
  theme: {
    background: "#ffffff", tileBg: "#f6efdf", tileText: "#1b3a4b",
    labelText: "#1e6c93", titleText: "#1d2a2a", timerTitleText: "#2b3a3a",
    buttonBg: "#1e6c93", buttonText: "#ffffff"
  },
  flip:             true,                 // animated flip cards; false = plain tiles
  // phase 2 adds: missionMonths (18), modes, style, styles, slides (section 9)
};
```

## 6. Responsive behavior

The widget fills the embed box, whose width is the iframe's viewport, so plain media
queries apply. Because Google Sites keeps the box's **aspect ratio**, height shrinks
with width. The layout therefore keys off both width and aspect ratio, and text and
tile sizes scale with the box (CSS `clamp()` with `vw`/`vh` units), always centered
vertically.

| Embed width | Layout |
|---|---|
| ≥ 900 px | One row: title, timer, button (the clone). |
| 600–899 px | Two rows: title and button on top, timer below. |
| < 600 px | Two rows: timer on top, then a smaller title and button side by side. Icon and timer title hidden. |

**Recommended embed shape:** I'll test the current 2.85 : 1 box first. If the bar
looks lost in it on desktop, a wider box (about 5 : 1) is the fix, set by dragging the
embed's handles in the Google Sites editor. Confirmed in build step 8 on both desktop
and mobile preview.

---

## 7. Technical design

### 7.1 Tech stack

| Layer | Choice | Why |
|---|---|---|
| Markup | Plain HTML5, one file (`index.html`) | Nothing to build or install; editable on github.com. |
| Styling | Plain CSS inlined in `<style>`: custom properties, Grid, media queries, `clamp()` | Works in every current browser; no framework weight. |
| Logic | Vanilla JavaScript (ES2020) inlined in `<script>`, ~4 KB | A countdown needs no framework; inline means no extra request. |
| Time zones | Browser built-in `Intl.DateTimeFormat` | IANA zones and DST without a 20–70 KB date library. |
| Font | Google Fonts, Lora | Same font service Google Sites uses. |
| Icons | Google's [Material Symbols](https://fonts.google.com/icons) (Apache 2.0), copied in as inline SVG: `alarm` now; `palette`, `hourglass` in phase 2 | Google's own icon set, matches Google Sites, professionally drawn. Inline SVG costs no requests. |
| Visual assets policy | Use existing, free, open-source libraries, fonts and icons for anything visual. Check license, size and maintenance before adopting; copy the files into the repo and pin the version. | Better polish than hand-drawn art; no surprise changes from a CDN. |
| Hosting | GitHub Pages (static files over HTTPS via Fastly's CDN) | Free, no view caps, deploys on every push to `main`. |
| Embedding | Google Sites **Embed → By URL** with the Pages URL (replacing the current Embed code) | Updates automatically when the repo changes. |
| Dependencies | None in phase 1. No npm, bundler or build step. | Nothing to keep updated or break. |
| Tests | `tests.html`: plain-JS assertions for the time math, opened in a browser | No Node or tooling required. |
| Dev preview | `preview.html`: the widget in iframes at Google Sites' ratio, at 360, 600, 900 and 1400 px wide | Checks every layout at once. |

### 7.2 Network calls

**Phase 1, every page view: 5 requests, all GET or HEAD, no API calls.** (Measured after
step 5: Lora is served as two variable-font files, one upright and one italic.)

| # | Request | Host | Size | When | Cached |
|---|---|---|---|---|---|
| 1 | `index.html` (CSS and JS inline) | chris-hildebrandt.github.io | ~10 KB gzipped | Page load | 10 min |
| 2 | Lora font CSS | fonts.googleapis.com | ~1 KB | Page load | 1 day |
| 3 | Lora font files (woff2, Latin subset) | fonts.gstatic.com | ~61 KB, 2 files (upright + italic) | Page load | 1 year |
| 4 | `HEAD` request to the widget's own URL (clock check) | chris-hildebrandt.github.io | ~0.5 KB, headers only | After the countdown is showing | Never (`no-store`) |

**Clock correction (request 4):**
- The countdown first renders immediately from the device clock, so nothing waits on the network.
- Then, once the browser is idle, one `HEAD` request is sent to the same address the
  widget came from. Same origin, so no permissions or CORS issues. The server's `Date`
  response header gives the true time to the second. Adding half the round-trip time
  gives an estimate within about ±0.5 s.
- If the device clock differs by more than 2 seconds, the countdown silently applies the
  difference from then on. Otherwise nothing changes.
- If the request fails (offline, blocked), the device clock is used. No error is shown.
- Cost: one header-only request of under 1 KB, after first paint. No effect on load speed.

Not called: analytics, tracking, external time servers, config files, Common Ninja.

### 7.3 Performance plan

**Budget (phase 1):** first visit ≤ 100 KB (measured ~73 KB), ≤ 5 requests; real numbers on screen in under
1 second on a mid-range phone over 4G; Lighthouse performance ≥ 95; layout shift (CLS) < 0.05.

**Loading**
1. **Everything inline:** CSS and JS ship inside `index.html`. No render-blocking files.
2. **Early font connection:** `preconnect` to both Google Fonts hosts at the top of `<head>`.
3. **`display=swap` with a matched fallback:** Georgia paints first; a `@font-face` fallback
   with `size-adjust` and ascent overrides tuned to Lora prevents a jump when Lora arrives.
4. **Only the styles used:** four Lora styles, Latin subset.
5. **No `00:00` flash:** the script at the end of `<body>` fills real numbers before first paint.
6. **Clock check after first paint:** sent with `requestIdleCallback`, never blocking render.

**Running**
7. **One timer, aligned to the second:** `setTimeout` to the next whole second, recomputed from the clock.
8. **Minimal DOM work:** elements created once; only changed digits are updated.
9. **No layout thrash:** fixed tile sizes and tabular figures, plus `contain: layout paint` on tiles.
10. **Sleeps when unseen:** stops while the tab is hidden (`visibilitychange`) and resyncs
    on return. (An off-screen pause was dropped; see 11a, step 3.)

## 8. Edge cases
- End time already passed on load → done state immediately.
- Viewer changes timezone or DST mid-session → no effect; math is in UTC.
- Days drop from 3 digits to 2 → leading tile removed once.
- Invalid date in config → `--:--:--:--` and a console error, not a crash.
- Clock check returns an implausible time (off by more than a day) → ignored.
- `#to=` missing or malformed (not base64url) → button link used as written; the redirect page
  shows "open this from the family site" (F5a).

---

## 9. Phase 2 — Wishlist

Proposed build order: **9.1 → 9.2 → 9.3**, then decide on 9.4. Each gets its own
short spec review before building.

### 9.1 Measurement modes (switchable by visitors)
Visitors switch the timer between ways of measuring the time left. Modes cycle in a
fixed order:

Single-unit modes show **whole units remaining**, rounded down, with no fraction or
remainder: "17 MONTHS" stays until it becomes 16.

| # | Mode | Example display | Changes |
|---|---|---|---|
| 1 | Full (default) | `528 : 00 : 43 : 55` DAY HR MIN SEC | every second |
| 2 | Months | `17` MONTHS | monthly |
| 3 | Transfers (6 weeks) | `12` TRANSFERS | every 42 days |
| 4 | Weeks | `75` WEEKS | weekly |
| 5 | Days | `528` DAYS | daily |
| 6 | Hours | `12,672` HOURS | hourly |
| 7 | Minutes | `760,363` MINUTES | every minute |
| 8 | Seconds | `45,621,835` SECONDS | every second |

Display reads "MONTH" (singular) at exactly 1.

- **How to switch:** tap or click the timer (the whole timer is one button), or use small
  ‹ › arrows beside it. Both work with the keyboard. A short label like "Months · tap to change"
  shows on hover or focus so people discover it.
- **Remembered per visitor:** the chosen mode is saved in the visitor's own browser
  (`localStorage`), so each visitor's favorite comes back next time and no one else is affected.
  If storage is blocked (some private-browsing modes), it falls back to the default with no error.
  Note: browsers keep embedded-widget storage per website, so the saved choice applies on this
  Google Site only, which is what we want.
- **Months** are calendar months (April 5 → May 5 is one month, whatever its length), not 30-day blocks.
- **Transfers** are 42 days each. They're a count of 6-week blocks, not the mission's actual
  transfer dates. If you can get the real transfer schedule, this could count real transfers instead.
- The owner can choose which modes appear and their order in the config.
- Size: about 1.5 KB of extra JavaScript, no extra requests.

### 9.2 Progress indicator
Shows how much of the 18 months away has passed.
- **Start is derived, not configured:** start = `endsAt` minus `missionMonths` (18) calendar
  months, so **2026-10-05 17:00 Boise time**. Changing `endsAt` moves the start with it.
- Progress = (now − start) ÷ (end − start), clamped to 0–100%.
- **Option A: progress bar.** A thin bar along the bottom edge of the widget that fills
  left to right, with "32% of the way home" in small text. Smallest and least busy.
- **Option B: hourglass.** A small hourglass beside the timer whose sand level shows progress.
  The artwork comes from an existing open-licensed source (Material Symbols' `hourglass` shapes,
  or an open-source animated hourglass if a good one turns up), not drawn by hand. A slow trickle
  animation runs unless the viewer prefers reduced motion.
- Both can exist and be switched in config. Recommendation: build A first, then B if you
  want more character. Either costs ~1–2 KB, no requests.

### 9.3 Display styles (split-flap and others)
**Visitors switch styles** with a small **palette button** (Material Symbols `palette` icon)
in the bar's corner. Each tap moves to the next style. The choice is saved per visitor in
`localStorage`, the same way as the measurement mode, with a silent fallback to the default.
The owner sets the default style and which styles are offered in config
(`style: "tiles"`, `styles: ["tiles", "flip", "digital", "elegant"]`).

**Styles, each built on an existing library or font:**

| Style | Look | Source (to be vetted before adopting) |
|---|---|---|
| Tiles (phase 1) | Current mint tiles | Lora (Google Fonts) |
| Flip | Smooth airport/train split-flap board | [PQINA Flip](https://github.com/pqina/flip), MIT license, built for exactly this |
| Digital | Calculator-style seven-segment numerals | [DSEG7](https://github.com/keshikan/DSEG) font, SIL Open Font License, the standard free seven-segment font |
| Elegant | Large serif numerals, no tiles | Playfair Display or Cormorant (Google Fonts) |

- **Vetting before adoption:** license allows free use, file size, recent maintenance, no
  network calls of its own. Results go in a short note in this spec before building.
- **Self-hosted:** library and font files are copied into the repo at a pinned version, not
  loaded from someone else's CDN, so they can't change or disappear underneath us.
- **Loaded on demand:** a style's files download only when that style is first shown, so the
  default view stays as light as phase 1. For a visitor whose saved style is Flip, those files
  load on page open instead.
- **Fallback:** if a style's files fail to load, the widget shows Tiles.
- Reduced motion: Flip changes digits without the fold animation.
- Display styles work with every measurement mode (9.1).

### 9.4 Background photo slideshow (maybe)
Deferred until the functional build is in place, to judge whether the widget feels too plain
or too busy. Spec from rev 2 stays as the plan if it goes ahead: shuffled São Paulo temple and
Brazil vistas, 8 s hold, 2 s cross-fade, translucent panel behind text for readability, one
static photo for reduced motion, AVIF/WebP at two widths ≤ 120 KB each, loaded one at a time.

### 9.5 Phase 2 budget
Widget code of our own stays under 15 KB gzipped. The default view keeps phase 1's 5 requests.
A non-default style adds its own files only when it's shown (sizes recorded during vetting),
and each slide shown adds one image.

---

## 10. Implementation steps (phase 1, after approval)
1. Static markup and CSS with hard-coded `528:00:43:55`; compare against the screenshot.
2. Config block wired in; theme values as CSS variables.
3. Time math, tick loop, done state and clock correction, plus `tests.html`.
4. Responsive layouts and `preview.html` at Google Sites' aspect ratio.
5. Performance pass: font fallback metrics, Lighthouse run.
6. Contrast check of every color pair.
7. README: editing settings and embedding in Google Sites.
8. Enable GitHub Pages; embed on a **test** page in Google Sites; run section 11 checks.
9. Your wife swaps the real page's embed to **By URL** with the Pages address.

Each step is its own commit. I pause for your review after steps 1, 4 and 8.

## 11. Acceptance checks
- [ ] Layout, sizes and fonts match the screenshot (background aside).
- [ ] Countdown matches a trusted clock (time.gov) to the second.
- [ ] Correct with the PC's timezone set to `America/Sao_Paulo`.
- [ ] With the PC clock set 5 minutes wrong, the countdown is still correct after the clock check.
- [ ] Done state verified with `endsAt` 1 minute ahead.
- [ ] Button opens the redirect page in a new tab from the Google Sites embed, on a PC and a phone.
- [ ] Correct at 360, 600, 900 and 1400 px in the Google Sites aspect-ratio box, and in Google Sites mobile preview.
- [ ] Lighthouse performance ≥ 95, accessibility ≥ 95.
- [ ] First visit ≤ 100 KB, ≤ 5 requests.
- [ ] All text ≥ 4.5:1 contrast.

## 11a. Review findings carried forward
Found by code review and adversarial testing of step 1; each is owned by the step named.

**Step 3 (time logic)**
- Ship ES2017-level JavaScript (no `?.`, `??`): one syntax error on an old iPad kills the
  whole script. Static markup shows `--` placeholders, never real-looking digits, so a
  failed script can't show a frozen, wrong countdown.
- `requestIdleCallback` doesn't exist in Safari/iOS: fall back to `setTimeout`.
- Clock check: the `Date` header has 1 s resolution (add ~0.5 s plus half the round trip).
  Do **not** reject cached responses: probes showed GitHub Pages' CDN stamps a fresh `Date`
  on cache hits too (rev 4's "ignore nonzero `Age`" rule would have disabled the correction).
  Discard samples with a round trip over 1 s. Anchor the correction to the monotonic clock
  (`performance.now()`), so an OS clock fix mid-session doesn't reintroduce an error. If the
  Google Sites frame gives the widget an opaque origin, `Date` may be unreadable (not
  CORS-safelisted). Then the check silently does nothing, which is acceptable. Log
  `self.origin` and readability on the step 8 test page.
- Config typos must show `--`, never a wrong countdown: `timeZone` is required and must be a
  region name like `America/Boise` (abbreviations like `MST` and `Etc/` zones are rejected);
  theme colors are validated; the resolved target time is logged and shown in `preview.html`
  so every edit can be sanity-checked.
- No off-screen pause: in testing, `IntersectionObserver` inside nested iframes didn't update
  after the host page scrolled, which would freeze the countdown. Only the hidden-tab pause
  remains (one update per second costs almost nothing).
- `Intl` time math: use `hourCycle: "h23"` and still map hour 24 → 0; two-pass offset for
  DST; `tests.html` covers endsAt at 02:30 on DST-change days.
- Days tiles: rebuild the days container only when its length changes. Update the screen
  reader summary from the same computed value in the same tick. No `aria-live`.

**Step 2 (config)**
- Set text with `textContent`, never `innerHTML`. Make `lang` configurable alongside labels.
- Long or translated unit labels can widen a unit beyond its tiles; keep labels short or
  let them overflow their unit centered.
- Derived colors (tile split line, hover overlay, focus ring) follow `buttonBg`; the
  contrast check (step 6) is a reusable script, re-run whenever config colors change.

**Step 4 (responsive)**
- Font size jumps at 599/600 and 899/900 (tiles grow ~60% across 1 px), and the one-row
  layout is stuck at its 15 px floor from 900 to ~1027 px. Make sizes continuous across
  breakpoints; unit labels never below ~11 px.
- Very short embed boxes (under ~110 px tall at desktop width) clip; fix or document a
  minimum height.
- `preview.html` includes 1 px breakpoint pairs, short boxes (360x60, 1200x80), a no-Lora
  variant, minimum font size 16, forced dark mode and Windows High Contrast.

**Step 5 (performance)**
- The Google Fonts stylesheet is render-blocking: load it with `preload` + `onload` swap and
  a `<noscript>` fallback. Tune Georgia `size-adjust` (fallback is 7–14 px wider at phone sizes).

**Known, accepted after step 5 review**
- Android has no Georgia, so the size-matched fallback font doesn't apply there; phones paint
  in the default serif until Lora arrives. Checked manually on a real Android phone in step 8.
- Georgia's digits stay old-style (uneven) in the fallback; cosmetic, only until Lora loads.

**Step 8 (Google Sites test)**
- Verify the redirect page completes its job (not just opens a tab) from the embed on
  iPhone, Android and PC, in case the sandbox applies to opened tabs.

**Phase 2**
- Palette, mode and arrow controls must be `<button>`s, not links.
- Wrap even the *access* to `localStorage` in try/catch (it throws in sandboxed frames).

## 12. Open questions
None. Decisions from rev 3's questions:
- Mission start = end date minus 18 months (9.2).
- Single-unit modes show whole units, rounded down (9.1).
- "Digital" means calculator-style seven-segment numerals; visuals come from existing libraries (9.3).
- Visitors can switch styles with a palette button (9.3).
