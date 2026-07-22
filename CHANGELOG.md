# Changelog

## Unreleased

## 0.2.34 - 2026-07-22

### Player browser

- Added a Stats button that opens a cached spec-representation chart for the
  current browser view, including class/spec-colored bars, counts, percentages,
  and selected-boss log scope when a boss filter is active.
- Refined the Statistics panel with the tabard background treatment, a built-in
  realm-aware boss selector, class-plus-spec labels beside spec icons, and a
  bottom sum of the displayed bars.
- Tightened the Statistics chart spacing by right-aligning class/spec labels
  into their spec icons and moving the summary line below the boss selector.
- Fixed browser and logs panel tabard backgrounds so the tabard texture stays
  visible above the black fallback backdrop unless a panel deliberately opts out.

### Data

- Carries forward the refreshed `0.2.33` Warmane datasets, including the
  expanded Onyxia top-500-per-specialization coverage.

## 0.2.33 - 2026-07-22

### Player browser

- Added a Boss DPS column when an individual boss filter is selected, using the
  already cached boss parse data for each visible row.

### Update center

- Added `/cs update`, a browser toolbar Update button, and a minimap menu entry
  with copyable Warperia and GitHub update links.
- Added lightweight peer version/data-age checks over addon messages for login,
  group roster changes, and manual `/cs versioncheck` checks.
- Improved manual group checks to audit the current raid/party roster, collect
  whisper replies, and print both responding players and players without
  coolstats installed.
- Removed the guild version check button/path; checks now focus on the current
  raid or party.
- Renamed the individual-boss parse sort header from boss abbreviations like
  `LJ` to `Parse`, with the full boss context kept in the sort tooltip.

### Onyxia logs

- Refreshed only the Warmane Onyxia TOGC/Koralon UwU Logs dataset, leaving
  Icecrown and Lordaeron data unchanged from `0.2.31`.
- Expanded Onyxia current rankings from the top 400 to the top 500 players per
  class/specialization, shipping 6,427 current active players and 14,761
  players total after retained history and TOGC boss-only coverage.
- Rebuilt all six current Onyxia boss leaderboards with 34,118 boss rows
  updated, no failed bulk-boss requests, and no missing configured encounters.
- Preserved Phase 2 Overall history and verified the locked historical Algalon
  records before writing the refreshed Onyxia dataset.

## 0.2.32 - 2026-07-21

### UI hotfix

- Moved the individual-player Warmane Armory icon farther inside the panel so
  it no longer clips against the border.
- Tightened the visible spacing between the "Link UwU Logs" icon and the close
  button on the individual-player panel.

### Data

- Carries forward the refreshed `0.2.31` Warmane weekly datasets for Onyxia,
  Icecrown, and Lordaeron.

## 0.2.31 - 2026-07-21

### UI refinements

- Aligned the individual-player panel's "Link UwU Logs" icon directly beside
  the close button so the title bar controls sit on the same line.
- Added a small blacksmithing-icon button on the left side of the
  individual-player panel that opens the player's Warmane Armory URL dialog.

### Data refresh

- Refreshed weekly Warmane UwU Logs datasets for Onyxia, Icecrown, and
  Lordaeron.
- Rebuilt every configured boss leaderboard with no failed ranking requests, no
  failed bulk-boss requests, and no missing configured encounters.
- Updated split Lua data chunks for all three load-on-demand realm data addons.

### Realm coverage

- Kept Icecrown capped at the top 1,500 ranked players per specialization,
  shipping 33,819 current ranked players and 34,201 players total with no
  boss-only/rankless ICC players added.
- Kept Lordaeron capped at the top 1,000 ranked players per specialization,
  shipping 14,990 current ranked players and 15,092 players total with no
  boss-only/rankless ICC players added.
- Kept Onyxia on the TOGC/Koralon phase profile, shipping 6,142 current active
  players and 14,541 players total with Phase 2 Overall and locked Algalon
  history retained.

### Validation

- Verified all configured Warmane boss profiles before the weekly pull.
- Preserved the locked historical Algalon records before writing the refreshed
  Onyxia dataset.
- Passed Warmane workspace validation for the canonical `coolstats_publish/`
  tree and the mirrored `coolstats/` tree.
- Passed Lua 5.1 validation for the Wrath 3.3.5 client target, including the
  split generated realm data chunks.
- Passed the Lua harness tests for browser analysis, realm loading, cache/talent
  handling, and target feedback.
- Passed the Onyxia phase-transition test suite.

## 0.2.30 - 2026-07-18

### Data refresh

- Refreshed weekly Warmane UwU Logs datasets for Onyxia, Icecrown, and
  Lordaeron.
- Rebuilt every configured boss leaderboard with no failed ranking requests, no
  failed bulk-boss requests, and no missing configured encounters.
- Updated split Lua data chunks for all three load-on-demand realm data addons.

### Realm coverage

- Kept Icecrown capped at the top 1,500 ranked players per specialization,
  shipping 33,808 current ranked players and 34,063 players total with no
  boss-only/rankless ICC players added.
- Kept Lordaeron capped at the top 1,000 ranked players per specialization,
  shipping 14,983 current ranked players and 15,040 players total with no
  boss-only/rankless ICC players added.
- Kept Onyxia on the TOGC/Koralon phase profile, shipping 5,607 current active
  players and 13,965 players total with Phase 2 Overall and locked Algalon
  history retained.

### Validation

- Verified all configured Warmane boss profiles before the weekly pull.
- Preserved the locked historical Algalon records before writing the refreshed
  Onyxia dataset.
- Passed Warmane workspace validation for the canonical `coolstats_publish/`
  tree and the mirrored `coolstats/` tree.
- Passed Lua 5.1 validation for the Wrath 3.3.5 client target, including the
  split generated realm data chunks.
- Passed the Lua harness tests for browser analysis, realm loading, cache/talent
  handling, and target feedback.
- Passed the Onyxia phase-transition test suite.

## 0.2.29 - 2026-07-16

### Data refresh

- Refreshed weekly Warmane UwU Logs datasets for Onyxia, Icecrown, and
  Lordaeron.
- Rebuilt all configured boss leaderboards with no failed ranking requests, no
  failed bulk-boss requests, and no missing configured encounters.
- Updated split Lua data chunks for all three load-on-demand realm data addons.

### Realm coverage

- Kept Icecrown capped at the top 1,500 ranked players per specialization,
  shipping 33,804 current ranked players and 33,988 players total with no
  boss-only/rankless ICC players added.
- Kept Lordaeron capped at the top 1,000 ranked players per specialization,
  shipping 14,982 current ranked players and 15,026 players total with no
  boss-only/rankless ICC players added.
- Kept Onyxia on the TOGC/Koralon phase profile, shipping 5,340 current active
  players and 13,605 players total with Phase 2 Overall and locked Algalon
  history retained.

### Validation

- Verified the locked historical Algalon records for 5,972 players before
  writing the refreshed Onyxia dataset.
- Rechecked all realm boss profiles before the weekly pull.
- Passed Warmane workspace validation for the canonical `coolstats_publish/`
  tree and the mirrored `coolstats/` tree.
- Passed Lua 5.1 validation for the Wrath 3.3.5 client target, including the
  split generated realm data chunks.
- Passed the Lua harness tests for browser analysis, realm loading, cache/talent
  handling, and target feedback.
- Passed the Onyxia phase-transition test suite.

## 0.2.28 - 2026-07-15

### Highlights

- Added an individual-boss dropdown to the player browser so players can filter
  and sort by a specific boss parse, not only by class and specialization.
- Made boss choices realm-aware: Onyxia uses the current TOGC/Koralon profile,
  while Icecrown and Lordaeron use their ICC-profile boss lists.
- Added a taller boss-parse distribution view with color-coded histogram bars,
  a player marker, and a readable white distribution curve.

### Onyxia logs

- Refreshed Warmane Onyxia TOGC/Koralon UwU Logs data for the current Onyxia
  phase.
- Verified the locked historical Algalon records for 5,972 players before
  writing the refreshed dataset.
- Preserved Phase 2 Overall history while writing 13,405 Onyxia players for the
  current TOGC phase.

### Player browser

- Added individual boss parse sorting alongside the existing class and
  specialization filters.
- Expanded the browser window while an individual boss filter is active so the
  histogram has enough vertical space and does not spill out of the frame.
- Removed the histogram background panel so only the bars, curve, axis labels,
  and player marker remain visible.
- Cached per-boss/spec browser lookups to reduce stutter when changing boss
  filters.

### Validation

- Passed Warmane workspace validation for the canonical `coolstats_publish/`
  tree and the mirrored `coolstats/` tree.
- Passed Lua 5.1 validation for the Wrath 3.3.5 client target.

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
