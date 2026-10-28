# Essence economy (premium currency)

**Essence** is the premium currency (saved as `convokeEssence` in the Lobby's `PlayerWeaponData`). It's spent on Convoke. Not to be confused with in-run Essence (the ability resource).

## Prices (`ConvokeBanners`, Lobby)
| | Essence |
|---|---|
| 1 pull (Call) | **165** |
| 10 pulls (Call 10) | **1650** (no bulk discount) |

- Paid up front; a call you can't afford doesn't start. The page shows your balance and each button's cost (red when you're short).
- **Duplicates refund** by star tier: 1★ **15**, 2★ **40**, 3★ **165** (a 3★ duplicate gives a pull back).

## Welcome (no tutorial)
- Every account gets **1650 Essence once** (one Call 10). New accounts get it when they pick their first Soul; existing accounts the next time they join.
- Their **first Call 10** guarantees, among the ten results:
  - a **2★ weapon for their starting Soul**: Raam → SavageGlaive, Fahlon → Crossbow, Bella → BroadDagger. Luther gets none yet (there's no 2★ Medium weapon).
  - an **extra Soul from the unique affinities** (Gauntlet, Conjurer: the weapon types without a fixed design rule), one they don't own, at random. Today that's always Zumin; a Conjurer Soul joins the draw once one exists.
  - The other eight are normal rolls. Data: `ConvokeBanners.WELCOME_*`.

## What a Story run pays (`RunRewardDefinitions`, Session)
Every reward is **per player** (co-op doesn't split it) and **kept even if the party wipes or you leave** (banked as you go). Story only; PVP pays nothing.

| Source | Essence |
|---|---|
| Each path left (overall: detours and Safe Havens count; entrances don't) | 10 |
| The Rehem | 300 |
| **The Chastity** (North Forest cleared) | **1650** (the 10-pull base) |
| Side boss: Wyvern | 100 |
| Side boss: Abominable Akavish | 150 |
| Flawless boss (nobody in the party died between the boss appearing and its death) | +50% of that boss's reward |
| Elite kill: Sufferer, Dohv(Corrupted), Host (not a turned player) | 15 each |
| Character strength: level reached × 10, once when your run ends (needs at least 1 path cleared) | up to 200 |
| **First clear ever** of the North Forest (once per account) | **1650** |

- A normal full clear (both side bosses, level 20, no extras): **2,900** (~17 pulls). The first clear ever adds 1650.
- All numbers are placeholders in one data module.

## How it gets to the Lobby
1. The Session (`RunRewards`) adds each reward to the **`PendingRunRewards`** DataStore (key `u_<userId>`, UpdateAsync) right away for paths and bosses, and once more when you leave or the server closes.
2. The Lobby (`claimRunRewards` in `ServerScriptService.Script`) moves it into your Essence when you join, emptying the pending entry in the same write so it can't be claimed twice.
3. First clears are recorded in the **`StageClears`** DataStore.

## The end of the stage
When The Chastity dies (`RunRewards.CompleteStage`):
1. No more path transitions; everyone gets an invisible shield.
2. First-clear check, level reward, final bank.
3. **Ending scene**: doesn't exist yet. A 5 s placeholder (fade to pale gold, "North Forest falls silent") plays in `RunEndUI`.
4. **Results**: each player sees their own breakdown, the total and roughly how many pulls it's worth.
5. After 15 s everyone is teleported back to the Lobby (fails in Studio).
