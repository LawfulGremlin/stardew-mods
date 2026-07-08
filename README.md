# stardew-mods

SMAPI update manifests for LawfulGremlin Stardew Valley mods.

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
