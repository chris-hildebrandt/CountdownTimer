# Countdown Bar

A countdown widget for Sister Hildebrandt's Google Site: a big flip-card
**days : hours : minutes : seconds** timer, and a "send her LOVE" button that opens the
email redirect page in a new tab. It is one web page (`index.html`) hosted free on
GitHub Pages and shown on the Google Site through an embed.

Live address: **https://chris-hildebrandt.github.io/CountdownTimer/**

---

## Changing the settings (on github.com, no tools needed)

**Edit on a computer.** Phones and Macs can turn `"` into curly quotes (“ ”), which breaks
the settings (the timer shows `--`). Always check preview.html after an edit (step 6).

1. Go to the repository on github.com and click **`index.html`**.
2. Click the **pencil icon** (Edit this file).
3. Near the top, find the block between the two lines of `=====` marked
   **SETTINGS: edit here**. Change only what is inside the quotes.
4. Click **Commit changes...**, then **Commit changes** again.
5. Wait about **10 minutes**, then reload the Google Site. (GitHub keeps a copy for up to
   ~10 minutes, so an older version may show until then.)
6. Sanity check: open **https://chris-hildebrandt.github.io/CountdownTimer/preview.html**.
   The **Countdown target** line at the top says exactly when the timer will hit zero.
   **Check the weekday, date and AM/PM** on that line (`T05:00` is 5 AM, `T17:00` is 5 PM).
   If it says **NONE**, the settings have a mistake and the message says which one.

### Each setting

| Setting | What it does | Example |
|---|---|---|
| `buttonText` | Words on the button. `""` hides the button. | `"send her LOVE"` |
| `buttonLink` | Where the button goes. Should start with `https://` (opens in a new tab) or `mailto:` (opens the visitor's mail app; on some setups nothing happens, which is why the redirect page exists, so keep the redirect link). `""` hides the button. The scrambled address from the Google Sites link is added to the end of an `https://` link automatically. | `"https://chris-hildebrandt.github.io/missionary-email-redirect/"` |
| `hideButtonOnDone` | `true` hides the button once the countdown reaches zero; `false` keeps it. No quotes. | `false` |
| `startsAt` | When the mission started, same format as `endsAt`. The progress bar runs from here to `endsAt`. | `"2026-10-05T17:00"` |
| `endsAt` | When the countdown ends, as **year-month-dayThour:minute** (24-hour clock), in the time zone below. | `"2028-04-05T17:00"` = April 5, 2028, 5:00 PM |
| `timeZone` | The time zone `endsAt` is written in. | `"America/Boise"`, `"America/Sao_Paulo"` |
| `lang` | Language of the text (helps screen readers). | `"en"`, `"pt"` |
| `labels` | Words under the timer. Keep them short. | `{ days: "DAY", hours: "HR", minutes: "MIN", seconds: "SEC" }` |
| `theme` | Colors (see below). | `tileBg: "#f6efdf"` |
| `flip` | `true` = the numbers flip like a split-flap clock; `false` = plain tiles. Visitors who turned on "reduce motion" always get plain tiles. | `true` |
| `modes` | Optional. Which ways of counting visitors can tap through, in order. The page always opens on the first. Leave it out for all eight. | `["full", "months", "transfers", "weeks", "days", "hours", "minutes", "seconds"]` |
| `progress` | `true` shows the progress bar at the bottom, with a small mark at each 6-week transfer. Visitors tap the percentage to switch to "day x of y". `false` hides it. | `true` |

**Colors** (`theme`): `background` (the bar), `tileBg` / `tileText` (number tiles),
`labelText` (DAY HR MIN SEC), `colonText` (the colons between numbers),
`progressTrack` (the unfilled part of the progress bar; the filled part uses `buttonBg`),
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
- **Fallback text**: the button text and button link also appear lower in
  `index.html` (marked "Fallback ... keep in sync with CONFIG"). They are shown only if the
  settings block is broken. If you change the link, it is nice (not required)
  to change them there too.

To undo a change: on github.com open `index.html`, click **History**, and copy the old
text back (or ask Chris).

---

## Putting it on the Google Site

Do this on a **test page first** (see the Launch checklist); only after it passes, repeat
on the real page.

1. Edit the Google Site, then **Insert → Embed → By URL**.
2. Paste the link made by the **Embed link maker**
   (https://chris-hildebrandt.github.io/CountdownTimer/embed-link.html): type her email
   address there and copy the link it gives. It is the widget's address plus `#to=…`, the
   scrambled email address that the "send her LOVE" page needs. The address is kept only
   in the private Google Site, never on GitHub. Scrambled is not secret: anyone who sees
   the full link can read the address back, so keep the site private. Without the `#to=…` part the countdown
   still works, but the email page will say to open it from the family site. If asked,
   choose **Whole page**, then **Insert**.
3. **Resize**: click the embed once, then drag the blue dots on its edges.
   - The numbers grow to fill the box. Wider than about 5 times its height, the button
     moves beside the numbers instead of below them. Trim extra white space by making the box shorter.
   - Keep it **wide** (full width of the section works best). The layout adjusts by
     itself for phones, tablets and computers.
4. Check it in **Preview** (the eye icon), both computer and phone views, then **Publish**.

---

## Helper pages

Open these over https (on GitHub Pages). Opened as local files they need the browser
started with `--allow-file-access-from-files`, so the GitHub Pages address is easier.

| Page | Address | What it's for |
|---|---|---|
| Embed link maker | https://chris-hildebrandt.github.io/CountdownTimer/embed-link.html | Makes the link to paste into Google Sites, with her email address scrambled into the end. Nothing typed there leaves the page. |
| Preview | https://chris-hildebrandt.github.io/CountdownTimer/preview.html | The widget at phone, tablet and computer sizes at once, plus the **Countdown target** line. Use after every settings change. |
| Contrast check | https://chris-hildebrandt.github.io/CountdownTimer/contrast.html | Checks that every text color is readable on its background. Use after changing colors; everything should say PASS. |
| Tests | https://chris-hildebrandt.github.io/CountdownTimer/tests.html | Checks the time math (time zones, daylight saving, clock correction). Should say **PASS: all ... tests passed**. |

---

## Launch checklist (step 8, for Chris)

**Turn on GitHub Pages**
- [ ] The repository is **Public** (Settings → General); free GitHub Pages needs that.
- [ ] Repository **Settings → Pages → Build and deployment: Deploy from a branch →
      `main` / `(root)` → Save**. Wait a few minutes, then open the live address.
- [ ] tests.html, contrast.html and preview.html all green on the live address.

**Embed on a *test* page in Google Sites** (not the real page yet), then check
(spec section 11):
- [ ] Layout, sizes and fonts match the original screenshot (background aside).
- [ ] Countdown matches https://time.gov to the second.
- [ ] Still correct with the PC's time zone set to `America/Sao_Paulo`.
- [ ] With the PC clock set 5 minutes wrong, still correct after a few seconds (clock check).
- [ ] Done state: set `endsAt` about 15 minutes ahead, open preview.html after ~10 minutes
      (once the change is live) and watch it reach zero. Then set `endsAt` back and confirm
      preview.html's **Countdown target** line shows the real date again.
- [ ] Correct at 360, 600, 900 and 1400 px wide in the Google Sites box, and in Google Sites' mobile preview.
- [ ] Lighthouse (Chrome DevTools → Lighthouse) on the live address: performance ≥ 95, accessibility ≥ 95.
- [ ] First visit ≤ 100 KB (DevTools → Network, cache disabled).
- [ ] contrast.html: all text ≥ 4.5 : 1.

**Manual device checks** (see the list at the top of preview.html)
- [ ] Chrome minimum font size 16: tiles grow, nothing clips.
- [ ] Android Chrome auto-dark: tiles keep their colors.
- [ ] Windows High Contrast: tiles get outlines, the button has a border.
- [ ] Android phone: no visible jump when the font loads (Android has no
      Georgia, so the matched fallback doesn't apply there).

**Record on the test page** (spec 11a), from the browser console with the widget's frame
selected in DevTools:
- [ ] `self.origin` (is it the GitHub Pages address, or `null`?).
- [ ] Whether the `Date` header is readable:
      `fetch(location.href, {method: 'HEAD', cache: 'no-store'}).then(r => console.log(r.headers.get('Date')))`
      (prints a date, or `null` = clock check silently off, which is acceptable).
- [ ] Scroll the widget off screen and back, and switch tabs and back: the seconds keep counting correctly.
- [ ] The button opens the redirect page **and the redirect finishes its job** (not just a
      new tab) from the embed on **iPhone**, **Android** and **PC**, showing her address.
      If it says "open it from the family website", Google Sites dropped the `#to=…` part:
      use **Insert → Embed → Embed code** with
      `<iframe src="PASTE THE LINK HERE" style="border:0;width:100%;height:100%"></iframe>` instead.

**Final launch (step 9)**
- [ ] On the real page, add the **By URL** embed with the link from the Embed link maker (as above), then
      **delete the old Common Ninja embed box**. Preview, then **Publish**.
- [ ] **Only now** push the updated `missionary-email-redirect` page (the one that reads the
      address from the link). Pushed earlier, the old Common Ninja button would show
      "open it from the family website" instead of her address. Then click "send her LOVE"
      on the real page once more to confirm. Old bookmarks of the email page stop working
      from here on; the button on the site is the way in.
