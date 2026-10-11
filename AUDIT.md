# Audit: ramsushistoryreviewgame.netlify.app

**Site:** "Industrial Empire: The Price of Progress", a 10-question decision game for U.S. History (1870–1900)
**Audited:** 2026-10-11, against the live deploy (etag `86e3311e…`)
**Method:** Downloaded the full source (one 22 KB HTML file). Played it in headless Chromium at 1280, 375 and 320 px. Ran axe-core 4.10 (WCAG 2.1 AA and best practice), checked keyboard focus, exported print to PDF, checked HTTP headers, worked out color contrast, and analysed the scoring for every answer strategy.

---

## TL;DR

The site is fast, private, stable and looks good. It throws no console errors, has no third-party requests, collects no data and has no horizontal scroll on phones. The serious problems are in **game design and teaching value**, not in the tech:

1. **The game can be beaten without knowing any history.** The right answer is never C. Picking B every time scores **775/1000**. Picking the option that costs money wins 9 times out of 10.
2. **A perfect game ends with the company bankrupt (`$-600`)** and a message saying "Industrial Master!". The four meters have no effect on anything.
3. **"History Points" reward the ethical choice, not historical knowledge.** This can teach the opposite of what happened: Carnegie *won* at Homestead, and the trusts made their owners very rich.
4. **One factual framing error:** 1873 is shown as a "Railroad Boom". 1873 was the year of the **Panic of 1873**, a railroad-driven crash.
5. **Classroom workflow gaps:** printouts have no student name, the printed header is nearly invisible, a refresh loses all progress, and "show your teacher" can be faked by replaying.
6. **Keyboard and screen-reader users lose their place** after every click, because focus drops to `<body>`.
7. **The source is not in version control.** This repo is empty, so the only copy is the Netlify deploy.

---

## 1. Game design and teaching value

### 1.1 You can game the answer key (Critical)

Where the best answer (100 points) sits in each question:

| Q | 1870 | 1873 | 1878 | 1882 | 1886 | 1889 | 1890 | 1892 | 1894 | 1900 |
|---|---|---|---|---|---|---|---|---|---|---|
| Best | A | A | B | B | B | A | B | B | B | B |

- **C is never correct.** Overall, A is right 3 times, B 7 times and C 0 times.
- Score for a fixed letter: **always B = 775** ("Good effort!"), always A = 300, always C = 145.
- **The best answer always costs cash** (−100 to −300), and the bad answers usually make cash. "Pick the option that loses money" works in 9 of 10 questions; Q9 is the only exception.
- The best answer is also always the longest and most balanced option ("…and improve safety", "…before tensions escalate").
- The Q9 (Pullman) wrong answers are absurd: "Factories no longer need railroads" and "All labor strikes were immediately successful". They test nothing.

**Fix:** Shuffle the option order on every play, or at least spread the correct answers evenly across A, B and C. Make the wrong answers believable. Separate cash cost from correctness, so that some good answers are cheap and some bad ones are expensive.

### 1.2 The simulation meters are decorative and contradict the score (High)

- Cash, Production, Morale and Reputation **do not affect the score, the story or the ending**.
- Perfect play ends at **Cash `$-600`** (also badly formatted: it should read `−$600`), Morale 100% and Reputation 100%. Both percentages hit the cap and stop changing halfway through.
- Production rises to 170 with no unit and no meaning.

This undercuts the game's own subtitle, "The Price of Progress". The trade-off between profit and people is the lesson, and the game never makes the player feel it.

**Fix (pick one):**
- **Make the meters matter.** For example, cash below $0 triggers a "creditors call your loans" event, and morale below 30 triggers a strike event. Use a two-axis final result such as "Profit vs. Social Cost" with a short historical comparison: "Carnegie's path looked like X."
- **Or remove the meters** and present the game honestly as a quiz.

### 1.3 "History Points" measure morals, not history (High, teaching)

Most questions reward the ethical choice. The feedback then says the opposite thing happened in real life:

- Homestead 1892: the ethical choice scores 100, but the feedback notes "the union was defeated". Carnegie's anti-union strategy *succeeded* for the company.
- Trusts in 1890: forming a trust scores 20, yet Standard Oil and later U.S. Steel (1901) became the dominant firms of the era.
- Q8 asks for the "most historically informed response" but rewards a counterfactual.

Students can come away thinking industrialists generally negotiated with labor and that doing so paid off. That is a misconception the unit is supposed to correct.

**Fix:** Score **understanding**, not virtue. Two ways:
- Keep the choices but add a follow-up question after each one: "What did most industrialists actually do?" / "Why did the Homestead strike fail?"
- Or score two meters, Profit and Society, and make the final reflection about the gap between them. That gap is the "price of progress".

### 1.4 Historical accuracy

| Item | Issue | Severity |
|---|---|---|
| **1873 "The Railroad Boom"** | The Panic of 1873 began in September 1873, when Jay Cooke & Co. failed after over-investing in the Northern Pacific Railroad. A depression followed until about 1879. Rewarding expansion in 1873 is backwards. | **High** |
| 1877 missing | The Great Railroad Strike of 1877, the first national strike, is the obvious bridge between 1873 and the union questions. | Medium |
| 1886 strike | Does not name **Haymarket** (May 1886), the most-tested event of that year and the key to the Knights of Labor's decline. | Medium |
| 1894 Pullman | "Federal intervention helped end the strike" is accurate but sanitized. Leave out the troops, the roughly 30 deaths, the injunction and *In re Debs* (1895), and students miss the main point: the government sided with capital. | Medium |
| 1900 | Fine. Optionally name the muckrakers (Riis 1890, Tarbell 1902–04). | Low |
| 1870 Bessemer, 1890 Sherman Act, 1892 Homestead/Pinkertons | Accurate. | — |

### 1.5 Smaller content notes

- The "15–20 min" estimate is long for 10 short questions. About 8–12 minutes is more realistic.
- Wrong answers show **"+0 history points"**. A plain "0 points" reads better.
- The 40-point partial answers get the red "low" styling, the same as 0-point answers (`o[2] < 50`).
- No standards alignment or teacher answer key is published, so another teacher could not adopt it easily.

---

## 2. Classroom operations

| # | Finding | Severity |
|---|---|---|
| 2.1 | **No student name, class period or date anywhere.** Printed or saved PDFs cannot be matched to a student. | **High** |
| 2.2 | **The printed header is almost blank.** Browsers drop background colors when printing by default, so the white title and dashboard text print onto white paper (checked in the PDF export). The results run to 2 pages. | **High** |
| 2.3 | **Refreshing, or a Chromebook discarding the tab, loses all progress.** Nothing is saved. | Medium |
| 2.4 | **The completion check is honor-system only.** Students can replay until they get 1000, and the answer key is readable in View Source (the `events` array). A screenshot of 1000/1000 proves nothing. | Medium |
| 2.5 | Gaming the answer key (see 1.1) makes the score a poor measure for grading. | High |
| 2.6 | There is no restart or exit during play, only at the end. | Low |

**Fixes:**
- Add a name and period field on the intro screen, and show it on the final screen and printout.
- Add a **completion code**: a short hash of name, choices and time, so the teacher can spot-check it. Show "attempt #N" and the first-attempt score.
- Save game state to `sessionStorage`.
- Add `print-color-adjust: exact` and print-specific dark-on-light colors for the header.
- If grades matter, send results to a Google Form. The footer promises no data collection, so get the policy right first (FERPA/COPPA) and update the footer.

---

## 3. Accessibility (WCAG 2.1 AA)

axe-core found **1 violation**. Manual testing found more, and they matter more:

| # | Finding | WCAG | Severity |
|---|---|---|---|
| 3.1 | **Focus is lost after every action.** `render()` rebuilds the panel with `replaceChildren()`, so after choosing an answer or pressing Continue, focus goes to `<body>`. Keyboard users have to tab through the whole page again, and screen readers lose their place. | 2.4.3 | **High** |
| 3.2 | **`aria-live="polite"` sits on the whole game panel.** Every re-render announces the entire panel, or, depending on the screen reader, nothing reliable. The feedback text is what should be announced. | 4.1.3 | Medium |
| 3.3 | `.progress` has `aria-label` on a plain `<div>` with no role, which axe flags as `aria-prohibited-attr` (serious). Use `role="progressbar"` with `aria-valuenow`, `aria-valuemin` and `aria-valuemax`, or a native `<progress>` element. | 4.1.2 | Medium |
| 3.4 | Once an answer is picked, the unchosen options fade to 72% opacity. Their description text drops to **3.24:1** contrast at 14 px. Disabled controls are technically exempt, but students are meant to read these next to the feedback. | 1.4.3 | Low–Medium |
| 3.5 | The footer text (`#647484` at 12 px on the gradient) is about **4.1:1**, below 4.5:1. | 1.4.3 | Low |
| 3.6 | Changes to the dashboard meters are never announced. | 4.1.3 | Low |
| 3.7 | No `prefers-reduced-motion` support for the hover lift, the progress bar animation or the smooth scroll. | 2.3.3 (AAA) | Low |
| 3.8 | The 🏆 emoji is read aloud as "trophy". It should have `aria-hidden`. The footer is a `<div>`, not a `<footer>` landmark. | Best practice | Low |

**Fix for 3.1 and 3.2:**
- After choosing, move focus to the result heading (`tabindex="-1"` plus `.focus()`).
- After Continue, move focus to the new question's `<h2>`.
- Remove `aria-live` from `#game` and put a small `aria-live` region on the result text only.

What already works: semantic `<button>` elements, visible `:focus-visible` styles, `lang="en"`, a correct heading order (h1 → h2 → h3), the decorative banner marked `aria-hidden`, all other contrast checks passing, and no layout break at 320 px.

---

## 4. UI and visual design

- Strong look overall: a consistent palette set up as CSS custom properties, good type scaling with `clamp()`, and a clear hierarchy.
- **Bug: the Print and Play Again buttons are misaligned** on the final screen. `button.primary` has `margin-top: 20px` and `button.secondary` does not, so Play Again sits 20 px lower (visible in the screenshot).
- On narrow screens the CSS factory drawing breaks up: chimneys float and overlap the sun. It is purely decorative, so this is minor.
- On mobile the dashboard falls into a 3 + 2 grid, which leaves an empty cell. A 2-column grid, or a 5-column grid with smaller text, would look tidier.
- Wrong answers are shown in red, with "What happened next?" as the heading. Consider a neutral heading that does not read as a punishment.

---

## 5. Performance

Excellent. Nothing needs changing.

- One request: **22 KB, or 8.2 KB brotli-compressed**. No fonts, images, frameworks or third-party scripts.
- Served from Netlify's edge with `cache-control: max-age=0, must-revalidate` and an etag, so updates reach students immediately.
- Zero console errors or warnings across three viewports and a full playthrough.

---

## 6. Security and privacy

The risk is low: a static page with no inputs, storage, cookies, third parties or network calls. The "No student data is collected" claim is **true** of the app itself. Netlify's normal access logs still record IP addresses, which is standard hosting.

| Header | Status |
|---|---|
| `Strict-Transport-Security` | Present (Netlify default, with preload) |
| `Content-Security-Policy` | **Missing** |
| `X-Content-Type-Options: nosniff` | **Missing** |
| `Referrer-Policy` | **Missing** |
| `Permissions-Policy` | **Missing** |
| `X-Frame-Options` / `frame-ancestors` | Missing. Leave it off, or allow your LMS, if the game is embedded in Canvas or Google Sites. |

- `innerHTML` is used only with fixed strings, and all dynamic text goes through `textContent`. **There is no XSS path.**
- Recommended `_headers` file (needs `'unsafe-inline'` or hashes, because the script and styles are inline):

```
/*
  Content-Security-Policy: default-src 'none'; script-src 'unsafe-inline'; style-src 'unsafe-inline'; img-src 'self'; base-uri 'none'; form-action 'none'
  X-Content-Type-Options: nosniff
  Referrer-Policy: no-referrer
  Permissions-Policy: camera=(), microphone=(), geolocation=()
```

---

## 7. SEO, sharing and metadata

- The `<title>` ("Industrial Empire | The Price of Progress") does not match the domain (`ramsushistoryreviewgame`).
- **No `<meta name="description">` and no Open Graph tags.** Links posted in Google Classroom, Teams or texts get no preview card.
- No `robots.txt` or sitemap (it returns Netlify's 404). That is acceptable for a classroom tool. Add `noindex` if you do not want it in search results.
- A `favicon.ico` exists, but no `apple-touch-icon` or `theme-color`.
- Netlify's default 404 page is shown for any other path.

---

## 8. Systems design and engineering

### 8.1 Source of truth (High)
**This repository is empty.** The deployed site is the only copy of the code. That means no history, no rollback outside Netlify's deploy list, no review, and no way for another teacher to contribute. It looks like a drag-and-drop deploy.

**Fix:** Commit the source here and connect Netlify to the repo, so every push to `main` deploys automatically and pull requests get preview URLs.

### 8.2 Code structure (Medium)
- One hand-minified file, with HTML, CSS, JS and content mixed together.
- Content is stored as **positional arrays**: `['label','desc',100,[-300,25,5,10],'feedback']`. Editing a question means counting commas, and one slip silently breaks scoring.
- **"10" is hard-coded in 6 places** (`step/10`, `'of 10'`, `step===9`, `step===10`, `score/10`, `'1,000'`). Adding or removing a question breaks the progress bar, the totals and the ending.
- Global mutable state (`state`, `step`, `score`, `choice`, `history`). Each render rebuilds the DOM, which causes the focus bug in 3.1.
- No tests or validation.

**Recommended layout (still zero-build and still one fast page):**

```
/index.html          # markup + <script type="module" src="game.js">
/styles.css
/game.js             # engine: state machine (intro → question → feedback → final)
/content/industrial-empire.json   # questions as named fields
/_headers            # security headers
/tests/content.test.js            # validates JSON: 3 opts, one 100-pt answer, answer positions balanced
```

- Store options as objects: `{ "label": …, "desc": …, "points": 100, "effects": {"cash": -300, …}, "feedback": … }`.
- Work out every total from `questions.length`.
- Shuffle the options at runtime.
- One `render(state)` function plus `focus()` management.
- With content in JSON, the same engine can run **other units** (Gilded Age politics, Progressive Era, the New Deal). That fits a domain called "history review game".

### 8.3 What is good here
- Zero dependencies, nothing to update, nothing to break.
- Safe DOM building through `textContent`.
- `'use strict'`, guards against double clicks, and `confirm()` before a reset.
- Works offline once loaded.

---

## 9. Prioritized fix list

| Priority | Fix | Effort |
|---|---|---|
| P0 | Put the source in this repo and connect Netlify | 15 min |
| P0 | Shuffle or rebalance answer positions; make wrong answers believable; separate cash from correctness | 1 hr |
| P0 | Correct the 1873 "Railroad Boom" to the Panic of 1873 (or move the expansion question to about 1879–1880) | 15 min |
| P1 | Name and period field on intro, final screen and printout; fix print colors | 45 min |
| P1 | Focus management and a scoped `aria-live`; fix the progressbar role | 30 min |
| P1 | Make the meters matter (bankruptcy and strike events, a Profit vs. Society ending) or remove them | 2–3 hr |
| P1 | Rethink "History Points" to reward understanding; add "what actually happened" follow-ups | 2–3 hr |
| P2 | Save progress to `sessionStorage`; completion code; show attempt number | 1 hr |
| P2 | Content to JSON, remove hard-coded "10", add validation test | 2 hr |
| P2 | `_headers`, meta description and OG tags, `−$600` formatting, button alignment | 30 min |
| P3 | Add 1877 and Haymarket; deepen Pullman; reduced motion; footer contrast; custom 404 | 1 hr |
