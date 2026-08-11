# Changelog

## 0.2.41 - 2026-08-11

### Data

- Refreshed all Warmane UwU Logs datasets on 2026-08-11 using the
  realm-aware weekly caps: Onyxia top 600 per class/spec, Lordaeron top 1,000
  per class/spec, and Icecrown top 1,500 per class/spec.
- Shipped refreshed dynamic ranked chunk tranches for the in-game data-load
  slider: Onyxia 18,371 total players in 7 chunks, Lordaeron 15,273 total
  players in 6 chunks, and Icecrown 34,998 total players in 12 chunks.
- Preserved the realm-aware raid layer shard layout for TOGC/VOA/Ulduar on
  Onyxia and ICC/RS/TOGC on Icecrown and Lordaeron.
- Ran duplicate-name verification and targeted boss-row repair during the
  refresh; generated data reports zero failed leaderboards, zero bulk boss
  failures, zero character fetch failures, and zero missing boss encounters.

### UI

- Added a first-run guide skip button and `/cs guide skip`, which marks the
  account-level guide completion flag so the guide will not prompt again.
- Added an Item Levels color mode that switches item-level badge text and slot
  border glow between the coolstats GearScore gradient and Blizzard item rarity
  colors.
- Reorganized settings so Character Panel owns character-frame controls,
  item-level placement/font/color controls, GearScore cleanup, and side-panel
  visuals directly on one page.
- Added upper-left and upper-right item-level badge positions and clarified the
  Item Level Colors section in settings.

### Distribution

- Rebuilt the install ZIP and generated Warperia branch from the same
  install-ready artifact so GitHub releases and Warperia launcher installs stay
  aligned.

## 0.2.40 - 2026-08-07

### Data

- Refreshed all Warmane UwU Logs datasets on 2026-08-07 using the
  realm-aware weekly caps: Onyxia top 600 per class/spec, Lordaeron top 1,000
  per class/spec, and Icecrown top 1,500 per class/spec.
- Shipped refreshed dynamic ranked chunk tranches for the in-game data-load
  slider: Onyxia 17,836 total players in 6 chunks, Lordaeron 15,222 total
  players in 6 chunks, and Icecrown 34,846 total players in 12 chunks.
- Preserved the realm-aware raid layer shard layout for TOGC/VOA/Ulduar on
  Onyxia and ICC/RS/TOGC on Icecrown and Lordaeron.
- Ran duplicate-name verification and targeted boss-row repair during the
  refresh so resolved ambiguous names keep current boss parses without falling
  back to stale reused-character rows.

### Distribution

- Rebuilt the install ZIP and generated Warperia branch from the same
  install-ready artifact so GitHub releases and Warperia launcher installs stay
  aligned.

## 0.2.39 - 2026-08-05

### UI

- Changed the first-run feature guide completion flag to be account-wide. Once
  the guide is completed on any character, it will stay completed for every
  character on that WoW account.
- Added migration from the previous per-character guide completion table into
  the new account-level completion version.

### Distribution

- Rebuilt the Warperia install branch from the release ZIP so launcher installs
  keep the correct multi-addon shard layout.

## 0.2.38 - 2026-08-04

### Documentation

- Expanded the README install guide for the new multi-addon data shard layout.
- Added a technical README section explaining ranked chunks, raid layer shards,
  runtime browser cleanup, and `/cs perf` diagnostics.

### UI

- Added an in-game changelog window, available from the minimap menu, `/cs
  changelog`, and a new browser toolbar Changes button.
- Added a first-run feature guide with character-aware completion, delayed
  login startup, tell-message sounds, and a pulsing welcome glow.
- Expanded feature guide callouts across the player browser, boss filters,
  Statistics, Update Center, changelog, UwU Logs, cached gear, cached talents,
  Log Analysis, sharing tools, and memory controls.
- Fixed first-run guide step ordering for Statistics, Update Center, changelog,
  cached talents, Log Analysis, and UwU panel close actions.
- Fixed the Warmane Armory and plain-text log summary copy dialogs so they open
  above the UwU Logs panel and cached gear panel.

## 0.2.37 - 2026-08-03

### Performance

- Coalesced character-panel refresh events so repeated stat/aura/inventory
  bursts queue one short-delayed update instead of forcing synchronous full UI
  refreshes.
- Added a bounded 64-entry LRU for rendered UwU tooltip lines to prevent
  session memory growth from hovering many unique players.
- Cached the merged player-browser base index between searches, filters, and
  sorts, with invalidation on gear/talent cache changes and realm-data reloads.
- Added `/cs perf` for lightweight in-game diagnostics covering Lua heap,
  coolstats memory, loaded UwU players/chunks, tooltip cache size, and browser
  index size.
- Updated the UwU data generator so future log refreshes can emit direct
  player assignments per chunk, avoiding the temporary outer chunk table during
  load.

### Data

- Refreshed all Warmane UwU Logs datasets on 2026-08-03 using the realm-aware
  weekly caps: Onyxia top 600 per class/spec, Lordaeron top 1,000 per
  class/spec, and Icecrown top 1,500 per class/spec.
- Shipped refreshed dynamic ranked chunk tranches for the in-game data-load
  slider: Onyxia 17,108 total players in 6 chunks, Lordaeron 15,199 total
  players in 6 chunks, and Icecrown 34,703 total players in 12 chunks.
- Verified the weekly bulk boss refresh completed with zero failed bulk
  leaderboard requests and no failed ranking leaderboards across all three
  realms.
- Ran duplicate-character-name verification and targeted boss-row repair during
  the refresh so resolved ambiguous names keep current boss parses without
  doing full per-player boss fetching.

### UI

- Removed the always-visible peer status lines from the Update Center while
  keeping manual group checks available.
- Added extra spacing around the Tooltip & Cache player-load slider labels so
  the selected player count no longer overlaps the slider.

## 0.2.35 - 2026-07-27

### Player browser

- Reworked the player right-click UWU Logs action into a detached coolstats
  button attached to Blizzard's menu, preserving the quick action while avoiding
  protected Focus Target menu taint.
- Fixed the detached right-click action so it can detect chat-name player menus
  and remain clickable after Blizzard closes the dropdown.
- Added a short close grace so the detached UWU Logs action disappears after
  clicking away instead of sticking on screen; it now only uses that grace when
  the cursor is moving through the dropdown-to-UWU-button lane.
- Hooked the actual dropdown-open path so the detached UWU Logs action appears
  on the initial player menu, not only after opening a submenu.
- Prevented secondary Blizzard menu hover refreshes from hiding an already shown
  detached UWU Logs action.
- Tracked open dropdown submenus so expandable menu interactions no longer make
  the detached UWU Logs action flicker.
- Hid the detached UWU Logs action immediately when Blizzard's Cancel menu item
  is pressed, without hiding it merely on Cancel hover.
- Hid the detached UWU Logs action immediately when any Blizzard dropdown action
  row is clicked.
- Changed Statistics chart labels to show specialization before class, matching
  the intended representation-chart layout.
- Restored the original Log Analysis window dimensions after the compact layout
  caused the comparison chart to overflow.
- Added a copyable plain-text UwU log summary button to individual player log
  panels, generating a single compact line instead of auto-sending chat spam.
- Moved the copyable log-summary button beside the Armory button and changed
  copied hard-mode markers to `{skull}` chat tokens.

### Data

- Refreshed all Warmane UwU Logs datasets on 2026-07-27 with realm-aware
  coverage caps: Onyxia top 600 per class/spec, Lordaeron top 1,000 per
  class/spec, and Icecrown top 1,500 per class/spec.
- Shipped refreshed dynamic chunk tranches for the in-game data-load slider:
  Onyxia 15,916 total players in 6 chunks, Lordaeron 15,140 total players in 6
  chunks, and Icecrown 34,427 total players in 12 chunks.
- Verified the weekly bulk boss refresh completed with zero failed bulk
  leaderboard requests across all three realms.
- Added duplicate-character-name safeguards to the Warmane UwU Logs generator:
  ambiguous ranking rows are confirmed through the character endpoint and
  automatically receive targeted boss-row repair after bulk leaderboard refresh.
- Made the weekly refresh wrapper realm-aware by default so a normal all-realm
  update preserves the intended per-realm caps instead of applying one cap to
  every realm.
- Updated the generator to prefer `coolstats_publish/` as the canonical output
  tree from this workspace and shortened duplicate-repair console logs to a
  count plus preview.

### Cached Gear And Talents

- Always show the cached-talents button in the standalone UWU gear panel when a
  player name is available, even if that player has no UwU log history yet.
- Refreshed the standalone UWU gear panel after talent inspect data arrives so
  newly cached talents are reflected without reopening the panel.
- Stored gem IDs from cached item links and guarded client gem API calls, without
  resolving gem item stats during tooltip rendering.
- Added small per-slot gem indicators so cached gear snapshots visibly confirm
  stored gems when the server exposes them.
- Restored the older gear-link refresh behavior for real inspected/targeted
  units, so fresh cache entries still pick up full gemmed item links from the
  current player without requiring a cache clear.
- Fixed the restored gear-link refresh path so it no longer calls the cached
  gear summary builder out of scope, and made per-slot item reads fail-soft.
- Added separate Tooltip options for cached gear and cached talents so users can
  disable either inspect cache path independently.
- Renamed and reorganized the settings page into Tooltip Lines, Inspect Cache,
  and Logs And Progress sections so cache-related troubleshooting controls are
  easier to find.
- Added a lightweight player-browser cache/memory indicator with a Social-icon
  info button that explains core, realm-data, and cache memory, then opens
  Tooltip & Cache settings for lag troubleshooting.
- Replaced the browser-only cap with a realm-aware UwU data load slider that
  snaps to generated data chunks, trims the lowest-ranked tranche first, and
  skips disabled chunks before their Lua tables are built after `/reload`.
- Made the UwU data load slider default/reset state explicitly load every
  current-realm player, and added the same efficiency hint to the browser memory
  tooltip.
- Made generated UwU data chunking dynamic by realm size, keeping smaller
  realms at 6 chunks while splitting Icecrown into 12 finer load tranches.
- Reused recently captured gear and talent snapshots for 15 minutes and nudged
  incremental garbage collection after browser rebuilds to reduce memory churn
  while clicking, sorting, and filtering.
- Added a cached-talents button to the cached gear panel beside standalone UwU
  Logs lookups.
- Fixed talent capture to retry safely across `INSPECT_READY` and
  `INSPECT_TALENT_READY`, preventing the talent panel from staying empty when
  gear inspection succeeds first.

### Options

- Split Loot Toasts into its own options page with separate sections for the
  global toggle, loot sources, quality threshold, sounds, and glow animations.
- Simplified the root coolstats options page so it focuses on core character
  panel settings and points detailed controls to the child settings pages.

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
