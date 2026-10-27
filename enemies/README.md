# Enemies & encounters

Data: `ReplicatedStorage.EnemyDefinitions` (HP, base damage, EXP, stagger, resistances). Each attack deals a **share of the enemy's base PhysicalDamage**, so scaling one number scales every move. Templates: `ReplicatedStorage.Enemies`. AIs: `ServerScriptService.*AI` or the enemy's own scripts.

## Scaling
- **Enemy level** is assigned at spawn from run progress (see [run-structure](../systems/run-structure.md)): +5% HP and damage per level past 1. EXP scales +25% per level.
- **Party size:** +10% HP and damage per extra player. Silver drops are shared in full.
- **Resistances** use the same points as players (`points / (points + 1000)`). They aren't level-scaled.
  - First half (up to and including the Rehem) stays low. The second stage is tougher.
  - Families: Brigands wear armour (strong vs physical, weak vs magic). Beasts have hide (mild both ways). The Infected carry the ill tree's essence (soft to steel, resistant to magic).
- **Aggro:** an enemy locks onto the nearest player. Whoever lands **5 hits first** (hit count, not damage) takes its aggro, then counts reset. Roaring enemies roar once per fight.
- **Stagger on players:** small things chip; armed humans hit solidly; anything bigger than a player can break you. **BREAK** = one-hit stagger regardless of your health.

## First half: the North Forest
| Enemy | HP | Base dmg | EXP | MaxStagger | Res (phys/mag) | Stagger dealt | Moves / notes |
|---|---|---|---|---|---|---|---|
| **Brigand (Light)** | 120 | 60 | 20 | 1 (0.5 s stagger) | 50 / 10 | Slash 3, Grab 4 | Dagger slashes (10/60, 8/60). **Grab** (full base) with a GrabTelegraph flash. The lightest, fastest Brigand. |
| **Brigand (Medium)** | 180 | 80 | 30 | 3 | 100 / 25 | 6 | Sword. No real attacks yet. |
| **Brigand (Heavy)** | 280 | 100 | 45 | 5 | 175 / 40 | 9 | Big two-handed sword: swings 24, spin 60. |
| **Zev** | 100 | 80 | 15 | 1 | 40 / 10 | 5 | A lean beast with quick bites. Comes in packs. |
| **Dohv** | 450 | 200 | 60 | 10 | 125 / 75 | 10; Attack3 14; Attack4 **BREAK** | A huge-headed beast. Swing 20%, bite 30%, combo 20%, and a finisher (outer 20% / inner 60%) that knocks you down (**Imbalance** + blur + flash). Roars. |
| **Imbedded Tree Totem** | 90 | 60 | 20 | 2 | 150 / 0 | 2 | Pops out of platform edges and shoots thorns (15/60 each). |
| **Akavish** | 80 | 60 | 15 | 1 | 25 / 0 | 3 | A spider. The bite (15/60) adds **Sick**: the bite's final damage ×3 more over 20 s. Swarms in caves. |

### Bosses (first half)
| Boss | HP | Base dmg | EXP | MaxStagger | Res | Notes |
|---|---|---|---|---|---|---|
| **Wyvern** | 1500 | 150 | 150 | 25 | 200 / 125 | Wyvern Nest boss. Rests until approached or hit, then howls. **Peck** (30%), **Wing Swipe** (40%, knockdown, stagger 16), **Special**: a talon grab that carries you into the air and drops you (grab 20% + landing 50%; BREAK). If the grab misses, it doesn't take off. |
| **The Rehem** | 2500 | 200 | 250 | never | 200 / 200 | **The first half's "graduation boss"**, meant to be its only real challenge. Lives in SecondHalfBossPath(TheRehem). It sleeps until the party crosses in, then a cutscene plays. **HeadSlam** (inner/outer ring, BREAK), **HeadRam** (surging ram with a big flank zone; always chosen when you're too close; stagger 16), **HeadSlamAndTail** (front-left slam, then a tail sweep hitting everyone within range, BREAK). Its **body is lethal** on every hit moment. Every hit knocks you down unless dodged. Bite 50/200, tail 35/200. It has a boss health bar. |

## Second stage: the corrupted grounds (Infected)
| Enemy | HP | Base dmg | EXP | MaxStagger | Res | Notes |
|---|---|---|---|---|---|---|
| **Deceased** | 70 | 50 | 12 | 2 | 0 / 150 | The dead, used as vessels by the roots. 5 looks. They crawl out of the **Corrupted Roots** 4–7 at a time. Each rises with a random weapon (a Brigand weapon or a 1★ GreatAxe/StraightSword/Dagger, maroon-styled) and uses its animations. Strike 12/50. Every landed hit fills **Infestation**. Stagger 4 (7 with heavy weapons). |
| **Host** | 220 | 70 | 0 | 4 | 100 / 250 | A turned person: a player whose Infestation filled, or a Brigand turned in the BrigandHost event. **Quick Thrust**. **Invading Grab**: on a hit you're held and stabbed 3 times, with big Infestation each. **Retaliation**: after several hits in a short window it strikes all around (stagger 12). Killing a turned teammate gives nothing. |
| **Sufferer** | 1200 | 220 | 90 | 12 | 250 / 150 | The second stage's mid powerhouse, "its Dohv", but far more lethal. **Hand Slam** (9–16 studs; a red circle grows, then a 7-stud blast, BREAK). **Arm Swing** (150°, 13 studs, knockback; 35% chance to chain into Hand Slam). **Scream** (an AoE that ticks damage; Dazes anyone close at the start). Fights players *and* Cleansers. Placeholder AI. |
| **Dohv (Corrupted)** | 550 | 200 | 80 | 10 | 75 / 175 | The second stage's Dohv: same beast, scripts, moves and drops, scaled up 20%, but far more aggressive. Charges in like the Dohv but **never howls**, straight into its attack cycle. **No flat cooldown**: after each attack finishes it waits a random **1–3 s**. Its special (Attack4) comes **1 in 3** instead of 1 in 5. **Every landed hit fills Infestation.** Set by model attributes (`SkipHowl`, `AttackGapMin/Max`, `Attack4ChanceDenominator`, `InflictsInfestation`) the Dohv's scripts read. |
| **Abominable Akavish** | 2400 | 230 | 220 | 30 | 225 / 175 | **The second stage's version of the Wyvern.** A giant root-grown spider waiting in RejuvenatingField(Corrupted) by the Corrupted Roots that block the cave. Boss bar. **Placeholder brain** (`AbominableAkavishAI`, no animations yet): planted, turns to face its target, wakes when you come within 55 studs or hit it. Telegraphed moves (dark red ground marker): **Bite** (close in front, 35%, plus **Sick**), **Sweep** (150° in front, 32 studs, 30%), **Slam** (24 studs all around, 40%, plus **Slow** 2 s). Killing it unlocks the corrupted field's lake. |
| **The Chastity** | 5000 | 250 | 500 | never staggers | 200 / 200 | **The second stage's boss and the North Forest's final boss.** Boss bar. Fight brain `ChastityAI` (no arena or cutscene yet; all numbers placeholders). Animations: Idle `96623766140045` (looped all fight), Descent `127871233443531`. The scripted drop follows its frames: pulled down slowly f0–45, fast f45–57, almost instantly to the floor f57–59; the slam lands on f59. After the animation she stays down playing the Idle. **Airborne**: flies to the arena's middle, 45 studs up, **untouchable** (hidden ForceField + heal-back, no damage numbers, no statuses) and runs **4 arena-wide attacks** picked from her airborne set. **Earth Spikes** (anim `137822517770917`, held on its cast pose): for ~4 s, waves of 6 spots every 0.2 s all over the arena shake (the Deceased's GroundImpact dust plus a faint circle), and 0.8 s later a big earth spike (the `GroundPike` union, ×1.1–1.6: ~60–90 studs tall; hit circle 7 studs × scale) bursts out of each, coloured like the ground it comes from, no shadows, hitting anyone on it for 20% (0.6 s between hits on one player). No tracking. **Descent**: Zumin's Devastation, arena wide. Gold rings on the floor for 1.6 s (the inner ring is half the arena radius), then she plunges onto the centre: everyone in the arena takes 45% (inner) / 25% (outer), is **Dazed** then **Slowed** 3 s, and the ground caves into Zumin's cracked-plate crater (arena wide), which keeps slowing for 5 s. Dodge i-frames avoid it. **Grounded**: she stays down and attackable for **12 s**, her head following the nearest player in the arena (from the landing until she lifts off), never staggers or reacts (immune to Immobility), then flies back up and repeats. **Gold shield**: an invisible gold sphere around her, always up. No attack plays its own hit effects on her (cut/blood VFX, hit sounds); every hit just makes the barrier glow faintly and throws a burst of gold sparks off it toward the nearest player. Damage still lands while she's grounded. Any model with the `Shielded` attribute gets the same no-hit-effects treatment. |

## Allies
**Cleansers** (`CleanserAI`, rig `ReplicatedStorage.Allies.Cleanser`): friendly Church soldiers met fighting the Sufferer. A squad of three, with the weapon deciding the kit: **Cleanser Silver Gauntlets** (3-hit punches), **SilverFlash** (5-hit combo), **DivineServant** (spellcaster at range, 4-lance combo). Tanky (about the Sufferer's HP) but weak (reduced damage that doesn't scale with level). They lose unless the players help. They can die. Survivors stand idle with a Talk prompt for **lore lines** (to be written).

## Encounters (`ServerStorage.PathEvents`)
| Event | Type | Where | What happens |
|---|---|---|---|
| **BrigandAmbushEncounter** | Combat | First half | Three Brigands (Light/Medium/Heavy) seem to be talking; talking to them triggers an ambush. |
| **ZevEncounter** | Combat | Any path | A pack of **5 Zevs** feeding on a dead Brigand, heads in. They wake when you get close or hit one. |
| **DohvEncounter** | Combat | Any path | A Dohv. |
| **TotemEncounter** | Combat | Locked to "Path version 5" | Totems pop in and out of TotemSpawn platforms. |
| **AkavishEncounter** | Combat | Locked to the caves | 3 Akavish feeding on a corpse; they wake together within 30 studs. |
| **WyvernEncounter** | Boss | Locked to the Wyvern Nest | The Wyvern rests mid-chamber facing away from the entrance. |
| **FleeingBeastsEncounter** | Combat | The path right before the Rehem | Dohvs and Zevs flee **past** the party toward the start, running from what's ahead. They don't fight back; kill what you can. |
| **RehemEncounter** | Boss | Locked to SecondHalfBossPath(TheRehem) | The Rehem boss fight. |
| **AbominableAkavishEncounter** | Boss | Locked to RejuvenatingField(Corrupted) | The Abominable Akavish waits by the blocked cave. The field's exit stays shut until it's dead; its death unlocks the lake. |
| **ChastityEncounter** | Boss | Locked to its arena node | The Chastity. The arena centre is the floor under the EventAnchor!; radius from the node's `ArenaRadius` attribute (default 135). |
| **DeceasedInfestation** | Combat | Second stage | 4–7 Deceased rise from the Corrupted Roots. |
| **BrigandHostEncounter** | Combat | Second stage only | A Medium and a Heavy Brigand mid-conversation (no Light). Talk to one and he's worried about his friend, who then turns into a Host, attacks him and turns him too. Both Hosts go after you. |
| **SuffererEncounter** | Combat | Second stage | A Cleanser squad is already fighting the Sufferer. Help them. |
| **CorruptedDohvEncounter** | Combat | Second stage (corrupted paths' pools) | Dohv(Corrupted) charges in from the side of the path (DohvCharge) and attacks straight away, no howl. |
| **BeneshCharmSeller** | Safe | Random (rarer, weighted) | Benesh sells charms. |
| **RossMerchant** | Safe | Guaranteed on the path **before every Safe Haven** | Ross the merchant: buy and sell. |

## Planned
- **Alpha Dohv** (pinnacle of the Dohv species, just under Lord Dohv) and **Lord Dohv** as a major enemy.
- **Valrek**: the intro's chasing monster (cutscene rig only).
- The final boss.
