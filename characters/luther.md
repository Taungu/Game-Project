# Luther: "Champion of Vonrow"

- **Affinity:** Medium · **Default weapon:** StraightSword · **Rarity:** 2★ starter
- **Role:** Bleed duelist. Stacks Bleed with Hemorrhage, then force-bursts the stored damage. He's the only class that **spends health** as a resource. No defensive tools; his answer to danger is killing faster.
- **Essence colour:** pale yellow · **Class weapon:** Luther's Rondel Dagger (left hand, visible only during Blood Letting)

| Stat | Value |
|---|---|
| Health | 140 |
| Physical / Magic Damage | 20 / 20 |
| Physical / Magic Res | 25 / 15 |
| CDR | 0 |
| MaxStagger | 15 (sturdier than the default 12) |

## Kit
| Slot | Name | What it does | Numbers |
|---|---|---|---|
| **E** | **Blood Letting** | A wind-up drift, then a server-driven lunge stab with his rondel dagger. Applies **Bleed** carrying **Hemorrhage**. | 10 Physical · cd 8 s · 15 Essence · stagger 1 · lunge 110 studs/s × 13/60 s after 29/60 s |
| **Q** | **Tainted Rage** | He cuts himself and sprays blood in a 120° cone. Enemies are **Blinded** (3 s, 1.5 s vs players). Point-blank enemies (inside 4.5 studs) instead take a heavy hit and get **Bleed + Hemorrhage**. Costs him health and slows him while casting. | 6 Physical (cone) / 20 (point blank) · radius 12.5 · self-damage 5 True · cd 18 s · 15 Essence · stagger 1 |
| **P** | **Anemic** | Whenever a Bleed **he applied** bursts, there's a **25% chance to Stun** (2.5 s). Today natural expiry counts as a burst too (`forcedBurstsOnly = false`). | — |
| **R** | **Hemostatic Impact** | A step forward, then one heavy 120° slash. It **force-bursts every Bleed** it hits, puts a **guaranteed 2× Hemorrhage** on every target, and **roots** them. | 20 Physical · radius 8 · root 0.75 s · cd 45 s · 35 Essence · stagger 3 |

## Bleed & Hemorrhage (how his damage pays off)
- **Bleed** (2026-11-16): a DoT for 5 s, each second a 4 Magic + 0.5 Physical hit scaled by his stats. Up to 3 stacks.
- **Hemorrhage** (2026-11-16) is the Bleed's crit: an instant True burst scaled by Physical and Crit Damage. Every Bleed rolls his Crit Chance; Blood Letting forces one. Nothing is stored any more.
- **Hemostatic Impact** guarantees a Hemorrhage on every target it hits, at **2×** (`guaranteedHemorrhage`, `hemorrhageMultiplier`). It still detonates their Bleed (ending it, rolling Anemic); that burst's old ×2 no longer adds damage.
