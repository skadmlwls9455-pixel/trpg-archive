# TRPG Archive

**A static archive for tabletop RPG session logs, plus TRPG ROOM, a
browser-based Call of Cthulhu 7e session player.**

Two tools for the same hobby, both plain HTML/CSS/JS with no build step and no
server:

- **Archive** (`trpg-backup/`): turns pasted chat logs from KakaoTalk,
  Discord, Roll20 or plain text into readable, novel-style log pages with cast
  profiles, and indexes them with search and rule/tag filters.
- **TRPG ROOM** (`trpg-backup/room/`): runs a CoC 7th edition session in one
  browser window, styled like a visual novel, with a rules engine for checks,
  sanity and dice.

The UI is in Korean.

## Why I built it

Online TRPG sessions live in chat apps, and chat apps are bad archives: logs
get buried, exports differ by platform, and image links rot. I wanted a
backup that is **just files**, survives any hosting service, and reads like the
story it was rather than a chat dump.

TRPG ROOM came from the other end of the session: running one without
juggling a character sheet, a dice bot and a notes doc in separate tabs.

## Design

**Archive**

- One generator (`tools/new-log.html`) parses four chat formats into one log
  format. A speaker name links a line to that character's profile card.
  Narration, scene breaks and continuation lines are detected.
- Adding a log is two steps: drop the generated HTML into `logs/`, then paste
  one entry into `data/scenarios.js`. There is no database to keep in sync.
- Pages are `noindex` by default, and the README warns that hot-linked images
  break when the source disappears.

**TRPG ROOM**

- **Rules engine with no DOM dependency** (`js/coc.js`). Success levels, bonus
  and penalty dice, critical and fumble thresholds (which depend on whether the
  skill is under 50), luck spending, pushed rolls and SAN loss all live in
  a pure module with Node tests.
- **Keeper view vs. player view.** Keeper notes, secret rolls and madness
  warnings only show in keeper view. The README is explicit that this is a
  single-screen convenience, *not* a security boundary: anyone can read the
  hidden log with dev tools. Real secrecy needs a server.
- **Scenario as data.** Characters, scenes, hotspots, clues and handouts are
  one JS object (`data/scenario.js`). Swapping the file swaps the scenario. The
  bundled one is a short three-scene demo written for this tool.
- **Works with no art.** Backgrounds fall back to CSS gradients and characters
  to initials. Edit mode lets you drop in images, which are resized and stored
  in IndexedDB, with export/import to move them between devices.
- Investigation hotspots, a clue board, a filterable session log (RP / dice /
  system / clue / OOC), three save slots, JSON export and three visual themes.

## Run it

Open `trpg-backup/index.html` (archive) or `trpg-backup/room/index.html`
(session player) in a browser. To publish, upload the `trpg-backup/` folder to
any static host. `netlify.toml` and `.github/workflows/netlify.yml` deploy it to
Netlify when a `NETLIFY_AUTH_TOKEN` secret is set.

```sh
node trpg-backup/room/tests/coc.test.js    # rules engine tests
```

## Files

```text
trpg-backup/
  index.html, js/, css/   archive list page and log page scripts
  data/scenarios.js       archive index (one entry per log)
  logs/                   one HTML file per session log (sample included)
  tools/new-log.html      log page + index entry generator
  room/                   TRPG ROOM (see room/README.md, Korean)
```

## How this was built

I defined what each tool needed to do for my own sessions. The code was built
with an AI coding assistant, and I did the final UI passes myself.
