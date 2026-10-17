# Scout Trooper player mod: analysis

Source: `ScoutTrooper_MaggVoren` archive, Version 1.0, by MaggVoren. Analyzed
from the archive listing and the package's directory index. The archive itself
is not committed here.

## What it is

A visual replacement for the default player outfit and helmet in STAR WARS
Galactic Racer (PC). The README says the replacement covers character
customization, walking, and racing, including boost. Voices, handling, and
progression are unchanged.

Credits in the package: model and textures from EA DICE's SWBF2 (2017),
model rip by Stym, port and game adaptation by MaggVoren.

## Package layout

```
Griffin/Content/Paks/
  ScoutTrooper_MaggVoren_P.pak    339 bytes   (IoStore container header)
  ScoutTrooper_MaggVoren_P.ucas   ~20 MB      (asset data)
  ScoutTrooper_MaggVoren_P.utoc   12 KB       (table of contents)
```

Install: copy the `Griffin` folder next to `SWGR.exe`. The three files must stay
together. Use the `_P` suffix so the package loads as a patch over the base game.

"Griffin" is the game's internal project name. Assets live under
`Griffin/Content/...`.

## Overridden asset paths

The index mounts at `../../../Griffin/Content/Griffin/Characters/Tomato01/`.
Tomato01 is the internal name of the default character that this mod replaces.
Asset names found in the index:

Skeletal meshes
- `SKM_Tomato01_Head_01`
- `SKM_Tomato01_Outfit_01`
- `SKM_Tomato01_Outfit_01_Cloak`

Materials
- `M_ScoutTechnicalInvisible`

Textures (`T_` prefix; `.uasset` headers, `.ubulk` pixel data)
- Head 01: `_D_Tintable`, `_N`, `_ORM`, `_UD`, `_UP`, `_U`
- Head 03: `_D`, `_N`, `_ORM`
- Outfit 01: `_D_Tintable`, `_N`, `_ORM`, `_UC`, `_UP`, `_U`

Folder names seen in the index: `Textures`, `ScoutTrooper`, `SkeletalMeshes`,
`Outfits`. Their exact mapping to assets was not verified.

The index has 30 names in total, with no skeleton, animation, or Blueprint
entries. The package therefore appears to reuse the game's own skeleton and
animation set. Any custom import must match that skeleton (not verified in
game).

## Implications for SWGR import

1. **Same paths, same names.** A custom character can ship as a pak with the
   same `SKM_Tomato01_*` names and get picked up without code changes.
2. **Skeleton match.** The FBX from SWTOR must be retargeted to the game's
   Tomato01 skeleton before import. The SWTOR skeleton will not match by name.
3. **Material slots.** The package includes `M_ScoutTechnicalInvisible`. A
   custom import should keep the material slot names and count the game's
   meshes use, since the mod replaces those meshes in place. Confirm this by
   inspecting the game's original meshes.
4. **Texture naming.** Keep the `_D`, `_N`, `_ORM` suffix convention used by
   the mod. `_D_Tintable` appears on the outfit and head 01 textures, which
   suggests a tintable color path. What it drives in game is unverified.
5. **Engine version.** The package format (IoStore `.utoc`/`.ucas`) is a UE4/UE5
   format. The exact engine version is not established from this archive.

## Limits of this analysis

- Only the directory index was inspected. Asset contents were not decoded.
- The package was not loaded in the game, and no UE tool was run on it.
- The strings-based listing confirms names, not folder structure or types.
