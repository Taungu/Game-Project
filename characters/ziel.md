# Ziel: "Gifted Sinner" (in progress)

- **Affinity:** Conjurer (the first Conjurer Soul) · **Default weapon:** ConjuredPebble · **Rarity:** 2★ (placeholder)
- **Not a starter.** Meant for Convoke, but **unreleased**: the Lobby's `ClassDefinitions` marks him `unreleased = true`, so the pull pool and the welcome Call 10 skip him. The dev accounts get him through `GRANT_EVERYTHING`; in the Session he's on the DEV panel's Soul switch.
- **Lore:** the Ziel of *Ziel's Thrash*. He had every quality of an Inquisitor, yet left the Cleansers to live in peace as the priest of a remote village. He fought with a flail, and every strike he sent from afar pierced deep.
- **Model:** none (like Zumin, you play as your avatar; no Lobby preview rig).
- **Status (2026-11-11):** basics wired; Q, E and R built. No passive or animations yet.
- **Look:** a former Cleanser's magic, **cloudy yellow**. Nothing is animated; the kit's visuals are drawn in code (`ZielKitClient`).

| Stat | Value (all placeholders) |
|---|---|
| Health | 120 |
| Physical / Magic Damage | 0 / 25 |
| Everything else | shared defaults (CDR 10, Essence 100 base, MaxStagger 12, no resistances) |
| Affinity Stat | Magic Damage (placeholder) |

## Role
A ranged caster whose damage is his **Q**, and specifically its inner circle. The **E** is crowd control and set-up (the Slow and the burning ground hold enemies where the Q will land); its full damage only comes from an enemy that eats every wave and stays in the burn. The **R** is mobility: chase down a low enemy and finish them, or escape on the free charges.

| Base damage | Bella | Ziel |
|---|---|---|
| Q | 18 (27 Curdle, up to 36 as a finisher), cd 8 | 14 outer / **35 inner**, cd 10 |
| E | 36 over 5 s, cd 12 | 14 flurry + 7 burn = 21, cd 8, Slow 2 s |
| R | 36 burst + 36 aura | 3 × (3 + 7) = 30, three dashes |

## Kit
Runs in its own module, `ServerScriptService.ZielKit` (registered in `AbilityScript`'s `KIT_MODULES`); numbers in `ClassAbilities.Ziel`; visuals in `StarterPlayerScripts.ZielKitClient`. All numbers are placeholders.

| Slot | Name | What it does | Numbers |
|---|---|---|---|
| **Q** | **Penitent's Flail** (placeholder name) | **Hold:** he walks at **half speed** (no jumping) and aims with the camera (like Fahlon's R) while spinning a chain flail above his head like a lasso. A circle on the ground slides away from him the longer he holds; everyone sees it. **Release** (or full range, where it throws itself): the chain is thrown and the mace slams down on the circle, hitting everything inside. The **inner circle**, where the mace's head lands, hits **×2.5** (measured flat to the nearest part of the body); the indicator shows it as a solid gold spot. The ground breaks into a crater of rocks around the inner circle with a few little craters around it (visual only). A stun, stagger or grab during the hold breaks the cast (Essence spent, cooldown starts). Once thrown, the mace lands regardless. | 14 Magic (35 in the inner circle, radius 3) · radius 9 · range 8 → 42 over 2.2 s of holding (0.3 s minimum windup) · lands 0.4 s after release · 0.35 s endlag · cd 10 s from the throw · 20 Essence · stagger 3 |
| **E** | **Lesser Javelin** (placeholder name) | The Chastity's Javelin of God on a much smaller, faster scale. **Hold:** planted and aiming like the Q; a **rectangle** on the ground shows where it lands while a small ring of flames forms above him. **Release** (or 3 s, where it fires itself): a flurry of lances of light rakes the rectangle in 5 waves. The first wave that lands **Slows for 2 s**. The scorched rectangle then **burns for 3 s**, dealing 50% of the move's damage in total to whoever stays in it (Magic, no crits, no stagger). Broken by a stun, stagger or grab during the hold. | 14 Magic over 5 waves · rectangle 30 × 8, starting 2 studs in front · 0.25 s minimum hold · flurry 0.5 s · burn 7 over 6 ticks · 0.25 s endlag · cd 8 s from firing · 15 Essence · stagger 2 |
| **R** | **Twin Penance** (placeholder name) | **Instant, three charges.** He faces the camera and throws two chained maces forward, one from each arm, leaning 14° outward. They **plant** in the ground (no slam): everything along either chain takes the first hit (two boxes, hit once even if caught by both). A beat later he's **pulled** along the chains to where they landed, and the **flourish** hits everything round him, a little bigger than the Q's circle. He's **free to move and act the moment he lands**, so the landing combos straight into a Q or an E (or the next charge): the maces swing once round him and follow him as he goes, then fade away. The first press spends the Essence and gives 3 uses within 10 s (the R slot shows the count); the cooldown starts when they're used up or the window ends. Anchored and uncancellable from the throw to the landing. | Plant 3 Magic (each chain: 16 long, 3 wide) · flourish 7 Magic, radius 10.5 · pulled ~15.5 studs over 0.25 s · ~1 s per use · cd 60 s · 40 Essence · stagger 1 / 2 |
| P | — | Not designed yet. | — |

**How the Q is wired:** the hold is the character attribute `ZielFlailCharge` (the server time it began), which every client draws the lasso and circle from; the caster's own client turns him to the camera and streams it as `QSteer`; the throw is the `ZielFlail` event.

**How the E is wired:** the hold is the attribute `ZielJavelinCharge`; the caster's client streams the camera as `QSteer` and reports letting go as `ERelease` (a new branch in `AbilityScript` → `ZielKit.ReleaseE`); firing is the `ZielJavelin` event. The visuals reuse The Chastity's `JavelinAura`, `JavelinLance` and `JavelinImpact` from `VFXModels`.

**How the R is wired:** `ZielKit` keeps the charges per player; each use fires `ZielChains` (both landing spots and the timings) for every client to draw, and moves the anchored character by hand for the pull, letting go on landing. The charge count uses the ability bar's existing `AbilityCharges` message (sending no count now hides it). `FreezeWatchdog` leaves a character with `ChargingSkill` alone.

## Where he's wired
| Place | What |
|---|---|
| Session + Lobby | `ClassDefinitions`, `ClassBaseStats`, `ClassAbilities`, `ClassAnimations` (empty), `LevelDefinitions` (Affinity Stat), `ItemTooltip` (display name) |
| Session only | `CraftingBookDefinitions` (section "Destruction", placeholder), PVP weapon lock → DivineServant |
| Lobby only | `unreleased = true` + `ClassDefinitions.IsReleased`, checked by Convoke's pool and the welcome guarantee in `ServerScriptService.Script` |

Still to do: the rest of the kit, animations, charm recommendations, Soul Mastery goals, essence colour and pick animation, Convoke banner colour, a Lobby preview rig if wanted, then remove `unreleased`.
