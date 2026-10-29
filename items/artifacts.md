# Artifacts (the Armory)

**Permanent** owned items, the third collectable next to Souls and weapons. Like charms, they give stats (and on 3★ a small passive), but they're **kept forever and weaker**. In lore, "a great enough forging of steel of essence is an artifact" ([world](../lore/world.md)).

Data: `ReplicatedStorage.ArtifactDefinitions` in **both** places (keep identical). All numbers are placeholders.

## Rules (decided)
| | |
|---|---|
| Rarity | 1★ to 3★ (like weapons) |
| Loadout | **4 slots, one universal loadout** shared by every Soul; one copy of each Artifact per loadout |
| Where you equip | Lobby, Inner Core page, new **Armory** tab (next to Soul and Weapon) |
| Where you get them | **Only from Story run rewards.** Not in Convoke (no third thing to spend premium currency on) |
| PVP | **Off** (they do nothing in PVP) until weapons, Souls and charms are balanced |
| Duplicates | Sold automatically for Essence: 1★ 10, 2★ 30, 3★ 100 |
| Bad-luck protection | Across runs: a 2★+ within every **6** rolls, a 3★ within **25** |
| Strength | 1★ ≈ half a tier-1 charm · 2★ ≈ one tier-1 charm · 3★ a bit under a tier-2 charm + a small passive. Flat stats, charm units |
| Descriptions | History, not looks (same as charms) |

## Artifact rolls (Story rewards)
Each roll becomes one Artifact, shown on the Results screen and claimed in the Lobby like Essence. Rolls already earned are kept on defeat.

| Source | Rolls | Base odds 1★ / 2★ / 3★ |
|---|---|---|
| The Rehem | 1 | 75 / 22 / 3 |
| Wyvern, Abominable Akavish | 1 each | 70 / 25 / 5 |
| Elite kill (Sufferer, Dohv(Corrupted), Host) | 10% chance of 1 | 80 / 18 / 2 |
| The Chastity (North Forest cleared) | 2 | 40 / 45 / 15 |

**Run quality raises the odds:** flawless boss → that roll's 3★ weight ×2; depth → 0.5 of 1★ weight moves to 2★ per overall path (max 20); level reached → +0.1 3★ weight per level on the Chastity's rolls. A full clear ≈ 5 rolls.

## Where they show
- **Inner Core → Armory tab:** 4 slots on top, owned Artifacts below sorted by star; hover tooltip like charms (plus passive and lore).
- **Session HUD:** 4 small locked icons next to the charm hotbar (can't be sold, crafted or moved), hoverable.
- **Party staging:** each member's 4 icons in the ready list.
- **Results screen:** a small reveal per Artifact earned.

## Starter set (draft, Lukas reviewing)
| ★ | Artifact | Section | Stats | Passive |
|---|---|---|---|---|
| 1 | Whetstone of the North Gate | Power | 50 PhysDmg | |
| 1 | Witch's Ash Vial | Destruction | 50 MagicDmg | |
| 1 | Wanderer's Patch | Invulnerability | 15 Health | |
| 1 | Outskirt Courier's Sandals | Momentum | 15 MoveSpd | |
| 2 | Pardian Mint Die | Power | 80 PhysDmg, 3% Crit | |
| 2 | Lumet Chalk | Destruction | 80 MagicDmg, 3% CDR | |
| 2 | Cleanser's Gorget | Invulnerability | 20 Health, 20 PhysRes | |
| 2 | Kanin Runner's Ribbon | Momentum | 20 MoveSpd, 20 AtkSpd | |
| 3 | Inquisitor's First Edge | Power | 150 PhysDmg, 5% Armor Pen | First hit within 1.5 s after a dodge +15% |
| 3 | The Unwritten Glyph | Destruction | 150 MagicDmg, 5% CDR | After an ability, the next costs 10% less Essence |
| 3 | Living Root Ward | Invulnerability | 40 Health, 20 PhysRes, 20 MagRes | Infected hits fill Infestation 15% slower |
| 3 | Apex Bounty Writ | Momentum | 100 PhysDmg, 20 MoveSpd | After a dodge +10% MoveSpd for 2 s |

## Built
- Lobby: saving (`ArtifactService`, DataStore `PlayerArtifactData`), the Inner Core **Armory tab** (equip/unequip, stats preview). Studio shows every Artifact as owned for testing.

## Not built yet
Applying stats/passives in the Session, reward rolls and pity, HUD icons, party icons, Results reveal, art.
