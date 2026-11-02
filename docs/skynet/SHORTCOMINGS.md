# SKYNET shortcomings tracker

One line per gap found in Overthrow. Source = read from code; Play = seen in play. Nothing here is fixed yet.
Status: OPEN until a commit on `skynet` closes it (note the hash).

| # | Area | Shortcoming | Found by | Where | Status |
|---|------|-------------|----------|-------|--------|
| 1 | Civilians | No visible relationship/trust meter. Convince is one permanent dice roll per civilian (shared by all players) on the Support skill, odds hidden, only a pass/fail hint | Source + play | `OVT_ConvertSupporterAction`, `OVT_ResistanceRequestComponent.RpcAsk_ConvertSupporter` | OPEN |
| 2 | Civilians | Recruit is a flat cash cost; ignores how the civilian feels | Source | `OVT_RecruitCivilianAction`, `OVT_RecruitRequestComponent` | OPEN |
| 3 | Economy | Drug sales pay cash only (1.25x buy price). No effect on trust, support, stability or heat | Source | `OVT_SellDrugsAction`, `OVT_EconomyRequestComponent.RpcAsk_SellDrugs` | OPEN |
| 4 | Buildings | No door lock/unlock system. Owning a building via Real Estate does not stop entry | Play + source | nothing exists; pattern to copy: `OVT_ToggleSitePrivacyAction` | OPEN |
| 5 | Storage | Only vehicles, ammo boxes and the warehouse building are stash holders. World crates and shelves are not; no player-placeable stash prefab | Source | `OVT_StorageComponent` (prefab bases) | OPEN |
| 6 | Wanted | Changing clothes does not clear or reduce a wanted level. `GetEffectiveWantedLevel()` is a stub ("may add later"). Level only steps down on timeout | Source | `OVT_PlayerWantedComponent` | OPEN |
| 7 | Wanted | Disguise only recognises the occupying faction's gear. No civilian-outfit rule or outfit filter | Source | `OVT_PlayerWantedComponent.CheckUpdate` | OPEN |
| 8 | Ranks | Officer is a bool, not a rank ladder | Source | `OVT_PlayerData.isOfficer`, `OVT_PlayerManagerComponent` | OPEN |
| 9 | Menu | U-menu buttons are fixed layout names bound in code; no data-driven list or scroll area for extra entries | Source | `OVT_MainMenuContext` | OPEN |
| 10 | Visuals | Hit indicators and blood are G-rated/near absent | Play | vanilla; candidate mods in notes, untested | OPEN |
| 11 | Setting | Town names and kit are French-style/US-vs-USSR; no East European fictional factions | Play | `Configs/Factions` | OPEN |
| 12 | Licence | README says MIT badge, `LICENSE.md` is APL-SA | Source | `README.md`, `LICENSE.md` | NOTE |

## Not shortcomings (checked)
- Containers/stockpiling exist (see README audit).
- Disguise itself is native and works at close range (15 m default).

## How to add one
Append a row, keep it one line, say how it was found, and name the file. Close a row with the commit hash.
