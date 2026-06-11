# First Light

A free, self-contained companion for the *A Course in Miracles* Workbook — one lesson a day for a year, with a simple practice tracker for choosing **forgiveness over judgment**.

It's a small web app you host yourself and add to your iPhone home screen. No app store, no accounts, no servers, no cost beyond the free hosting. There's also an optional home-screen **widget** (Part 2) that shows the day's lesson.

---

## What it's for

The Workbook is a year-long daily practice: read a short lesson, then carry its idea through the day. First Light is built around that rhythm and adds the one thing a paper book can't — a place to notice, in the moment, when you chose forgiveness instead of judgment, and to watch that shift accumulate over the year.

**What the app does**

- **Today** — shows the current lesson (number, title, and full text) and a button to watch the accompanying commentary video for that lesson.
- **Practice** — one tap to log a moment: *chose forgiveness* or *noticed judgment*, with an optional note.
- **Journey** — your streak, days practiced, a 365-square year grid, and a balance that leans toward light as forgiveness outweighs judgment over time.
- **Self-paced or by the calendar** — move through lessons yourself (and repeat any lesson as long as you like), or let it advance one per day.
- **Yours and private** — everything stays on your device. No account, nothing sent anywhere. A backup button saves a copy you can restore on a new phone.

> **Lessons:** the app doesn't ship the Workbook text — you load it once from your own copy. The Workbook's first edition is in the US public domain and is freely available (e.g. openacim.org). See *Load the lessons* below.

---

## What lives in this repo

All of these sit together at the repo root:

| File | Purpose |
|------|---------|
| `index.html` | The app itself (this is the site homepage) |
| `lessons.md` | The Workbook text the app and widget read |
| `manifest.json` | Makes the app installable and sets the icon |
| `apple-touch-icon.png`, `icon-192.png`, `icon-512.png` | Home-screen icons |
| `.nojekyll` | Empty file — tells GitHub Pages to serve `lessons.md` as-is |
| `firstlight-widget.js` | The optional widget script (stored here for backup; you run it from Scriptable, not from the site) |
| `README.md` | This file |

---

# Part 1 — The App

## Deploy it (GitHub Pages)

1. Create a repository and upload these files to the root: `index.html`, `manifest.json`, the three `.png` icons, your `lessons.md`, and an empty `.nojekyll`.
2. In the repo: **Settings → Pages**. Under *Source*, choose the `main` branch and the root (`/`) folder. Save.
3. Wait about a minute. Pages gives you an address like `https://your-username.github.io/your-repo`. That's your app.

**The `.nojekyll` file matters.** Without it, GitHub Pages runs Jekyll and turns `lessons.md` into `lessons.html` — so the app and widget can't fetch `lessons.md`. The empty `.nojekyll` file at the root prevents that. To confirm, open `https://your-site/lessons.md` in a browser: you should see raw lesson text, not a 404 or a styled page.

## Load the lessons

You only do this once; afterward the lessons live on your device.

- **Automatic (easiest):** with `lessons.md` in the repo, the app fetches and loads all 365 the first time you open it.
- **Manual:** open **Settings → Load the lessons** and either paste the full Workbook text, load a `.txt`/`.md` file, or tap **Pull from repo**.

The parser splits the text by each `LESSON N` heading and handles the combined final reading (`LESSON 361 TO 365`), so you get all 365 with nothing missing.

## Install on iPhone

1. Open your app address in **Safari**.
2. Tap **Share → Add to Home Screen**.
3. It opens full-screen like a native app, shows the First Light icon, and works offline.

> Adding it to the home screen also protects your saved data: Safari clears this kind of storage for sites you haven't visited in ~7 days, but home-screen apps are exempt. Opening it daily (which the reminders below encourage) keeps it safe — and back up now and then regardless.

## Reminders

A home-screen web app can't reliably send scheduled notifications on iPhone (that needs a server, which static hosting doesn't provide). The dependable, free approach is your phone's own **Reminders** — the app holds the lessons and tracking, your phone handles the nudge, and you can retime or remove reminders anytime.

A rhythm that works well: one reminder in the morning to read the lesson, and two or three through the day to catch yourself in real moments (e.g. 8 AM, then 11 AM / 3 PM / 8 PM).

- **Fastest:** tell Siri, *"every day at 8 AM, remind me to read my Course in Miracles lesson,"* and repeat for the practice times.
- **Manual:** Reminders app → new reminder → tap **ⓘ** → turn on **Time** and set **Repeat → Daily**.

## Backup

Your practice lives only on the device. In **Settings → Backup**, tap **Download backup** every few weeks (and after milestones). **Restore** rebuilds everything on a new phone or after a reset.

---

# Part 2 — The Widget (optional)

A small home-screen widget that shows the day's lesson number and title, in the same dawn palette. Tapping it opens the app. It runs in **Scriptable** (a free iOS app) and reads the lesson title live from your hosted `lessons.md`.

The widget file at the repo root is harmless to the app — `index.html` never references it. You don't run it from the site; you paste it into Scriptable. Hosting it here is just backup.

## Install the widget

1. Install **Scriptable** from the App Store (free).
2. Open Scriptable and tap **+** (top right) to create a new script.
3. Delete the placeholder, then paste in the full contents of `firstlight-widget.js`.
4. Tap the script name at the top, rename it to **First Light**, then tap **Done**.

## Configure it

Edit the three marked lines at the top of the script:

```js
const SITE_URL   = "https://your-username.github.io/your-repo"; // your app address, no trailing slash
const START_DATE = "2026-06-11";   // the date you began Lesson 1
const PIN_LESSON = null;            // leave null to follow the date
```

- **`SITE_URL`** — where `index.html` and `lessons.md` live.
- **`START_DATE`** — the widget counts one lesson per day from this date.
- **`PIN_LESSON`** — leave as `null` normally (see *Repeating a lesson*).

## Test

Tap the **▶** (play) button inside Scriptable. If it works, you'll see the current lesson number and title. If not, a diagnostic popup tells you exactly what it tried and the result, so you can fix it.

## Add it to your home screen

1. Long-press an empty area of the home screen → tap **+** (top left).
2. Search for **Scriptable** and choose a size (small or medium) → **Add Widget**.
3. Long-press the new widget → **Edit Widget** → set **Script** to **First Light**.

The widget refreshes itself roughly hourly; the lesson changes at most once a day.

## Repeating or pausing a lesson

The widget tracks the day's lesson from `START_DATE`, assuming one lesson per day. If you stay on a lesson for several days, the date count will run ahead of where you are. To pin it:

```js
const PIN_LESSON = 42;   // shows Lesson 42 until you change it
```

Set it back to `null` to resume the daily count.

> Scriptable runs in its own sandbox and can't read the app's saved progress, which is why the widget tracks by date rather than from the app directly.

---

## Troubleshooting

**App or widget won't load the lessons**
Open `https://your-site/lessons.md` in a browser.
- A **404** or a styled page → add the empty **`.nojekyll`** file to the repo root and wait for Pages to redeploy.
- Raw text shows, but the widget still fails → check `SITE_URL` for typos (no trailing slash) and confirm the file is named `lessons.md` (lowercase). The widget also tries `lessons.txt`.

**Widget shows "Open to read today's lesson"**
That's the fallback when `lessons.md` can't be read. Tap **▶** in Scriptable for the diagnostic popup; it's almost always the `.nojekyll` or `SITE_URL` issue above.

**Widget shows the wrong lesson number**
Check `START_DATE`, or set `PIN_LESSON` to the lesson you're actually on.

**Widget looks blank after adding it**
Set **Edit Widget → Script → First Light**. A freshly added Scriptable widget isn't bound to a script until you choose one.

**Lost your practice data**
Restore from a backup file (**Settings → Backup → Restore**). Without a backup, cleared Safari data can't be recovered — so download one periodically.

---

*First Light is a personal, non-commercial companion app. A Course in Miracles Workbook text is loaded by you from your own copy; commentary videos link to their respective creators on YouTube.*
