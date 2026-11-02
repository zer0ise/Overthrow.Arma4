# Searchable furniture: design

Status: DESIGN ONLY. Nothing built. Assumptions are marked CHECK and must be verified in Workbench or play.

## Player experience
1. Look at a wardrobe in a building. Prompt reads **Search** (not "Open").
2. Hold the interact key for **5 seconds**. A progress ring/spinner shows while holding. Moving away cancels.
3. On completion the server rolls the loot, fills the container, then the normal inventory screen opens on it.
4. Searching in a place that is not yours, in view of a witness, raises wanted (see Witnesses).

## Building blocks that already exist
| Need | What exists | Source |
|---|---|---|
| Open a container's inventory | `OVT_OpenStorageAction` calls `manager.SetStorageToOpen(entity)` then `manager.OpenInventory()` | `UserActions/OVT_OpenStorageAction.c` |
| Hold-to-perform with progress | Actions carry a `Duration` (seconds) set per prefab instance; the engine draws the hold progress. Existing Overthrow actions use 2 s | prefab `.et` files, `OVT_BuyEquippedRecruitAction` header |
| Rename the prompt | `GetActionNameScript(out string outName)` override | e.g. `OVT_SellDrugsAction`, `OVT_RecruitCivilianAction` |
| Loot hook | vanilla `SCR_LootAction` is modded to fire `m_OnPlayerLoot` | `UserActions/Modded/SCR_LootAction.c` |
| Wanted check on loot | `OVT_PlayerWantedComponent.OnPlayerLoot` | `Components/Player/` |
| Random slot-and-choice data | `OVT_LoadoutConfig` / `OVT_LoadoutSlot` with `m_aChoices`, `m_fSkipChance` | `Configs/Civilians/CivilianClothes.conf` |
| Server-authoritative request pattern | `*RequestComponent` classes with `RpcAsk_*` | `Components/Controller/` |
| Whether a building is yours | `OVT_RealEstateManagerComponent.IsOwned(id)` | `GameMode/Managers/` |

The 5 s spinner is therefore mostly free: set `Duration 5` on the action. A custom spinning-circle widget is
only needed if the stock hold ring is not what you want (CHECK in play: Overthrow's 2 s actions show a ring).

## New pieces
1. **Search action** (script): `OVT_SearchContainerAction`. `GetActionNameScript` returns `#OVT-Search`.
   `PerformAction` (runs after the hold completes) sends one request to the server, then waits for the fill.
2. **Server request**: `OVT_LootRequestComponent.RpcAsk_SearchContainer(RplId container)`. Server checks
   distance (10 m, same as convert), a per-player rate limit, and that the container is not already searched.
3. **Loot table config** (`Configs/Loot/*.conf`): pool name -> entries `{ResourceName, weight, min, max}` plus
   per-container-type roll counts. Data only, so tuning needs no script change.
4. **Filler**: rolls the pool and inserts items into the container's inventory storage. First fill happens on
   first search, not at world load (cheap, and means nothing is spawned for houses never visited).
5. **Searched state**: per container: never searched / searched at time T. Decides refill (see Open questions).
6. **Prefab overrides** (Workbench, yours): add the searchable action and a storage component to
   `Wardrobe_01`, `Wardrobe_02`. Scripts cannot add components at runtime (confirmed in the storage
   component's own comments), so this is the one non-script step. Override the plain prefabs and every
   placed wardrobe in the world follows.

## CHECK first: does a vanilla wardrobe already have an inventory?
Open `Prefabs/Props/Furniture/Wardrobe_01.et` in Workbench. If it has no storage component, add a
`UniversalInventoryStorageComponent` sized for roughly 8-12 slots. If it has one, only the action is needed.
This single check decides how much prefab work phase 1 is.

## Witnesses
Today (source): only occupying-faction AI can see the player; loot raises wanted only `if(m_bIsSeen)`.
Wanted: any neutral NPC or enemy who sees you search a container NOT on your property raises wanted.
- Property test: `RealEstate.IsOwned(building)` on the building holding the container, plus your own
  camps/FOBs/placed objects. Unowned = theft.
- Witness test: extend the existing line-of-sight sweep (`CheckDisguisedAsOccupying` style: AI agents within
  range, `TraceLOS`) to also accept civilians and hostile factions, for this event only. Do not change who can
  spot you in general; that would affect all stealth.
- Civilian numbers (source): `Configs/Civilians/CivilianAmbience.conf` spawns 0.1 civilians per head of town
  population, min 2, max 40 per town; 40% placed at doorways/POIs, the rest on roads. There is no time-of-day
  logic. So a typical village (pop 38) has about 4 civilians, a city (pop 283) about 28. Most houses have no
  one in them, so witnesses will be rare indoors and common on streets. That is probably the right feel.
- Consequence tiers (proposal): witnessed by a civilian = wanted level 1 plus the civilian flees or reports;
  witnessed by occupier = level 2 as today.

## Persistence
CHECK: container contents live in the engine inventory (vanilla persistence) or the Overthrow ledger; not yet
traced. If not saved, a reload refills every container, which is an exploit. Trace `Persistence/` before
shipping phase 1. Safest rule: persist only a per-container "searched" bit plus the leftover items.

## Build order
1. CHECK wardrobe storage in Workbench (you).
2. Loot table config + CIV_CLOTHES pool (data only).
3. Search action with `Duration 5` and the renamed prompt, wired to a server request and filler.
4. Wardrobe prefab overrides.
5. Playtest: fill, reload, refill behaviour.
6. Wanted/witness rule (row 20) after the loop works without it.
7. More object types and pools from `OBJECT_MATRIX.md`.

## Open questions for you
- Refill: never, after N in-game days, or only when a town's stability recovers?
- Does a searched, emptied wardrobe stay visibly "searched" (e.g. door ajar)? Needs prefab animation; skip for now.
- Should the player hear/see a search noise that alerts nearby AI (cheap extra tension)?
