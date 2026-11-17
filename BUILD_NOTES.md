# Glowmoth — best-game-01 entry build notes

## What it is
**Glowmoth** — an original night-garden arcade game, written from scratch for
make.fun `best-game-01` (prizes: 1st $400 / 2nd $200 / 3rd $120 / 4th–10th $40;
entries close **2026-11-18T18:10Z**, i.e. 2026-11-19 01:10 WIB).

You are Pip, a glow-moth. Catch golden pollen drifting through the garden,
chain pickups into combos (up to x5), grab blue moonpetals (freeze the
ash-wisps for 3s) and green leafhearts (+1 life), and dodge red cinders and
the grey ash-wisps that hunt your light. 3 lives; reach **750 points to earn
the Night's Crown** (level 5 = win), then keep flying in endless mode.

## How to play
- **Fly:** Arrow keys / WASD, or hold mouse / touch and drag to steer.
- **P** pause · **M** mute · **Enter / Space** start / restart / continue.
- **HUD:** score, best (localStorage), lives (hearts), level + progress bar,
  combo meter, frost timer. Synthesised WebAudio sounds (pickup pitch rises
  with combo, hurt/level/win/game-over cues) — no audio files.
- **Extras:** parallax starfield + moon + fireflies, particles, screen shake,
  floating score text, invulnerability blink, auto-pause when the tab hides,
  screen-reader status line.

## Layout (this folder is the repo root for submission)
- `game/index.html` — the whole game, single self-contained file (31 KB,
  inline CSS + JS, **zero external references**, no network requests).
- `main.ts` — the submission program (kit starter adapted for Glowmoth):
  copies `game/` + `screenshots/` to `/output`, writes `dossier.json`.
- `vendor/makefun-game.ts` — kit copy, **byte-identical, unchanged** (sha256
  dc671875…).
- `serve.ts` — kit's tiny static server (`node serve.ts` → localhost:8080).
- `screenshots/1-start.jpg`, `2-play.jpg`, `3-end.jpg` — real renders of the
  game in headless Chromium (1280×720, JPEG, 30–55 KB each, cap 256 KiB).
  First by name is the cover. Taken via deterministic `?shot=start|play|end`
  + `&seed=1` modes built into the game (seeded RNG, fixed tableau).
- `shots.json` — the three shot queries (kit convention).
- `LICENSE` — MIT, copyright topandina.
- `.makefun/owner.json` — `{"wallet": "GaYzaeTv12xNWTUaU56wSgwBPv371ZMfjdrcr84tKcz7"}`
  (the **agent wallet** on the @topandina account). ⚠️ Confirm before
  submitting whether the payout/owner wallet should be this agent wallet or
  the account wallet `2s2oPKbqy2mRoCDaHpQ3StW4upfwPnEPRF5AUFKg9V5J`.
- `requirements.json` — full competition requirements (reference, do NOT commit).
- `kit/` — the downloaded kit (reference, do NOT commit).

## Verification done (2026-11-17, headless Chromium 131 via Playwright)
- `node --check` on the extracted inline script: **OK**.
- No external refs: no `src=`/`href=` to outside files, zero `http` strings.
- Playwright run (`/tmp/pw/test.mjs`, re-run after final edits): **zero
  console errors, zero page errors** (an early favicon 404 was fixed with an
  inline `data:,` icon).
- State machine verified live: title → play (Enter) → pause (P) → play (P) →
  game-over screen → restart (Enter); mouse-drag steering works.
- Catch test (`/tmp/pw/catch-test.mjs`): auto-steered moth caught a pollen,
  **score 0 → 10**; a later full run also scored 10 organically via mouse.
- Spawning verified: motes grew 6 → 17 over ~5 s of play; wisps correctly
  absent at level 1 (they unlock at level 2+).
- Screenshots visually inspected: title panel, HUD (score/combo/level/hearts),
  pollen/petal/heart/cinder/wisp sprites, game-over panel — all render cleanly.
- `MAKEFUN_OUTPUT_DIR=/tmp/glowmoth-out node main.ts`: **OK** — output has
  `game/index.html`, 3 screenshots, `dossier.json` (schema
  `makefun/dossier/v1`, cover 0, text 32,178 bytes incl. full game source).
  Total output 345 KB, no symlinks, 1 game file (cap 993 / 48 MiB).

## Not verified live
- The **win path** (750 points → Night's Crown screen → endless mode):
  logic reviewed, overlay + button wired like the tested game-over flow, but
  reaching 750 in a live session was not played through (takes minutes).
  The `?shot=` modes also don't include a win shot.
- Sound was not heard (headless run has no audio output); code paths are the
  same ones exercised by the silent play-tests (AudioContext created on
  gesture; all `tone()` calls guarded).

## Still needed to submit (parent / Topan)
1. **Create a public GitHub repo** (must be created after the competition
   opened) and commit ONLY the repo files listed above (not `kit/` or
   `requirements.json`). Get the full 40-char commit SHA.
2. **Confirm the payout/owner wallet** for `.makefun/owner.json` (agent wallet
   vs account wallet — see above).
3. **Transcript bundle**: `node cli/makefun.mjs bundle` from inside the
   unpacked kit — it bundles the agent sessions that built the entry (the
   parent/main session; bundle discovers subagent sessions). Redact personal
   details of Topan if any appear (none are in the game or repo files).
4. **Entry message + signature**: sign the entry message with a wallet on the
   account (Ed25519; `node cli/makefun.mjs wallet add --keypair …` or the API)
   within 10 minutes of submitting.
5. Submit via the kit CLI / site with repo URL + commit SHA + bundle +
   title + signed entry message, before **2026-11-18T18:10Z**.
6. Optional polish if time allows: a 4th screenshot (live-action shot is at
   `/tmp/pw/5-live-later.jpg`), and a live play-through to 750 to confirm
   the win screen.
