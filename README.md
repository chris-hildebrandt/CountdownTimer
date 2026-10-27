# Countdown Bar

A small countdown bar for Sister Hildebrandt's Google Site: a title, a ticking
**days : hours : minutes : seconds** timer, and a "send her LOVE" button that opens the
email redirect page in a new tab. It is one web page (`index.html`) hosted free on
GitHub Pages and shown on the Google Site through an embed.

Live address: **https://chris-hildebrandt.github.io/CountdownTimer/**

---

## Changing the settings (on github.com, no tools needed)

1. Go to the repository on github.com and click **`index.html`**.
2. Click the **pencil icon** (Edit this file).
3. Near the top, find the block between the two lines of `=====` marked
   **SETTINGS: edit here**. Change only what is inside the quotes.
4. Click **Commit changes...**, then **Commit changes** again.
5. Wait about **10 minutes**, then reload the Google Site. (The site keeps a copy for up to
   10 minutes, so an older version may show until then.)
6. Sanity check: open **https://chris-hildebrandt.github.io/CountdownTimer/preview.html**.
   The **Countdown target** line at the top says exactly when the timer will hit zero.
   If it says **NONE**, the settings have a mistake and the message says which one.

### Each setting

| Setting | What it does | Example |
|---|---|---|
| `title` | Text on the left, beside the alarm-clock icon. `""` hides it. | `"Sister Hildebrandt- CALLED to SERVE!"` |
| `timerTitle` | Small italic line above the timer (hidden on phones). `""` hides it. | `"Be right back in...."` |
| `buttonText` | Words on the button. `""` hides the button. | `"send her LOVE"` |
| `buttonLink` | Where the button goes (opens in a new tab). Must start with `https://`. | `"https://chris-hildebrandt.github.io/missionary-email-redirect/"` |
| `hideButtonOnDone` | `true` hides the button once the countdown reaches zero; `false` keeps it. No quotes. | `false` |
| `endsAt` | When the countdown ends, as **year-month-dayThour:minute** (24-hour clock), in the time zone below. | `"2028-04-05T17:00"` = April 5, 2028, 5:00 PM |
| `timeZone` | The time zone `endsAt` is written in. | `"America/Boise"`, `"America/Sao_Paulo"` |
| `lang` | Language of the text (helps screen readers). | `"en"`, `"pt"` |
| `labels` | Words under the timer. Keep them short. | `{ days: "DAY", hours: "HR", minutes: "MIN", seconds: "SEC" }` |
| `icon` | `"alarm"` shows the alarm clock, `"none"` hides it. | `"alarm"` |
| `theme` | Colors (see below). | `tileBg: "#5fe0b0"` |

**Colors** (`theme`): `background` (the bar), `tileBg` / `tileText` (number tiles),
`labelText` (DAY HR MIN SEC), `titleText` (title and colons), `timerTitleText`,
`buttonBg` / `buttonText`. Pick colors at e.g. https://htmlcolorcodes.com and copy the
`#` code. After changing colors, open the **contrast check** (below) to make sure
everything is still easy to read.

### Rules that prevent mistakes

- **Keep the quotes and commas** exactly as they are: don't add or remove any. Change
  only the text between the quotes (or `true`/`false`). Don't type a `"` inside the text;
  use `'` instead.
- **Time zone** must be a full name like `America/Boise` or `America/Sao_Paulo`.
  Abbreviations like `MST` or `MDT` are not accepted (they ignore daylight saving).
- **Colors need the `#`**: `"#0f6b4f"`, not `"0f6b4f"`. A bad color is ignored and the
  default is used.
- **Dates**: two digits for month, day, hour and minute: `"2028-04-05T17:00"`.
- If something is wrong, the timer shows `--` instead of a wrong countdown, so a
  mistake is obvious rather than silently wrong.
- **Fallback text**: the title, button text and button link also appear lower in
  `index.html` (marked "Fallback ... keep in sync with CONFIG"). They are shown only if the
  settings block is broken. If you change the title or link, it is nice (not required)
  to change them there too.

To undo a change: on github.com open `index.html`, click **History**, and copy the old
text back (or ask Chris).

---

## Putting it on the Google Site

1. Edit the Google Site, then **Insert → Embed → By URL**.
2. Paste **https://chris-hildebrandt.github.io/CountdownTimer/** and click **Insert**.
   (Replace the old "Embed code" box, if there is one.)
3. **Resize**: click the embed once, then drag the blue dots on its edges.
   - Keep it **at least 110 pixels tall**; shorter boxes shrink the text.
   - Keep it **wide** (full width of the section works best). The layout adjusts by
     itself for phones, tablets and computers.
4. Check it in **Preview** (the eye icon), both computer and phone views, then **Publish**.

---

## Helper pages

Open these over https (on GitHub Pages). Opened as local files they need the browser
started with `--allow-file-access-from-files`, so the GitHub Pages address is easier.

| Page | Address | What it's for |
|---|---|---|
| Preview | https://chris-hildebrandt.github.io/CountdownTimer/preview.html | The widget at phone, tablet and computer sizes at once, plus the **Countdown target** line. Use after every settings change. |
| Contrast check | https://chris-hildebrandt.github.io/CountdownTimer/contrast.html | Checks that every text color is readable on its background. Use after changing colors; everything should say PASS. |
| Tests | https://chris-hildebrandt.github.io/CountdownTimer/tests.html | Checks the time math (time zones, daylight saving, clock correction). Should say **PASS: all ... tests passed**. |

---

## Launch checklist (step 8, for Chris)

**Turn on GitHub Pages**
- [ ] Repository **Settings → Pages → Build and deployment: Deploy from a branch →
      `main` / `(root)` → Save**. Wait a few minutes, then open the live address.
- [ ] tests.html, contrast.html and preview.html all green on the live address.

**Embed on a *test* page in Google Sites** (not the real page yet), then check
(spec section 11):
- [ ] Layout, sizes and fonts match the original screenshot (background aside).
- [ ] Countdown matches https://time.gov to the second.
- [ ] Still correct with the PC's time zone set to `America/Sao_Paulo`.
- [ ] With the PC clock set 5 minutes wrong, still correct after a few seconds (clock check).
- [ ] Done state: temporarily set `endsAt` 1 minute ahead, watch it reach zero, then set it back.
- [ ] Correct at 360, 600, 900 and 1400 px wide in the Google Sites box, and in Google Sites' mobile preview.
- [ ] Lighthouse (Chrome DevTools → Lighthouse) on the live address: performance ≥ 95, accessibility ≥ 95.
- [ ] First visit ≤ 100 KB (DevTools → Network, cache disabled).
- [ ] contrast.html: all text ≥ 4.5 : 1.

**Manual device checks** (see the list at the top of preview.html)
- [ ] Chrome minimum font size 16: tiles grow, nothing clips.
- [ ] Android Chrome auto-dark: tiles keep their colors.
- [ ] Windows High Contrast: tiles get outlines, the button has a border.

**Record on the test page** (spec 11a), from the browser console with the widget's frame
selected in DevTools:
- [ ] `self.origin` (is it the GitHub Pages address, or `null`?).
- [ ] Whether the `Date` header is readable:
      `fetch(location.href, {method: 'HEAD', cache: 'no-store'}).then(r => console.log(r.headers.get('Date')))`
      (prints a date, or `null` = clock check silently off, which is acceptable).
- [ ] Scroll the widget off screen and back, and switch tabs and back: the seconds keep counting correctly.
- [ ] The button opens the redirect page **and the redirect finishes its job** (not just a
      new tab) from the embed on **iPhone**, **Android** and **PC**.

Then: swap the real page's embed to **By URL** with the live address (step 9).
