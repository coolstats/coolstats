# Realm Data Maintenance

The in-game addon never makes web requests. Realm datasets are generated
outside the game and bundled into install-ready releases.

This document describes the Warmane data workflow. Keep Rising Gods work in its
own workspace and repository; do not copy generated Rising Gods files into the
Warmane `coolstats_publish` tree.

## Authoritative Workspace

- `coolstats_publish/` is the Warmane GitHub/release workspace.
- `coolstats/` may be used as a local mirror or unpacked working copy, but it
  is not the Git remote used for Warmane releases.
- The only correct Warmane GitHub repository is
  `coolstats/coolstats` (`https://github.com/coolstats/coolstats.git`).
  Do not publish from any other owner unless the repository target is
  explicitly changed.
- Make addon/runtime changes in `coolstats_publish/` first, then mirror them to
  `coolstats/` before handoff. The workspace guard below fails if the mirror
  drifts or if TOC versions disagree.
- Generated release ZIPs live under `coolstats_publish/releases/`.
- Runtime releases contain sibling addon folders:

  ```text
  coolstats/
  coolstats_Cache/
  coolstats_Data_Icecrown/
  coolstats_Data_Lordaeron/
  coolstats_Data_Onyxia/
  ```

## Realm Profiles

- **Onyxia:** ICC-profile data covering Icecrown Citadel and Toravon, with the
  final TOGC snapshot retained as Phase 3 Overall score/rank. Until UwU exposes
  usable Onyxia ICC and Toravon leaderboards, boss parses are intentionally
  empty. TOGC, Ulduar, Anub'arak, Algalon, Koralon, and Ruby Sanctum boss parses
  are not retained in the active Onyxia ICC dataset.
- **Icecrown:** ICC-profile data covering Icecrown Citadel, Halion, and
  Anub'arak. Vault of Archavon bosses are intentionally excluded.
- **Lordaeron:** ICC-profile data covering Icecrown Citadel, Halion, and
  Anub'arak. Vault of Archavon bosses are intentionally excluded.

Onyxia, Icecrown, and Lordaeron data is generated as separate load-on-demand
addons. Only the dataset matching the player's current realm is loaded in-game.

Onyxia's deprecated Ulduar and TOGC boss rows are not shipped in the ICC
dataset. Onyxia's Lair and Ruby Sanctum are excluded until they are present in
the official player logs for the active phase.

## Current Caps And Size Guardrails

As of `0.2.45` and the Onyxia ICC transition prep:

- **Icecrown:** top 1,500 ranked players per class/specialization, resulting in
  33,896 current ranked players and 35,803 total players in 12 player chunks.
- **Lordaeron:** top 1,000 ranked players per class/specialization, resulting
  in 15,090 current ranked players and 15,517 total players in 6 player chunks.
- **Onyxia:** top 600 ranked players per class/specialization. During ICC
  transition, run one final TOGC refresh first and keep
  `coolstats_publish/data/uwu_logs_onyxia_toc.json` as the Phase 3 Overall
  source. The active ICC handoff intentionally contains 9,864 Phase 3 Overall
  historical rows, 0 active Phase 4 ranked rows, and 6 player chunks until UwU
  exposes usable Onyxia ICC and Toravon leaderboards.

For ICC realms, boss-only/rankless leaderboard players must not be added to the
shipped dataset. Boss parses should be attached only to players already present
in the current ranked ICC coverage. This prevents the oversized rankless-player
dataset issue seen in `0.2.24`.

Duplicate display names must be resolved against the UwU character endpoint
before generated rows are written. If a realm has old and current characters
with the same displayed name, the updater must keep only the character-confirmed
ranking row and run targeted boss-row repair for that player after the bulk
leaderboard pass. This is a limited parallel repair pass, not the normal bulk
boss-update path.

To keep this check fast enough for Icecrown-sized leaderboards, only duplicate
names relevant to the retained current dataset should be verified. The updater
first marks normalized player names that have at least one row inside the
realm's current per-spec cap, then checks only ambiguous identities that also
have a retained row. Below-cap sibling rows for the same identity are still kept
as evidence, so a stale top row can be dropped if the character endpoint proves
the current character has moved below the cap or changed class. Duplicate
identities that are entirely below the retained cap are ignored because they
cannot become active current rows in the generated addon data.

Large realm datasets are split into ranked Lua chunk tranches. The refresh
tool chooses the chunk count dynamically from the total player count, targeting
roughly 3,000 players per chunk, with a 6 chunk minimum for normal realm
datasets and a 16 chunk cap. This currently keeps Onyxia and Lordaeron at 6
chunks while Icecrown uses 12 smaller chunks. Keep the chunked data layout and
validate with Lua 5.1 before handing off, because the Wrath client uses Lua 5.1
and can fail on syntax or chunk-size/local-limit issues that newer tooling
misses.

Boss payloads are split into realm-aware raid layer shard addons. These layer
shards stay aligned with the ranked player chunks so raising the in-game load
slider can load missing boss rows without a reload. To keep side raids smaller,
generated layers omit `player[8]` when the overall boss payload is identical to
the player's best-spec boss payload; the runtime derives that view from
`player[9][bestSpec]`.

The in-game UwU data load slider snaps to these generated chunk boundaries.
Skipped chunks must return before `local chunk = { ... }` so the Lua client does
not allocate disabled player tables after `/reload`.

## Runtime Memory Model

The bundled realm data and the browser runtime index are intentionally separate:

- Realm data is loaded from the current realm's LoadOnDemand player shards and
  enabled raid-layer shards. Loaded shard data remains resident until `/reload`;
  lowering the data-load slider or disabling raid layers saves memory after the
  next reload because skipped shards return before building their large tables.
- Tooltips do not build the player browser index. A tooltip lookup normalizes
  the hovered player name and reads one record directly from
  `coolstatsUwUData.players`.
- Opening the player browser builds disposable searchable/sortable row state
  from the already-loaded realm data. This can raise the Lua heap while the
  browser is open and working, especially while sorting or filtering.
- Repeated non-boss browser sorts may keep up to three sorted row-reference
  arrays on the disposable browser index. These arrays must not copy player
  records and must be released with the browser index on close.
- Closing the player browser must release the browser rows, active query cache,
  per-boss transient row fields, tooltip cache, and base browser index when no
  statistics panel is still using them. The browser index should return to
  `0` entries in `/coolstats perf` after close.
- Do not retain per-player boss-info caches for boss filters. Boss-mode rows
  may store only compact active-view fields such as score, ranks, DPS, and
  spec index; avoid retaining boss entry tables, boss names, or formatted spec
  strings on every row.
- Use small scheduled `collectgarbage("step", ...)` cleanup after heavy browser
  work or close. Do not add automatic full `collectgarbage("collect")` calls in
  gameplay, because full collection can cause visible freezes in the Wrath
  Lua 5.1 client.

When profiling memory, treat browser-open memory as working memory. The more
important health check is that closing the browser and waiting briefly releases
the browser index and that repeated open/filter/close cycles stabilize instead
of climbing indefinitely.

The character panel uses the same conservative rule: show/hide and first paint
remain full updates, while frequent stat, aura, resource, equipment, and
item-info events should route through dirty update categories so they refresh
only stat rows or gear/badge summaries when possible.

## Data Update Commands

Run these from the workspace root that contains `tools/` and
`coolstats_publish/`. Adjust paths if your checkout is elsewhere.

Validate configured leaderboard profiles without writing release data:

```powershell
.\tools\update_all_uwu_realms.ps1 -Mode Validate
```

Pull scores only for quick coverage checks:

```powershell
.\tools\update_all_uwu_realms.ps1 -Mode Scores -Realms Icecrown -MaxPerSpec 1500
.\tools\update_all_uwu_realms.ps1 -Mode Scores -Realms Lordaeron -MaxPerSpec 1000
```

Run full weekly pulls for the current realm-specific caps:

```powershell
.\tools\update_all_uwu_realms.ps1 -Mode Weekly
```

The wrapper defaults to Onyxia 600, Lordaeron 1,000, and Icecrown 1,500 per
class/spec. To run one realm manually:

```powershell
.\tools\update_all_uwu_realms.ps1 -Mode Weekly -Realms Onyxia
.\tools\update_all_uwu_realms.ps1 -Mode Weekly -Realms Icecrown -MaxPerSpec 1500
.\tools\update_all_uwu_realms.ps1 -Mode Weekly -Realms Lordaeron -MaxPerSpec 1000
```

For Onyxia ICC transition data, run one final TOGC refresh and keep the final
TOGC JSON snapshot as `coolstats_publish/data/uwu_logs_onyxia_toc.json` so it
can seed Phase 3 Overall:

```powershell
.\tools\update_all_uwu_realms.ps1 -Mode Weekly -Realms Onyxia -PhaseOverrides @{ Onyxia = "toc" } -ActivatePhase
```

Then build the intentional empty ICC handoff dataset:

```powershell
python .\tools\build_empty_onyxia_icc_dataset.py
```

Once UwU exposes usable Onyxia ICC and Toravon leaderboards, generate the
active dataset with:

```powershell
.\tools\update_all_uwu_realms.ps1 -Mode Weekly -Realms Onyxia -PhaseOverrides @{ Onyxia = "icc" } -ActivatePhase
```

The ICC generator profile intentionally has no retained boss list, so old
TOGC/Ulduar boss parses are filtered out instead of being carried into the new
phase. The offline empty handoff builder is the only intentional exception to
the "do not ship an empty configured boss leaderboard" guard.

Generated load-on-demand addon sources are written to:

```text
coolstats_publish/realm_data/coolstats_Data_Icecrown/
coolstats_publish/realm_data/coolstats_Data_Lordaeron/
coolstats_publish/realm_data/coolstats_Data_Onyxia/
```

The updater should refuse to overwrite output when:

- A weekly class/spec rankings request fails.
- The fresh active-player count is below the minimum.
- An entire configured boss leaderboard produces no rows.
- Onyxia ICC/Toravon leaderboards are still empty during a normal online ICC
  pull; use the explicit empty handoff builder instead of overwriting active
  data with a partially fetched online phase switch.

## Pre-Release Safety Checklist

Before installing to a live game folder, packaging, pushing, or publishing:

1. Confirm the data counts and failure fields in the generated JSON files.
   For ICC realms, verify `boss-only players added` is `0`.
2. Run the Warmane workspace/version guard:

   ```powershell
   powershell -NoProfile -ExecutionPolicy Bypass -File .\tools\validate_warmane_workspace.ps1
   ```

   This treats `coolstats_publish/` as canonical, verifies the `coolstats/`
   mirror, checks all release TOC versions, and runs Lua 5.1 validation for
   both trees by default.
3. Run phase-transition tests:

   ```powershell
   python -m pytest -q tools\test_uwu_phase_transition.py
   ```

4. Run Lua 5.1 validation:

   ```powershell
   powershell -NoProfile -ExecutionPolicy Bypass -File .\tools\validate_lua51.ps1 -PublishDirectory .\coolstats_publish
   ```

5. Package with release validation enabled:

   ```powershell
   powershell -NoProfile -ExecutionPolicy Bypass -File .\tools\package_coolstats_release.ps1 -Version <version> -PublishDirectory .\coolstats_publish -OutputDirectory .\coolstats_publish\releases
   ```

6. Install the exact packaged ZIP into a local game folder and verify the addon
   loads, tooltips work, the browser is populated, and the correct realm data
   addon version is shown.

   After installing to a live AddOns folder, re-run the workspace guard with
   `-LiveAddonsDirectory "<path-to-Interface\Addons>"` to verify the installed
   core, cache, and realm-data addon folders match the package.

The release packaging script lifts realm data addons out of `realm_data` and
places them beside the core `coolstats` addon inside the install-ready ZIP.

## Release Naming And Publishing

Use the manual release flow for this repository:

1. Bump all TOC versions.
2. Update `CHANGELOG.md`.
3. Create `coolstats_<version>.zip` with the packaging script.
4. Commit intended files explicitly. Include newly generated chunk files.
5. Push `main`.
6. Create and push tag `v<version>`.
7. Create or update the GitHub Release titled `coolstats <version>`.
8. Upload `coolstats_<version>.zip` as the release asset.
9. Regenerate and push the `warperia` branch from that exact ZIP. The GitHub
   repository default branch should stay set to `warperia`; source work and
   release commits stay on `main`.

### Warperia Launcher Branch

Warperia's GitHub launcher flow consumes the repository source tree from the
default branch. For `coolstats/coolstats`, the GitHub default branch should be
`warperia`. Do not make `main` the default branch for launcher installs, because
`main` is the source/release workspace and has root-level files such as
`coolstats.toc`, `cache_addon/`, `realm_data/`, and maintenance docs.

The `warperia` branch is a build artifact, not a second maintained codebase. It
is rebuilt from the same install-ready `coolstats_<version>.zip` that is uploaded
to GitHub Releases. Its repository root must contain sibling addon folders such
as `coolstats/`, `coolstats_Cache/`, each `coolstats_Data_<Realm>/`, and every
generated `coolstats_Data_*_UWU_*` player or raid-layer shard. It also has a
root `README.md` copied from the source branch README, with local asset paths
rewritten for the install-shaped branch. It must not contain root-level
source-workspace paths like `cache_addon/` or `realm_data/`.

After packaging and validating a release, run the Warperia branch publisher from
the workspace root:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File .\tools\publish_warperia_branch.ps1 -Version <version> -Push
```

If Warperia users report that only one `coolstats` folder was installed, inspect
the installed folder. Seeing `cache_addon/` or `realm_data/` inside it means the
launcher is still using the source branch; check that GitHub's default branch is
`warperia` and that the Warperia site has refreshed its GitHub metadata.

Prefer local `git` plus the GitHub UI or GitHub API for
`coolstats/coolstats`. Before pushing, tagging, or creating a release, verify
the release checkout remote points at `https://github.com/coolstats/coolstats.git`.
Run `tools\validate_coolstats_release_git.ps1` against the release checkout
before any commit, tag, push, or release upload. Normal release commits should
be made through `tools\commit_coolstats_release.ps1`, which pins author and
committer identity to `coolstats <coolstats@users.noreply.github.com>` and
refuses other remotes or reachable non-coolstats author identities. Do not use
a GitHub app/PR workflow for normal Warmane releases unless the release process
changes.

If the GitHub repository is deleted and recreated to purge stale GitHub object
cache, republish from a fresh clean-room import only:

1. Create the GitHub repository empty, without README, license, or `.gitignore`.
2. Copy the current `coolstats_publish/` tree into a new local directory, but
   do not copy any existing `.git` directory.
3. Initialize a new repository, set `origin` to
   `https://github.com/coolstats/coolstats.git`, and set local Git identity to
   `coolstats <coolstats@users.noreply.github.com>`.
4. Create one release commit and the current `v<version>` tag only.
5. Run the release git guard before pushing `main` and the tag.

Never push an old clone, mirrored repository, reflog, or stale scratch checkout
into a recreated GitHub repository.

Treat Git "dubious ownership" or "unsafe repository" warnings as release
blockers. Add explicit `safe.directory` entries for the exact repository roots
that Git reports, and do not use a wildcard safe-directory setting.

## Data Integrity Rules

- Keep player/spec data separated. Do not average or merge a player's parses
  across different specs.
- Keep locked Onyxia Phase 3 Overall data separate from current Phase 4 ICC
  data.
- Do not carry Phase 2 Overall, retained Algalon, Ulduar, TOGC boss rows, or
  Ruby Sanctum rows into the active Onyxia ICC dataset.
- Keep `coolstats_publish/data/uwu_logs_onyxia_toc.json` as the Phase 3
  historical source unless intentionally refreshing the final TOGC snapshot
  before ICC launch.
- Do not use TOGC achievement/statistics fallback on Onyxia legacy views unless
  Warmane's 10H/25H cross-crediting issue is proven fixed.
- Do not ship boss-only/rankless ICC players merely because they appear on a
  boss leaderboard.
- Do not co-mingle Warmane and Rising Gods generated data, scripts, or release
  artifacts.
