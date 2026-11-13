# Changelog

Notable design/content changes. Newest first. Dates are when they landed in Studio (approximate before 2026-10-23).

## 2026-11-13
- Roran Q counter: reach 16 studs (was 9). He now slides to the target during the swing-back, arriving on the slash and released after it, instead of snapping. The target is picked at the parry. Knobs `counterRange`, `slashDistance`, `slashReach`.
- Roran Q ripostes play at 1.5× speed (`counterSpeed`); the counter's hit moves with them (0.33 s / 0.52 s).
- Roran Q parry window cut to 0.2 s (was 0.4), as tight as the dodge's i-frame.
- Roran Q animations: the stance pose `103156680326170` is held for the parry window. A successful parry plays one of two ripostes at random (`70402213623639`, slash on f30; `105336066233942`, slash on f47), and the counter lands on the slash frame. He's protected and committed until it lands.

## 2026-11-12
- Roran bare-handed F **Earthfall** (placeholder name): during his R, F is a leap and ground slam, The Commandment's last hit bigger: 20-stud leap, 13-stud slam for 22 Physical, a bigger crater. Cooldown 12 s. Data `FBare`; WeaponSkillScript routes F to `RoranKit.BareSkill`.
- Roran bare-handed E **Skull Breaker** (placeholder name): during his R, E is a lunge that grabs the first enemy in his path, holds them 0.8 s, knees them in the head (12 Physical) and ragdolls them on release (Imbalance 1.5 s). Cooldown 10 s, 15 Essence. Data `EBare`.
- Roran bare-handed Q **Twin Haymakers** (placeholder name): during his R, Q is two wide punches. Each pushes him 3 studs forward and hits a 10 × 7 box in front for 8 Physical + Daze. Cooldown 6 s, 10 Essence. Data `QBare`.
- Roran R: the extra health now scales with Essence capacity: 25% of max Essence (+44 at level 1, ~+100 on a capacity build). Was a flat 40. Knobs `bonusHealth` + `bonusHealthPerEssence`.
- Roran R fix: his sword's swing trail no longer turns on during bare-handed M1s, and hit effects no longer spawn at the hidden sword (`CombatScript` ignores the weapon visual while bare-handed).
- Roran R now also swaps his M1s for his own moveset (`WeaponMoves.RoranBareHands`: the Leather Gloves' punches, empowered to 10.5 damage, no gloves shown) and turns off the weapon's effects and F skill while it lasts.
- Roran R **Bare Hands** (placeholder name): he throws his sword away for 12 s, gaining +40 max HP (healed), +15% Physical / Magic resistance and a 15 Magic burst (r 10) on activation. His changed Q / E are still to design (`RoranBareHanded` attribute). Filler visuals: the sword hidden and thrown, a red ring and a glow.
- Roran E **Precise Plunge** (placeholder name): a 12-stud lunge into the first enemy in his path. 8 Physical, plus True damage of 3% max HP per Bleed stack (cap 12%), or the full 12% if Hemorrhage landed on them in the last 2 s. New `StatusEffectManager.GetBleedStacks` / `HadHemorrhageWithin` (`HemorrhageAt` attribute).
- Lobby Character Page fix: the Soul roster's scroll area stopped short (`AutomaticCanvasSize` gave ~630 px for ~1,100 px of rows), so the last owned Souls (Ziel, Roran) couldn't be reached. `CharacterPageController` now sizes the canvas from the list layout's real content height.
- Roran Q **Riposte** (placeholder name): a parry and counter. The parried hit is blocked, and the nearest enemy is countered with a Bleed; every 3rd parry in a row adds Hemorrhage. No cooldown unless the parry whiffs (5 s). New `RoranKit` / `RoranKitClient` (filler visuals). New i-frame kind `PlayerInvulnerability.PARRY_KIND` and `PlayerDamageReduction.SetParryHandler`. Roran is a Hemorrhage contributor.
- New Soul **Roran**: an empty shell, Medium affinity, 3★ (the first standard Medium 3★). Registered in both places with placeholder stats and an empty kit. Unreleased in the Lobby, so he's not in Convoke yet. See [characters/roran.md](characters/roran.md).
- New OverStatus **Impaired**: an upgraded Slow with the same half speed plus no dodging at all (dodge, roll, air dash, skill dodges), 2 s by default. Ziel's E now Impairs instead of Slowing (`E.slowEffect`). StatusEffects is mirrored to the Lobby. See [systems/combat.md](systems/combat.md).
- Immobility statuses (Stun, Daze, Aerial, Stagger, Imbalance) now lock all player actions until they wear off: M1, E/Q/R (every kit), dodge, roll, air dash and jump. Server and client both check. F was already gated. See [systems/combat.md](systems/combat.md).
- Zumin R (Devastation): only the inner circle (r 18) Dazes now. The outer ring is only Slowed (3 s). Before, the whole 36-stud shockwave Dazed everyone in it.
- Ziel Q can be redirected during the throw: the range is locked on release, but the direction follows his camera until impact. His circle follows on his screen, the chain bends toward the new spot, and the hit lands where he's aiming at impact. Knobs: `Q.redirect`, `Q.redirectRate`.
- Ziel's Q circle and E rectangle are drawn only on his own screen. Others still see the whirling chain and the ring of flames.
- Ziel E marks: the texture itself now glows hot yellow: the decal's colour is pushed past white (`markGlow` 4) and pulses gently. The glow copy and the floor light are gone, and so are the round lit patches. Marks are 11 studs across (was 6).
- Ziel passive heal raised: 20% max HP over 4 s (was 5%).
- Ziel animation fixes: his idle and walk run at Action priority, so his skill tracks were blending half-and-half with them and looked weak. They now sit above: Q hold Action2, Q slam / E cast / R Action3, prayer Action2. R `spinTime` is 0.35 → 0.5, so the spin plays at full speed (visual only). The R chains draw in as he's pulled toward the maces instead of reaching past them into the ground, and the maces rest on the surface.
- Ziel's `ClassAnimations` (both places): idle `76861990864361`, jump `75265926269215`, passive `70581799346883`, walk `122948945550363`. Death is still the default.
- Ziel R spin: the Cleave's air slashes (`SlashAir`, `SlashAir2`) are skipped too.
- Ziel tuning: the R spin's Cleave slashes are now 0.55× size and much fainter (`faint` 0.55, Raam's own fade and glow). The E marks' heat glow is now the image itself, tinted yellow and a little larger, instead of a neon disc, so there are no flat yellow circles.
- Ziel passive **Quiet Absolution** (placeholder name): every 15 s out of combat he prays (animation `70581799346883`). A fire on his left hand goes into his heart at f27, he flashes pastel yellow, and he heals 5% max HP over 4 s. Only combat or death cuts it; his skills don't, so he can flee with the R and keep healing. Data: `ClassAbilities` Ziel `Passive`.
- Ziel E marks: much bigger (6 studs, was 2.6) with a hot yellow glow and light under each, breathing until the burn ends. The smouldering burn emits 4× more (24/s) at 20% of the hit's speed so it stays low.
- Ziel R spin: plays Raam's Liberating Cleave slashes on arrival, without the blood slash, recoloured gold (dark slashes deep gold, bright ones pale) and held more solid. Knobs in `Ultimate.vfx.cleave`.
- Ziel E look: each lance leaves Lukas's image `17067103637` on the ground (replacing the dark scorch circles) and its impact keeps smouldering there, fainter, until the burn ends. The orange glow box under the burn is gone. Knobs in `E.vfx`: `markTexture`, `markTransparency`, `smoulderTransparency`, `smoulderRate`.
- Ziel R uses Lukas's animation: body `100958304389046` plus the one-chain "Ziel R Attach and Sp" track (`ZielChainMotion.TWIN`), played on two chain copies (`ZielsTwinRight`, `ZielsTwinLeft`, the left one mirrored). The maces are fitted to the real landing spots and pinned during the pull; the phases are stretched to the R's timing. A Q or E out of the spin cuts it.
- Ziel E uses Lukas's cast animation `82165859076474`: held on frame 0, played on release, with a golden swing trail and the glyphs Ranah · Ithav · Lumet · Tzimra · Marek · Akhar (Javelin of God without Ovin). The lances fly on f14 (data: `castAnimationId`, `fireFrame`, `swingTrail`, `spell`).
- Ziel Q throw uses Lukas's slam: the body animation `133182986549336` plus the chain's "Ziel Q slam" tracks (`ZielChainMotion.SLAM`); the last link stretches so the mace lands on the circle at any range. The mace lands at 37/60 s (was 0.4). The chain gained bridge beams between links.
- **Ziel's Q uses Lukas's chain.** The code-drawn lasso is replaced by a real chain (`ClassWeapons.ZielsChain`, five links on his right arm with golden beams, trails and the mace) spun exactly as animated, from the motion copied into `ZielChainMotion`, while his body animation `106989997206174` plays. The drawn lasso stays as a fallback.
- **Ziel: two bursts per ultimate.** His burst came back far more often than Bella's, so each burst is now smaller and he gets two per R, together matching her one (126 base). Q 14 → 12 outer (35 → 30 inner), cd 10 → 14. E 14 → 12 (burn 7 → 6), cd 8 → 12. R charge window 10 → 25 s, long enough to burst, retreat, wait for the Q and go back in.

## 2026-11-11
- Ziel's R: he can act the instant he lands (it was blocked for the maces' 0.35 s swing), so the landing combos straight into a Q or an E.
- **Ziel balance** (weighed against Bella): Q 32 → 14 outer, inner ×1.5 → ×2.5 (48 → 35): the burst lives in the sweet spot. E 26 → 14 (burn 13 → 7): CC and set-up first. R plant 8 → 3, flourish 18 → 7 (78 → 30 over three charges): mobility first.
- Ziel: he can walk at half speed while holding the Q (`holdWalkMultiplier`), and the R no longer holds him after landing (no endlag, no body spin; the maces still swing round him).
- Ziel's **R** (placeholder name Twin Penance): three charges. Two chained maces plant 16 studs out (8 Magic along each chain), he's pulled to them and spins once (18 Magic, radius 10.5). The ability bar can now hide a slot's charge count.
- Ziel's **E** (placeholder name Lesser Javelin): hold to aim a 30 × 8 rectangle, release for a flurry of lances (26 Magic over 5 waves, Slow 2 s), then the ground burns 3 s for 50% of the move's damage. The Chastity's Javelin of God, small and fast.
- Ziel's Q: an **inner circle** (radius 3, where the mace's head lands) deals ×1.5, shown on the indicator; the slam leaves a crater of rocks around it and a few little craters nearby (`innerRadius`, `innerMultiplier`, `vfx.crater`).
- **Ziel** is the "Gifted Sinner", and has his Q (placeholder name Penitent's Flail): hold to aim a circle that slides away from him (8 → 42 studs over 2.2 s), release to throw a chain flail whose mace slams down on it (32 Magic, radius 9). New `ZielKit` + `ZielKitClient`; cloudy-yellow visuals drawn in code, no animations.
- **Ziel**, the first Conjurer Soul: the basics wired in both places (identity, placeholder stats, empty kit and animations, Affinity Stat, PVP lock to DivineServant). Unreleased: a new `unreleased` flag in the Lobby's `ClassDefinitions` keeps him out of Convoke and the welcome Call 10. See [characters/ziel.md](characters/ziel.md).
- Zumin's stored-Vengeance steam turned down so it stays semi-transparent: at full, 10 puffs a second per vent at 0.7 transparency (was 30 at 0.35). `ClassAbilities.Zumin.Q.steam`.

## 2026-11-10
- ConjuredPebble keeps only 25% of the Magic Damage bonus (`m1StatScaling = 0.25`): it was doing too much, and mages aren't meant to deal a lot of M1 damage.
- ConjuredPebble's look: a clump of square chunks instead of a smooth ball, a small air wave off the hand at the throw, and a trail behind it in flight (`conjure.chunks`, `throwWave`, `trail` in `WeaponMoves`).
- **ConjuredPebble** (placeholder name): the 1★ Conjurer weapon and the affinity's default. No model: a pebble grows in the right hand for 0.5 s and leaves it on f46 of the throw animation (one throw per second); 7 Magic, level with the Bow. Reuses the rifle's projectile path with a new `conjure` table; drawn by `ConjuredPebbleClient` (CombatScript sends `ConjureGrow`). In the Lobby it has no tier yet (not in Convoke).
Docs caught up with Studio. The dated entries below (3–5 Nov) come from dated notes in the code. The ones here were found in the survey without a date.
- New weapon pages: [SilverFlash](weapons/silverflash.md) and [Akhzarims Tenacity](weapons/akhzarims-tenacity.md).
- **IllThorn**: ThornBurst cooldown 3 → 4 s; the wielder can't be hit from the stab until the release; the blood coat lasts 1.5 s (was 2) and coated slices leave two Thorn Marks. The coat's extra reach shows on the blade (`BloodCoatExtension`).
- **Late-game Health growth**: from level 11 Health grows at ×0.8 instead of ×0.4 (level 20 = ×10.9 base, was ×7.6), so late fights aren't decided by a few M1s.
- **Soul ability power**: class abilities deal +4% per level above 1 (L20 ×1.76); weapons don't get it.
- **Status chips above heads** (`StatusOverheadClient`): every status effect on enemies and players, plus your own Thorn Marks, replacing the old on-body text.
- **Roll and air dash** documented in [combat](systems/combat.md#dodge): double-tap roll (9 studs), air dash (6 studs, no i-frames, once per jump, 2 s cooldown), dark-blue mobility flash, limb streaks.
- Screen shakes now also show under Shift Lock and lock-on (`CameraShakeState`).
- Fixed-size UI text follows the screen height on every device (`UIScreenFit`). Ambient wind streaks around your own character (`WindStreaksClient`).
- SilverFlash FlashMode cooldown 13 s (was 16, then 10). Akhzarims Tenacity GutPunch cooldown 5 s (was 10).
- Noted: the Lobby's `WeaponSkills` / `WeaponMoves` copies don't have the 3★ skills or the 4 November numbers. No default (1★) Conjurer weapon exists yet.

## 2026-11-05
- **Zumin's Brace and Vengeance is a double tap**: releasing Q ends the brace and stores the soaked damage; a second tap unleashes Vengeance whenever he likes, and the cooldown starts then. He vents steam while a charge is stored, thicker the more he soaked (`ZuminSteamClient`).
- **Akhzarims Tenacity F: GutPunch.** A gut punch (18) that stuns for the victim animation's length, then Slows 2 s. A target left under 5% health is executed: impaled on the right arm, then thrown behind. Bigger monsters are just stunned or killed.
- Relentless Pursuit: an execution sends the heart racing (the +20% damage bonus for 5 s, a beat every 0.4 s).

## 2026-11-04
- **3★ balance pass**: every 3★ now sits at about the Imperial Rifle's ~21.8 M1 DPS with passives counted, Heavy a little above. SilverFlash 10/10/11/11/16 → 11/11/12/12/17. IllThorn 7/7/8 → 8/8/9. The Commandment 16/18/15/16/30 → 22/25/21/22/(23 + 18 Magic). DivineServant 10/10/11/19 → 14/14/15/25. Akhzarims Tenacity 9/9/11/11/16 → (6+5)/(6+5)/(7+6)/13/19.
- **Every 3★ should be hybrid in some way** (new design rule, see [weapons](weapons/README.md#design-rules)).
- **SilverFlash**: Inner Sensation's True hit scales off Physical and Magic added together; Magic from charms counts ×3.5 on its Magic counterpart.
- **SilverFlash F: FlashMode.** 2 s: +100% move speed, every M1 a lucky swing, every incoming hit dodged. The mode's M1 damage is held back and lands at the end at 50% (`FlashModeStash`). Half-inverted world, souls shown through bodies, frozen afterimages.
- **Akhzarims Tenacity** goes hybrid: hits 1–3 are part Physical, part Magic (same totals); each heartbeat sends out a Magic pulse (20 base, 7 studs); Magic from charms quickens the heartbeat. The server now times the heartbeat (`WeaponHeartbeat`).
- **The Commandment** goes hybrid: the leap slam is 23 Physical + 18 Magic and raises a small ring of spikes; CraterSlam's inner circle stays Physical while its spikes deal Magic (charm Magic ×3.5). The short-lived magic-into-physical conversion is gone.
- **CraterSlam is charge and release**: hold F to the top of the windup, release to slam. The charge sets size (×0.35–1) and damage (×0.5–1); no stat-based size growth. The wielder flashes white as the charge starts.
- The **roll** gets a dodge frame of its own (0.2 s; a perfect dodge counts).

## 2026-11-03
- **PVP Test is open to everyone** (it used to be hidden outside Studio for non-developers).
- Fahlon's Retreating Leap i-frames 1 s → 0.25 s (PVP balance: only the takeoff is safe).

## 2026-11-02
- DEV Soul switch fixed and opened to every mode: the Session's `ClassDefinitions` lacked `GetAllClasses` (the Lobby has it), so the Soul list never loaded. Added it; the switch is no longer limited to PVP Test.
- DEV panel: **Soul switch** in PVP Test (and Studio). Pick any Soul mid-run; you respawn on the spot as it. Session only, never saved.
- IllThorn buff (passive, not numbers): SiphoningLife now triggers on every ThornBurst that lands (no longer needs 6 marks), and coated slices swing 50% faster for the coat's 2 s.
- Vulnerable protection: a target made Vulnerable can't be made Vulnerable again for 30 s (any source, refreshes included; `StatusEffects.Vulnerable.reapplyCooldown`).
- Imperial Rifle ~12% weaker now it has its F skill: shot 24 → 21, bayonet slash 18 → 16, leap-shot explosion (and the gunpowder blast) 20 → 18, Bayonet Charge 22 → 19.

## 2026-11-01
- The run (fast walk) plays faster the further the speed buff is past its trigger (+40% → 1.17×, capped 1.6×), keeping the legs in step with the ground (`ClassAnimations.GetFastWalkRate`).
- Mobility charms halved: Congregant's Combat Wear 10% → 5% MoveSpd, Cleansing's Way of Flash 25% → 12.5% MoveSpd, Core Outbreak 10% → 5% MoveSpd (before its Essence scaling) (descriptions updated; prices and Cleansing's 20% speed-burst passive unchanged).
- **Run flourish**: while the run (fast walk) animation plays, arms and legs carry the dash's limb trails and the body leans a little into turns / strafes (max 8°). `RunFlourishClient`, `MovementTuning.RunFlourish`.
- Footsteps finally follow the feet: steps play at each heel strike (a leg at its furthest-forward point). The earlier versions measured foot height, which never changes on an R6 walk, so they always fell back to distance-based steps.
- Footsteps: timing reworked (they tapped rapidly). A step is now the feet trading places under the body, twice per walk cycle, with a jitter dead-zone and a speed-scaled minimum gap.
- Open PVP lobby: everyone readies up (Ready / Unready, names show who's ready). The countdown runs only with 2+ in and everyone ready; a new joiner or an un-ready pauses it.
- **PVP: one open lobby** instead of hosting/joining parties. Pressing PVP joins the lobby server's open lobby (up to 6); with 2+ in, a 20 s countdown (5 s when full) ends in random teams and a teleport into one match. New window `PVPLobbyFrame` in the game's style; knobs `SessionConfig.PVPLobby`.
- Finn / the Seedling craftsman: PVP detection made stricter so PVP always gets the full recipe library (GameMode "PVP", PVP Test, or any player on a PVP team), never the Story book.
- PVP: losing an Ambush now sends the team to its own Safe Haven (SafeHaven / NavatSafeHaven) instead of back to its entrance, which could lock the Navats out of the path system.
- Footsteps now follow the animations: a step plays when a foot touches down in whatever walk is playing (each Soul's walk, the fast walk, the default walk), instead of every 3.6 studs (kept only as a fallback).
- **Footsteps** by ground (`FootstepSounds`, `FootstepClient`): grass, dirt, gravel, cave stone and wood, chosen from the ground's Material (override with attribute `FootstepSurface`), several variations each, 3D for other players. The entrance's Rock unions are tagged Gravel. Roblox's default running sound is muted.
- Banner showcase: the wind is now only the wave, running horizontally as a real 3D beam behind the banner (the flat UI images showed the textures' black background as a bar; the streak layer is gone). Weapons lie with their broad face to the camera (SilverFlash showed its edge).
- **Convoke banners fixed**: the featured item no longer takes 50% of its tier; it's just 2× as likely as any other item inside its tier (`FEATURED_WEIGHT`), tier rates unchanged. Souls banners can't give 3★ weapons and Weapons banners can't give 3★ Souls (`OTHER_KIND_MAX_TIER` = 2).
- **Banner showcase**: banners show the featured Soul's rig (idle) or weapon (flat, floating) over drifting wind beams in the item's colour (`ITEM_THEMES`, placeholder colours) instead of banner art.
- Convoke's Called Forth cards now show each pull in its rarity border with a live view (Soul rig idling / weapon spinning), name and status below, instead of plain name boxes. New shared `ItemBoxVisuals` module (Lobby).
- Convoke banner restyled to match its neighbours: translucent gradient body (lighter at the top, denser at the name plate) and a softer, thinner border, instead of a solid slab with a bright white frame.
- Character Page: the background behind your character is blurred (depth of field focused on the copy, fades in on open, out on close).
- Fix: Convoke's "Called Forth" cards were invisible. The results fade-in also faded the card template, so the cards were cloned see-through. UIMotion now never fades templates (`...Template`, `...Templates`, or `UIMotionSkip`; the Character Page's `WeaponSlot` got the attribute).
- Button hover sound toned down (`UIButtonSounds`, both places): volume 0.35 (was 0.6), at most one every 0.15 s, silent when re-entering the same button within 1 s, and sweeping across several buttons fades each tick softer (×0.7, down to 35%).
- **UI motion** (`UIMotion`, both places; see [systems/ui-motion.md](systems/ui-motion.md)): the Soul whisper's smoothness across the UIs. Shop, merchant sell, small inventory, settings, buff badge, NPC dialogue (lines write themselves in, replies pop in turn), Convoke (banner flips, fading entrance, card reveal with names writing in), Character Page (tab cross-fades, quotes write in), and every lobby panel/button eases in when shown (`UIMotionAuto`).
- Soul whispers reanimated: a soft plum haze, an accent line that grows from its middle with a moving sheen, the name drifting in, the line written letter by letter (each fades and rises in). It fades away instead of vanishing: the letters dissolve left to right over 1.4 s while the block drifts up, and the haze lingers.
- **Soul whispers**: the equipped Soul speaks on the right side of the screen to hint at the cave (entering a Rejuvenating Field), the lake (after the Wyvern / the Abominable Akavish die) and the Abominable Akavish ("Good luck with that."). Data in `SoulWhisperLines`, sent by `SoulWhisper`, drawn by `SoulWhisperClient`.
- **Imbedded Tree Totems** no longer sink and resurface at other platforms (whack-a-mole removed). Each rises once when a player comes near and stays stuck at its tree until it dies. In return the thorns fly twice as fast (`THORN_SPEED` 120, was 60; `THORN_LIFE` 1.2 s).
- **Hit kinds** (`ReplicatedStorage.HitKinds`): player weapon M1s and weapon skills are tagged **Cut** or **Blunt**. Blunt deals +5% to armored targets (Physical Resistance 100+), cut +5% to armorless ones (under 100). Magic gets no extra (it already ignores physical armor). Arrows, bolts, rifle shots and the rifle explosion are untagged. Applied in `PlayerCoreStats.ApplyResistance` (new 6th arg `hitKind`) before resistance; True damage skips it.
- Getting hit now plays one of the existing hit markers by the hit's damage type (Physical: `HitBlade`, Magic: `OpharimLanceImpact`, True: `ThornBurstImpact`) at 70% of its loudness, instead of the pitched-down fist sound (`PlayerHurt`, removed). `SoundEffects.GetVolume` added.
- BayonetCharge's landing dust now kicks up as he touches down (f103, `dustAt`), where he lands. It used to wait for the end of the roll (f132), since the dash now carries through it.
- BayonetCharge holds full speed (48 studs/s) through the whole flight and only slows during the landing roll, stopping at the animation's end (f132): ~62 studs, busy until f132 (`dashHoldTime`, `dashStopPower`; replaces the front-loaded ease).
- **BayonetCharge** (Imperial Rifle F): steers with the camera from the press through the thrust (it used to lock to your facing at launch); much faster launch (144 studs/s, front-loaded ease `dashEasePower` 3, ~41 studs, was 60 studs/s / ~34); the air dash's ring wave, 2.2× bigger, at the start of the thrust (`thrustWave`, drawn by CameraLockOnScript, kind "Thrust"). `GetLongestSteerWindow` now counts a dash's windup.
- Overhead bars bigger (6 studs wide, was 4.2; 2.6 above the head, was 2.2). The break is one quick red flash (0.1 s) that bursts straight from the red.
- Overhead bar break reworked (the red tint didn't read and the halves didn't land): the bar now flashes as a solid bright-red silhouette 3 times, then bursts into pieces in its own colours that fly out and fall.
- **Overhead bar break on death**: flashes red, shakes, then snaps in two and the halves fall away (CanvasGroups). The DevTesting dummies (`PlayerStyleBar = "Enemy"`) now show the enemy player bar (EnemyOverheadUI skips them) and play `PlayerKill` when it breaks, to simulate a PVP kill.
- Overhead bars: the **border** marks enemies, not the fill. Allies lavender (the HUD's), enemy players a dark-red border (plate, rims, crest recoloured; groove kept plum for readability). Fill is the HUD red for everyone.
- **Player overhead health bars**: other players show a copy of the HUD health bar over their head instead of Roblox's default (allies HUD red, enemy players in PVP darker blood red). `PlayerOverheadBars`, `OverheadBarSettings`.
- **Fix: the Rehem getting stuck on small bumps.** All 31 of its parts were solid (torso, neck and the dragging tail snagged on any rise, and with no legs it couldn't step up). `RehemBossAI.Start` now makes only its HumanoidRootPart solid and the rest non-colliding + massless, like the Dohv / Wyvern / Akavish, and floats it at HipHeight (at least `MIN_HIP_HEIGHT` = 2). Its body-contact hits check part boxes, so they're unchanged.

## 2026-10-31
- PVP boss messages rewritten as felt lines (no mechanics), nothing for the team that reaches the boss: "The grounds rumble in the wake of the beast of darkness" etc. (`BOSS_LINES` in `MatchCountdownController`).
- **PVP: The Chastity uses the Rehem's claim rule** (claim, 30 s to contest, locked out after, carry on once she's slain; her death doesn't end the match). A team that wins the contest Ambush now gets the boss raised for it (it used to leave with the losers' event). Both-sides-wiped resets the race. **Boss messages** for claimed / out of reach / slain, both teams.
- **Fix + PVP rule: claiming the Rehem.** A team that lost the fight on the Rehem's path was stuck on first-stage paths forever (its boss was marked done, but it never left the boss path, so it never reached the second stage). Now the first team due claims the Rehem; the other team has 30 s to contest it (an Ambush with the Rehem in it); after that, or after losing that Ambush, it's locked out and moves on to the second stage once the Rehem is slain. See [systems/pvp.md](systems/pvp.md).
- Pebbles cover the middle half of the path's real width (measured per cluster against the side barriers), leaving the edges bare; ~180 stones over the last 45 studs of Path version 5. Knobs `pebbleWidthShare`, `pebbleWidthProbe`, `pebbleWidthMax`, `pebblesPerStud`.
- Pebbles spread wider: 2–4 per cluster (was 1–3), scattered 3 studs either side widening to 7 at the exit (was 1.4), ±2 studs along the path.
- Pebbles only on the last 45 studs before each exit (`pebbleLastStretch`), a little closer together (every 5 studs, was 7).
- Guide wind rides all the way to the exit (`windToExit`: each streak lives until it arrives, was ~40 studs), 8 streaks at once (was 4).
- Guide wind stays by the player: slower (15 studs/s, was 26), starts 10 behind to 4 ahead of you and blows past (was 4 behind to 10 ahead), so it no longer races out of sight.
- Guide wind made visible: it reused the ambient streak's fade (~25% solid at best) at 75%, so it peaked near 19%. Now its own shape at 60% solid, 1.8× wide, brightness 1.6, warmer gold, 4 streaks living 3.2 s (`windPeakOpacity`, `windWidthScale`, `windBrightness`).
- **Path guide**: a trail of pale pebbles along the walking route to every exit (always), and pale-gold guide wind drifting toward the exit once a path's fight is over. Client-only per player (`PathGuideClient`), tuning in `PathGuideSettings`, exits published by `PathProgressionScript`. See [systems/run-structure.md](systems/run-structure.md).
- **The Chastity writes Javelin of God in Opharim glyphs**: Ranah · Ithav · Lumet · Ovin · Tzimra · Marek · Akhar appear one by one, right to left, in a line in front of her during her writing (frames 12–62), drawn like DivineServant's letters (1.6× size). Data in `ChastityAI` `JAVELIN.spell`; `WeaponMoves.GetGlyphTexture` exposes DivineServant's glyph list; `DivineServantClient` draws any spell sent as `OpharimSpell`.
- Docs: **the Opharim's language** written up (root and modifier glyphs, right-to-left grammar, example spells, DivineServant's incantations, glyph image ids) in [lore/world.md](lore/world.md#the-opharims-language), plus a [DivineServant page](weapons/divine-servant.md). Lore only, no Studio change.

## 2026-10-30
- BayonetCharge now pulls the holstered rifle into his hands for the skill (weapon skills call `WeaponVisuals.DrawFor`, like the M1s; no-op for weapons without a holster).
- **Imperial Rifle's F skill: BayonetCharge** (placeholder name). A bayonet thrust dash (fires f37, lands f105 into a roll, no endlag). The cut triggers **Trained Veteran** (Vulnerable, sharing the slash's 8 s per-target limit) and marks the target with **gunpowder** for 3 s: your next focused shot that connects explodes on them with the S leap shot's ground explosion. New `GunpowderMarks`; `DashSlash` gained `dashEaseOut`, `hybridScaling`, `dashSounds`, `hitSounds`, `veteran`, `gunpowderMark`. See [weapons/imperial-rifle.md](weapons/imperial-rifle.md).
- CraterSlam **inner hit**: anything within 4 studs of where the hammer lands (grows with size) takes ×1.5 damage (`innerRadius`, `innerMultiplier`).
- CraterSlam now triggers **Rumble**: every hit on a staggerable target smashes for +20% with the BluntHit burst (`rumble = true` on the skill entry).
- CraterSlam: size scaling halved (×0.25 per Physical Damage, cap 1.4, was ×0.5 / 1.75); spikes faster (eruption 180 studs/s, was 90; rise 0.1 s, was 0.18).
- **The Commandment's F skill: CraterSlam** (placeholder name). The hammer slams down on frame 100 of its new animation and the ground erupts into a crater of GroundPike spikes: a ring around the hammer and a cone reaching further forward. Everything inside takes 20 Physical (scales with Physical Damage) and is staggered without fail; the area and spikes grow with Physical Damage (up to 1.75×). Cooldown 10 s. See [weapons/the-commandment.md](weapons/the-commandment.md).
- **Polish pass: sound wiring.** New `SoundEffects` entries: `Dodge` (every dodge, from the dodger), `ImbalanceFall` (body-fall thud whenever anyone is knocked into Imbalance), `PlayerHurt` (only the hit player hears it, off a real health drop, throttled, tiny DoT drops skipped; `PlayerFeedbackSounds`), plus empty slots (`rbxassetid://0`, silent until given an id) for `PlayerDown`, `EssenceAbsorb` (your EXP orbs arriving) and `SafeHavenHeal` (stepping into a heal zone). Pro Sound Effects picks are first guesses.
- **Cast sounds for every class/key**: `ClassAbilities` `CAST_SOUNDS` + `GetCastSound`, played by new `AbilityCastSounds` beside every `ItemPassives.OnAbilityUsed` (AbilityScript, BellaKit, ZuminKit). Placeholders so far: Raam's Unchained (The Commandment's ground smash), Fahlon's Retreating Leap and Zumin's Fimble Decision (the rifle's dash cloth). Raam's E keeps its own `castSound`. The rest are listed in the table's comment as still silent.
- **UI consistency pass**: buff badges (`BuffUI`, both places), combat tag, PVP countdown text, the merchant's close X, and runtime-built text (status labels over enemies, inventory counts and popups, drag ghost, hotbar hide arrow) moved off Roblox defaults (SourceSans/Gotham, grey boxes, black outlines) to Accanthis and the Inner Core plum palette (`#1C1228` panels, `#3C294F` edges, `#EDDEFF` text, `#FFD678` gold). Outlines that stay for readability are soft deep plum, not black.
- **Hit flash**: every hit a player lands flashes its target white (`HitReaction` → `HitFeelEvent` → `HitFeelClient`). Not on dodges or Shielded/Untouchable targets. Knobs in `HitFeel`.
- **Hitstop**: when an M1 connects, the swing and the struck enemy freeze briefly, then play faster for `catchUpTime` so the swing still ends on schedule (`HitStop`, server-side). Per-weapon lengths in `WeaponMoves` `HIT_STOPS` (Dagger 0.04 s … The Commandment's leap slam 0.14 s; ranged weapons none); crits ×1.4; big bosses don't freeze (`HitFeel.noVictimFreeze`). Every place CombatScript pauses or re-speeds a track on purpose cancels a running hitstop first.
- **Fix: ~5,300 stray UI sounds saved into SoundService** (Session 5,108, Lobby 188). `SoundEffects` built its UI sound pool whenever it was required on a "client", and Studio's Edit mode counts as one, so every Edit-mode require (plugins, assistant checks) piled up another batch that got saved. The pool now only builds in a running game and its sounds are `Archivable = false`. Unused `SE_TEST` copy moved to ServerStorage.
- Results screens (cleared and Defeat): rewards appear one by one, the level reward included: each slides in and counts its Essence up into the total, then the Mastery bar appears and fills. Knobs in RunEndUI: `REVEAL_START`, `REVEAL_COUNT_TIME`, `REVEAL_GAP`, `MASTERY_AFTER_SHOW`.
- Mastery results bar: slower fill (3 s per level), a sweeping glint and sparks off its edge; a level-up bursts the bar (swelling bright copy that fades, ring of sparks).
- **Mastery on the results screen**: both end screens (cleared / Defeat) fill your Soul's Mastery bar with the run's EXP, popping on each level-up and naming the level 5 / 10 unlock.
- **Fix: dashes into trees** (Fahlon's Q on Path version 5): the dash check was one thin ray from the body's centre that stopped at the first leaf it touched. It now sweeps a body-wide sphere and skips non-colliding parts.
- **Reset button = unstuck**: in runs, Reset puts you back at the start of your current path instead of killing you (8 s cooldown). See [systems/run-structure.md](systems/run-structure.md).

## 2026-10-29
- **Fix: enemies spawning on the roof** of paths without a `Path` folder (e.g. Path version 5 (corrupted)): the floor ray started 150 studs up and hit PathSurroundings ~140 studs overhead, so the Corrupted Dohv (and any event there) landed on top of the cliffs. Those nodes now start the ray 40 studs above the spawn spot (PathProgressionScript `floorRayLift`, DohvCharge `hasFloor`).
- Dev: Marcus's account also owns every Soul, weapon and Artifact (`GRANT_EVERYTHING`).
- **Soul Mastery goals**: Bella 10 (instant, longer Injection), Fahlon 5 (Royal Haste) and 10 (two Marking charges), Zumin 5 (Settled Ground armor) and 10 (E cancels Brace and Vengeance).
- **Settings menu** (both places): top-left button; master/music/SFX volume, screen shake, damage numbers, FPS counter; saved per account. Music now has its own SoundGroup. See [systems/settings.md](systems/settings.md).
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
