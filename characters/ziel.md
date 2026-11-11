# Ziel: "Gifted Sinner" (in progress)

- **Affinity:** Conjurer (the first Conjurer Soul) · **Default weapon:** ConjuredPebble · **Rarity:** 2★ (placeholder)
- **Not a starter.** Meant for Convoke, but **unreleased**: the Lobby's `ClassDefinitions` marks him `unreleased = true`, so the pull pool and the welcome Call 10 skip him. The dev accounts get him through `GRANT_EVERYTHING`; in the Session he's on the DEV panel's Soul switch.
- **Lore:** the Ziel of *Ziel's Thrash*. He had every quality of an Inquisitor, yet left the Cleansers to live in peace as the priest of a remote village. He fought with a flail, and every strike he sent from afar pierced deep.
- **Model:** none (like Zumin, you play as your avatar; no Lobby preview rig).
- **Status (2026-11-11):** basics wired and the Q built. No E, R, passive or animations yet.
- **Look:** a former Cleanser's magic, **cloudy yellow**. Nothing is animated; the kit's visuals are drawn in code (`ZielKitClient`).

| Stat | Value (all placeholders) |
|---|---|
| Health | 120 |
| Physical / Magic Damage | 0 / 25 |
| Everything else | shared defaults (CDR 10, Essence 100 base, MaxStagger 12, no resistances) |
| Affinity Stat | Magic Damage (placeholder) |

## Kit
Runs in its own module, `ServerScriptService.ZielKit` (registered in `AbilityScript`'s `KIT_MODULES`); numbers in `ClassAbilities.Ziel`; visuals in `StarterPlayerScripts.ZielKitClient`. All numbers are placeholders.

| Slot | Name | What it does | Numbers |
|---|---|---|---|
| **Q** | **Penitent's Flail** (placeholder name) | **Hold:** he's planted and aims with the camera (like Fahlon's R) while spinning a chain flail above his head like a lasso. A circle on the ground slides away from him the longer he holds; everyone sees it. **Release** (or full range, where it throws itself): the chain is thrown and the mace slams down on the circle, hitting everything inside. A stun, stagger or grab during the hold breaks the cast (Essence spent, cooldown starts). Once thrown, the mace lands regardless. | 32 Magic · radius 9 · range 8 → 42 over 2.2 s of holding (0.3 s minimum windup) · lands 0.4 s after release · 0.35 s endlag · cd 10 s from the throw · 20 Essence · stagger 3 |
| E / R / P | — | Not designed yet. | — |

**How the Q is wired:** the hold is the character attribute `ZielFlailCharge` (the server time it began), which every client draws the lasso and circle from; the caster's own client turns him to the camera and streams it as `QSteer`; the throw is the `ZielFlail` event. `FreezeWatchdog` leaves a character with `ChargingSkill` alone.

## Where he's wired
| Place | What |
|---|---|
| Session + Lobby | `ClassDefinitions`, `ClassBaseStats`, `ClassAbilities`, `ClassAnimations` (empty), `LevelDefinitions` (Affinity Stat), `ItemTooltip` (display name) |
| Session only | `CraftingBookDefinitions` (section "Destruction", placeholder), PVP weapon lock → DivineServant |
| Lobby only | `unreleased = true` + `ClassDefinitions.IsReleased`, checked by Convoke's pool and the welcome guarantee in `ServerScriptService.Script` |

Still to do: the rest of the kit, animations, charm recommendations, Soul Mastery goals, essence colour and pick animation, Convoke banner colour, a Lobby preview rig if wanted, then remove `unreleased`.
