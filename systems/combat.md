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

## Infestation (Story only) (`PlayerInfestation`)
- A hidden meter that hits from **Infected** enemies fill. It's shown as roots growing over the health bar.
- Each hit fills `12.5 × (100 / maxEssence)^0.5` (8 hits at 100 Essence, 16 at 400) and also by damage share (3× the share of health taken).
- It drains after 3 s without hits, faster with more Essence.
- When full, the player dies on the spot and becomes a **Host** (`HostSpawner`). Solo, that's game over. Zumin's brace blocks build-up.

## Hit feedback
Damage numbers (`DamageIndicator`), enemy overhead health and status markers (`EnemyOverheadUI`), screen shake (`ScreenShakeScript`), blood hit reactions (`BloodSplatterVFX`), the boss health bar (`BossHealthBar`: a skinny bar across the top in the same colour scheme as the other health bars: the enemy bar's fill, gradient and plum outline, read live from `StarterGui.EnemyHealthBar`, plus the player bar's groove colour and the pink damage trail. The boss name sits small and left-aligned above it, with health numbers below. No black strokes; the text uses a soft purple shadow instead. One decorative notch. Shows while a listed boss is awake), combat music.
