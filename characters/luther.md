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
| MaxStagger | 25 |

## Kit
| Slot | Name | What it does | Numbers |
|---|---|---|---|
| **E** | **Blood Letting** | A wind-up drift, then a server-driven lunge stab with his rondel dagger. Applies **Bleed** carrying **Hemorrhage**. | 10 Physical · cd 8 s · 15 Essence · stagger 1 · lunge 110 studs/s × 13/60 s after 29/60 s |
| **Q** | **Tainted Rage** | He cuts himself and sprays blood in a 120° cone. Enemies are **Blinded** (3 s, 1.5 s vs players). Point-blank enemies (inside 4.5 studs) instead take a heavy hit and get **Bleed + Hemorrhage**. Costs him health and slows him while casting. | 6 Physical (cone) / 20 (point blank) · radius 12.5 · self-damage 5 True · cd 1 s (marked in code as a temporary testing value; the ship value noted there is 18 s) · 15 Essence · stagger 1 |
| **P** | **Anemic** | Whenever a Bleed **he applied** bursts, there's a **25% chance to Stun** (2.5 s). Today natural expiry counts as a burst too (`forcedBurstsOnly = false`). | — |
| **R** | **Hemostatic Impact** | A step forward, then one heavy 120° slash. It **force-bursts every Bleed** it hits, **doubles** the stored pool, and **roots** the targets. | 20 Physical · radius 8 · root 0.75 s · cd 45 s · 35 Essence · stagger 3 |

## Bleed & Hemorrhage (how his damage pays off)
- **Bleed** alone is a small DoT: 0.5% of the target's max health per second for 5 s. Up to 3 stacks.
- **Hemorrhage** turns a Bleed into an accumulator. While it's on, **10% of every Physical hit** from a Hemorrhage-capable class (only Luther today) goes into a pool. The pool lands all at once when the Bleed bursts: naturally at the end, or forced early by Hemostatic Impact (×2).
- The physical damage stat feeds him twice: once on the hit, once more through the pool.
