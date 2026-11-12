# Roran (in progress)

- **Affinity:** Medium · **Default weapon:** the Medium default (StraightSword) · **Rarity:** 3★, the first **standard** 3★ Soul for Medium
- **Not a starter.** Meant for Convoke's standard pool, but **unreleased** for now: the Lobby's `ClassDefinitions` marks him `unreleased = true`, so Convoke skips him. The dev accounts get him through `GRANT_EVERYTHING`; in the Session he's on the DEV panel's Soul switch.
- **Title, lore, model:** none yet.
- **Status (2026-11-12):** Q, E and R built (filler visuals). His bare-handed Q / E and the passive aren't designed yet. No animations.
- **Identity:** a **Bleed** Soul like Luther. He's listed as a Hemorrhage contributor in `StatusEffects`, so his Physical hits feed a Hemorrhage bleed's burst.

| Stat | Value (all placeholders) |
|---|---|
| Health | 150 |
| PhysicalDamage | 25 |
| PhysicalResistance | 25 |
| MagicResistance | 15 |
| MaxStagger | 15 |
| Everything else | Shared defaults |
| Affinity Stat (Mastery kicker) | PhysicalDamage (placeholder) |

## Kit
Run by `ServerScriptService.RoranKit` (`kit = "RoranKit"`, registered in AbilityScript's `KIT_MODULES`). Visuals are code-drawn fillers in `StarterPlayerScripts.RoranKitClient`. All numbers are placeholders in `ClassAbilities.Roran`.

| Slot | Name | What it does | Numbers |
|---|---|---|---|
| **Q** | **Riposte** (placeholder name) | **Parry and counter**, a laid-back skill. Press Q for a **0.4 s parry stance**. The first hit that lands in it is **blocked**, and he **counters** the nearest enemy within 9 studs: damage plus a **Bleed**, his first Bleed source. Every **3rd successful parry in a row** also puts **Hemorrhage** on that bleed. After a parry he gets 0.25 s of i-frames so a multi-hit's next tick doesn't land. **No cooldown while he keeps parrying**: Q is ready again 0.3 s after a parry. **A missed parry** (the window runs out with nothing to parry) leaves him stuck 0.35 s, puts Q on a **5 s cooldown** and resets the streak. | Counter 10 Physical · stagger 2 · Bleed 5 s · 0 Essence · window 0.4 s · miss cd 5 s |
| **E** | **Precise Plunge** (placeholder name) | He lunges **12 studs** the camera's way in 0.18 s (anchored and moved by hand, stopping short of walls) and runs the **first enemy in his narrow path** through, stopping there. The plunge's own Physical hit, then **True damage** of **3% of the target's max health per Bleed stack** they carry (0–3 stacks), capped at **12%**. If **Hemorrhage** was put on them within the last **2 s**, it's the full **12% outright**. A miss is just the lunge. 0.2 s endlag. | 8 Physical + 0–12% max HP True · cd 8 s from the press · 15 Essence · path 3 wide · stagger 2 |
| Passive | — | Not designed | — |
| **Q** (bare-handed) | **Twin Haymakers** (placeholder name) | While his R is up, Q is **two wide punches** instead of the parry. Each punch **carries him 3 studs forward** (anchored and moved by hand, stopping short of walls) and hits everything in a **big box in front** (10 wide, 7 deep, 6 tall). **Every hit Dazes.** Filler animations: the Leather Gloves' two punches. Data `ClassAbilities.Roran.QBare`. | 8 Physical per punch · Daze · stagger 1.5 per punch · 0.3 s between punches · cd 6 s from the press · 10 Essence · 0.25 s endlag |
| **R** | **Bare Hands** (placeholder name) | He **throws his sword away** and fights bare-handed for **12 s**. He gets **extra max health equal to 25% of his Essence capacity** (max Essence; healed by as much on activation): +44 at level 1 (175 Essence), about +100 on an Essence-capacity build and **+15% Physical and Magic resistance**. Activating it sends a **burst of Magic damage** round him (radius 10). His **Q and E change** in this mode: Q becomes **Twin Haymakers** (above); E is still to design. The character's `RoranBareHanded` attribute (the server time it ends) and `RoranKit.IsBareHanded(character)` are what they'll check. **His M1s change too**: while it's up, `CombatScript` swaps whatever weapon he has for his own moveset (`R.m1Moveset`, the `RoranBareHandsMoveset` attribute). That drops the weapon's move effects (lucky hits and the like), and his F weapon skill is refused (`WeaponSkillScript`). For now the moveset is **`WeaponMoves.RoranBareHands`**: the Leather Gloves' two punches, empowered (10.5 damage instead of 6, stagger 1.2 instead of 0.8) and with no gloves shown, until he has his signature M1s. The bonuses come off when it ends, or at once if he dies. | Burst 15 Magic, r 10, stagger 2 · +25% of max Essence as HP · +150 units (15%) Phys / Magic Res · 12 s · cd 60 s from the press · 40 Essence · 0.35 s cast lock |

**How the parry is wired:** the stance is an i-frame window of a new kind, `PlayerInvulnerability.PARRY_KIND`. `PlayerDamageReduction.ApplyToDamage`, the shared damage choke point, blocks a hit landing in it and hands it to the handler the kit registered (`SetParryHandler`). It isn't a perfect dodge. The counter targets the nearest enemy, not necessarily the attacker, because the choke point doesn't know who hit. Damage that skips the choke point (bleed ticks, for example) isn't parried. `RoranKit` keeps real cooldowns even in Studio (`TEST_NO_COOLDOWNS = false`), because the miss cooldown is the mechanic.

**E's helpers:** `StatusEffectManager.GetBleedStacks(humanoid)` and `HadHemorrhageWithin(humanoid, seconds)`. `ApplyAmplifier` now stamps a `HemorrhageAt` server-time attribute on the target when Hemorrhage lands. The True % is of **max** health, so it hits bosses hard; this is a placeholder.

**Filler look:**
- **Stance:** a pale glint over him and a widening ring of light.
- **Parry:** a white flash, a red slash to the target and a spray of blood, darker and bigger on a Hemorrhage counter.
- **Whiff:** a dull grey flicker.
- **R:** his sword is hidden and a copy of it spins away behind him, a blood-red ring of force runs out to the burst radius, and a faint red glow stays on him while it lasts.
- **Bare-handed Q:** a wide sheet of air sweeps across the hitbox with each punch, tilted left then right, with a puff of dust at his feet.
- **E:** a pale streak behind him and a line of light down his path. On a hit, a spray of blood that grows with the %, plus a dark red flash and ring on a capped post-Hemorrhage hit.

## Where he's registered
| Place | Module | Entry |
|---|---|---|
| Both | `ClassDefinitions` | `weaponType = "Medium"`, `tier = 3` (+ `unreleased` in the Lobby) |
| Both | `ClassBaseStats` | the stats above |
| Both | `LevelDefinitions` | `AFFINITY_STATS.Roran = "PhysicalDamage"` |
| Both | `ClassAnimations` | all nil (default Roblox animations) |
| Both | `ClassAbilities` | the empty shape: E / Q / Passive / Ultimate = nil, so his skills do nothing |
| Both | `ItemTooltip` | `CLASS_DISPLAY.Roran` |
| Session | `CraftingBookDefinitions` | class section `Power` (placeholder) |
| Session | `ServerScriptService.Script` | `PVP_LOCKED_WEAPONS.Roran = "SilverFlash"` |

To release him: delete `unreleased = true` from the Lobby's `ClassDefinitions` entry.
