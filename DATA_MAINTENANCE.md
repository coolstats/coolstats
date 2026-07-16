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

- **Onyxia:** current Phase 3 Trial of the Grand Crusader 25 heroic bosses and
  Koralon. Phase 2 Overall score/rank and the locked historical Algalon parse
  remain as previous-season indicators.
- **Icecrown:** ICC-profile data covering Icecrown Citadel, Halion, and
  Anub'arak. Vault of Archavon bosses are intentionally excluded.
- **Lordaeron:** ICC-profile data covering Icecrown Citadel, Halion, and
  Anub'arak. Vault of Archavon bosses are intentionally excluded.

Onyxia, Icecrown, and Lordaeron data is generated as separate load-on-demand
addons. Only the dataset matching the player's current realm is loaded in-game.

Onyxia's deprecated Ulduar boss rows are not shipped in the current dataset;
only Algalon remains as a historical boss indicator. Onyxia's Lair is excluded
until it is present in the official player logs.

## Current Caps And Size Guardrails

As of `0.2.29`:

- **Icecrown:** top 1,500 ranked players per class/specialization, resulting in
  33,804 current ranked players.
- **Lordaeron:** top 1,000 ranked players per class/specialization, resulting
  in 14,982 current ranked players.
- **Onyxia:** TOGC phase dataset with 5,340 current active players, 13,605
  total players after retained history and TOGC boss-only coverage, Phase 2
  Overall, and locked Algalon history retained.

For ICC realms, boss-only/rankless leaderboard players must not be added to the
shipped dataset. Boss parses should be attached only to players already present
in the current ranked ICC coverage. This prevents the oversized rankless-player
dataset issue seen in `0.2.24`.

Large realm datasets are split into multiple Lua chunks. Keep the chunked data
layout and validate with Lua 5.1 before handing off, because the Wrath client
uses Lua 5.1 and can fail on syntax or chunk-size/local-limit issues that newer
tooling misses.

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

Run full weekly pulls for the current ICC realm caps:

```powershell
.\tools\update_all_uwu_realms.ps1 -Mode Weekly -Realms Icecrown -MaxPerSpec 1500
.\tools\update_all_uwu_realms.ps1 -Mode Weekly -Realms Lordaeron -MaxPerSpec 1000
```

For Onyxia phase data, keep the TOGC profile active and preserve Phase 2 Overall
plus Algalon history. Do not remove or recompute the locked Algalon historical
records until Onyxia moves to the next phase where that history is no longer
needed.

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
- Historical Onyxia Algalon records change or disappear unexpectedly.

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
9. Upload the same install-ready ZIP to Warperia.

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
- Keep Onyxia Phase 2 Overall data separate from current Phase 3 TOGC data.
- Keep Onyxia historical Algalon immutable until the next planned phase change.
- Do not use TOGC achievement/statistics fallback on Onyxia unless Warmane's
  10H/25H cross-crediting issue is proven fixed.
- Do not ship boss-only/rankless ICC players merely because they appear on a
  boss leaderboard.
- Do not co-mingle Warmane and Rising Gods generated data, scripts, or release
  artifacts.
