# PVP: Lanes (design spec, not built yet)

Agreed with Lukas on 2026-11-17. **Replaces the current PVP mode entirely** ([pvp.md](pvp.md) describes what's live today). Nothing here is built yet; numbers are placeholders.

**Why:** today's PVP lives on chance meetings and a pool of lives, which gives players little purpose together. Lanes give every player a **role**, a **rival** and a reason to play with their team.

## Teams and roles
- **5v5.** Each team: **1 Vanguard**, **2 Rearguards**, **2 Scouts**.
- Roles are **picked in the lobby** (new UI, to be mocked up first). Picking the Soul right before the match starts is wanted too.
- **Idle or disconnected players are their team's problem.** Nothing waits on a timeout for them.

## Lanes
- Each team walks **three lanes**: the Vanguard alone, the 2 Rearguards together, the 2 Scouts together. Six lanes in a match.
- Every lane gets **its own copy** of the path it's on, built when it enters and removed when it leaves, placed apart in the world so lanes never touch. Paths are 570–860 parts, so about six live at once is fine. (Check: no path relies on Terrain; copies far enough apart that fog/lighting zones don't overlap.)
- Enemy meetings are **decided by the system**, which moves the players into one lane's copy. They never happen by accident.
- **PVP keeps one locked base movement speed** for every Soul. Clearing speed comes from kits, which is a deliberate Soul-to-role lever (Fahlon, Bella and Raam read as natural Vanguards; a tank like Zumin clears slower).

## Vanguard vs Vanguard: the running duel
- **First bout:** both Vanguards get **mirrored combat events** of about equal clear time. Whoever clears first moves on and **ambushes** the slower one.
- **After that:** they meet on a fixed rhythm, **every second path**, so at least once between Safe Havens. Being behind means getting caught earlier.
- **Losing costs only time** (respawn at the team's Safe Haven).
- **Bounty:** a win streak grows a bounty of **Silver and levels**, paid to whoever ends it.

## Rearguards
- **Hunt the enemy Scouts:** chance encounters using **today's Ambush rules** (both sides locked in until one side is wiped). Rearguard–Scout encounters are the only chance meetings in the mode.
- **Reinforce the Vanguard:** every time they leave a Safe Haven, **both** Rearguards skip their own lane and join their Vanguard wherever he is, making a duel up to **3v1**. Paths cleared together **still count for their own lane**. Once that path is cleared they return to their lane.
- This is the losing Vanguard's comeback chance. If it fails, the winner simply keeps the lead; if it works, the winner loses it.

## Scouts
- **Grow strong quietly**: farm, level, build. They never meet anyone except enemy Rearguards, or at team fights.
- **Only safe inside Safe Havens.** Their other job is tying enemy Rearguards up so those can't reach their own Vanguard.

## Team fights: the main bosses
- **The path before the Rehem and before The Chastity** is a waiting room: **no enemies**, and a **barrier across the middle** that stays locked until the **whole team** has arrived. A player who arrives first hears their Soul say: *"Don't continue alone. Wait for your allies."*
- Then the contest for the boss works like today's boss race (claim, contest window, locked out on a loss).

### Rewards (agreed; numbers are placeholders)
| | Winners | Losers |
|---|---|---|
| **The Rehem** | Silver for each player + **1 level** each, plus **"Spoils of the Beast"**: ~+10% damage and resistances until they next leave a Safe Haven | **Half** the Silver, no level |
| **The Chastity** | Her buff, **only during the final fight** (~+10% damage and resistances) | **Home ground:** time to shop, heal and plan in their Safe Haven |

Principle: Silver and levels for the winners, something smaller for the losers, and no combat buff that lasts long enough to snowball.

## The ending: how a match is won
1. The team that **loses the contest for The Chastity** is locked in its own Safe Haven to prepare.
2. The other team fights The Chastity, gets her buff, and may shop once more in **their own** Safe Haven.
3. They leave straight into the **losers' Safe Haven**, where the final team fight happens.
4. **Whoever wins that fight wins the match.** There are no lives.

## The side path (Rejuvenating Field and the cave)
- Still counts toward **path progression**, so a team can plan to meet there and take the miniboss together.
- **Shared and contested:** enemy lanes can meet there.
- **One life inside** the field and the cave: dying throws you out. You may walk back in.
- Once the **miniboss dies** the path is **locked completely**, and anyone still inside (enemies who re-entered included) is thrown out.

## Deaths
- **To monsters:** respawn as today (10 s).
- **To players:** respawn at your team's Safe Haven (time lost). Bounties pay out.

## Built so far
**Step 1, lanes (2026-11-17, Session).** Not playtested yet.
- `ReplicatedStorage.PVPLaneSettings`: sides, roles, seats (1/2/2), the role fill order and `laneSpacing` (8,000 studs).
- `ServerScriptService.PVPLanes`: a lane per side + role; `Acquire` makes the lane's own copy of a path (named and tagged like the template, marked `LaneCopyOf` / `LaneId`, moved to its slot), `Release` removes it once the lane has moved on. Slots alternate ±X: +8k, −8k, +16k… The original paths span X −1,073…2,022, so nothing overlaps.
- `PathProgressionScript` runs each lane as a **track** (same machinery as the old teams): own random paths, events, enemy level and Safe Haven every 5 paths. With `Workspace.PVPLanes` on it builds six lanes. Each side's **entrance is shared** (the team leaves together, splits at the first path). The path pools, parking and name lookups never pick a copy. Lanes never meet by sharing a path (meetings come in step 2). The Studio "Secound Stage first" override doesn't apply to lanes.
- `SessionPartyState`: roles per player (`GetRole` / `AssignRole`; from `teleportData.roles` later). The player gets a `PVPRole` attribute.
- **Studio test:** tick `Workspace.StudioTestPVPLanes`. Players get teams as in the PVP test and roles in the fill order per team (Vanguard, Rearguard, Scout, Rearguard, Scout). Set `Workspace.StudioTestRole` to make the first player another role. The lobby will set `teleportData.lanes` / `.roles` (step 5).
- **Still on from the old PVP until replaced:** lives (unused, since nobody meets yet) and the boss race, which now runs **per lane** (each lane would reach its own Rehem at path 27). Step 3 replaces it. The Rejuvenating Field's lake on a lane copy is untested (step 4).

## Build order
1. **Lanes:** per-lane path copies and the lane track system (no PVP yet).
2. **Roles and matchups:** Vanguard duel rhythm and mirrored first events; Rearguard–Scout Ambushes; Rearguard reinforcement; the bounty.
3. **Team fights and the ending:** waiting-room barrier and line, boss contests and rewards, the final Safe Haven fight.
4. **Side path:** contested field and cave, one life inside, lock on the miniboss's death.
5. **Lobby:** 10-player lobby, role pick UI (mock-up first), Soul pick before the match.
