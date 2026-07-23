# stardew-mods

SMAPI update manifests for LawfulGremlin Stardew Valley mods. This is also the **hub repo** for the
whole Stardew mod/tooling ecosystem: it owns the shared CI workflow and hosts the GitHub Releases that
the other repos publish to and read from.

## Ecosystem

| Repo | Role | Depends on |
|---|---|---|
| **stardew-mods** (this repo) | Hub. Owns the shared `stardew-mod-release.yml` workflow, `mods/update-keys.json` (SMAPI update manifest), and the GitHub Releases that mod zips + `stardew-sync`'s tools get published to. | — |
| [`stardew-mods-boulderlogs`](https://github.com/LawfulGremlin/stardew-mods-boulderlogs) | *Landscaping* mod source. | stardew-mods (shared workflow), [`stardew-dll`](https://github.com/LawfulGremlin/stardew-dll) (CI compile refs) |
| [`stardew-mods-edgeindicator`](https://github.com/LawfulGremlin/stardew-mods-edgeindicator) | *Off-Screen Forage Indicators* mod source. | stardew-mods, `stardew-dll` |
| [`stardew-dll`](https://github.com/LawfulGremlin/stardew-dll) | Private Stardew/SMAPI reference DLLs, checked out into `ci/refs` for any mod's CI build. | — |
| [`stardew-sync`](https://github.com/LawfulGremlin/stardew-sync) | Player-facing installer: downloads SMAPI + managed mod zips from this repo's Releases and installs them. Also owns the "Stardew Sync Tools" release published here. | stardew-mods (releases), [`stardew-vm`](https://github.com/LawfulGremlin/stardew-vm) (VM install path) |
| [`stardew-vm`](https://github.com/LawfulGremlin/stardew-vm) | GPU-less dev-VM launch fix (Mesa software renderer). Consumed both directly and via `stardew-sync`'s VM install path. | — |

New mod repos should follow the pattern in `stardew-mods-boulderlogs`/`stardew-mods-edgeindicator`'s
READMEs ("Repository workflow" section): own the mod source + `manifest.json`, call this repo's shared
workflow from `build.yml`/`release.yml`, and rely on `stardew-dll` for CI reference assemblies.

## Reusable release workflow

Stardew mod repos can call `.github/workflows/stardew-mod-release.yml` to build on every push, publish a GitHub release on `v*` tags, and update `mods/update-keys.json`.

```yaml
name: build

on:
  push:
  workflow_dispatch:

permissions:
  contents: write

jobs:
  build:
    uses: LawfulGremlin/stardew-mods/.github/workflows/stardew-mod-release.yml@main
    with:
      project_path: Path/To/Mod.csproj
    secrets:
      DLL_REPO_TOKEN: ${{ secrets.DLL_REPO_TOKEN }}
      STARDEW_MODS_TOKEN: ${{ secrets.STARDEW_MODS_TOKEN }}
```
