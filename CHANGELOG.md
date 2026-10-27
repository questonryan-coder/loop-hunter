# Loop Hunter — Changelog

## v1.4 — 2026-10-27 (weekly autopilot run) — load speed + 13 Nights skin + first-100 unlock line

1. **Improve — load speed:** while a segment plays, the next two upcoming segments are preloaded (`preloadNext`, scripted daily stream or free-play queue), so the next segment starts without a fetch gap. No game-logic change; pool, durations, and daily determinism untouched.
2. **Add — 13 Nights seasonal realm skin:** during the Phoenix Oct 19–31 window the game wears its Halloween skin — pumpkin moon, warm gold palette, and a "13 Nights of Haunting" banner under the tagline. The window is date-checked, so the skin retires itself on Nov 1 with no further push.
3. **Pending one-shot (FIRST-100 push plan §3.7 / step M5, spent in this run):** the win-to-unlock card gains one secondary line directly under the existing auraflowfocus.com unlock link (site link stays first and primary): "Own the full sessions 🎃 The Haunted Sessions — all 7 hours, one download: https://qstnryan.gumroad.com/l/tmhxjo".

Verified: Node DOM/Audio-stub harness — 10/10 checks (syntax, seasonal skin active on Phoenix Oct 27, pumpkin moon + banner, unlock line present directly after the unlock link with the site link first, footer v1.4, preload cache holds the next 2 mp3s after a round start); live recheck after push (see loop-hunter-log.jsonl).

## v1.3 — 2026-10-23 (gate ALLOW seq 382) — the EPIC pass

The proven Wordle/Heardle mechanics, adapted honestly to a listening
game. All client-side, single index.html, no backend:

1. **Spoiler-free visual share strip** — the daily share text gains a
   block strip: one ⬛ per segment heard and let pass, with the final
   block (the call segment, or the repeat that got away) graded 🟩
   caught · 🟨 near (call within ~2s of the true loop point) · 🟥 far
   or missed. No names, no numbers — the strip cannot leak which
   segment loops. Stored on the daily record (`strip`); v1.2 records
   without one simply share without a strip.
2. **Stats view** — a "📊 Your stats" panel on the home screen
   (`localStorage lh_stats_v1`): hunts played (every completed round,
   both modes), loops caught + catch %, current & best daily streak
   (daily-only, as streaks always were — `bestStreak` now tracked in
   `lh_daily_v1`), fastest catch, closest near-miss ever (false-alarm
   calls only — a full miss doesn't count), sessions unlocked (n of 7).
3. **Next-daily countdown** — "Next hunt in HH:MM:SS" on the home
   daily card and the daily result card, ticking every second toward
   the next America/Phoenix midnight; at rollover the home card
   re-renders onto the new day's challenge.
4. **One hint per daily** — Daily Challenge only (free play stays
   pure). Pressing 💡 Hint spends the day's hint (`hintKey` in
   `lh_daily_v1`) and arms a 20-second window centred on the true loop
   point; when the stream reaches it, a subtle pulsing "👂 the loop is
   close… listen sharp" indicator glows. A hinted official result is
   flagged on the record and the share text owns it: "· hint used".
5. **Streak milestones** — at 3 / 7 / 14 / 30 days a celebration line
   ("🔥 7-day streak — a full week of hunting.") appears on the daily
   result and the home card.
6. **Personal bests** — beating your fastest catch (earliest catch
   time into a round) shows "⚡ New best: caught in M:SS!" in the
   catch feedback and on the result screen. A first-ever catch sets
   the mark silently — nothing to beat yet.

Notes for the weekly autopilot: v1.2 behaviour is unchanged underneath
(deterministic daily, official-first-attempt, near-miss meter, unlock
shelf — all regression-checked). New storage keys: `lh_stats_v1`;
`lh_daily_v1` gained `bestStreak` + `hintKey`, and daily records gained
`strip` + `hint`. If the segment pool changes, the hint window and
strip need no changes (both derive from the stream/`SEG_DUR`).

Verified: Node harness (DOM/Audio stubs, real handlers) — 70 checks:
determinism byte-identical across loads; forced daily win (strip,
stats, unlock, no overwrite on replay, PB on the faster replay);
hint exactly-once + window math + indicator on/off + share flag +
free-play purity; near-miss wolf (dist −0.8s, 🟨 strip, closestMiss);
missed daily (🟥 strip, no closestMiss); countdown lands exactly on
Phoenix midnight; seeded 2-day streak → milestone at day 3; free-play
regression. Live URL re-fetched after push (HTTP 200, v1.3 markers).

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
