# Changelog

## 0.2.28 - 2026-07-15

### Onyxia logs

- Refreshed Warmane Onyxia TOGC/Koralon UwU Logs data.
- Verified the locked historical Algalon records for 5,972 players before
  writing the refreshed dataset.
- Preserved Phase 2 Overall history while writing 13,405 Onyxia players for the
  current TOGC phase.

### Player browser

- Added realm-aware individual boss filtering and boss parse sorting.
- Added a taller color-coded boss histogram with a player marker and readable
  white distribution curve.
- Cached per-boss/spec browser lookups to reduce stutter when changing boss
  filters.

## 0.2.27 - 2026-07-14

### Data refresh

- Refreshed Warmane UwU Logs data for Onyxia, Icecrown, and Lordaeron.
- Rebuilt all configured boss leaderboards with no failed ranking or bulk-boss
  requests.
- Preserved Onyxia Phase 2 Overall history and the locked historical Algalon
  records while refreshing the current TOGC/Koralon phase data.

### ICC coverage

- Removed Toravon/Vault of Archavon from the Icecrown and Lordaeron bundled
  ICC datasets.
- Kept those realms focused on Icecrown Citadel, Halion, and Anub'arak only;
  no extra Trial of the Grand Crusader bosses are included.
- Kept Icecrown and Lordaeron ranked-player-only: 33,794 active Icecrown
  players and 14,978 active Lordaeron players, with no boss-only/rankless rows
  added.

## 0.2.26 - 2026-07-11

### ICC coverage

- Expanded Icecrown ICC data to the top 1,500 ranked players per specialization, shipping 33,784 current ranked players without importing boss-only or rankless rows.
- Expanded Lordaeron ICC data to the top 1,000 ranked players per specialization, shipping 14,978 current ranked players without importing boss-only or rankless rows.
- Kept Icecrown and Lordaeron boss parses limited to players with current ranked ICC coverage to avoid the oversized rankless-player data issue from 0.2.24.

### Stability

- Hardened the achievement-comparison fallback initialization so first-hover raid progress checks do not break when Blizzard_AchievementUI has not finished loading.
- Added Lua 5.1 release validation to catch Wrath-client syntax/chunk issues before packaging.

## 0.2.25 - 2026-07-10

### Hotfix

- Rebuilt Icecrown and Lordaeron ICC data without importing boss-only leaderboard players into the shipped dataset.
- Reduced Icecrown from roughly 189k shipped players back to 10.6k current ranked players, and Lordaeron from roughly 49.6k back to 7.5k.
- Kept ICC boss parses for current ranked players while leaving Onyxia TOGC boss-only/Koralon behavior unchanged.

## 0.2.24 - 2026-07-08

### Data refresh

- Refreshed Warmane UwU Logs data for Onyxia, Icecrown, and Lordaeron.
- Rebuilt Icecrown and Lordaeron ICC boss data with no failed leaderboard or bulk-boss requests.
- Kept Onyxia on the TOGC phase dataset while preserving Phase 2 overall history and the locked Algalon historical records.

## 0.2.23 - 2026-07-06

### Log integrity

- Locked all 6,970 per-spec Algalon records across 5,972 players for Onyxia's TOC phase; data generation now aborts if any historical result changes or disappears.
- Keep every boss parse strictly separated by specialization; default views use one coherent specialization and historical Algalon is labeled with its source spec.
- Import current boss-leaderboard players even when they have no overall TOGC ranking or Phase 2 history.

### Interface

- Added a chain-link button to UwU Logs panels for inserting the displayed player into chat.
- Removed the duplicate Historical Algalon row from the panel summary.

## 0.2.22 - 2026-07-05

### Onyxia logs

- Refreshed Onyxia's Phase 3 rankings and all five TOGC 25H boss leaderboards plus Koralon.
- Retained Phase 2 overall rankings and Algalon as historical indicators.
- Preserve retained Algalon parses when a player's current TOGC best specialization differs from their historical Ulduar specialization.
- Disabled TOGC achievement/statistics fallback because Warmane cross-credits some 10H progress into the 25H client data.
- Show one honest TOGC logs row, including an empty `0/5` row when no verified TOGC logs exist.

### Log links

- Added compact `[coolstats: Player]` chat tokens from browser rows, browser actions, and UwU Logs panel titles.
- Convert received tokens into clickable UwU Logs links for players running version 0.2.22 or later.
- Keep shared tokens short and readable for players without a compatible coolstats version.

### Interface and stability

- Made Phase 3 and Phase 2 browser labels specific to Onyxia's TOGC phase.
- Fixed achievement comparison cleanup so closing Blizzard's comparison UI no longer leaves tooltips permanently stuck on "Achievement UI busy" or reuses stale player statistics.
- Colored player/specialization titles and specialization rows in UwU Logs panels with the player's class color.

## 0.2.21 - 2026-07-05

### Onyxia Phase 3

- Activated Trial of the Grand Crusader as Onyxia's current logs phase.
- Added the five 25-player heroic TOGC encounters and Koralon.
- Retained Algalon as the single historical boss indicator from Phase 2.
- Removed the deprecated Ulduar boss rows from the shipped Onyxia dataset.
- Left Onyxia's Lair out until it appears in the official player logs.

### Rankings and player browser

- Preserved Phase 2 overall parse scores and ranks separately from current data.
- Added a Phase 2 Overall row to player tooltips and logs panels.
- Made Phase 3 parse, rank, and specialization data the primary player-browser values.
- Added a sortable P2 Overall browser column containing the historical parse and rank.
- Added separate Phase 3 ranked-player and Phase 2 history counts.
- Prevented historical-only Ulduar results from appearing as current TOGC rankings.

### Stability

- Fixed the Wrath Lua 5.1 local-variable limit regression that prevented the tooltip module, player browser, and related features from loading.
- Added phase-transition validation for the retained history schema and boss roster.

## 0.2.20 - 2026-06-28

- Refreshed the bundled Onyxia, Icecrown, and Lordaeron logs datasets.
- Added preparation for Onyxia's TOGC phase transition.
