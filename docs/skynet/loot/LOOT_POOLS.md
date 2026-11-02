# Vanilla loot pools

Source: the installed game's resource database (see `OBJECT_MATRIX.md`). Vanilla only.
Paths are relative to `Prefabs/`. `{a|b}` and `..` are shorthand for variants. Weights are starting guesses
for tuning (higher = more common). Dirty variants carry a lower weight so they read as worn clothing.

Roll shape (proposal): a container rolls N items from its pool by weight; clothes pools roll by SLOT
(top, bottom, jacket, hat) so a wardrobe never holds three pairs of pants. Matches the slot-and-choices
shape of `Configs/Civilians/CivilianClothes.conf`, which can be reused for the data format.

## CIV_CLOTHES (wardrobes, dressers)
Slot TOP (weight)
- `Characters/Uniforms/Shirt_CottonShirt_01/Shirt_CottonShirt_01_V1..V6.et` (10 each)
- same `_V1..V6_Dirty.et` (4 each)
- `Characters/Uniforms/Shirt_Turtleneck_01/Shirt_Turtleneck_01_{beige|DarkBlue|grey}.et` (8 each), `_Dirty` (3 each)

Slot JACKET (weight; roll skips this slot ~40%)
- `Characters/Uniforms/Jacket_Denim_01/Jacket_Denim_01_{brownShirt|strippedShirt|strippedShirt_dirty}.et` (8/8/3)
- `Characters/Uniforms/Jacket_Raincoat_01/Jacket_Raincoat_01_{green|orange|yellow}.et` (6)
- `Characters/Uniforms/Jacket_Suit_01/Jacket_Suit_01_{black_V1|Brown_V1|Grey_V1}.et` (5)

Slot BOTTOM
- `Characters/Uniforms/Pants_Trousers_01/Pants_Trousers_01_{DarkBrown|DarkGrey|LightBrown}.et` (10), `_DarkGrey_Dirty.et` (3)
- `Characters/Uniforms/Pants_Suit_01/Pants_Suit_01_{black|brown|grey}_V1.et` (5)
- `Characters/Uniforms/Pants_Fisherman_01/Pants_Fisherman_01_{green|orange|yellow}.et` (3)

Slot HAT (roll skips ~50%)
- `Characters/HeadGear/Hat_FlatCap_01/Hat_FlatCap_01_{blue|darkgrey|lightgrey}.et` (+ `_Dirty`) (6/2)
- `Characters/HeadGear/Hat_Knitted_01/Hat_Knitted_01_{black|blue|brown|red}.et` (6)
- `Characters/HeadGear/Hat_TruckerCap_01/Hat_TruckerCap_01_{alsworth|enp|everon|fkdynamo|moto|pohona|sawmillregina}.et` (4)
- `Characters/HeadGear/Hat_Hard_01/Hat_Hard_01_{blue|green|white|yellow}.et` (2, workwear)
- `Characters/HeadGear/Hat_Ushanka_01/Hat_Ushanka_01_{folded|unfolded}.et` (4)
- `Characters/HeadGear/Hat_Panama_Soviet/Hat_Panama_Soviet_01_{v1|v2}.et` (2)

Slot HANDS (rare)
- `Characters/Handwear/Gloves_Wool_01/Gloves_Wool_01.et` (6), `Gloves_Leather_01/Gloves_Leather_01.et` (4)

Gap: no civilian footwear exists in vanilla (only military boots: see FOOTWEAR). Civilians in this build wear
the same boots, so wardrobes either skip shoes or reuse CombatBoots_Soviet_01/_Dirty.

## FOOTWEAR (shoe cabinets, wardrobes low weight)
- `Characters/Footwear/CombatBoots_Soviet_01.et` (6), `CombatBoots_Soviet_01_Dirty.et` (6)
- `Characters/Footwear/CombatBoots_US_01.et` (3), `JungleBoots_01/JungleBoots_01.et` (2), `TankerBoots_US_01/TankerBoots_US_01.et` (1)

## MIL_UNIFORMS (military crates, wardrobes in barracks, rare in homes)
Jacket: `Characters/Uniforms/Jacket_{M88|M88_rolledup|M88_TTsKO|M88_rolledup_TTsKO|M70|KZS/Jacket_KZS|TAZ83/Jacket_TAZ83_01|US_BDU|US_BDU_02|US_BDU_rolledup|US_BDU_rolledup_02|US_BDU_TigerStripe|US_BDU_rolledup_TigerStripe}.et`
Pants: `Characters/Uniforms/Pants_{M88|M88_TTsKO|M70|M70_Dirty|KZS/Pants_KZS|TAZ83/Pants_TAZ83_01|US_BDU|US_BDU_TigerStripe|US_M65}.et`
Specialist (weight 1): `Suit_Pilot_US_01`, `Suit_Tanker_US_01{|_tucked}`, `Jacket_Pilot_USSR_01`, `Jacket_Tanker_USSR_01`, `Pants_Pilot_USSR_01`, `Pants_Tanker_USSR_01`
Headgear: `Characters/HeadGear/` Helmet_{M1_01|PASGT_01|SSh68_01|SPH4_01|TSh4_01|ZSh5_01|DH132_01}, Hat_{M88_01|M70_01|Patrol_US_01|Boonie_US_01|RadiovkaBeret_01|ZmijovkaCap_01|PeakedCap_USSR_01}
Vests (weight low; only from military crates): `Characters/Vests/` Vest_{ALICE|SovietHarness|Lifchik|M69|PASGT|6B2|6B3|NAZ|Type56|SRU21P|M79GrenadeCarrier}
Packs: `Items/Equipment/Backpacks/` Backpack_{ALICE_Medium|Kolobok|M70_Swiss|Veshmeshok|IIFS_FieldPack|Medical_M5|Medical_Soviet}

Note: do NOT hand out wanted-flagging gear (occupier uniforms, helmets) from civilian homes. In this setting a
uniform is a disguise item, see the wanted-system rows in `SHORTCOMINGS.md` (6, 7, 13-15). Keep MIL pools on military containers.

## SUPPLIES_HOME (cupboards, shelves, chests, nightstands)
- Food: `Items/Food/{ArmyCrackers_Soviet_01|Meat_01_Dry|MeatLeg_01|MRE}.et` (MRE 2, crackers 6, dry meat 5)
- Medicine: `Items/Medicine/{AspirinBottle|FieldDressing_01/Gauze_01|Gauze_02/Gauze_02|FieldDressing_01/FieldDressing_US_01|FieldDressing_USSR_01}.et` (aspirin 6, gauze 6, dressing 3)
- Misc: `Items/Equipment/{Flashlights/Flashlight_Soviet_01/Flashlight_Soviet_01|Watches/Watch_{Vostok|Orlik38|SandY184A}|Canteens/Canteen_{Soviet_01|US_01}|Maps/Map_Paper_01/PaperMap_01_folded}.et`
- `Items/PersonalBelongings/PersonalBelongings_CIV.et` (a ready-made civilian items bundle; check what it holds)

## SUPPLIES_WORK (metal cabinets, crates)
- `Items/Fuel/Jerrycan_01/Jerrycan_01_item.et`, `Items/Equipment/Kits/RepairKit_01/RepairKit_01_wrench.et`
- `Items/Equipment/Flashlights/Flashlight_MX991/Flashlight_MX991.et`, Compass_{Adrianov|SY183}, Binoculars_{B8|B12|M22}
- `Items/Equipment/Radios/{Radio_R148|Radio_R107M|Radio_RF10}.et` (rare)

## MEDICAL (medical cabinets)
- `Items/Medicine/{Tourniquet_01/Tourniquet_US_01|Tourniquet_USSR_01|SalineBag_01/SalineBag_US_01|SalineBag_USSR_01|MorphineInjection_01/MorphineInjection_01|FieldDressing_01/FieldDressing_US_01|FieldDressing_USSR_01|AspirinBottle}.et`
- `Items/Equipment/Kits/MedicalKit_01/{MedicalKit_01_FIA|MedicalKit_01_US|MedicalKit_01_USSR}.et` (rare)

## SUPPLIES_SHOP (shop shelves, shop crates)
- Mix of SUPPLIES_HOME food and medicine plus Watches, Canteens, Flashlights, Maps, Compass at higher counts.

## PAPERS (bookshelves, card files)
- `Items/Equipment/Maps/Map_Paper_01/{PaperMap_01_folded|PaperMap_01_folded_FIA|PaperMap_01_folded_US|PaperMap_01_folded_USSR}.et`, `Map_Paper_02_unfolded`
- `Items/Misc/Caches/Campaign_CacheNote_FIA.et` (rare; tied to the campaign cache system, check before using)

## GUNS (very low weight in homes; common only in military containers)
Handguns (home-safe, weight 3): `Weapons/Handguns/PM/Handgun_PM.et` (+ `Magazines/Magazine_9x18_PM_8rnd_Ball.et`), `Weapons/Handguns/M9/Handgun_M9.et` (+ `Magazine_9x19_M9_15rnd_Ball.et`)
Rifles (military crates): `Weapons/Rifles/AK74/Rifle_{AK74|AK74N}.et`, `AKS74U/Rifle_{AKS74U|AKS74UN}.et`, `M16/Rifle_{M16A2|M16A2_carbine}.et`, `M14/Rifle_M21.et`, `SVD/Rifle_SVD.et`, `VZ58/Rifle_{VZ58P|VZ58V}.et`
MG (rare, military only): `MachineGuns/{RPK74/MG_RPK74|PKM/MG_PKM|M60/MG_M60|M249/MG_M249|UK59/MG_UK59}.et`
Launchers (very rare): `Launchers/{RPG7/Launcher_RPG7|RPG22|M72/Launcher_M72A3|RPG75}.et`
Grenades: `Grenades/{Grenade_M67|Grenade_RGD5|Smoke_M18_Green|Smoke_ANM8HC|Smoke_RDG2}.et`
Mags to pair: `Weapons/Magazines/Magazine_545x39_AK_30rnd_Ball.et`, `Magazine_556x45_STANAG_30rnd_M855_Ball.et` / `M193_Ball`, `Magazine_762x51_M14_20rnd_M80.et`, `Magazine_762x54_SVD_10rnd_Sniper.et`, `Magazine_545x39_RPK_45rnd_Ball.et`, `Magazine_9x18_PM_8rnd_Ball.et`, `Magazine_9x19_M9_15rnd_Ball.et`
Rule: a gun roll always also rolls 1-3 matching magazines (use the table above), so a found weapon is usable.

## Open items
- Verify each path resolves (names come from the database, but a typo in shorthand would not).
- GUIDs: configs need `{GUID}Path`; Workbench fills them in when the file is saved from the config editor.
- Which of these Overthrow's economy already prices (a looted item with no price may break selling).
