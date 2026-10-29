# Soul Mastery (the Inner Core's Mastery tab)

Each **Soul** has its own Mastery, **0 to 10**, kept forever. It's not the in-run "Mastery levels" (5/10/15/20) from [stats-and-levels](stats-and-levels.md).

Data: `ReplicatedStorage.SoulMasteryDefinitions` in **both** places (keep identical). All numbers are placeholders.

## Rules (decided)
| | |
|---|---|
| EXP | **Story runs only**, to the Soul you played |
| Levels 1–4, 6–9 | small bumps: +1% Affinity Stat (PhysDmg if none) and +5 Health each (+5 more Health if Health is the Affinity Stat) |
| Level 5 | an extra passive (per Soul) |
| Level 10 | one skill changes slightly (per Soul) |
| PVP | **off** |

## EXP
| Source | EXP |
|---|---|
| Each path cleared | 2 |
| The Rehem | 30 |
| Wyvern, Abominable Akavish | 20 each |
| The Chastity | 100 |
| In-run level reached | ×5 |

Needed per level: 100, 150, 200, 250, 300 (L5 = 1,000 total), 350, 400, 450, 500, 550 (L10 = 3,250). A full clear ≈ 370 → L5 in ~3 clears, L10 in ~9.

## Goals
| Soul | Level 5 passive | Level 10 skill change |
|---|---|---|
| Raam | **Breaking Point**: dropping to 30% health gives 30% DR for 2 s (once per 60 s) | **Liberating Cleave**: inner ring deals half of the outer's damage (was a quarter) |
| Bella | **Steady Hands**: a skill cast on the same vial as her previous skill deals +15% | **Injection**: no charge-up (the clip skips straight to the leap), leap reaches 50% further at the same speed |
| Fahlon | **Royal Haste**: any ability (E/Q/R) gives +25% move speed for 2 s | **Marking**: two charges, each with its own cooldown (ability bar shows the count; the countdown only when none are left) |
| Zumin | **Settled Ground**: Devastation gives him (only him) +30 Physical and Magic Resistance for 8 s | **Brace and Vengeance**: Fimble Decision (E) can be used any time during the hold or the release; it cuts the Q short (a brace cut before release still starts Q's cooldown) |
| Luther | *his owner designs it* | *his owner designs it* |
| Authan | *after his kit exists* | *after his kit exists* |

Names marked by Claude where Lukas gave none (Royal Haste, Settled Ground): rename freely in `SoulMasteryDefinitions.GOALS`.

## How it's wired
- **Session**: `RunRewards` banks Mastery EXP into the same `PendingRunRewards` entry as Essence (`mastery = { [Soul] = exp }`). `SoulMastery` (module) reads `PlayerMasteryData` on join, adds the bumps once the Soul is known, runs Breaking Point, and answers `HasPassive` / `GetSkillChange` for the kits (`AbilityScript` Liberating Cleave, `BellaKit` Steady Hands). Your level during a run is the one you started with.
- **Lobby**: `SoulMasteryService` owns `PlayerMasteryData` (`{ exp = { [Soul] = n } }`), claims the pending EXP on join, and sends it via `SoulMasteryEvent`. The Mastery tab takes over the whole left panel: level, progress bar, goal marks 0–10 (5 and 10 gold), the two goal cards. **Studio only**: a "+250 EXP" button (never saved).
- Not built yet: showing Mastery EXP earned on the Results screen.
