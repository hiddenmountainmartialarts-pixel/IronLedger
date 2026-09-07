[README.md](https://github.com/user-attachments/files/31895371/README.md)
# Iron Ledger — install on your phone

This is a self-contained web app. Everything runs and stores data locally on
your phone — nothing is sent anywhere. To get the offline install + service
worker behavior working properly, it needs to be served from a real URL
(not opened directly as a file), because Android/Chrome only registers
service workers over http(s).

**Easiest free option: GitHub Pages (5 minutes, no ongoing cost)**

1. Create a free GitHub account if you don't have one.
2. Create a new repository (e.g. `iron-ledger`), and upload all 5 files in
   this folder (`index.html`, `manifest.json`, `sw.js`, `icon-192.png`,
   `icon-512.png`) to it.
3. In the repo, go to **Settings → Pages**, set Source to the `main` branch
   / root, and save. GitHub will give you a URL like
   `https://yourname.github.io/iron-ledger/`.
4. Open that URL on your Android phone in Chrome.
5. Tap the **⋮** menu → **Add to Home screen** (Chrome may also prompt you
   automatically with "Install app"). Confirm.
6. Launch it from your home screen — it opens full-screen, no browser bar,
   and works offline after the first load.

**Alternative:** any static host works the same way (Netlify, Vercel,
Cloudflare Pages — all have free tiers and a drag-and-drop upload).

## Backing up your data

All your mesocycles and logs live in your phone browser's local storage —
tied to that browser, that device. Two things worth doing:
- Use **Settings → Export backup** inside the app now and then. It downloads
  a JSON file you can save to Drive/email to yourself.
- If you ever clear Chrome's site data, reinstall the app, or switch phones,
  use **Settings → Import backup** to restore from that file.

## What's new in this version

- **Weight increments**: progression now works as double progression instead
  of percentage math. Reps climb each successful session until you hit the
  top of an exercise's rep range at target RIR — only then does weight jump,
  by a fixed real-world increment (Settings → Weight increments: default
  barbell 10lb, dumbbell 5lb, leg press/other machines 10lb). Weight never
  moves down; if a session comes in harder than planned, reps ease back
  instead.
- **Body weight & protein**: log your weight weekly in Settings. Progress
  shows training-day and rest-day protein targets (g/lb, both editable) and
  a weight trend line.
- **Milestones**: Progress now shows this week's and this month's total
  volume (weight × reps, summed) with percent change vs. the prior period,
  plus an end-of-cycle card comparing week 1 to your final accumulation week
  once you reach it.
- **Volume per muscle group**: a weekly bar breakdown of total pounds lifted
  per muscle, separate from the set-count landmarks, so you can see where
  you're actually falling short even though the program auto-corrects for it
  going forward.

## Roadmap (not built yet)

Cross-device competitions — syncing this to a second device (e.g. an
iPhone) and comparing progress by **percentage improvement** rather than
absolute pounds lifted, so two lifters at very different strength levels
can compete fairly — is a real goal but a separate, later project. It would
need real backend sync (this version is purely local/offline by design), so
it's intentionally out of scope for now.

Sustainment / maintenance cycles — a mesocycle mode for easing off
progression or holding current size/strength steady (life stress, an
injury easing back in, or just being happy where you are) instead of
always ramping toward MRV. Would introduce a fourth landmark below MEV
(RP calls this MV — Maintenance Volume), run at flat volume with a
conservative RIR instead of climbing toward failure, skip the forced
deload since fatigue stays low, and run open-ended instead of a fixed
6-8 week block. Not built yet — noted here so the idea doesn't get lost.

## Editing the program logic

Everything is in `index.html` in one `<script>` block, organized top to
bottom:
- `EQUIPMENT` — list of what you have access to. Change this first if your
  gym changes.
- `EXERCISES` — the exercise database, each tagged with required equipment,
  primary/secondary muscle, and rep range. Add or remove exercises here.
- `MUSCLES` — weekly volume landmarks (MEV/MAV/MRV) per muscle group. You
  can also tune these live in the app's Settings tab.
- `SPLITS` / `DAY_TYPE_MUSCLES` — how training days map to muscle groups
  for 2–6 day splits.
- `suggestNextLoad()` — the RIR-based autoregulation logic that decides how
  much weight to suggest next time you do an exercise.

If you edit `index.html` after it's hosted, just re-upload the file — no
build step, no dependencies.
