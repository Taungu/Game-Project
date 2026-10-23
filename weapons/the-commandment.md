# The Commandment

- **Tier:** 3★ (the first 3★ Heavy weapon) · **Affinity:** Heavy · **Type:** war hammer
- **Model:** `WeaponModels["The Commandment"]`. The grip `Value` CFrameValue sits on the model with the `AsC1` attribute (both places).
- One Moon Animator animation (`rbxassetid://133268594507338`) is sliced into a **5-hit M1 combo** with two ground slams: a light one, and a heavy leap slam that cracks the ground.
- F skill: none yet.

## Combo
| Step | Slice (frames) | Hit frame | Damage | Notes |
|---|---|---|---|---|
| 1 | 0–60 | 40 | 16 | Lunge 3 studs |
| 2 | 60–115 | 100 | 18 | **Light ground slam** |
| 3 | 115–193 | 150 | 15 | Lunge 3 studs |
| 4 | 193–233 | 213 | 16 | Lunge 3 studs |
| 5 | 233–350 | 276 | **30** | **Leap** (f243 → lands f276) into a **heavy cratering slam**, offset –8. Stagger doubled (8.8). |

- Stagger 4.4 per hit, 8.8 on step 5. Swing cooldown 1.0 s.
- Slows the wielder to ×0.5 while swinging.
- The leap arc aims at the real ground (raycast up to 30 studs) so it handles slopes.
- **Trails:** 4 grey four-sided trails on the hammer head.

## Sounds (`SoundEffects`)
- Swing: `CommandmentSwing`
- Hit: `CommandmentHitBlunt` at maximum volume (10). Other layers were turned down 60% so the hit cuts through.
- Slams: `CommandmentMetalImpact` (+ `Light` version for step 2), `CommandmentGroundSmash` (+ `Light`), `CommandmentRubble`. The first slam plays at half loudness.
