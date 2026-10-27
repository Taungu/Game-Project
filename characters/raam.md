# Raam: "Opressed"

- **Affinity:** Heavy · **Default weapon:** GreatAxe · **Rarity:** 2★ starter
- **Role:** Bruiser. Wide AoE, damage reduction, and power that rises as his health falls.
- **Affinity Stat:** Physical Damage
- **Essence colour:** red

| Stat | Value |
|---|---|
| Health | 180 |
| Physical Damage | 25 |
| Physical / Magic Res | 25 / 0 |
| CDR | 10 |
| MaxStagger | 15 (sturdier than the default 12) |

## Kit
| Slot | Name | What it does | Numbers |
|---|---|---|---|
| **E** | **Liberating Cleave** | A short forced dash forward, then a wide circular cleave around him. Sweet-spot design: the **outer ring always crits** at full damage; enemies caught too close land in the inner ring for a quarter of the damage. A StarWarning telegraph shows 0.3 s before the slash. Blood hit reaction. | 35 Physical outer / 8.75 inner · radius 10 · cd 5 s · 20 Essence · stagger 3 · dash 40 studs/s × 0.25 s · hit at 1.0 s |
| **Q** | **Repressed Cry** | A cry in a radius around him. Applies **Daze, then Slow** once the Daze ends. Gives Raam **20% damage reduction for 1 s**. | 8 Magic · radius 10 · cd 15 s · 15 Essence · stagger 1 |
| **P** | **Yearnings of Path** | At or below **30% health**, all his outgoing damage (M1, E, Q, R) is **+20%**. His eyes glow red (RedEyeGlow) while it's active. | — |
| **R** | **Unchained** | A dash that hits and launches (**Aerial**). **Resets E/Q cooldowns**, **doubles his total CDR for 20 s**, and gives **20% damage reduction for 20 s** (stacks with Q's to 40%). Shows an OverloadingEssence aura for the duration. | 25 Physical · dash 50 studs/s × 0.8 s · hitbox 8×8×8 · cd 60 s · 40 Essence · stagger 4 |

Unbuilt draft: Unchained's DR rises to 40% while below 30% health (`incomingDamageReductionLowHealth`), but this isn't wired.
