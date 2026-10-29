# Stats, levels & damage math

## Stat taxonomy
| Layer | Stats | Module |
|---|---|---|
| **Core stats** (raw power, class-owned) | Health, Physical Damage, Magic Damage, Physical Resistance, Magic Resistance, Essence (pool), Essence Regen, Health Regen | `CoreStats` + per-player `PlayerCoreStats` |
| **Unique stats** (how combat resolves) | CDR, Movement Speed, Attack Speed, Crit Chance, Crit Damage, Armor Pen, Magic Pen, Health Steal, True Damage (a flag) | `UniqueStats` + per-player `PlayerUniqueStats` |
| **Status effects** | See [combat.md](combat.md) | `StatusEffects` + `StatusEffectManager` |

- "Armor" isn't a stat. It's just how we describe having both resistances.
- **Damage type vs scaling stat** are separate concepts. `damageType` (Physical/Magic/True) is the universal label used for mitigation. `scalingStat` is the owned stat pool that powers the hit. They often match but don't have to.

## Units
- Most stats use **10 units = 1%** (25 PhysDmg = +2.5% physical damage; 100 MoveSpd = +10% walk speed over the base 16).
- **Health** and **Essence** are raw pool values.
- **Resistance** uses a diminishing curve: `reduction = points / (points + 1000)`. Every 100 points ≈ +10% effective health, and it never reaches 100%. Armor Pen / Magic Pen cut the resulting percent.
- **Crit:** Crit Chance rolls; a crit adds **+50%** damage (base `CRIT_DAMAGE_BONUS`).
- **True damage** bypasses resistances *and* damage reduction.
- Defaults: Health 100, Essence 100, Essence regen 1/3 per sec, Health regen 1/30 HP per sec (scales with Essence, capped at 1/sec), CDR 10 units, MaxStagger 12 (Raam and Luther 15, Zumin 20).

## Final value formula (order-independent)
```
final = classBase × levelMultiplier × (1 + itemPercent) × morale  +  flatItemBonus
```
- Level growth and item bonuses are tracked separately and recomputed fresh. Leveling then itemizing gives exactly the same result as the reverse.
- Charms currently use **flat** bonuses (class-independent, added after the multipliers). Percent bonuses exist mainly for Essence Regen.
- **Morale** (–10% per stack, max –50%) multiplies the four class Core stats: Phys/Magic Damage and Phys/Magic Res.

## Levels (1–20) — `LevelDefinitions`
- **Normal levels** (1–4, 6–9, 11–14, 16–19): +**75%** of base to every stat (level 1 counts too).
- **Mastery levels** (5, 10, 15, 20): +**112.5%** to every stat, plus +**37.5%** extra to the class's **Affinity Stat** (Raam PhysDmg · Bella Phys+Magic · Zumin Health · Hayut Magic · Fahlon none).
- **Stat growth** scales how much of each level bonus a stat receives. This is the balance knob:

| Stat | Growth | Notes |
|---|---|---|
| Physical / Magic Damage | ×3 | Level-20 Raam M1 ≈ 2.3× level-1 damage |
| Health | ×0.4 | Every class ≈ 1k HP at level 20 with no items (~7.6× base) |
| Essence, Essence Regen | ×1 | |
| Move Speed, Attack Speed, CDR | ×0.25 | |
| Phys / Magic Res | ×0.15 | Percent-type stats barely grow |
| Armor/Magic Pen, Crit Chance | 0 | Items only |

- **EXP to next level** (a table, `LevelDefinitions.EXP_TABLE`; **37,720** in total to reach 20):

| Level → next | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| EXP | 100 | 120 | 140 | 160 | 550 | 700 | 850 | 1000 | 1400 | 1800 | 2200 | 2600 | 2900 | 3200 | 3500 | 3900 | 4100 | 4200 | 4300 |

- **Fitted to the run's income** (simulated 50-path runs: event pool, enemy EXP, enemy level tiers Lv 1/20/40/60/80 every 10 paths, EXP +25% of base per enemy level). Normal solo play: ~5 at path 10, ~12 at the Rehem, ~15 at path 40, **~17 at the end**. Every extra step (both Rejuvenating Field detours, both lakes, the Wyvern, the Abominable Akavish): **~19**, and level 20 in about a third of runs (around path 48). The jumps at 5 and 9 follow the enemy level jumps at paths 10 and 20.
- EXP comes from **essence orbs** dropped by enemies (`EnemyExpDropScript` / `EssenceOrbClient`). **Every party member still standing gets the full EXP of a kill** (like Silver), so the curve holds solo or in a group. Levels, items and Silver are **per run**; nothing is saved.
- Balance targets: a level-20 vs level-20 no-item duel used to take 173 M1s. The growth table was retuned so tanks are tanky but killable and full damage builds kill in seconds.

## Essence (resource)
- A mana-like pool spent by E/Q/R (`essenceCost`). It's shown as a cup on the hotbar.
- Regen is slow by design: an empty bar takes about 5 minutes to refill. Safe Havens, edibles and items speed this up.
- Max Essence also drives **Infestation immunity** and **Core Outbreak**.

## Damage pipeline (one hit)
1. Base damage from the move (WeaponMoves / ClassAbilities / enemy share of base).
2. × class M1 multiplier (M1 only) × attacker's damage bonus (Physical or Magic stat %), plus passives (Yearnings of Path, Deadly Precision, item passives…).
3. Crit roll.
4. Target **resistance** (after Armor/Magic Pen), skipped for True.
5. Target **damage reduction** (named, stacking sources in `PlayerDamageReduction`; i-frames from `PlayerInvulnerability`; Sensitive increases damage taken).
6. **Blind** check on the attacker: the hit lands but its damage is nulled ("Miss").
7. Apply damage, stagger, status effects, aggro, hit reaction and the damage number.

## Level-up feedback (`PlayerLevelState`, `LevelUpVfxClient`)
- Every level up plays **LevelUp**; every 5th level (5, 10, 15, 20) plays **LevelUpSpecial** instead.
- Every player sees it: the body pulses twice like Akhzarims Tenacity's heartbeat (your own screen shakes on each beat) and a small burst of Raam's ultimate aura (`OverloadingEssence`) comes out of the chest.
- Normal levels are **lavender**; every 5th level is **pastel yellow**, with a bigger burst, stronger pulse and stronger shake.
