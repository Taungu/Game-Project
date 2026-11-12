# Roran (empty shell)

- **Affinity:** Medium · **Default weapon:** the Medium default (StraightSword) · **Rarity:** 3★, the first **standard** 3★ Soul for Medium
- **Not a starter.** Meant for Convoke's standard pool, but **unreleased** for now: the Lobby's `ClassDefinitions` marks him `unreleased = true`, so Convoke skips him. The dev accounts get him through `GRANT_EVERYTHING`; in the Session he's on the DEV panel's Soul switch.
- **Title, lore, model:** none yet.
- **Status (2026-11-12):** registered only. No kit or animations yet.

| Stat | Value (all placeholders) |
|---|---|
| Health | 150 |
| PhysicalDamage | 25 |
| PhysicalResistance | 25 |
| MagicResistance | 15 |
| MaxStagger | 15 |
| Everything else | Shared defaults |
| Affinity Stat (Mastery kicker) | PhysicalDamage (placeholder) |

## Where he's registered
| Place | Module | Entry |
|---|---|---|
| Both | `ClassDefinitions` | `weaponType = "Medium"`, `tier = 3` (+ `unreleased` in the Lobby) |
| Both | `ClassBaseStats` | the stats above |
| Both | `LevelDefinitions` | `AFFINITY_STATS.Roran = "PhysicalDamage"` |
| Both | `ClassAnimations` | all nil (default Roblox animations) |
| Both | `ClassAbilities` | the empty shape: E / Q / Passive / Ultimate = nil, so his skills do nothing |
| Both | `ItemTooltip` | `CLASS_DISPLAY.Roran` |
| Session | `CraftingBookDefinitions` | class section `Power` (placeholder) |
| Session | `ServerScriptService.Script` | `PVP_LOCKED_WEAPONS.Roran = "SilverFlash"` |

To release him: delete `unreleased = true` from the Lobby's `ClassDefinitions` entry.
