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
  - No respawns until it ends. The **losers** go back to their entrance (they keep items and levels and only lose time). Dead **winners** respawn on the ambush path.
- **Lives:** **10 per player**, lost **only to enemy players**. A monster finishing someone an enemy player hit recently still counts as a player kill. The killer gets **100 Silver**. At 0 lives you're out.
- **Monster deaths** outside an Ambush just respawn you after **10 s**.
- The match ends when a whole team is out of lives. Everyone returns to the Lobby.
- Infestation is off in PVP.
- **PVP Test** mode (testing only): random teams sharing one track, kill feed and Silver, no lives lost.

## Planned
Each Soul gets a locked **signature weapon** in PVP. For now they're locked to their best 2★.
