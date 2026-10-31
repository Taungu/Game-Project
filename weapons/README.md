# Weapons

A weapon provides the **M1 combo** and, at 2★+, an **F weapon skill**. Every 3★ weapon also has a **weapon passive**. It can only be equipped by a Soul of the same **affinity** (Authan can use any). Data: `WeaponDefinitions` (identity/tier), `WeaponMoves` (M1 data and weapon passives), `WeaponSkills` (F skills). Models: `ReplicatedStorage.WeaponModels`.

## Roster
| Weapon | Affinity | Tier | Default? | M1 hits | Damage per hit | Swing cd per step (s) | Stagger/hit | F skill |
|---|---|---|---|---|---|---|---|---|
| **GreatAxe** | Heavy | 1★ | ✔ (Raam) | 2 | 9 / 9 | 1.2 | 2 | — |
| **WarHammer** | Heavy | 2★ | | — (no combo yet) | — | 1.2 | 1 | — |
| **SavageGlaive** | Heavy | 2★ | | 3 | 12 / 12 / 15 | 1.5 | 2 | **PuncturingGlaive** |
| **The Commandment** | Heavy | 3★ | | 5 | 16 / 18 / 15 / 16 / 30 | 1.0 / 0.92 / 1.3 / 0.67 / 1.95 | 4.4 (step 5: 8.8) | — · [details](the-commandment.md) |
| **StraightSword** | Medium | 1★ | ✔ (Luther) | 2 | 6 / 6 | 0.7 | 1 | — |
| **SilverFlash** | Medium | 3★ | | 5 | 10 / 10 / 11 / 11 / 16 | 0.72 / 0.62 / 0.58 / 0.5 / 0.98 | 1.3 | — |
| **Dagger** | Light | 1★ | ✔ (Bella) | 2 | 5 / 5 | 0.5 | 0.75 | — |
| **BroadDagger** | Light | 2★ | | 3 | 7 / 7 / 7 | 0.55 | 0.9 | **EnhancedDodge** |
| **IllThorn** | Light | 3★ | | 3 | 7 / 7 / 8 (20% stat scaling) | 0.7 / 0.23 / 0.57 | 1 | **ThornBurst** · [details](illthorn.md) |
| **Bow** | Artillery | 1★ | ✔ (Fahlon) | 1 (draw/release) | 8 | 1.2 | 0.5 | — |
| **Crossbow** | Artillery | 2★ | | 3 | 8 / 8 / 8 | 1.2 | 0.8 | **CrossbowVolley** |
| **Imperial Rifle** | Artillery | 3★ | | Branching (shot + follow-up) | Shot 24 · W slash 18 · S leap shot 20 · A/D dash 0 | 1.03 / 0.67 / 1.33 / 0.47 / 0.47 | 1.6 / 1.2 / 2 / 1.2 / 1.2 | — · [details](imperial-rifle.md) |
| **LeatherGloves** | Gauntlet | 1★ | ✔ (Zumin) | 2 | 6 / 6 | 0.45 / 1.0 | 0.8 | — |
| **CleanserSilverGauntlets** | Gauntlet | 2★ | | 3 | 5 / 5 / 8 | 0.35 / 0.3 / 0.6 | 1 | **CleanserKick** |
| **Akhzarims Tenacity** | Gauntlet | 3★ | | 5 | 9 / 9 / 11 / 11 / 16 | 0.35 / 0.55 / 0.6 / 0.37 / 1.45 | 1.3 (step 5: 2.6) | — |
| **DivineServant** | Conjurer | 3★ | | 4 (3 lances + sky lance) | 10 / 10 / 11 / 19 **Magic** | 0.72 / 0.62 / 0.73 / 1.1 | 1 | — |
| **Than** | Authan | Personal | ✔ (Authan) | 5 | 9 / 8 / 9 / 9 / 14 | 0.65 / 0.95 / 0.52 / 0.65 / 1.38 | 1.5 | — |

All M1 damage is Physical except DivineServant (Magic). **Hybrid** M1s (Imperial Rifle) deal whichever type the wielder's build is stronger in. Damages are placeholders. Class M1 multipliers apply on top (only Fahlon's ×1.14 today; Authan and Hayut copy it).

Hit sounds by weapon: Dagger/BroadDagger `HitLightBlade`, StraightSword/SavageGlaive/IllThorn `HitBlade`, GreatAxe `HitHeavyBlade`, WarHammer `HitBlunt`, gloves `HitFist`, gauntlets `HitGauntlet`, Than/Bow/Crossbow `HitPierce`, The Commandment `CommandmentHitBlunt`, Imperial Rifle `HitPierce` (shot) / `HitBlade` (slash) / `ImpactHeavy` (explosion).

## Weapon passives (3★)
| Weapon | Passive | What it does |
|---|---|---|
| SilverFlash | **Inner Sensation** | Each swing has a 1 in 5 chance to strike true: that swing deals **True** damage and lands a second hit of the same base damage as **Magic**. |
| The Commandment | **Rumble** | A hit that staggers an enemy, or lands on one already staggered, deals **+20%** as a heavy blunt blow. |
| IllThorn | **SiphoningLife** | A ThornBurst at max marks **heals 20%** of its damage and blood-coats the blade for **2 s**: slices reach 2.5 studs further and deal **True** damage. |
| Imperial Rifle | **Trained Veteran** | The bayonet slash inflicts **Vulnerable** (once every 8 s per target). Missing an attack gives **+20% move speed for 0.5 s**. |
| Akhzarims Tenacity | **Relentless Pursuit** | Recover from your own endlag **twice as fast**, leaping 8 studs the way you're moving (WASD) the moment you're free. Each heartbeat grants **+20% damage for 3 s**. |
| DivineServant | **OverflowingSynapse** | Each cast takes **1 s** off every cooldown that's counting down. |

Passive names and descriptions come from `WeaponMoves.GetPassiveInfo` (for the planned Character Page overhaul).

## Weapon notes
- **SilverFlash**: the first pullable 3★ weapon. A Cleanser's silver blade, featured on the "Cleanser's Silver" Weapons banner. 5-hit combo sliced from one animation; the dash hit is the strongest.
- **DivineServant** ([page](divine-servant.md)): the first Conjurer weapon, an **Opharim**. It's worn on the **torso**, not held, and pulses its emitters. The wielder writes an incantation and the Opharim fires lances; hit 4 is a "sky lance". No Conjurer Soul exists yet. The Cleanser spellcaster NPC uses it. It's 3★ and in the Convoke pool.
- **Akhzarims Tenacity**: 3★ Gauntlet with **no physical model** (`noModel`); like the Opharim, VFX make up for it. One animation sliced into 5 hits: impale, two claw swings, two kicks. Hit 5 is a lunging slam along the ground.
- **Imperial Rifle**: 3★ Artillery rifle with a bayonet. Its M1s alternate a shot with a follow-up chosen by the held key ([details](imperial-rifle.md)). The S leap shot's ground explosion inflicts **Imbalance** (1 s) on human-shaped enemies. It holsters on the back after 3 s idle.
- **Than**: Authan's trident staff. Gripped at its `Value` CFrameValue. The only weapon outside Convoke.
- **LeatherGloves / CleanserSilverGauntlets**: modelled as a left/right **pair**, one per hand. Saved data from the old placeholder "HandWraps" is renamed on load.
- **SavageGlaive**: granted in the Lobby by a SavageGlaiveCrate pickup (renamed from GreatGlaive).
- The **Deceased** enemies wield maroon copies of the 1★ weapons (wood everywhere, dark red slate blade).

## F weapon skills (`WeaponSkills`)
| Weapon | Skill | Kind | What it does | cd |
|---|---|---|---|---|
| SavageGlaive | **PuncturingGlaive** | DashSlash | A dash with a slash (13 dmg); slow wind-up, endlag, applies **Stun** on hit | 6 s |
| Crossbow | **CrossbowVolley** | — | A volley of bolts (4 dmg each) | 8 s |
| BroadDagger | **EnhancedDodge** | Dodge | An improved directional dodge (client slide, CameraLockOnScript). Zumin's E reuses it. | 3 s |
| CleanserSilverGauntlets | **CleanserKick** | DashSlash | A dashing kick (11 dmg) | 6 s |
| IllThorn | **ThornBurst** | ThornBurst | A steerable dash; on contact, stab and grab, then burst the Thorn Marks ([details](illthorn.md)) | 3 s |

## Design rules
- 1★ = M1 only. 2★–3★ weapons get an F skill (SilverFlash, The Commandment, Akhzarims Tenacity, Imperial Rifle and DivineServant don't have theirs yet).
- Every 3★ weapon has a weapon passive.
- Weapons are compared on raw M1 damage (swing speed + output) plus passives. F skills are mostly extra utility and aren't counted, except IllThorn's ThornBurst, which is its identity.
- Weapons are independent of classes in animations, moves and skills. The only link is affinity.
- **Hybrid weapons** deal whichever damage type the wielder's build is stronger in: IllThorn's burst and the Imperial Rifle.
- In PVP each Soul will be locked to a **signature weapon**. Until those exist, PVP locks each Soul to its affinity's 3★ (see [pvp](../systems/pvp.md)).
