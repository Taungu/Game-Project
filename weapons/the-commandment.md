# The Commandment

- **Tier:** 3★ (the first 3★ Heavy weapon) · **Affinity:** Heavy · **Type:** war hammer
- **Model:** `WeaponModels["The Commandment"]`. The grip `Value` CFrameValue sits on the model with the `AsC1` attribute (both places).
- One Moon Animator animation (`rbxassetid://133268594507338`) is sliced into a **5-hit M1 combo** with two ground slams: a light one, and a heavy leap slam that cracks the ground.
- F skill: **CraterSlam** (placeholder name), see below.
- Passive: **Rumble**: a hit that staggers an enemy, or lands on one already staggered, smashes in as a heavy blunt blow for **+20%** damage.

## Combo
| Step | Slice (frames) | Hit frame | Damage | Notes |
|---|---|---|---|---|
| 1 | 0–60 | 40 | 16 | Lunge 3 studs |
| 2 | 60–115 | 100 | 18 | **Light ground slam** |
| 3 | 115–193 | 150 | 15 | Lunge 3 studs |
| 4 | 193–233 | 213 | 16 | Lunge 3 studs |
| 5 | 233–350 | 276 | **30** | **Leap** (f243 → lands f276) into a **heavy cratering slam**, offset –8. Stagger doubled (8.8). |

- Stagger 4.4 per hit, 8.8 on step 5. Swing cooldowns per step: 1.0 / 0.92 / 1.3 / 0.67 / 1.95 s.
- Slows the wielder to ×0.5 while swinging.
- The leap arc aims at the real ground (raycast up to 30 studs) so it handles slopes.
- **Trails:** 4 grey four-sided trails on the hammer head.

## Sounds (`SoundEffects`)
- Swing: `CommandmentSwing`
- Hit: `CommandmentHitBlunt` at maximum volume (10). Other layers were turned down 60% so the hit cuts through.
- Slams: `CommandmentMetalImpact` (+ `Light` version for step 2), `CommandmentGroundSmash` (+ `Light`), `CommandmentRubble`. The first slam plays at half loudness.

## F skill: CraterSlam (`WeaponSkills`, kind `CraterSlam` in `WeaponSkillScript`)
The hammer slams down and the ground extrudes into a crater of spikes (`VFXModels.GroundPike`) that reaches further in front.

| | |
|---|---|
| Animation | `rbxassetid://90935231435216`, 143 frames @60. Hammer lands on **f100**; busy until f118; ×0.2 speed while swinging |
| Impact | Offset (–0.9, 0, –8), the leap slam's spot. Draws the leap-slam crater (`groundSlams[5]`), its three slam sounds, caster screen shake |
| Area (size 1) | Ring radius 8 + forward cone 26 studs long, 60° wide; ±8 studs height. Big enemies count by their body size |
| Eruption | Runs outward at 180 studs/s: spikes and hits further out land later |
| Spikes | 7 in a ring leaning out 40°, 4 cone rows (2, 3, 4, 5) leaning 28°; scale 0.14 → 0.26 of GroundPike. Ground-coloured, no collision, rise in 0.1 s, stand 1.6 s, then sink |
| Hit | 20 Physical (scales with Physical Damage, crits, resistances as usual) + **guaranteed stagger** (`StaggerSystem.ForceStagger`; stagger-immune bosses and Zumin's brace excepted, dodges respected) |
| Size | `1 + (physical multiplier − 1) × 0.25`, capped at 1.4: scales the area and the spikes |
| Cooldown | 10 s |

Cancelled if the wielder is stunned/staggered/ragdolled before f100. All numbers are PLACEHOLDERS.

**Synergy with Rumble** (`rumble = true`): every crater hit on a staggerable target is a Rumble smash (+20%, BluntHit burst), checked before the damage like the M1s. Stagger-immune bosses don't trigger it.
