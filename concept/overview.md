# Concept & design rules

## One-line pitch
A co-op (1–4 players) session-based roguelike action RPG on Roblox. You own characters and weapons permanently. Each run you take one loadout into a randomized forest, build a temporary charm set, level up and fight bosses. **The build dies; the collection stays.**

## Three layers (from the design bible)
| Layer | Meaning | In the game |
|---|---|---|
| **Beacon** | Saved, permanent player data (ownership) | DataStores in the Lobby (`PlayerClassData`, `PlayerWeaponData`) |
| **Lobby** | The hub where you stand, pick a loadout and start a run | Place "Servitude" |
| **Session** | The disposable run | Place "Session(NorthForest)"; one private server per party |

## Permanent collectables
1. **Souls (characters).** A specific person, not a generic job. Each brings a unique kit, a weapon affinity and a base-stat identity. Code calls them "class" on purpose: you don't play *as* the person. You play their identity through their kit, with the character as a vessel.
2. **Weapons.** Equippable only when the weapon's **affinity** matches the Soul's. A weapon adds its own M1 combo, and at 2★+ an **F skill**. It also has a stat identity.

Same Soul with a different weapon = a different route through that person. Same weapon on a different Soul = the same addition on a different body.

### Affinities (the equip gate)
**Heavy, Medium, Light, Conjurer, Artillery, Gauntlet**, plus personal types for dev characters (Authan, Conduit).
- Hard rule: **kit and affinity are orthogonal.** A Heavy character can have a mobile kit. If you can predict the kit from the affinity, the system has failed.
- One weapon equipped at a time. The only equip rule is matching affinity. Exception: Authan has `anyWeapon`.

### Rarity (stars)
- 1★: defaults/commons. M1 combo only.
- 2★ and 3★: also get an **F weapon skill**. 3★ weapons are the showpieces and each has a **weapon passive** (SilverFlash, IllThorn, The Commandment, DivineServant, Akhzarims Tenacity, Imperial Rifle). Every affinity now has a 3★.
- "Personal" tier: never in Convoke (Authan, Hayut, Than).

## Character kit shape
| Slot | Key | What it is |
|---|---|---|
| Basic attack | M1 | Comes from the weapon (combo, damage, range) |
| Skill 1 | E | Personal ability |
| Skill 2 | Q | Personal ability |
| Passive | P | Always on, no input |
| Ultimate | R | Defines the character's identity |
| Weapon skill | F | From the weapon (2★+) |
| Dodge | — | Shared dodge; some characters replace it |

Abilities cost **Essence** (a mana-like pool) and have cooldowns. Some characters lean AoE, others single-target.

## The run (Story mode)
Lobby → party ready → teleport to a private session server → **EntranceToNorthForest** → a chain of randomly picked paths. Each path pulls a random **event** (fight, merchant, NPC). A **Safe Haven** comes every 5 paths. The first half ends at the **Rehem** boss. After it comes the corrupted **second stage** with the Infected. A final boss is planned. Details: [systems/run-structure.md](../systems/run-structure.md).

## In-run power (temporary)
- **Levels 1–20** (EXP from enemy essence orbs). Every 5th level is a **Mastery** level.
- **Charms**: a 6-slot hotbar of tiered items bought, dropped or crafted. Tier 1 = materials-like, tier 2 = mid, tier 3 = big stats plus a personal passive. One copy of each tier-3 item per player.
- **Silver** (currency) resets every run.
- Item value depends on your loadout. There's no universal "best item".

## Balance philosophy (stated)
- Levels scale health and damage moderately ("in the middle for both"). Items decide the real build.
- A full tank build is tanky but not unkillable. A full damage build nearly kills anyone in seconds (tanks excepted).
- M1s only get strong through M1-focused builds (e.g. Suit of Kanins), not through any physical build.
- Resistances follow a diminishing curve. 100% (immune) is never reachable.
- **First half** (up to the Rehem) preys on players at their weakest. Modest enemies, halved economy, and one item can carry most events. The **second half** is harder (players are usually past level 10 with items). Income can be higher, but never high enough to make Fahlon's sell passive pointless.
- Final stats must be **order-independent**: leveling then itemizing gives the same result as the reverse.

## Game modes
- **Story**: co-op PvE run (main mode, still needs more content).
- **PVP**: two teams race through the same path pool from opposite ends. They fight when they collide ("Ambush"). See [systems/pvp.md](../systems/pvp.md).

## Meta-progression (planned)
Winning a stage gives a special currency for permanent buffs that make the next run start easier (relic-style, like Elden Ring Nightreign).
