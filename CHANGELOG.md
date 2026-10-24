# Loop Hunter — Changelog

## v1.2 — 2026-10-23 (gate ALLOW seq 381)

Three features, all client-side (no backend, no accounts, no analytics):

1. **Daily Challenge** — a distinct mode on the home screen, labeled with
   today's America/Phoenix date. One deterministic stream per day (mulberry32
   seeded by YYYYMMDD): same victim segment, same order, same repeat
   placement for every player. The first completed attempt of the day is the
   official result (stored in `localStorage lh_daily_v1`); replays are
   practice and never overwrite it. A daily-play streak (consecutive Phoenix
   days) is tracked and shown on the home card, the result card, and the
   share text. Result screen carries a formatted share block
   ("🎧 Loop Hunter Daily — Oct 23, 2026 · Caught it in 0:43 · 3 hunts this
   week") with a Copy button and `navigator.share` where available.
   *This closes the "daily-challenge seed" item on the autopilot backlog.*
2. **Near-miss meter** — in all modes. The true loop point is the moment the
   repeat segment starts on the round timeline; after any call (or a missed
   repeat) the result shows the signed distance in seconds, a two-marker
   timeline (🎯 true loop point vs 👆 your call), and plain-language text
   ("0.8s early — so close it hurts"). Free-play false alarms before the
   repeat is placed show an exact *minimum* distance and say so. Catches
   also get a compact near-miss line in the in-game feedback.
3. **Win-to-unlock** — catching a loop unlocks the full session that segment
   was cut from. Unlock card on the result screen + a persistent "Unlocked
   sessions" shelf on the home screen (`localStorage lh_unlocked_v1`).
   Segment→session mapping lives in `SEG_SOURCE` in index.html, derived
   from the build order in the project records (v1: Midnight Forest /
   Haunted Rain / Gothic Cathedral, 8 each, seg01–24; v1.1 log: +4 each
   from Fog Over the Moors / The Cellar / Witching Hour / Pumpkin Hollow,
   seg25–40). Links always go to https://auraflowfocus.com — no per-session
   deep links are published, so none are invented.

Notes for the weekly autopilot: free play is unchanged except the near-miss
additions and a longer catch pause (2.6s, so the near-miss/unlock feedback
is readable). Segment durations are hard-coded in `SEG_DUR` (ffprobe
measured) — if the pool changes, re-measure and update it, and extend
`SEG_SOURCE` for any new segments. Daily stream length mirrors free-play
round 1 (repeat lands as segment 7–9).

Verified: Node harness (DOM/Audio stubs) — 31 checks: daily determinism
across independent loads, near-miss distance math against known segment
timings incl. an exact daily false-alarm distance, forced daily win through
the real handlers (unlock + share + streak + no overwrite on replay), and a
free-play regression pass. Live URL re-fetched after push (HTTP 200, new
feature markers present).

## v1.1 — 2026-10-23

OG/Twitter share tags + share image; pool 24 → 40 segments (Fog Over the
Moors, The Cellar, Witching Hour, Pumpkin Hollow, +4 each).

## v1 — 2026-10-23

Playable game: 24 real AuraFlow segments, one secret repeat per round,
THAT'S A LOOP catch mechanic, streak/score/best in localStorage, game-over
funnel to auraflowfocus.com.
