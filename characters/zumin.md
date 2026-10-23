# Zumin: "Depths Delver"

- **Affinity:** Gauntlet (the first Gauntlet user) · **Default weapon:** LeatherGloves · **Rarity:** 2★
- **Not a starter.** Only obtainable through **Convoke** (featured on the "Depths Delver" Souls banner) or a grant.
- **Role:** Counter-puncher tank. Holds his ground, takes the hit, gives it back. The whole kit is Physical and scales on Physical Damage.
- **Affinity Stat:** Health (Mastery levels grow his health most)
- Kit runs in `ServerScriptService.ZuminKit`; numbers in `ClassAbilities.Zumin`.

| Stat | Value |
|---|---|
| Health | 250 |
| Physical Damage | 10 |
| Physical / Magic Res | 50 / 50 |

## Kit
| Slot | Name | What it does | Numbers |
|---|---|---|---|
| **P** | **Balance Control** | Every immobilizing effect on him (Stun, Daze, Aerial, Stagger, Rooted, Ragdoll) lasts **50% shorter**. | — |
| **P2** | **Thirst for Danger** | Every E opens a 1 s window where he takes **20% less damage** from everything (True damage still bypasses it). Taking a hit inside the window **resets E's cooldown** (once per window). | window 1 s · 20% |
| **E** | **Fimble Decision** | The Broad Dagger's Enhanced Dodge built in: a directional dash with his own four direction animations. Half the distance, no i-frames. | 8 studs × 0.2 s · cd 4 s · 7.5 Essence |
| **Q** | **Brace and Vengeance** | **Hold** (up to 5 s): he plants completely (can't move, jump or turn) and braces for **+1000 Phys Res / +500 Magic Res**. Immune to every status, stagger, knockback and grab, so only raw damage gets through. It can't be cancelled. **Release:** Vengeance, a cone punch for 30 base + a **flat 50% of the health he lost while bracing** (uncapped). The cone grows with damage taken. Daze. | cd 12 s · 15 Essence · cone 8 long, 60° (+40% per 100 HP lost, max 180°) · stagger 2 |
| **R** | **Devastation** | Lifts his leg and stomps. The shockwave **Dazes**, then **Slows 3 s**. The uneven ground left behind slows enemies standing in it for 5 s. | 100 inner (r 18) / 50 outer (r 36) · cd 45 s · 35 Essence · stagger 3 |
