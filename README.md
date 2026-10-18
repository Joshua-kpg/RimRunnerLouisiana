# RimRunnerLouisiana

Unofficial Louisiana HS boys basketball ratings. Not affiliated with the LHSAA or MaxPreps.
Status: **HELD - waiting for hosting.** Planned home: https://rimrunnerlouisiana.web.app/ (Firebase Hosting). The Firebase project could not be
created yet (Google project quota on the hosting account) and Cloudflare Pages is at its project limit. Working preview: this repo's GitHub Pages copy.
Until the site's Firebase project exists, accounts, picks and follows save only in the visitor's browser.

## Files
- `index.html` — the whole app (data embedded). `logos/` — small WebP school logos (from MaxPreps school profiles), served with the site.
- `data/results.json` — 2026-27 finals ledger written by the nightly updater.
- `firebase/` — Firestore rules and auth config for the site's Firebase project (accounts, picks, follows).

## Nightly scores

`.github/workflows/nightly-scores.yml` runs `scripts/update_scores.py` at 07:30 and 15:00 UTC
(1:30 am / 9:00 am Central standard time) and on manual dispatch. Before the Louisiana boys season opens
(`SEASON_START` in index.html = first game on the MaxPreps schedules, Mon Nov 9, 2026) the script makes no requests.
Games involving a Non-Select or Select school before Mon Nov 23, 2026 (LHSAA first contest date for Classes 5A-1A / Divisions I-IV) are ignored;
Class B and C games count from Nov 9 (their first contest date).
From then on it reads boys basketball finals for the last 3 days from the MaxPreps Louisiana scores-by-date page
(team-page fallback), matches them to the schedule by MaxPreps team id + date, records them in `data/results.json` and
bakes scores and W-L records into `index.html`. The page recomputes Elo from those finals. It commits only when data
changed, then deploys (index.html + logos/) to Cloudflare Pages if the `CLOUDFLARE_API_TOKEN` repository secret is set.
