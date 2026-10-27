# Bella: "Substance Fanatic"

- **Affinity:** Light · **Default weapon:** Dagger · **Rarity:** 2★ starter
- **Role:** An assassin-like mage at close range. Big damage over time plus a big burst. Lukas calls her "by far the most complex character".
- **Scales with both** Physical and Magic damage (both are her Affinity Stats). Her **kit is her damage**; M1s only support. Her M1s deal **full weapon damage** (M1 multiplier ×1.0; the old ×0.4 class penalty was removed). Any future M1 weakening is meant to be per class, not per weapon, so a future M1-focused Light character can keep the Broad Dagger strong.
- **Essence colour:** dark pink · **Class weapon:** Bella's Kleilev (her injector, only visible during Peak Health)
- Full kit runs in `ServerScriptService.BellaKit`. All numbers live in `ClassAbilities.Bella` (placeholders).

| Stat | Value |
|---|---|
| Health | 130 |
| Physical / Magic Damage | 25 / 25 |
| Max Essence (L1) | 200 |

## Passives
- **Vial Set:** every 5 s she cycles **Curdle → Distortion → Power**. The current vial is locked in the moment she casts a skill and changes what Q, E and R do. The P slot on the ability bar shows the active vial.
- **pleasentEssence:** anything from her Q or R that reaches a **teammate heals them** for 50% of the move's damage instead of hurting them, with no vial debuff.

## Kit
| Slot | Name | What it does | Numbers |
|---|---|---|---|
| **Q** | **Injection** (Physical) | A dash-grab (the Brigand's grab animations), then a stab with the current vial. **Finisher:** up to +100% damage based on the target's missing health. Ignores 20% armour, +20% crit chance. *Curdle:* +50% damage. *Distortion:* Blind 2 s + Weakened 20% for 5 s. *Power:* Sensitive (takes +20%) for 5 s. | 18 base · range 22 · dash 16 studs · hold 1.5 s · cd 8 s |
| **E** | **Toxin Waste** (Magic) | Throws a vial that becomes a 5 s gas cloud. **While inside she's invisible** and enemies lose her. Enemies inside take ticks with a weaker vial effect (*Curdle:* ×1.5 ticks · *Distortion:* Weakened 10% · *Power:* Sensitive 10%). | 3.6 per 0.5 s · radius 12 · cd 12 s |
| **R** | **Peak Health** (Magic) | Injects herself with her Kleilev: a 1 s fiery burst (3 hits), then a 20 s aura that follows her and ticks burst/20 every second. *Curdle:* burst ×2, no aura (everything up front). *Distortion:* burst Slows 2 s; aura Weakens 20%. *Power:* Bella gets 10% DR + 50% Physical damage for 20 s; teammates in the aura get half. | 36 burst · radius 10 · cd 60 s |

## Build identity
- **Magic build:** Q immobilizes, R is the big burst, and E deals the most total damage while letting her flee.
- **Physical build:** only M1s and Q deal damage. R is mainly for buffs (especially Power). E is a lethal surprise/lurk tool and Q delivers the killing blow.
