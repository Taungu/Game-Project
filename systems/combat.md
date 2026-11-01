# Combat

## Controls
| Input | Action |
|---|---|
| M1 | Weapon combo (`M1InputScript` → `CombatScript`) |
| E / Q / R | Class abilities (`AbilityInputScript` → `AbilityScript`, or the class's kit module) |
| F | Weapon skill (`WeaponSkillScript`) |
| Right click | Dodge (`CameraLockOnScript` client slide + `DodgeServerScript` i-frames) |
| Lock-on | `CameraLockOnScript` (respects Grabbed / FacingLocked) |

## M1 combos (`WeaponMoves`)
- Each weapon defines combo **segments** (slices of one animation or separate animations), **hit delays**, per-hit **damage**, **cooldowns**, **hitbox offsets/sizes**, **stagger points** (per step allowed), and swing/hit **sounds** and VFX.
- Optional per-step movement: **lunges**, **leaps** (an arc that aims at the real ground), **ground slams**, **step VFX** and screen shake. While swinging, a slow is applied to the wielder (e.g. ×0.5 for The Commandment).
- `m1StatScaling` sets how much of your damage stat an M1 uses (IllThorn 0.2).
- **Attack Speed** shortens time between swings. **Fahlon ×1.14** (also copied by Authan and Hayut) is the only class multiplier on M1 damage; everyone else is ×1.0.
- Server-side lag compensation for hits on moving enemies: `EnemyHitRewind`. It replays where each enemy's **whole visible body** was a moment ago (its HitBox plus every visible part attached to it, measured once at spawn), not just its HitBox.
- A hit counts on an enemy's `HitBox` part or any **visible** part; invisible parts (like the Dohv's attack zones) never count, and bullets fly through them.
- **Bullets** (Imperial Rifle): the enemy the bullet's cast struck counts as hit on arrival even if it moved off the impact spot meanwhile (up to 30 studs away).

## Hit kinds (`HitKinds`)
What a weapon hit physically is, on top of its damage type. Applied in `PlayerCoreStats.ApplyResistance` (6th arg) before resistance; judged by the target's raw Physical Resistance points. True damage skips it.

| Kind | Bonus | Weapons |
|---|---|---|
| **Cut** | +5% vs armorless (Physical Resistance < 100) | Dagger, BroadDagger, StraightSword, SavageGlaive, GreatAxe, IllThorn, SilverFlash, Than, Akhzarims Tenacity steps 1–3, Imperial Rifle slash (step 2) + Bayonet Charge |
| **Blunt** | +5% vs armored (100+) | WarHammer, The Commandment, LeatherGloves, CleanserSilverGauntlets, Akhzarims Tenacity, SilverFlash steps 3–4, Than step 2 |
| Untagged | none | Bow, Crossbow, rifle shots and explosion, DivineServant (magic already ignores physical armor) |

- Per-step kinds follow each step's authored hit sound. Weapon skills use the weapon's base kind (`SKILLS` overrides: Imperial Rifle → Cut).
- Bonuses are PLACEHOLDERS (`SETTINGS`). Class abilities and enemy attacks aren't tagged yet.
- Armored by that line: Brigand Medium/Heavy, Tree Totems, Dohv, Wyvern, Abominable Akavish, Chastity, Rehem, Host, Sufferer. Armorless: Akavish, Zev, Light Brigands, Corrupted Dohv, Deceased.

## Abilities (E / Q / R)
- Data in `ClassAbilities`; behaviour in `AbilityScript`. Bella and Zumin run their own kit modules.
- Each has a **cooldown** (reduced by CDR), an **Essence cost**, a **busy duration** that locks out other actions, damage type, stagger points, status effects and VFX.
- Abilities belong to the **character**; weapon skills belong to the **weapon**.

## Dodge
- Right-click slide. The server grants a short **i-frame** window (a perfect dodge).
- A **streak system**: several dodges back to back, then a cooldown. The streak resets if you wait.
- Some kits replace it: Authan's Plunging Footwork (Zumin's dash), and BroadDagger's EnhancedDodge as its F.

## Stagger (`StaggerSystem`)
- A universal meter on **every** humanoid, players included. Every hit adds stagger points.
- When it reaches **MaxStagger**, it resets to 0 and the target is **Staggered**: interrupted and locked out for **1 s** (red flash on players). There's no passive decay.
- Enemy MaxStagger comes from `EnemyDefinitions` (bosses can be `math.huge`). Players' comes from their class, and **grows with health** (square root).
- Some moves **force** a stagger (IllThorn at 4+ marks). Enemy **BREAK** moves stagger players in one hit.
- Stagger is the highest-priority immobility effect: "no one is safe from being staggered".

## Status effects (`StatusEffects`)
| Effect | Category | Default duration | What it does |
|---|---|---|---|
| **Stun** | Immobility | 2.5 s | Full interrupt. The heaviest control. |
| **Daze** | Immobility | 0.75 s | Can't move. |
| **Aerial** | Immobility | 0.8 s | Launched upward (50 studs/s), then held. |
| **Imbalance** | Immobility | 2 s (callers set their own) | The knockdown: thrown off balance and ragdolled onto the floor, paired with a screen blur. Applied by every ragdoll in the game: grab releases (Light Brigand, Bella's Q, ThornBurst, the Host), Dohv's finisher, the Rehem and the Wyvern. Outranks Stun, ranks under Stagger. |
| **Stagger** | Immobility | 1 s | The stagger payoff. Top priority, can't be preempted. |
| **Vulnerable** | OverStatus | ≤15 s | Bursts early after taking 10% max HP of damage (+5% max HP bonus damage). Fahlon's mark. |
| **Bleed** | OverStatus | 5 s | DoT of 0.5% max HP per second, up to 3 stacks. |
| **Sick** | OverStatus | 20 s | Akavish poison: the bite's damage ×3 over its duration. |
| **Blind** | OverStatus | 3 s | Outward-facing: the blinded character's hits still connect but deal no damage and apply no statuses ("Miss"). Self-effects are untouched. |
| **Slow** | OverStatus | 1 s | 50% walk speed. Refreshes, doesn't stack. |
| **Rooted** | OverStatus | 1 s | Can't walk but can still attack. Wins over Slow. |
| **Weakened** | OverStatus | 5 s | Deals 20% less (strength can vary; the strongest wins). |
| **Sensitive** | OverStatus | 5 s | Takes 20% more from everything. |
| **Hemorrhage** | StatusAmplifier | — | Turns Bleed into a pool of 10% of physical hits, burst at the end (Luther). |

- **Immobility** effects fight for control of WalkSpeed. Only one is in control at a time, by priority (Daze < Aerial < Stun < Imbalance < Stagger), and it shows as the marker above the head. Imbalance's ragdoll physics always happens, even when Stagger holds the marker.
- **OverStatus** effects are tracked independently, can stack with each other, and render on the target.
- **Immobilizing** (for resist passives like Zumin's): Stun, Daze, Aerial, Imbalance, Stagger, Rooted.
- `"Ragdoll"` is the old name of Imbalance and still works as an alias in `StatusEffectManager.Apply`. Death ragdolls (`RagdollCorpse`) aren't a status effect.

## Grabs
Several moves grab and hold a target: the Light Brigand, Bella's Injection, the Host's Invading Grab, the Wyvern's talon, IllThorn's ThornBurst. Grabbed players see a GrabVisual double, get screen blur/flashes, and the camera respects `Grabbed`. Dodging through the catch moment makes a grab miss. Zumin's brace is immune.

## Aggro & combat tag
- **Aggro race:** the first hitter takes aggro; after that, whoever lands **5 hits** first takes or keeps it (`EnemyAggroManager`).
- **Combat tag:** while enemies that noticed you are alive, you can only **jump once a second** (`CombatTagManager` sets `InCombat`; the client's `CombatJumpCooldown` limits the jumps).
- **Leg i-frames:** from a jump until you land (max 2.5 s), your **legs can't be hit**: their CanTouch/CanQuery are switched off (`JumpLegIFrames`), so part-based enemy hitboxes miss them. Torso, arms and head still count, and attacks measured to the body's centre still land.

## Infestation (Story only) (`PlayerInfestation`)
- A hidden meter that hits from **Infected** enemies fill. It's shown as roots growing over the health bar.
- Each hit fills `12.5 × (100 / maxEssence)^0.5` (8 hits at 100 Essence, 16 at 400) and also by damage share (3× the share of health taken).
- It drains after 3 s without hits, faster with more Essence.
- When full, the player dies on the spot and becomes a **Host** (`HostSpawner`). Solo, that's game over. Zumin's brace blocks build-up.

## Hit feedback
Damage numbers (`DamageIndicator`), enemy overhead health and status markers (`EnemyOverheadUI`), screen shake (`ScreenShakeScript`), blood hit reactions (`BloodSplatterVFX`), the boss health bar (`BossHealthBar`: a skinny bar across the top in the same colour scheme as the other health bars: the enemy bar's fill, gradient and plum outline, read live from `StarterGui.EnemyHealthBar`, plus the player bar's groove colour and the pink damage trail. The boss name sits small and left-aligned above it, with health numbers below. No black strokes; the text uses a soft purple shadow instead. One decorative notch. Shows while a listed boss is awake), combat music.

- **Player overhead bars** (`PlayerOverheadBars`, tuning in `OverheadBarSettings`): every other player gets a live copy of your HUD health bar over their head (plate, groove, fills draining toward the crest, white damage chips) instead of Roblox's default bar. The red fill is the same for everyone; the **border** tells sides apart: allies keep the HUD's lavender border, enemy players (the other team in PVP) get it recoloured dark red (`enemyBorder`: every piece keeps its own light/dark shading, turned red). The groove behind the fill stays plum on both, so the health stays readable. **On death the bar breaks:** the whole bar flashes solid bright red once (0.1 s) and bursts straight from the red into 22 pieces in its own colours that fly out, spin, fall and fade (0.85 s) (`breakFlash*`, `shard*` knobs). Test dummies with attribute `PlayerStyleBar = "Enemy"` (TankDummy, BellaDummy) get this bar instead of the monster bar, and their break plays the PVP kill sound (`dummyBreakSound`) so a kill can be simulated in Studio. Restyling the HUD bar restyles these too.
- **Hitstop** (`HitStop`, tuning in `HitFeel`, lengths in `WeaponMoves` `HIT_STOPS`): once per swing, on the first real hit, the attacker's M1 track and the struck enemy's animations freeze, then play faster for `catchUpTime` to win the time back, so hitboxes, slice ends and cooldowns (all on CombatScript's clock) stay in sync. Server-side because M1 tracks are server-played. Ranged weapons have none; PVP victims and `noVictimFreeze` bosses don't freeze.
- **Hit flash** (`HitReaction` → `HitFeelEvent` → `HitFeelClient`): a short white Highlight on anything a player's hit lands on; throttled per enemy (`flashMinGap`) so DoT ticks don't strobe.
- **Feedback sounds**: `Dodge`, `ImbalanceFall`, getting hit plays an existing hit marker by damage type (Physical `HitBlade`, Magic `OpharimLanceImpact`, True `ThornBurstImpact`; `HURT_SOUNDS` in `PlayerFeedbackSounds`), `PlayerDown`, `EssenceAbsorb`, `SafeHavenHeal`; per-skill cast sounds via `ClassAbilities` `CAST_SOUNDS` (`AbilityCastSounds`).

## Run flourish (`RunFlourishClient`, tuning `MovementTuning.RunFlourish`)
- **Run pace:** the run plays faster the further your movement-speed buff is past its +20% trigger: `rate = 1 + (speed / triggerSpeed − 1) × rateScale`, capped at `maxRate` (`ClassAnimations` FAST_WALK; rateScale 1 keeps the legs in step with the ground). +30% → 1.08×, +40% → 1.17×, +60% → 1.33×, +100% → 1.6×. Footsteps follow automatically.
While a character's **run animation** plays (the shared fast walk, `ClassAnimations.GetFastWalk()`, swapped in by `ClassMovementAnimations` once the movement-speed buff passes its threshold; 3★ Souls have none):
- its **arms and legs carry the dash's limb trails** (`BodyTrail`'s texture and lavender colour, a touch softer: 0.5 start transparency), on for as long as it runs and fading 0.25 s after;
- the body **leans a little into its turns** (2.2° per rad/s of turning) and toward sideways movement when strafing on lock-on (7° fully sideways), capped at **8°**, easing in and out.
- Every client draws it for every player character. The lean is a roll on the root joint's **C1**, so it never fights the air dash spin (which uses C0). "Moving" is read from the character's real speed, so other players' runs show too.
