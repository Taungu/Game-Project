# Akhzarims Tenacity

- **Tier:** 3★ · **Affinity:** Gauntlet · In the Convoke pool
- **No physical model** (`noModel`): the fists are the weapon. Like the Opharim, VFX make up for it: every 5–9 s the wielder's eye flares and the body pulses (the **heartbeat**).
- **Hybrid:** the sharp hits are part Physical, part Magic (like Than); the kicks stay Physical. The heartbeat is its magic side.
- F skill: **GutPunch** (placeholder name), see below.

## M1: 5-hit combo
One animation sliced into 5 hits: an impale, two claw swings, two kicks. Hit 5 is a lunging slam along the ground.

| Step | Hit | Damage | Swing cd (s) | Stagger |
|---|---|---|---|---|
| 1 | Impale | 6 Physical + 5 Magic | 0.35 | 1.3 |
| 2 | Claw | 6 Physical + 5 Magic | 0.55 | 1.3 |
| 3 | Claw | 7 Physical + 6 Magic | 0.6 | 1.3 |
| 4 | Kick | 13 Physical | 0.37 | 1.3 |
| 5 | Lunging slam | 19 Physical | 1.45 | 2.6 |

- Was 9 / 9 / 11 / 11 / 16, all Physical, before the 2026-11-04 balance pass.
- Hit kind: Cut on steps 1–3, Blunt on the kicks.

## Passive: Relentless Pursuit (`pursuit` in `WeaponMoves`)
- **Endlag:** you recover from your own endlag **twice as fast**, and leap 8 studs (0.25 s) the way you're moving (WASD) the moment you're free. Shared handling in `EndlagRecovery`.
- **Heartbeat** (`WeaponHeartbeat` on the server, drawn by `WeaponIdleVfxClient`): each beat grants **+20% damage for 3 s** and sends out a **Magic pulse**: 20 base Magic to every enemy within 7 studs, scaled by Magic Damage. The pulse borrows the look of Bella's Peak Health, much smaller and very see-through.
- **Magic quickens the heart:** the gap to the next beat is divided by `1 + (Magic Damage from charms as a %) × 0.7`, never below 1.5 s. With about +250% charm Magic the 5–9 s gap becomes about 1.8–3.3 s.
- **The heart races:** an execution with GutPunch gives the +20% damage bonus for **5 s** outright, with the heartbeat's eye flare and body pulse every 0.4 s (no extra magic pulses).

## F skill: GutPunch (`WeaponSkills`, kind `GutPunch` in `WeaponSkillScript`)
| | |
|---|---|
| Animations | Punch `rbxassetid://90863615115717` (68 frames @60), execution `rbxassetid://130642616276848` (99 frames), victim `rbxassetid://118350216046434` (107 frames). The fist lands on **f20**. |
| While punching | The wielder walks at ×0.6 speed |
| Hit | 18 Physical to the closest enemy in a 4×5×4 box in front |
| Stun | The target is **stunned for the victim animation's length** (107/60 s), then **Slowed for 2 s**. Human-shaped (R6) bodies play the victim animation, parked 2.6 studs in front and turned to face you; bigger monsters are just stunned. |
| Execution | A target the hit leaves **under 5%** of its max health is executed. The wielder switches to the execution animation; an R6 body is ragdolled and **impaled on the right arm** through the stomach, then thrown behind at f74 and killed. Anything else is simply killed. |
| Feel | Screen shake on the punch, the impale and the throw; a crimson tint on the wielder's screen; a 0.09 s hitstop on the impale; ThornBurst's blood burst at full size; sound `GutPunchImpale` |
| Cooldown | **5 s** (was 10) |

All numbers are PLACEHOLDERS.
