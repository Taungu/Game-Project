# PVP mode

A race to get strong. Two teams walk the same random path pool from opposite ends, building items and levels exactly like Story. When they collide, they fight.

## Teams
| Team | Faction | Start | Walks shared paths | Safe Haven |
|---|---|---|---|---|
| A | **Embracers** | EntranceToNorthForest | StartOfPath → EndOfPath | SafeHaven |
| B | **Navats** (members: "Seedlings") | EntranceToNorthForest(NavatSide) | **Backwards**: EndOfPath → StartOfPath | NavatSafeHaven (Seedling craftsman) |

The Navat at their entrance says: *"Eliminate those who follow the false prophet."*

## Matchmaking (Lobby: `SessionManager`, `PVPQueue`, `PVPTeamBuilder`)
- Queue as **solo, duo or trio**. A **global queue** across all lobby servers (MemoryStore). Groups are never split.
- Even teams only: **3v3** preferred, **2v2** after a wait, **1v1** (two solos) after a longer wait. Max 6 players.
- One reserved session server per match. Each group is told its team (A/B).

## Match rules (Session: `PathProgressionScript` TRACKS/AMBUSH, `PVPRules`)
- Each team has its **own track**: its own random paths and events and its own Safe Haven every 5 paths. No forced combat event at either entrance.
- **Start:** everyone stays frozen until all players have loaded (max 45 s wait), then the pick pose plays together.
- **Ambush:** when a team pulls the path the other team is on (or both pull it at once):
  - No new event is pulled. The path's monsters stay and fight whoever is closest.
  - Both teams are **locked in** (exits solid) until one team is wiped.
  - Only during an Ambush can the teams hurt each other. Teammates can never hurt each other.
  - No respawns until it ends. The **losers** go to their own team's **Safe Haven** (SafeHaven / NavatSafeHaven) and respawn there: healed, items and levels kept, only time lost; any half-done detour (cave chain, field route) is dropped and they carry on along normal paths. (It used to be their entrance, and the Navats' separate entrance could leave them locked out of the path system.) Dead **winners** respawn on the ambush path.
- **Lives:** **10 per player**, lost **only to enemy players**. A monster finishing someone an enemy player hit recently still counts as a player kill. The killer gets **100 Silver**. At 0 lives you're out.
- **Monster deaths** outside an Ambush just respawn you after **10 s**.
- The match ends when a whole team is out of lives. Everyone returns to the Lobby.
- Infestation is off in PVP.
- **PVP Test** mode (testing only): random teams sharing one track, kill feed and Silver, no lives lost.

## The main bosses in PVP (claiming the Rehem and The Chastity)
Both main bosses give big rewards, so they're a race (`PathProgressionScript`, `BOSS_CONTEST_WINDOW`). The same rule for both:
- The first team whose boss comes due (the Rehem at path 27, The Chastity at path 50 overall) **claims** it and goes to it.
- For **30 s** after the claim, the other team may still reach it and **contest** it: both teams on the boss's path is an Ambush, with the boss still in the fight. If the contesting team wins the Ambush, the boss (which belonged to the losers' event) rises again for the winners.
- After those 30 s, or after **losing** that Ambush, a team is **locked out**: no fight, no boss rewards. It keeps getting normal paths until the other team has slain the boss, then carries on: past the Rehem straight into the second stage; past The Chastity on through the second stage.
- If both teams wipe in that Ambush, nobody is locked out and the race starts over.
- A boss counts as slain when its killers walk out of its path (the exit stays locked until it's dead).
- Slaying The Chastity doesn't end a PVP match: lives decide the winner (the Story stage end is Story-only).

**Messages** (`MatchCountdownController`, `BOSS_LINES`): no mechanics, only a felt line, one small line under the banner. Nothing for the team that reaches the boss.
| When | Rehem | The Chastity |
|---|---|---|
| Other team reached it (shown to you) | "The grounds rumble in the wake of the beast of darkness" | "The skies burn gold above the corrupted grounds" |
| Too late to contest (shown to you) | "The rumbling fades beyond your reach" | "The golden light withdraws beyond your reach" |
| You slew it | "The beast of darkness falls silent" | "The golden light gutters out" |
| The other team slew it | "A distant howl dies out. The beast of darkness has fallen" | "The burning skies fall dark" |

Story mode is unchanged.

## Planned
Each Soul gets a locked **signature weapon** in PVP. Until those exist, every Soul is locked to its affinity's 3★, whatever it has equipped (session-only, never saved):

| Soul | Locked PVP weapon |
|---|---|
| Raam | The Commandment |
| Fahlon | Imperial Rifle |
| Bella | IllThorn |
| Luther | SilverFlash |
| Zumin | Akhzarims Tenacity |
| Authan | Than |
