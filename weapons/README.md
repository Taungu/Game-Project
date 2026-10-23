# Weapons

A weapon provides the **M1 combo** and, at 2★+, an **F weapon skill**. It can only be equipped by a Soul of the same **affinity** (Authan can use any). Data: `WeaponDefinitions` (identity/tier), `WeaponMoves` (M1 data), `WeaponSkills` (F skills). Models: `ReplicatedStorage.WeaponModels`.

## Roster
| Weapon | Affinity | Tier | Default? | M1 hits | Damage per hit | Swing cd (s) | Stagger/hit | F skill |
|---|---|---|---|---|---|---|---|---|
| **GreatAxe** | Heavy | 1★ | ✔ (Raam) | 2 | 9 / 9 | 1.2 | 2 | — |
| **WarHammer** | Heavy | 2★ | | — (no combo yet) | — | 1.2 | 1 | — |
| **SavageGlaive** | Heavy | 2★ | | 3 | 12 / 12 / 15 | 1.5 | 2 | **PuncturingGlaive** |
| **The Commandment** | Heavy | 3★ | | 5 | 16 / 18 / 15 / 16 / 30 | 1.0 | 4.4 (step 5: 8.8) | — · [details](the-commandment.md) |
| **StraightSword** | Medium | 1★ | ✔ (Luther) | 2 | 6 / 6 | 0.7 | 1 | — |
| **SilverFlash** | Medium | 3★ | | 5 | 10 / 10 / 11 / 11 / 16 | 0.72 | 1.3 | — |
| **Dagger** | Light | 1★ | ✔ (Bella) | 2 | 5 / 5 | 0.5 | 0.75 | — |
| **BroadDagger** | Light | 2★ | | 3 | 7 / 7 / 7 | 0.55 | 0.9 | **EnhancedDodge** |
| **IllThorn** | Light | 3★ | | 3 | 3 / 3 / 4 (20% stat scaling) | 0.7 | 1 | **ThornBurst** · [details](illthorn.md) |
| **Bow** | Artillery | 1★ | ✔ (Fahlon) | 1 (draw/release) | 8 | 1.2 | 0.5 | — |
| **Crossbow** | Artillery | 2★ | | 3 | 8 / 8 / 8 | 1.2 | 0.8 | **CrossbowVolley** |
| **LeatherGloves** | Gauntlet | 1★ | ✔ (Zumin) | 2 | 6 / 6 | 0.45 | 0.8 | — |
| **CleanserSilverGauntlets** | Gauntlet | 2★ | | 3 | 7 / 7 / 11 | 0.55 | 1 | **CleanserKick** |
| **DivineServant** | Conjurer | Personal for now | | 4 (3 lances + sky lance) | 8 / 8 / 9 / 15 **Magic** | 0.72 | 1 | — |
| **Than** | Authan | Personal | ✔ (Authan) | 5 | 9 / 8 / 9 / 9 / 14 | 0.65 | 1.5 | — |

All M1 damage is Physical except DivineServant (Magic). Damages are placeholders. Bella's class multiplier (×0.4) and Fahlon's (×1.14) apply on top.

Hit sounds by weapon: Dagger/BroadDagger `HitLightBlade`, StraightSword/SavageGlaive/IllThorn `HitBlade`, GreatAxe `HitHeavyBlade`, WarHammer `HitBlunt`, gloves `HitFist`, gauntlets `HitGauntlet`, Than/Bow/Crossbow `HitPierce`, The Commandment `CommandmentHitBlunt`.

## Weapon notes
- **SilverFlash**: the first pullable 3★ weapon. A Cleanser's silver blade, featured on the "Cleanser's Silver" Weapons banner. 5-hit combo sliced from one animation; the dash hit is the strongest.
- **DivineServant**: the first Conjurer weapon, an **Opharim**. It's worn on the **torso**, not held, and pulses its emitters. The wielder writes an incantation and the Opharim fires lances; hit 4 is a "sky lance". No Conjurer Soul exists yet. The Cleanser spellcaster NPC uses it.
- **Than**: Authan's trident staff. Gripped at its `Value` CFrameValue.
- **LeatherGloves / CleanserSilverGauntlets**: modelled as a left/right **pair**, one per hand. Saved data from the old placeholder "HandWraps" is renamed on load.
- **SavageGlaive**: granted in the Lobby by a SavageGlaiveCrate pickup (renamed from GreatGlaive).
- The **Deceased** enemies wield maroon copies of the 1★ weapons (wood everywhere, dark red slate blade).

## F weapon skills (`WeaponSkills`)
| Weapon | Skill | Kind | What it does | cd |
|---|---|---|---|---|
| SavageGlaive | **PuncturingGlaive** | DashSlash | A dash with a slash; slow wind-up, endlag, applies **Stun** on hit | 6 s |
| Crossbow | **CrossbowVolley** | — | A volley of bolts | 8 s |
| BroadDagger | **EnhancedDodge** | Dodge | An improved directional dodge (client slide, CameraLockOnScript). Zumin's E reuses it. | 3 s |
| CleanserSilverGauntlets | **CleanserKick** | DashSlash | A dashing kick | 6 s |
| IllThorn | **ThornBurst** | ThornBurst | A steerable dash; on contact, stab and grab, then burst the Thorn Marks ([details](illthorn.md)) | 8 s |

## Design rules
- 1★ = M1 only. 2★–3★ weapons get an F skill (SilverFlash and The Commandment don't have theirs yet).
- Weapons are independent of classes in animations, moves and skills. The only link is affinity.
- **Hybrid weapons** deal whichever damage type the wielder's build is stronger in. IllThorn's burst is the first.
- In PVP each Soul will be locked to a **signature weapon**. Until those exist, PVP uses each Soul's best current 2★.
