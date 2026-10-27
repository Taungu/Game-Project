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

- **EXP to next level:** `100 + 25 × (level − 1)` (100, 125, 150 … 550 at 19→20).
- EXP comes from **essence orbs** dropped by enemies (`EnemyExpDropScript` / `EssenceOrbClient`). Levels, items and Silver are **per run**; nothing is saved.
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
