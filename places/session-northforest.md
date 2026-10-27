# Code map: Session(NorthForest) (placeId 97441436334476)

Where things live, for quick navigation. ~185 live scripts (plus backups in ServerStorage).

## ReplicatedStorage: data modules (tune here)
| Module | Holds |
|---|---|
| `ClassDefinitions` | Classes → weaponType, tier, class weapon, anyWeapon |
| `ClassBaseStats` | Per-class base stats |
| `ClassAbilities` | E/Q/Passive/R data for every class (the biggest module) |
| `ClassAnimations`, `DodgeAnimations`, `ClassPresentation` | Animations, pick pose, essence colours |
| `WeaponDefinitions` | Weapons → weaponType, isDefault, tier |
| `WeaponMoves` | M1 combo data per weapon |
| `WeaponSkills` | F skills per weapon |
| `LevelDefinitions` | Level bonuses, affinity stats, stat growth, EXP curve |
| `CoreStats`, `UniqueStats`, `StatUnits`, `DamageTypes` | Stat formulas and constants |
| `StatusEffects` | Status effect identity data |
| `EnemyDefinitions` | Enemy HP/damage/EXP/stagger/resistances, level and party scaling |
| `ItemDefinitions` | Charms (stats, passives, tiers, prices, recipes, lore text) |
| `SmallInventoryItemDefinitions`, `CraftingRecipes` | Materials, edibles, crafting lookups |
| `NPCDefinitions` | NPC idle animations + dialogue trees |
| `SoundEffects` | Every sound: volume, speed, variants, reverb/echo/eq, fades |
| `SessionConfig` | Modes (Story/PVP/PVPTest), place IDs, stages |
| `CharmRecommendations`, `ItemTooltip`, `ItemUIHelper` | Shop/UI helpers |
| Folders | `WeaponModels`, `ClassWeapons`, `ClassOutfits`, `VFXModels`, `HitboxModels`, `Enemies`, `Allies`, `DeceasedWeapons`, `SafeEventProps` |

## ServerScriptService
| Area | Scripts / modules |
|---|---|
| Player setup & data | `Script` (class/weapon equip), `SessionServerScript`, `SessionPartyState`, `SpecialCharacterLooks` (Authan/Hayut looks), `WeaponVisuals`, `ClassWeaponVisuals` |
| Combat | `CombatScript` (M1), `AbilityScript` (E/Q/R), `WeaponSkillScript` (F), `BellaKit`, `ZuminKit`, `ThornMarks`, `HitScan`, `HitboxAnchor`, `EnemyHitRewind`, `CombatSource`, `HitReaction`, `DodgeServerScript`, `PVPRules` |
| Stats | `PlayerCoreStats`, `PlayerUniqueStats`, `PlayerLevelState`, `PlayerExpState`, `PlayerDamageReduction`, `PlayerDamageBonus`, `PlayerInvulnerability`, `PlayerPassiveBuffs`, `MoraleSystem`, `CharacterStatsService` |
| Status | `StatusEffectManager`, `StaggerSystem`, `PlayerInfestation` |
| Run | `PathProgressionScript` (nodes, events, safe havens, PVP tracks/Ambush), `RunProgressState`, `EncounterControllers/*` (BrigandAmbush, ZevFeast, DohvCharge, TotemGrove, RehemBoss, WyvernNest, FleeingBeasts, RootEmergence, BrigandHost, CleanserSquad) |
| Enemies | `RehemBossAI` (+`RehemSceneExport`), `WyvernAI` (+`WyvernGrabTrack`), `AkavishAI`, `DeceasedAI`, `HostAI`, `HostSpawner`, `HostTransform`, `SuffererAI`, `CleanserAI`, `EnemyAggroManager`, `EnemyLevelState`, `EnemyOverheadUI`, `CombatTagManager`. Brigands/Zev/Dohv/Totems use their own scripts inside `ReplicatedStorage.Enemies.*`. |
| Economy & items | `PlayerInventoryState`, `InventoryServerScript`, `ItemEffects`, `ItemPassives`, `ItemEquipServerScript`, `ItemOwnership`, `PlayerSmallInventoryState`, `SmallInventoryServerScript`, `ShopServerScript`, `ShopContext`, `SellServerScript`, `PlayerSilverState`, `EnemyDropDefinitions`, `EnemyDropScript`, `EnemyExpDropScript`, `CraftingService`, `CraftsmanNPCScript`, `CharmAutoEquip`, `CharmPartsPurchase`, `CharmBuildInfoServer` |
| World | `SafeHavenHealScript`, `RejuvenatingLake` (the field's G-to-touch lake: full heal + one level, needs the cave cleared, once before and once after the Rehem), `NPCInteractionScript`, `LavaHazardScript` (test pads), `CollisionSetup`, `PassiveGlowVFX`, `DamageIndicator` |
| Safety | `FreezeWatchdog`, `AnimationLoopGuard`, `PreloadManifestServer`, `SoundPrep`, `DevTestingStudioOnly` |

## StarterPlayerScripts (client)
`M1InputScript`, `AbilityInputScript`, `CameraLockOnScript` (lock-on + dodge), `ScreenShakeScript`, `ThornBurstClient`, `ThornMarksClient`, `GrabDoubleClient`, `DivineServantClient`, `ProjectileVfxClient`, `BellaKitClient`, `ZuminKitClient`, `RehemClient`, `BossHealthBar`, `EssenceOrbClient`, `SessionIntro`, `ForestAmbience`, `ForestMusic`, `CombatMusic`, `UIButtonSounds`, `SFXVolumeGroup`, `SoundPreloader`, `AnimationPreloader`, `UIResolutionScaler`, `MuteWalkingSound`, `DisableResetButton`, `FreezeWatchdogClient`.

## Workspace (world)
EntranceToNorthForest, EntranceToNorthForest(NavatSide), PathVersion1–4, "Path version 5", CavePath1–2, WyvernNest, RejuvenatingField, SafeHaven, NavatSafeHaven, SecondHalfBossPath(TheRehem), "Secound Stage", Garrison (NPC), DevTesting folder.

## RemoteEvents (ReplicatedStorage)
`CombatEvent`, `AbilityEvent`, `DodgeEvent`, `PerfectDodgeEvent`, `InventoryEvent`, `SmallInventoryEvent`, `ItemEquipEvent`, `ShopEvent`, `ShopPurchaseEvent`, `MerchantSellEvent`, `SellItemEvent`, `MoraleChangedEvent`, `ProjectileVfxEvent`, `ClassSelectionEvent`, `GetCharacterStats` (RemoteFunction).
