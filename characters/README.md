# Souls (characters)

Code calls them **classes**. Convoke and the Character Page call them **Souls**. Each has a kit (E / Q / Passive / R), a weapon affinity, base stats and an essence colour.

| Soul | Title | Affinity | Rarity | How you get it | Default weapon | Role | File |
|---|---|---|---|---|---|---|---|
| **Raam** | Opressed | Heavy | 2★ | Starter pick | GreatAxe | Bruiser, AoE cleaves, gets stronger near death | [raam.md](raam.md) |
| **Fahlon** | Lost Ruler | Artillery | 2★ | Starter pick | Bow | Ranged DPS, marks targets, trade passive | [fahlon.md](fahlon.md) |
| **Luther** | Champion of Vonrow | Medium | 2★ | Starter pick | StraightSword | Bleed/burst duelist, spends health | [luther.md](luther.md) |
| **Bella** | Substance Fanatic | Light | 2★ | Starter pick | Dagger | Assassin-mage with rotating vials, hybrid scaling | [bella.md](bella.md) |
| **Zumin** | Depths Delver | Gauntlet | 2★ | Convoke only (not a starter) | LeatherGloves | Counter-puncher tank | [zumin.md](zumin.md) |
| **Authan** | — | Authan (any weapon) | Personal | Lukas's account only | Than | Lukas's own character | [authan.md](authan.md) |
| **Hayut** | Ancient Conduit | Conduit | Personal | Dev character (Session only) | none | Test bed | [hayut.md](hayut.md) |

New players pick one of the four **starters** (Raam, Fahlon, Luther, Bella) in The Embracer's scene. Others come from Convoke or grants.

## Base stats at a glance
Units: damage, resistance, CDR and movement speed use **10 units = 1%**. Health and Essence are raw pool values. A blank cell means the shared default is used.

| Soul | Health | PhysDmg | MagicDmg | PhysRes | MagRes | CDR | MoveSpd | Max Essence (L1) | MaxStagger | M1 mult |
|---|---|---|---|---|---|---|---|---|---|---|
| Raam | 180 | 25 | 0 | 25 | 0 | 10 | 0 | 175 | 20 | 1.0 |
| Fahlon | 120 | 25 | 15 | 0 | 0 | 10 | 100 (+10%) | 100 | 20 | 1.14 |
| Luther | 140 | 20 | 20 | 25 | 15 | 0 | 0 | 175 | 25 | 1.0 |
| Bella | 130 | 25 | 25 | 0 | 0 | 10 | 0 | 200 | 20 | 1.0 |
| Zumin | 250 | 10 | 0 | 50 | 50 | 10 | 0 | 175 | 20 | 1.0 |
| Authan | 150 | 40 | 30 | 0 | 0 | 10 | 100 | 100 | 20 | 1.14 |
| Hayut | 120 | 0 | 30 | 20 | 25 | 10 | 0 | 175 | 15 | 1.14 |

Defaults: Health 100, Essence 100 (×1.75 at level 1, so 175), Essence regen 1/3 per sec (an empty bar refills in about 5 min), Health regen 1 HP / 30 s, MaxStagger 20, CDR 10. Fahlon and Bella's Essence bases are set so level 1 lands exactly on 100 and 200. Their regen is scaled so the refill *time* matches everyone else's. Source: `ReplicatedStorage.ClassBaseStats`.

## Essence colours (Character Page sparkle)
Raam red · Fahlon dark blue · Bella dark pink · Luther pale yellow · default white.

## Personal "class weapons"
Some Souls carry a cosmetic personal weapon on top of their equipped one (`ClassWeaponVisuals`):
- **Fahlon's Knife**: always visible, part of his identity.
- **Luther's Rondel Dagger**: left hand, only visible during Blood Letting.
- **Bella's Kleilev**: her injector, only visible during Peak Health.
