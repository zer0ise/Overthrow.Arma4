# SKYNET private patch layer

Private fork of Overthrow.Arma4 for a persistent-warfare setup (East European conflict, fictional factions,
Red v Blue, difficulty notches). Not for upstream. Licence is APL-SA (see `LICENSE.md`): non-commercial,
Arma-only, share-alike. The README MIT badge disagrees with it; treat `LICENSE.md` as authoritative.

## Git workflow
- `main` tracks `origin/main` (the fork), `upstream` is ArmaOverthrow/Overthrow.Arma4.
- All work lives on `skynet`. Small, separate commits so each patch can be dropped or re-applied.
- After an upstream or Reforger update: `git fetch upstream && git rebase upstream/main` on `skynet`.
- Pin the Reforger build and Overthrow commit that were last tested together (log below).
- Prefer `modded class` in a separate overlay over editing upstream files. Edit upstream files only when a
  layout or config has no hook.

## Tested pin
- Overthrow base commit: 9dbb8fa (merge of PR #177)
- Reforger build: 1.8.0.13 (not yet tested against this fork)

## System audit: civilian interaction (read from source, not yet played)

There is NO hidden per-civilian relationship meter. What exists:

| Action | File | What it really does |
|---|---|---|
| Convince (`OVT_ConvertSupporterAction`) | `UserActions/`, server in `OVT_ResistanceRequestComponent.RpcAsk_ConvertSupporter` | One roll per civilian, ever, across ALL players (`TryMarkCivilianConvertAttempted`). Success chance = `player.diplomacy` (starts 0.1, set by the Support skill via `OVT_SupportSkillEffect`). Success adds +1 support to the nearest town. Result is only a hint. |
| Recruit (`OVT_RecruitCivilianAction`) | `UserActions/`, server in `OVT_RecruitRequestComponent` | Flat money cost `Difficulty.baseRecruitCost`, capped by `CanRecruit` limit. No trust or relationship input. |
| Sell drugs (`OVT_SellDrugsAction`) | `UserActions/`, server in `OVT_EconomyRequestComponent.RpcAsk_SellDrugs` | Removes one `DrugsWeed_01` item, pays buy price x 1.25. 25% chance the dealer stays usable. Touches no support, stability or relationship value. |

So the player's guess is right that it is unclear: the three actions are unconnected. Drugs do not feed
convince or recruit. Convince is a once-only dice roll on a skill stat, with no feedback on the odds.

### Gaps (candidates for the patch layer)
1. No per-civilian state. A failed convince is permanent for that civilian, for everyone.
2. Success odds are invisible to the player (no chance shown, only a pass/fail hint).
3. Drug sales have no consequence or benefit beyond cash (no heat, no stability effect, no trust).
4. Recruit ignores how the civilian feels about the resistance.
5. Officer is a bool (`OVT_PlayerData.isOfficer`), not a rank.

### Candidate design (not built)
A small per-civilian or per-town trust value that convince, recruit and drug sales all read and write,
shown in the interaction prompt. Needs a persistence decision first (town-level fits the existing
Population/Stability/Supporters model and its save path; per-civilian does not).

## Menu
`OVT_MainMenuContext` binds fixed layout buttons by name. Extra entries need a layout change plus code,
or a `modded class OVT_MainMenuContext` in an overlay mod. Not started.

## Open questions
- How the start menu builds its faction list, and whether a Red v Blue pair is config-only.
- Whether civilian state survives save/load (check `Persistence/` before adding any state).
