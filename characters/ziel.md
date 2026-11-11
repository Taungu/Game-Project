# Ziel (in progress)

- **Affinity:** Conjurer (the first Conjurer Soul) · **Default weapon:** ConjuredPebble · **Rarity:** 2★ (placeholder)
- **Not a starter.** Meant for Convoke, but **unreleased**: the Lobby's `ClassDefinitions` marks him `unreleased = true`, so the pull pool and the welcome Call 10 skip him. The dev accounts get him through `GRANT_EVERYTHING`; in the Session he's on the DEV panel's Soul switch.
- **Lore:** the Ziel of *Ziel's Thrash*. He had every quality of an Inquisitor, yet left the Cleansers to live in peace as the priest of a remote village. He fought with a flail, and every strike he sent from afar pierced deep.
- **Title:** not given yet. **Model:** none (like Zumin, you play as your avatar; no Lobby preview rig).
- **Status (2026-11-11):** only the basics are wired. No kit, no animations.

| Stat | Value (all placeholders) |
|---|---|
| Health | 120 |
| Physical / Magic Damage | 0 / 25 |
| Everything else | shared defaults (CDR 10, Essence 100 base, MaxStagger 12, no resistances) |
| Affinity Stat | Magic Damage (placeholder) |

## Kit
Not designed yet. `ClassAbilities.Ziel` is the declared-but-empty shape, so E / Q / R do nothing. A kit too unlike the shared shapes gets its own module (`kit = "ZielKit"`, registered in `AbilityScript`'s `KIT_MODULES`), like Bella's and Zumin's.

## Where he's wired
| Place | What |
|---|---|
| Session + Lobby | `ClassDefinitions`, `ClassBaseStats`, `ClassAbilities` (empty), `ClassAnimations` (empty), `LevelDefinitions` (Affinity Stat), `ItemTooltip` (display name) |
| Session only | `CraftingBookDefinitions` (section "Destruction", placeholder), PVP weapon lock → DivineServant |
| Lobby only | `unreleased = true` + `ClassDefinitions.IsReleased`, checked by Convoke's pool and the welcome guarantee in `ServerScriptService.Script` |

Still to do when he's designed: kit and animations, title, charm recommendations, Soul Mastery goals, essence colour and pick animation, Convoke banner colour, a Lobby preview rig if wanted, then remove `unreleased`.
