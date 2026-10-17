# SWGR: Custom Player Character Import

Goal: export a SWTOR character with the
[SWTOR Model Viewer](https://github.com/joystick1898/SWTOR-Model-Viewer) and
import it into **STAR WARS Galactic Racer (PC)** as the campaign player character.

Status: **planning and reference only.** Nothing in this folder has been run
against the game yet. See [Open questions](#open-questions).

## Reference points

- Another modder already replaced the default outfit and helmet with the
  Scout Trooper from SWBF2 (2017). Their package is documented in
  [reference/scout-trooper-mod.md](reference/scout-trooper-mod.md). It is a
  pak-level override of the existing player character assets, which suggests
  the game accepts replaced character assets without code changes (not yet
  verified for a custom import).
- SWTOR character export is established. The viewer already produces textured
  FBX files, a skeleton file, and a `paths.json` package for ZG Tools.

## Pipeline

| Stage | Input | Output | Tooling | Status |
|---|---|---|---|---|
| 1. Export | SWTOR character (Model Viewer) | Posed-in-rest FBX, textures, `skeleton.json` | SWTOR Model Viewer (Windows) | Known to work for SWTOR, not yet tested for this project |
| 2. Retarget | SWTOR FBX + SWGR skeleton | FBX with bones renamed/mapped to SWGR's skeleton | Blender (bundled with the viewer, or 4.3.2) | Not started. Needs the SWGR skeleton's bone list |
| 3. Import | Retargeted FBX | `SKM_*` skeletal meshes and materials in an Unreal project | Unreal Editor matching SWGR's engine version | Not started. Engine version unknown |
| 4. Cook & package | Unreal project | `_P` pak / ucas / utoc using the **same asset paths** the game loads | UE cook + IoStore packager (e.g. UnrealPak / retoc) | Not started |
| 5. Test | Packaged mod in `Griffin\Content\Paks\` | Character appears in customization, walking, and racing | Game on Windows | Not started |

Stage 4 is the crux. The override approach only works if the mesh, skeleton
binding, and material names match what the game expects.

## Repo layout

```
SWGR/
  README.md                       this file
  reference/
    scout-trooper-mod.md          analysis of the SWBF2 Scout Trooper package
```

Game assets, extracted resources, and third-party mod archives are not committed
here. Tooling and documentation only.

## Open questions

1. **Engine version.** Which Unreal Engine version SWGR ships on. This decides
   which editor Stage 3 needs. Check the game's `.uasset` package headers or the
   engine version the game reports.
2. **Player character identity.** The Scout Trooper mod overrides
   `Characters/Tomato01/`. That is probably the campaign player character, but
   confirm it in game before building anything on it.
3. **Skeleton.** The mod's package has no skeleton asset, so it reuses the
   game's. Extracting that skeleton's bone names is the first real task for
   Stage 2.
4. **Access.** Stages 3 to 5 need Windows, the installed game, and an Unreal
   Editor of the matching version. This sandbox is Linux and has none of these.

## Legal and scope

SWGR, SWTOR, and Star Wars assets belong to their respective rights holders. The
viewer's license does not grant rights to game content. This project is for
personal modding. Redistributing extracted game assets is out of scope for this
repo.
