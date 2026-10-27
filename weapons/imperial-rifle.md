# Imperial Rifle

- **Tier:** 3★ · **Affinity:** Artillery · **Type:** rifle with a bayonet
- A **hybrid** weapon (`hybridScaling`): its M1s deal whichever damage type your build is stronger in, like IllThorn's burst.
- Holsters on the back after 3 s idle.
- F skill: none yet.

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
