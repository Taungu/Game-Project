# IllThorn

- **Tier:** 3★ · **Affinity:** Light · **Type:** blade
- The **first hybrid weapon**: its burst deals whichever damage type your build is stronger in (Physical or Magic).
- Reuses blood VFX assets from Lukas's "Unlimited Arts" place.

## M1: 3-slice combo
- Animation `83994177729760`, slices f0–42 / f42–56 / f56–90, hits at f11 / f47 / f80.
- Damage 8 / 8 / 9, with only **20% stat scaling** (`m1StatScaling = 0.2`). The M1s exist to place marks, not to kill. (~16.7 DPS raw, ~22.2 with the blood coat kept up.)
- Swing cooldowns 0.7 / 0.23 / 0.57 s.
- Hit sound `HitBlade`.

## Thorn Marks (`ServerScriptService.ThornMarks`, `ThornMarksClient`)
- Every slice that lands leaves **one Thorn Mark** on the enemy (**two** while the blade is blood-coated).
- Marks are **per player**: a teammate's slices don't add to your burst.
- Cap **6** (two full combos). They clear **6 s** after your last slice on that enemy.
- A 0.15 s same-slice guard stops one slice from adding several marks.
- Stored as attributes on the enemy model (`ThornMarks_<userId>`); your client shows them as a row of crimson diamonds above the enemy's head (`StatusOverheadClient`).

## F skill: ThornBurst
| Phase | Behaviour |
|---|---|
| Wind-up | A red **GrabTelegraph** flash. Animation `100122349063501`. |
| Dash | 45 studs/s for 0.55 s, **steered by your camera** while dashing. Uses Fahlon's Q dash VFX (without the WindPassage parts). Shows the stab frames while dashing. |
| Contact | A 4×5×3 contact box. On touching an enemy: stab at f33, impale at f37. |
| Grab | The victim plays `112074130587235` as a **GrabVisual double** held at 3.65 studs, facing you. Your facing is locked (`FacingLocked`); the lock-on camera respects it. Once the stab connects you **can't be hit** until the release (`holdInvulnerable`). |
| Impact | Shock + Wave rings, screen shake 0.35, and a quick **1.5-stud forward thrust that springs back** to the real distance. |
| Burst | After 0.45 s: **12 + 3 per mark** (30 at 6 marks, before stats) in your dominant damage type. Stagger 3. |
| Thresholds | **4+ marks**: force **Stagger**. **6 marks**: the burst becomes **True damage** with ×1.5 blood. |
| Release | Victim is knocked down (**Imbalance**) for 1 s. Players also get a dizzy blur (2 s) and a red flash (0.6 s). |
| Sounds | `ThanThrust` on the stab, `ThornBurstImpact` (`71770751712389`) on the burst |

Cooldown **4 s** (was 3; it breaks the guaranteed full-burst loop after a double-mark coat).

## Passive: SiphoningLife
**Every ThornBurst that lands** (any number of marks; it used to need max marks) heals you for **20%** of its damage and coats the blade in the victim's blood for **1.5 s** (was 2; about two coated slices). While coated, your slices swing **50% faster** (`m1SpeedBonus`: the whole swing (cooldown, hit timing, animation) takes 1/1.5 the time), reach **2.5 studs** further, deal **True** damage and leave **two Thorn Marks** each (`marksPerSlice`). The extra reach shows on the blade as a wave growing out past the tip (`BloodCoatExtension`).
