# Changelog

Notable design/content changes. Newest first. Dates are when they landed in Studio (approximate before 2026-10-23).

## 2026-10-28
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
