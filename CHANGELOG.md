# Changelog

Notable design/content changes. Newest first. Dates are when they landed in Studio (approximate before 2026-10-23).

## 2026-10-29
- Abominable Akavish body slam: the Dohv's GroundImpact effect, deadlier close (inner box 50%) and longer reach (outer box 84×92, 20%).
- Abominable Akavish: attack animations at Action4 priority; the post-swing freeze now waits for the clip's last frame and holds it.
- Abominable Akavish: moves picked at random among those in reach (the body slam had no real chance before).
- Abominable Akavish: Dohv-style welded hitboxes (bigger reach) decide and resolve its moves; it freezes 0.6 s in its swung pose after a swing.
- Abominable Akavish: red ground warning circles removed.
- Abominable Akavish: all parts massless + non-colliding, HumanoidRootPart invisible (template and AI).
- (Studio only, testing) The Abominable Akavish spawns at the Story entrance (`FORCED_ENTRANCE_EVENT_NAME` in PathProgressionScript).
- **Abominable Akavish** Sweep and Slam now use Lukas's left/right swing and body slam animations (hits timed off the clips).
- **Abominable Akavish Bite + Grab** combo (3 hits, holds the player at its jaw). Draft animation `AbominableBiteGrab` (KeyframeSequence) for Lukas to refine/upload; Studio plays the draft.
- **Abominable Akavish** uses the Akavish's Idle/Chase/Bite clips and now walks after players within a 70-stud leash of the cave. Its HumanoidRootPart was turned 180° to face its head (model pivot unchanged).
- **Test content is Studio-only**: the forced first path / Chastity event only apply in Studio; `DevContentGuard` removes `Workspace.DevTesting` and `LavaHazard`/`DevOnly` parts outside Studio; Convoke's fake test Soul only pulls in Studio; PVP Test mode is Studio or developers (`DevAccess`, now also in the Lobby).
- Dev: Lukas's account owns every Soul, weapon and Artifact (`GRANT_EVERYTHING`, topped up each join).
- **Soul Mastery** (0–10 per Soul, Story only): EXP from runs (banked with Essence, claimed in the Lobby), small bumps, Raam's Breaking Point + Liberating Cleave change, Bella's Steady Hands. Inner Core **Mastery tab** built. See [systems/soul-mastery.md](systems/soul-mastery.md).
- **Inner Core redesign** (Lobby): tab rail (Weapons / Armory / Mastery-locked), Soul name + stars + stats panel, roster of Soul boxes on the right, hard edges. New **Armory tab** + `ArtifactService` saving. New `3StarBorder` template. `ClassDefinitions.GetTitle` added; Lobby `ClassBaseStats` numbers mirrored from the Session (Luther's, Fahlon's and Bella's were stale).
- **Artifacts (the Armory)** planned: permanent 1–3★ items, 4-slot universal loadout, Story rewards only (not Convoke, not PVP), dupes sold for Essence, cross-run pity. `ArtifactDefinitions` (12 draft Artifacts) added to both places. See [items/artifacts.md](items/artifacts.md).
- Lore: the Ill Father Tree isn't dying but overgrowing on the wrong energy; Cleansers go wherever corruption appears.
- **Leg i-frames on jumps**: from a jump until landing, a player's legs can't be hit (part-based hitboxes skip them).
- **Combat tag** no longer disables jumping: in combat you can jump once a second.
- **The Chastity** fourth airborne attack, **Frost**: arena fog + slow blue highlight on players, then a cloudy-blue air wave; anyone who doesn't dodge it is frozen in ice blocks for 2.5 s. `ZuminKit.PlayStompDust` exported (any size/colour).
- Wind Slashes height check now uses the player's feet/head instead of the torso, so jumping over a slash actually clears it (knob `feetClearance`).
- Frost fog made clearly visible: puffs fade in to ~35% see-through (was ~99%), 450/s, less affected by arena lighting. Knobs `fogOpacity` / `fogFadeIn`.
- **The Chastity** third airborne attack, **Wind Slashes**: teleports to each arena corner and sends waves of huge Hayut-style slashes across the arena. `ProjectileVfxClient` can now draw a server-described projectile ("Spec").
- **Javelin of God**: fewer aimed lances (1 in 20, was 1 in 5) and the rest now cover the arena evenly (shuffled golden-angle spread) instead of random clumps.
- **Javelin of God**: lances doubled again to 112 a second (still 1 in 5 aimed at a player).
- **Sufferer** Hand Slam no longer shows the red ground circle.
- **SuffererEncounter** added to every corrupted path's event pool (1 in 5 per path); it only ever ran as an entrance test before.
- **Corrupted enemies pass through the Corrupted Roots** (Deceased, Host, Sufferer, Dohv(Corrupted), Abominable Akavish): new collision groups so they stop getting stuck. Players still collide with the roots.
- **Spawning stuck in / falling through the floor** fixed at the source: side lanes no longer land inside walls, the floor ray finds the real walkable surface near the entrance's height (skipping invisible/non-solid parts) instead of shooting from 150 studs up at the `Path` model only, and players are set at their real feet height + 0.75 instead of a flat 3 studs. **Off-path guard**: anyone far outside the path or fallen below it is teleported back to the path's start.
- **Tooltip ingredients** glow gold when you have them and are greyed out when you don't.
- **Item tooltips** now show what a tier 2/3 is made from (ingredient icons with counts) and tier-3 passives in a short form.
- **Finn limited in Story** (PVP unchanged): a rotating recipe book per Safe Haven visit (all tier 1, 5 random tier 2, 2 random tier 3 with one from your class section), plus **recipe pages** that keep a tier 3 in your book for the run: the Rehem lets you pick 1 of 3; side bosses 50% and elites 10% drop a random page.
- **PathVersion3 rarer**: random paths are now weighted by a `PickWeight` node attribute; PathVersion3 and its corrupted twin are 0.6 (~13% of picks, was 20%).
- **Levelling slowed down**: EXP to level 20 is now 37,720 (was 6,175, which a solo player had by path ~24). New per-level table fitted to simulated runs: normal play ends ~17, every extra step ~19, 20 in about a third of such runs. **Party EXP**: every standing party member now gets the full EXP of a kill (was a 70/30 killer/assist split).

## 2026-10-28
- **Level-up feedback**: LevelUp / LevelUpSpecial (every 5th level) sounds, a heartbeat pulse (Akhzarims Tenacity's) with screen shake and a burst of Raam's ultimate aura; lavender normally, pastel yellow and bigger every 5th level.
- **Dev (Studio only):** a "DEV  Level up" button (top-left) raises your level by one per click (`DevStudioTools` / `DevStudioButtons`; inactive outside Studio).
- **Story death rules**: solo gets 2 lives (refilled at Safe Havens); groups get 1 life each and fallen players stay down (spectating) until the party reaches a Safe Haven. Fallen players earn no EXP, Silver or Essence and aren't waited for at exits. Defeat (nobody standing / solo out of lives) shows results with **Retry** (fresh run, new server) or **Return to the Lobby**. Developers can toggle revives on (`DevAccess`, top-left button).
- **Welcome Essence**: every account gets 1650 once (new accounts on picking their first Soul, existing ones on their next join). The first Call 10 guarantees a 2★ weapon for the starting Soul (Raam SavageGlaive, Fahlon Crossbow, Bella BroadDagger; Luther none yet) and a Soul from the Gauntlet/Conjurer affinities (Zumin today). Fixed: the Weapons tab's 2★ pity and both 3★ pity counters weren't saved, so they reset on every rejoin.
- **Essence economy**: Convoke now costs **165** a pull / **1650** for 10 (was free); duplicates refund 15/40/165 by tier. Story runs pay Essence per player, kept on a loss: 10 per path, Rehem 300, Chastity 1650, Wyvern 100, Abominable Akavish 150, +50% flawless boss, 15 per elite, level × 10, first clear 1650. Banked in `PendingRunRewards`, claimed on joining the Lobby. Killing The Chastity ends the run: placeholder ending scene → results screen → back to the Lobby. See [systems/economy.md](systems/economy.md).
- **Javelin of God**: lances doubled again to 56 a second for more coverage; still 1 in 5 aimed at a player.
- **Javelin of God**: every 5th lance aims at a random player in the arena (still with its ground warning).
- **Javelin of God**: twice the lances (28 a second, was 14).
- **Javelin of God**: the flame ring faces straight down and is back to ~50 studs wide (no longer scaled with the bigger suction ball).
- **Javelin of God** fixes: the flame ring now faces like the Moon file (forward and 30° down) and turns slowly (~45°/s); it flickers every frame during the spasm (client-side, new `ChastityVfxClient`); the suction ball grows to ~100 studs (was ~37).
- **The Chastity** second airborne attack, **Javelin of God**: she writes the spell, a Divine-Servant-style suction ball grows above her, a ring of flames forms and it spasms, raining warned lances of light across the arena; lances shatter Earth Spikes pillars in their way. New VFX templates `JavelinAura`, `JavelinLance` (meshes from Unlimited Arts) and `JavelinImpact`.

## 2026-10-27
- **The Chastity's path 50 counts overall**: every path left counts toward it, detours and Safe Havens included (the Rehem's path-27 counter still skips detours).
- **The Chastity at path 50 without an arena:** until `FinalBossPath(TheChastity)` is built, she takes over whatever second-stage path comes up at path 50 (overriding its locked event). Earth Spikes skip spots with no floor under them.
- **Story run reset from testing:** the run no longer jumps straight from the entrance to the corrupted grounds with the Deceased (`FORCED_FIRST_PATH`/`FORCED_FIRST_EVENT` = nil), the entrance no longer forces the Sufferer event (empty again), and Luther's Tainted Rage cooldown is back to its real **18 s** (was 1 s for animation testing).
- **Ross** is a chance encounter again: back in the random event pool (weight 0.5), no longer guaranteed on the path before every Safe Haven.
- **The Chastity scheduled at path 50** (second stage): `PathProgressionScript` sends the party to `FinalBossPath(TheChastity)` once per run, like the Rehem at 27. The arena isn't built yet; until it exists the run carries on with random paths. The entrance preview is removed (back to the Sufferer test).
- **Earth Spikes** pillars doubled in size (×1.5–2.2 of `GroundPike`, ~83–122 studs tall); spots now keep 28 studs apart.
- **Earth Spikes** reworked: no ground outline, bigger dust, fewer spots (2 every 0.3 s), pillars slightly smaller and ease up instead of snapping, shake nearby screens, and **stay** as solid pillars that fill the arena until her descent shatters them all.
- **Earth Spikes** now use the `GroundPike` union (stood up along its new Base/Tip attachments), ~60–90 studs tall, no shadows, coloured to match the ground under each one.
- **The Chastity** first airborne attack, **Earth Spikes**: arena-wide waves of shaking spots (Deceased rise dust + a faint circle) that burst into big earth spikes for ~4 s. Her cast animation holds while they come.
- **The Chastity** head tracking: from the moment she lands until she flies back up, her head follows the nearest player in the arena (same neck math as Garrison/the Charm Seller).
- **The Chastity's descent**: bigger (default arena radius 90 → 135 studs) and now leaves Zumin's Devastation crater (the four cracked plates) at arena size for the 5 s slow zone (`ZuminKit.SpawnDevastationCrater`).
- **The Chastity's barrier**: always up. Hits on her no longer play the attacks' hit effects (M1 cut/blood VFX and hit sounds, ability hit VFX, ThornBurst blood, Zumin's Vengeance impact), airborne or grounded; the barrier glows and throws a burst of gold sparks on every hit. Applies to any model marked `Shielded`.
- **The Chastity** animated: Idle loops all fight; the Descent animation plays for her plunge, with the drop following its frames (slow pull down f0–45, fast f45–57, near-instant to the floor f57–59; the hit lands on f59; rings still give 1.6 s of warning). She then stays down playing the Idle.
- **The Chastity** (second stage boss, North Forest final boss) wired up: `ChastityAI`, `EncounterControllers.TheChastity`, `ChastityEncounter`. Untouchable while airborne; after 4 airborne attacks (placeholders) she descends with an arena-wide Devastation and stays grounded and attackable for 12 s, never staggering, then flies back up. Invisible gold shield that flashes on hit. Supporting changes: every hit bumps a `HitTick` on the target and `Untouchable` models show no damage numbers; a humanoid `ImmobilityImmune` attribute blocks Imbalance/Stun-type effects; Zumin's impact visuals exposed as `ZuminKit.PlayDevastationImpact` to play at any size.
- **Hit detection fix** (attacks slipping past the Dohv): lag compensation now replays the enemy's whole visible body instead of only its HitBox (the Dohv's only covered its legs and belly; enemies without a HitBox used a tiny 4×5×2 box). Bullets fly through invisible enemy parts and count on the enemy they struck even if it moved before they landed.
- **Dohv(Corrupted)** set up as the second stage's Dohv: same rig (scaled up 20%), moves and drops, 550 HP (Dohv 450), same damage; no howl, a random 1–3 s gap after each attack instead of the flat 4 s cooldown, its special 1 in 3, and every landed hit fills Infestation. Driven by new optional model attributes the Dohv's own scripts read (the normal Dohv is unchanged). New CorruptedDohvEncounter in every corrupted path's pool.
- **Abominable Akavish** wired up as the second stage's Wyvern: it waits in RejuvenatingField(Corrupted) (the cave is blocked by roots), which the post-Rehem detour now leads to. Placeholder brain until its kit and animations exist; boss bar; killing it unlocks the corrupted field's lake.
- **Rejuvenating Field lake**: press G at the water for a full heal (health and Essence) plus one level up. Needs the cave cleared; claimable once before and once after the Rehem, per player.
- **After the Wyvern** the party returns to the Rejuvenating Field it entered the cave from instead of the random path pool, and that field's CaveEntrance closes for the rest of the run.
- **Boss health bar** slimmed down: a thin bar in the same colour scheme as the enemy and player health bars (fill, gradient and plum outline read from the `EnemyHealthBar` template), the name small and left-aligned above it (no longer all caps), health numbers kept. No black strokes; text gets a soft purple shadow. The metal frame, diamond pommels, title ornaments and tenth ticks are gone.
- **Imperial Rifle**: the S leap shot's ground explosion now inflicts **Imbalance** for 1 s on human-shaped enemies (new per-step `stepStatusEffects` in `WeaponMoves`). Player victims also get ThornBurst's after effect (2 s dizzy blur, 0.6 s red flash).
- **Imbalance**: the ragdoll knockdown is now a named Immobility status effect (was the unnamed "Ragdoll" in a Physics category). Every ragdoll applies it, including Bella's Q knockdown; it shows as the marker above the head and ranks between Stun and Stagger.
- **Player stagger lowered** so stagger-based effects (The Commandment's Rumble) work in PVP: default MaxStagger 20 → **12** (Fahlon, Bella, Authan); Raam **15**; Luther 25 → **15**; Zumin **20** (highest, the tank); Hayut 15 → 10. It still grows with health.
- **Zumin's Devastation** is now a crowd-control tool rather than a nuke: 100/50 → **30 inner / 15 outer** damage. CC unchanged.
- Docs re-synced with Studio (current values only):
  - **Akhzarims Tenacity** (3★ Gauntlet, no model) and **Imperial Rifle** (3★ Artillery, hybrid, branching shot/follow-up M1) added to the weapon roster.
  - **Weapon passives** documented for every 3★: Inner Sensation, Rumble, SiphoningLife, Trained Veteran, Relentless Pursuit, OverflowingSynapse.
  - Weapon numbers updated: IllThorn M1 7/7/8, ThornBurst 12 + 3/mark on a 3 s cooldown; CleanserSilverGauntlets 5/5/8; DivineServant 10/10/11/19 and now 3★ in the Convoke pool; per-step swing cooldowns.
  - Bella's ×0.4 M1 penalty is gone (she deals full weapon damage).
  - PVP locks each Soul to its affinity's 3★ (was the best 2★).

## 2026-10-23
- Knowledge base created from a full survey of both Studio places and the design notes.

## Mid/late October 2026
- **IllThorn** (3★ Light) added: 3-slice M1 with Thorn Marks; F skill **ThornBurst** (steerable dash, grab-stab, burst scaling with marks, 4+ marks stagger, 6 marks True damage, ragdoll + dizzy on release).
- **The Commandment** (3★ Heavy war hammer) added: 5-hit combo with a light slam and a heavy leap slam; grey head trails; a dedicated sound set.
- **Second stage**: Infected enemies (Deceased, Host), the Infestation meter, the BrigandHost event, the Sufferer + a Cleanser squad (SilverFlash, Gauntlets, DivineServant spellcaster).
- **DivineServant** (Conjurer, torso-worn Opharim) and **SilverFlash** (first pullable 3★) added.
- **Rehem** boss fight and **Wyvern** boss in the Wyvern Nest. Akavish in the caves. FleeingBeasts before the Rehem.
- Convoke revamped: Souls/Weapons tabs, featured banners, 3★ pity at 70.
- Zumin (Gauntlet, Convoke-only), Authan (personal) and Hayut (dev) added. Session PVP built (tracks, Ambush, lives).

## Early October 2026
- Bella added as the fourth starter. Lobby menu, intro cutscene, Embracer class-selection scene, Character Page step 1.
- PVP mode designed. Lobby matchmaking built.
- Charm roster expanded to three tiers (11 / 13 / 20) with lore descriptions. Crafting at Finn.

## September 2026
- Core/Unique stat split, levels 1–20 with Mastery levels, Essence pool, Silver economy, shops, path/event system, safe havens, stagger, aggro race, morale, Luther added.
