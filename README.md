<p align="center">
  <img src="assets/coolstats_readme_title.svg" alt="coolstats" width="420">
</p>

**A faster way to understand a player before the pull.**

**Supported realms:**

- Onyxia
- Lordaeron
- Icecrown

coolstats brings UwU Logs lookups, a searchable player browser, cached gear and
talent inspection, and an optional character-panel overhaul together inside the
game.

It is built for raid formation and pugging: instead of repeatedly leaving the
game to look players up, you can quickly see their parses, rankings, available
specializations, and any equipment or talent snapshots you have previously
cached.

## Current Bundled Logs Coverage

As of `0.2.38`, the install-ready release ships separate load-on-demand data
addons for each supported realm:

- **Onyxia:** current Phase 3 Trial of the Grand Crusader 25 heroic bosses and
  Koralon, with Phase 2 Overall rank/parse and the locked historical Algalon
  parse retained as previous-season indicators. Current rankings are capped at
  the top 600 players per class/specialization, with 17,108 total bundled
  player rows in 6 ranked load chunks.
- **Icecrown:** ICC-profile data covering Icecrown Citadel, Halion, and
  Anub'arak, currently capped at the top 1,500 ranked players per
  class/specialization, with 34,703 total bundled player rows in 12 ranked load
  chunks.
- **Lordaeron:** ICC-profile data covering Icecrown Citadel, Halion, and
  Anub'arak, currently capped at the top 1,000 ranked players per
  class/specialization, with 15,199 total bundled player rows in 6 ranked load
  chunks.

The ICC realm datasets intentionally exclude boss-only/rankless leaderboard
players. This keeps the addon size and load behavior stable while still showing
boss parses for players who are present in the current ranked ICC coverage.
Bundled realm data is split into ranked load chunks by realm size; the Tooltip
& Cache data-load slider snaps to those chunks and can skip lower-ranked
tranches after `/reload` for lower memory use.

## Core Features

### UwU Logs In Game

| Tooltip logs | Direct player lookup |
| --- | --- |
| <img src="https://i.imgur.com/n2PYD7q.png" alt="UwU Logs inside a player tooltip" width="460"> | <img src="https://i.imgur.com/c057HzR.png" alt="Direct UwU Logs lookup from a player menu" width="354"> |

- Bundled realm-specific UwU Logs databases; no in-game web requests are made.
- Overall raid score, best rank, and specialization-specific parse data.
- Individual boss parses directly inside player tooltips by holding `ALT`.
- Parse colors and specialization icons make results easy to scan.
- Dedicated logs panels show every available specialization for a player.
- Selecting an individual boss in the browser shows boss parse, boss rank, and
  boss DPS directly in sortable columns.
- Side-by-side compare mode cross-references your logs with another player.
- UwU Logs action added to supported player and chat-name right-click menus.
- Compact `[coolstats: Player]` chat links let users with a compatible addon
  version open the linked player's logs directly.
- Individual player log panels include quick buttons for chat-linking logs and
  opening the player's Warmane Armory URL.
- Raid-progress fallback checks can show achievement/statistic progress when
  verified logs are missing on realms where the underlying client data is
  reliable.

The logs database is bundled with each addon release. coolstats does not make
web requests while the game is running.

### Player Browser

![coolstats player browser](https://i.imgur.com/NakLDmq.png)

The player browser brings all available player information into one searchable
table:

- Search players by name with responsive, delayed filtering.
- Filter by class, favourites, or main specialization.
- Sort columns in ascending, descending, or default order.
- Open a lightweight Statistics view for the current browser filters, showing
  class/spec representation with counts, percentages, a total sum, and an
  optional boss drilldown selector.
- View main spec, off spec, parses, best rank, and cache availability.
- See whether logs, gear, and talents are available before opening a player.
- Favourite players so they remain at the top of the default list.
- Right-click players to compare logs, whisper, invite, view cached talents,
  or favourite them.
- Open the normal logs and cached-armory panels by clicking a player.
- Escape closes open coolstats windows from front to back.
- Clear the locally stored inspection cache from inside the browser.

### Cached Gear And Talents

| Cached gear | Cached talents |
| --- | --- |
| ![Cached gear armory](https://i.imgur.com/C2z4SLy.png) | ![Cached talents](https://i.imgur.com/uTRNko9.png) |

When a player is available within inspection range, clicking, inspecting, or
looking them up can store a local snapshot of their equipment and talents.
Those snapshots can then be viewed later, even when the player is no longer
nearby.

- Paperdoll-style cached gear view with item icons, rarity borders, and item
  levels.
- Cached GearScore, equipped item level, and implied combat ratings.
- Cached talent builds with specialization backgrounds, rank indicators,
  specialization switching, and Blizzard-style talent tooltips when available.
- Separate Tooltip & Cache options can disable cached gear updates and cached
  talent updates independently for players who prefer less inspect work.
- The player browser shows cached gear/talent counts and coolstats memory at a
  glance, with a Social-icon info button that opens the cache settings when
  troubleshooting lag.
- Up to 1,500 recent player snapshots are retained.
- Snapshots older than 14 days are automatically removed.

Cached gear statistics are estimates derived from item data. Cached gem presence
is shown when available and enchant bonuses are included when available, while
some effects, talents, buffs, socket bonuses, gem stat bonuses, and other
character-specific modifiers may not be represented accurately.

### Performance Analysis

coolstats Player Statistics and Distributions

| Player Statistics | Per-Boss Performance Distribution |
| --- | --- |
| <img src="https://i.imgur.com/gM7pajr.png" alt="Specialization Representation" width="320"> | <img src="https://i.imgur.com/aaIM5CQ.png" alt="Per Boss Performance Histogram" width="440"> |

- Statistics panel from the player browser, with class/spec representation
  bars, counts, percentages, a bottom sum, and a realm-aware boss drilldown
  selector.
- Individual boss performance analysis via the parse histogram, so you can
  compare a player against everyone else or against their own class and
  specialization.

### Optional Character Panel Improvements

![coolstats character panel](https://i.imgur.com/rJTGDsf.png)

| Character stats panel | Pop-out mode |
| --- | --- |
| <img src="https://i.imgur.com/hEzocO6.png" alt="Custom coolstats character stats panel" width="320"> | <img src="https://i.imgur.com/nxFxYDo.png" alt="Custom stat pop-out mode" width="440"> |

coolstats also includes an optional overhaul of the default character panel:

- Extended stats panel with GearScore, item level, ratings, durability, repair
  cost, movement speed, and additional class-relevant statistics.
- Reorderable stat rows and configurable sections with quick bulk toggles.
- Favourite important statistics.
- Detachable stat popouts.
- Configurable backgrounds, opacity, zoom, contrast, and text palettes.
- Item-level badges and rarity-colored equipment-slot borders.
- Cleaner item tooltips when GearScore is installed.
- Configurable loot-alert toasts for looted items, roll wins, and crafts.

Character-panel features can be disabled in settings while keeping the logs
browser, tooltip parses, and lookup functionality enabled. Changing this option
requires a UI reload.

## Quick Start

1. Download the latest install-ready ZIP, named `coolstats_<version>.zip`, from
   the GitHub Release titled `coolstats <version>`.
2. Close the game, or at least exit to the character screen before replacing
   addon files.
3. Extract every included top-level addon folder directly into
   `Interface/AddOns/`. The ZIP contains multiple folders, not one folder to
   nest inside another folder.
4. Ensure these folders exist after extraction:
   `Interface/AddOns/coolstats/`, `Interface/AddOns/coolstats_Cache/`,
   `Interface/AddOns/coolstats_Data_<Realm>/`, and the included
   `Interface/AddOns/coolstats_Data_<Realm>_UWU_.../` shard folders.
5. If you are updating from a much older release and see duplicate or stale
   coolstats data addons on the character-select AddOns screen, remove the old
   `coolstats*` folders and extract the ZIP again.
6. Restart the game or run `/reload`.
7. Left-click the coolstats minimap button to open the player browser.

Warperia launcher installs are served from the generated default branch,
`warperia`, whose repository root is shaped like the install ZIP plus a small
GitHub README. The source and release workspace remains on `main`. If the
launcher creates only one `Interface/AddOns/coolstats/` folder containing
`cache_addon/`, `realm_data/`, `.gitignore`, or repository docs, Warperia is
pointed at the source branch instead of the generated install branch.

On login, coolstats confirms that it loaded successfully and shows the
freshness date of the bundled UwU Logs data. If the data is more than seven
days old, the addon displays a red update warning. `/cs update` opens copyable
Warperia and GitHub update links, and `/cs versioncheck` can ask nearby
coolstats users which addon/data version they have. Group checks audit the
current raid or party roster and call out players that do not have coolstats
installed.

Minimap controls:

- **Left-click:** open the player browser.
- **Right-click:** open the coolstats menu.
- **Left-click and drag:** move the minimap button.

## Using Player Data

- **Hover a player:** view their overall UwU Logs result.
- **Hold `ALT` while hovering:** view individual boss parses.
- **Right-click a supported player name:** open their UwU Logs panel.
- **Click a player in the browser:** open their logs and cached gear.
- **Right-click a browser row:** compare logs, whisper, invite, view talents,
  or favourite.
- **Inspect or interact with a nearby player:** update their local gear and
  talent snapshots when inspection data is available.

## Commands

| Command | Action |
| --- | --- |
| `/coolstats` or `/cs` | Open settings |
| `/coolstats settings` | Open settings |
| `/coolstats browser` | Open the player browser |
| `/coolstats uwu [player name]` | Open UwU Logs for a player |

If no name is supplied to `/coolstats uwu`, the current target is used. If
there is no target, coolstats uses your character.

## Settings And Optional Dependencies

Most visual and quality-of-life features can be configured or disabled from
the coolstats settings panel.

- **GearScore:** optional. coolstats includes its own GearScore calculation;
  installing GearScore additionally enables compatibility and tooltip-cleanup
  behavior.
- **BonusScanner:** optional. Without it, the addon still works normally, but
  some detailed gear-contribution lines in stat tooltips are unavailable.

Neither dependency is required for logs, the player browser, cached gear and
talents, loot alerts, or the character-panel improvements.

## Data And Privacy

- coolstats does not make web requests or send telemetry from inside the game.
- Bundled logs are updated by installing a newer addon release.
- Realm data is split into separate load-on-demand addons, and only the addon
  for your current realm is loaded during play.
- Cached gear, talents, favourites, and settings are stored locally in
  `coolstatsDB`.
- Cached inspection data can be cleared from the player browser.

## For Nerds: Data Loading And Cleanup

Warmane realm logs are bundled as load-on-demand Lua addons so the Wrath client
does not have to parse every realm and every boss table at login.

- `coolstats/` contains the UI and runtime logic.
- `coolstats_Cache/` stores the separate saved-variable cache addon.
- `coolstats_Data_<Realm>/` is the small realm manifest and compatibility
  loader.
- `coolstats_Data_<Realm>_UWU_XX/` shards contain ranked player tranches. The
  player-load slider snaps to those tranches, keeping the highest-ranked
  players first and skipping lower tranches after `/reload`.
- `coolstats_Data_<Realm>_UWU_<RAID>_XX/` shards contain individual boss rows.
  Raid layers are enabled by default, but users can disable raids they do not
  care about in Tooltip & Cache settings and reclaim that memory after
  `/reload`.

The generator targets roughly 3,000 ranked players per chunk, keeps normal
realm datasets at 6 or more chunks, and allows larger realms like Icecrown to
use more chunks. This also avoids the Wrath Lua 5.1 local-variable and giant
chunk failure modes that can silently break addons on 3.3.5 clients.

The player browser treats search rows, sort orders, per-boss result rows,
histogram inputs, and rendered tooltip lines as disposable runtime state.
Opening, sorting, and boss filtering can raise Lua working memory while the UI
is doing useful work. Closing the browser clears those references, drops the
tooltip line cache, clears selected-boss browser filters, and schedules bounded
incremental garbage-collection steps. coolstats intentionally avoids automatic
full `collectgarbage("collect")` calls during gameplay because those can cause
visible hitches. `/cs perf` prints the current Lua heap, loaded realm shards,
browser row/index counts, and approximate cache weights for troubleshooting.

## Support

Use the blue **HELP** button inside the player browser to begin an in-game
whisper to **Jumpscared** for questions, suggestions, or bug reports.
