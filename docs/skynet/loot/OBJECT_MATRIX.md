# Vanilla object matrix (searchable world objects)

Source: the installed game's `resourceDatabase.rdb` (19,439 prefab paths; full list saved on the PC at
`C:\Mod Storage\Reforger\vanilla-prefab-paths.txt`). Vanilla only; modded objects ignored for now.
Paths are relative to the game data root. GUIDs are NOT resolved (Workbench does that on save).

Each prefab family below has an editable twin under `PrefabsEditable/Auto/...` with an `E_` prefix.
Those are the ones placed by the editor/Game Master. World buildings place the plain `Prefabs/...` versions.

Columns: **Tier** = how rich the loot is. **Pool** = which pool in `LOOT_POOLS.md`.
**Storage today** = unverified until a prefab is opened in Workbench; "unknown" is the honest default.

## Phase 1: wardrobes (decided)
| Family | Prefabs | Pool | Tier | Storage today |
|---|---|---|---|---|
| Wardrobe | `Prefabs/Props/Furniture/Wardrobe_01.et`, `Wardrobe_02.et` | CIV_CLOTHES | low | unknown |

## Phase 2 candidates (home/civilian)
| Family | Prefabs | Pool | Tier | Storage today |
|---|---|---|---|---|
| Dresser | `Props/Furniture/Dresser_01.et` | CIV_CLOTHES (small) | low | unknown |
| Chest | `Props/Furniture/Chest_01.et` | SUPPLIES_HOME | low-mid | unknown |
| Nightstand | `Props/Furniture/Nightstand_01/_02.et` | SUPPLIES_HOME (tiny) | low | unknown |
| Shoe cabinet | `Props/Furniture/ShoeCabinet_01.et`, `ShoeCabinetOld_01.et` | FOOTWEAR | low | unknown |
| Cupboard | `Props/Furniture/Cupboard_01/Cupboard_01_A..F.et` | SUPPLIES_HOME | low | unknown |
| Old cupboard | `Props/Furniture/CupboardOld_01/_small,_medium,_large.et` | SUPPLIES_HOME | low | unknown |
| Bookshelf | `Props/Furniture/Bookshelf_01/Bookshelf_01.et`, `Bookshelf_02`, `Bookshelf_USSR_01.et` | PAPERS (maps, notes) | low | unknown |
| Metal cabinet | `Props/Furniture/CabinetMetal_01_grey_V1..V3, _olive_V1..V3, CabinetMetal_02.et` | SUPPLIES_WORK | mid | Overthrow already adds a stash component to `_grey_V1` |
| Shelves | `Props/Furniture/Shelf_01.et`, `Shelf_01_long`, `ShelfOld_01`, `ShelfOld_01_small`, `ShelfDecorative_01_*` | SUPPLIES_HOME | low | unknown |
| Card file cabinet | `Props/Commercial/CabinetCardFile_01/CabinetCardFile_01_green/_grey.et` | PAPERS | low | unknown |

## Phase 3 candidates (commercial / medical / work)
| Family | Prefabs | Pool | Tier | Storage today |
|---|---|---|---|---|
| Medical cabinet | `Props/Services/Healthcare/CabinetMedical_01.et`, `_single.et` | MEDICAL | mid | unknown |
| Shop shelves | `Props/Commercial/ShelfShop_01.et`, `ShelfShop_02/*`, `ShelfUniversal_01_*` | SUPPLIES_SHOP | mid | unknown |
| Crates (shop) | `Props/Crates/Crate_01_brown..yellow.et`, `CrateWooden_01_plain/_blue.et`, `BoxWooden_01.et` | SUPPLIES_WORK | low-mid | unknown |
| Cardboard boxes | `Props/Crates/CardboardBox/CardboardBox_01..09.et` | SUPPLIES_HOME | low | unknown |
| Kindling crate | `Props/Civilian/WoodKindling_Crate_01.et` | none (fuel/flavour) | low | unknown |
| Electrical cabinets | `Structures/Infrastructure/Power/Utility/ElectricCabinet_01*.et`, `UtilityCabinet_02.et` | TOOLS (rare) | low | unknown |

## Phase 4 candidates (military: handle with care, guns live here)
| Family | Prefabs | Pool | Tier | Storage today |
|---|---|---|---|---|
| Weapon rack cabinet | `Props/Military/Arsenal/WeaponRacks/WeaponRackCabinetArsenal_FIA/_US/_USSR.et`, `Props/Military/Camps/WeaponRackCabinet_01.et` | GUNS | high | arsenal components (player arsenal, do not reuse as loot) |
| Supply crate | `Props/Military/SupplyBox/SupplyCrate/SupplyCrate_01/SupplyCrate_01_storage.et` | SUPPLIES_MIL | high | has storage |
| Military crate stack | `Props/Military/MilitaryCrates/CrateStack_01/*` | AMMO / SUPPLIES_MIL | mid-high | unknown |
| Cluttered crates | `Compositions/Misc/SubCompositions/CrateWooden_Cluttered_US_01..03, _USSR_01..02, CrateWooden_Ammo_USSR_01.et` | SUPPLIES_MIL | mid | compositions (children) |
| Furniture compositions | `Compositions/Misc/SubCompositions/Furniture/Cabinet_Food_US_01, Cabinet_MRE_US_01, Cabinet_Medical_FIA_01, Cabinet_Radio_US_01/02, Cabinet_Books_USSR_01, Cabinet_Folders_US_01, Cabinet_Ashtray_US_01` | mixed by name | mid | compositions |

## Notes
- Compositions are groups of child prefabs, so storage would sit on children; do not treat as single holders.
- Destructible variants (`*_Dst`, `*_dst`) and `*_base` prefabs are excluded: base prefabs are inheritance
  parents, and a component added to a base appears on every child. That is also the mechanism for
  "one override covers all wardrobes": override `Wardrobe_01`/`Wardrobe_02` once each.
- Unknown column must be filled by opening each prefab in Workbench (ground truth), a short manual pass.
- Deliberately excluded from loot for now: arsenal racks (player arsenal), mortar/launcher heavy ordnance,
  mines, detonators, vehicle weapons, tripods, warheads.
