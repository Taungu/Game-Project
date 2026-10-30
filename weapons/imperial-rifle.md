# Imperial Rifle

- **Tier:** 3★ · **Affinity:** Artillery · **Type:** rifle with a bayonet
- A **hybrid** weapon (`hybridScaling`): its M1s deal whichever damage type your build is stronger in, like IllThorn's burst.
- Holsters on the back after 3 s idle.
- F skill: **BayonetCharge** (placeholder name), see below.

## M1: a shot, then a follow-up
The M1s aren't a combo. They alternate a **shot** with a **follow-up** picked by the key you're holding (`branching` in `WeaponMoves`). After 2.5 s idle the next M1 is a shot again.

| Step | Trigger | What it does | Damage | Stagger | cd (s) |
|---|---|---|---|---|---|
| 1 | Shot (every other M1) | Aims and fires a bullet (260 studs/s, range 120). You're frozen while aiming. | 24 | 1.6 | 1.03 |
| 2 | Follow-up, **W** (also the default) | Bayonet slash with a 3-stud forward dash. Inflicts **Vulnerable** (Trained Veteran). | 18 | 1.2 | 0.67 |
| 3 | Follow-up, **S** | Back-leap (12 studs, height 7) that shoots the ground where you stood; it explodes (12-stud hitbox). The explosion knocks enemies down: **Imbalance** for 1 s (human-shaped bodies only). A player victim also gets ThornBurst's after effect: a 2 s dizzy blur and a 0.6 s red flash. | 20 | 2 | 1.33 |
| 4 | Follow-up, **A** | Side dash left (10 studs, 0.25 s). No hit. | 0 | — | 0.47 |
| 5 | Follow-up, **D** | Side dash right (10 studs, 0.25 s). No hit. | 0 | — | 0.47 |

- Held-key priority when several are held: S, A, D, W.
- Damages are placeholders.
- Per-step status effects live in `stepStatusEffects` (`WeaponMoves`). The explosion's Imbalance skips big monsters and bosses (`humanShapedOnly = true`).

## Passive: Trained Veteran
- The bayonet slash (step 2) inflicts **Vulnerable**, at most once every **8 s** per target (Vulnerable's default duration).
- **Missing** an attack gives **+20% move speed for 0.5 s**.

## Sounds
`RifleAim` on the shot and leap shot, `RifleShot` + `RifleShotBoom` when firing, `RifleDashCloth` on side dashes, `SwingSword` on the slash. The explosion borrows `CommandmentGroundSmashLight` (placeholder).

## F skill: BayonetCharge (`WeaponSkills`, the `DashSlash` branch)
A bayonet thrust dash, like Fahlon's Q.

| | |
|---|---|
| Animation | `rbxassetid://127743375990612`, 132 frames @60. Dash fires on **f37**, flies (body horizontal) until he lands on **f105** into a roll. No endlag: free on landing |
| Dash | ~34 studs over f37–f105, launching at 2× and slowing into the landing (`dashEaseOut`, average `dashSpeed` 30). Box 5×5×7 attached in front; each target hit once |
| Hit | 22, hybrid (the stronger of Physical/Magic Damage, like the M1s), stagger 2. Sounds: dash cloth + sword swing on launch, `HitBlade` on the first cut |
| Trained Veteran | The cut inflicts **Vulnerable**, sharing the slash M1's once-per-8-s-per-target limit (`VeteranVulnerableReady` attribute, read by both scripts) |
| Gunpowder | Marks what it cuts for **3 s** (`GunpowderMarks`; "GUNPOWDER" label over them). Your next **focused shot** that connects with a marked target explodes on it, even if the shot killed it: the S leap shot's ground explosion centred on the target (12-stud blast, 20 damage, stagger 2, Imbalance on human-shaped bodies, dizzy blur/flash on players, its look, sound and shake). One shot uses the mark up; only the marker's shots detonate it. Data: WeaponMoves `gunpowder = { duration, explosionStep }` |
| Cooldown | 8 s |

All numbers are PLACEHOLDERS.

