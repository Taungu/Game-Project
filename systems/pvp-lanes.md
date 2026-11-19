# PVP: Lanes (design spec, being built)

Agreed with Lukas on 2026-11-17. **Replaces the current PVP mode entirely** ([pvp.md](pvp.md) describes what's live today). Steps 1–4 are built (see **Built so far**); numbers are placeholders.

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
**Step 1, lanes (2026-11-17, Session).** Checked in a solo Studio playtest: leaving the entrance put the Embracers Vanguard on its own copy of the path, 8,000 studs along X, with the event spawned on it and the original parked. Both Studio test overrides ("Secound Stage first" and the entrance's forced Akavish) are kept out of lanes.
- `ReplicatedStorage.PVPLaneSettings`: sides, roles, seats (1/2/2), the role fill order and `laneSpacing` (8,000 studs).
- `ServerScriptService.PVPLanes`: a lane per side + role; `Acquire` makes the lane's own copy of a path (named and tagged like the template, marked `LaneCopyOf` / `LaneId`, moved to its slot), `Release` removes it once the lane has moved on. Slots alternate ±X: +8k, −8k, +16k… The original paths span X −1,073…2,022, so nothing overlaps.
- `PathProgressionScript` runs each lane as a **track** (same machinery as the old teams): own random paths, events, enemy level and Safe Haven every 5 paths. With `Workspace.PVPLanes` on it builds six lanes. Each side's **entrance is shared** (the team leaves together, splits at the first path). The path pools, parking and name lookups never pick a copy. Lanes never meet by sharing a path (meetings come in step 2). The Studio "Secound Stage first" override doesn't apply to lanes.
- `SessionPartyState`: roles per player (`GetRole` / `AssignRole`; from `teleportData.roles` later). The player gets a `PVPRole` attribute.
- **Studio test:** tick `Workspace.StudioTestPVPLanes`. Players get teams as in the PVP test and roles in the fill order per team (Vanguard, Rearguard, Scout, Rearguard, Scout). Set `Workspace.StudioTestRole` to make the first player another role. The lobby will set `teleportData.lanes` / `.roles` (step 5).

**Step 2, roles and matchups (2026-11-17, Session).** Checked in a solo playtest: the Vanguard's first path after the entrance was the opening (PathVersion3 with the Zev fight) on his own copy. Meetings, reinforcement and the bounty need two or more players to see; they're checked by code reading only so far.
- **Meetings are decided by the system.** When a lane leaves a path, `laneNextNode` may send it onto an enemy lane's **own copy** instead of a new path. That starts today's **Ambush** there (locked until one side is wiped, losers to their own Safe Haven). A lane can only be reached on a path copy with players on it: never in a Safe Haven, on a boss path, on a detour (side path), while moving or while already fighting. When two lanes share a copy it's removed once nobody stands on it.
- **Vanguard duel.** The opening: the same first path and the same fight for both Vanguards (`openingEvents`, empty = any normal fight). The first to clear it lands on the slower one's path. After that a Vanguard leaving a path meets the enemy Vanguard once he's walked `duelEveryPaths` (2) paths since their last duel; if the enemy can't be reached he's tried again on every path after.
- **Rearguards vs Scouts.** Each path a Rearguard lane leaves rolls `encounterChance` (25%) to land on the enemy Scouts' path, and Scouts roll the same against the enemy Rearguards. Rearguards never meet Rearguards.
- **Reinforcement.** A Rearguard lane leaving a Safe Haven joins its Vanguard on his current path (waits up to `reinforceWait` s if he's between paths; skipped if he's in a haven or at the entrance). They count as his players for that path (exit gate, Ambush, respawns, Essence); if he's fighting they're in the fight. When he leaves the path they go back to their own lane's next path, and the path counts for their lane. If he loses an Ambush they go to their own Safe Haven.
- **No lives in lanes.** Killing an enemy player pays their **bounty**: `baseSilver` + `silverPerStreak` × their streak, plus 1 level per `streakPerLevel` streak. A streak is enemy players killed since your own last death to a player. Your bounty shows where the lives counter used to be ("Bounty on you: N Silver"). The match doesn't end yet (step 3).
- Fixes: a lane could pick another lane's copy of a Safe Haven as its haven (now only templates), the Ambush banner now names the side (not the lane), and losing says "Back to your Safe Haven".
- **To test with players:** Studio's *Clients and Servers* test with 2+ players and `StudioTestPVPLanes` ticked. Teams split players as in the PVP test; with 2 players each side has a Vanguard, so the opening and duels happen. Roles via `StudioTestRole` (first player).

**Step 3, team fights and the ending (2026-11-19, Session).** Walked through solo in Studio: entrance → opening → Rehem waiting room → Rehem (+300 Silver, Spoils on, dropped on leaving the next Safe Haven) → Chastity waiting room → The Chastity → with nobody on the other team the match ended in the slayers' favour. The barrier, the Soul's line, contests and the final fight need 2+ players per team to see.
- **Waiting room.** When a lane's next path would be the one before the Rehem (its path 26) or The Chastity (overall path 49), it goes to its **team's** waiting room instead: one shared copy per team (any path of the right stage; tag a path `NoWaitingRoom` to keep it out), no enemies, and a **barrier** through the middle, square to the line from start to end, so every way through crosses it. Each lane that arrives early hears its Soul: *"Don't continue alone. Wait for your allies."* Once **every player of the team** is in the room, the barrier fades and the team walks on as **one group** (the Vanguard's lane; the others are its guests, like reinforcing Rearguards).
- **The boss.** The gathered team claims the boss; the other team can contest it **until it falls** (`bossContestWindow` = 0; set seconds for a timer) once it has gathered too: it's sent onto the claimers' copy, which is an Ambush with the boss in it. Losing a Rehem fight locks the **whole team** out; it splits into its lanes and later skips to the second stage.
- **Rehem rewards.** Slayers: 300 Silver + 1 level each and **Spoils of the Beast** (+10% damage dealt, −10% damage taken) until they next leave a Safe Haven. The other team: 150 Silver each. After the Rehem the team splits into its lanes again. "Resistances" are done as damage taken −10% (`PlayerDamageReduction`), not resistance points.
- **The ending.** The team that loses The Chastity (loses her contest, or she falls to the others first) is **held together in its own Safe Haven** (exits locked; any fight its lanes were in is called off). The slayers go to **their own** Safe Haven to shop; its exit stays shut until the losers are in theirs. Leaving it they walk into the losers' haven: **the final fight** (an Ambush), with **her blessing** (+10% dealt, −10% taken) for that fight only. **Winning it wins the match.** Both sides wiped at once: everyone stands up and it goes on. A team with nobody left loses at once.
- No more meetings once The Chastity has fallen. A gathered team meets nobody on the way.
- Boss messages now name the side (not the lane), so "slain by you / by them" lines are right in lanes.
- Tunables: `waitingRoom` (barrier look, Soul line delay), `bossContestWindow`, `rehem` (Silver, levels, loser share, Spoils %), `chastity.blessingPercent`. Soul line `WaitForAllies` in `SoulWhisperLines`.

**Step 4, the side path (2026-11-19, Session).** Walked through solo in Studio: PathVersion3 → Path version 5 → the shared field (slot +32,000, PvP flag on) → dying there threw the lane out onto a normal path → field → caves (a beaten cave stayed empty on the way back through) → Wyvern Nest → killing the Wyvern locked stage 1 for the killer's side, and PathVersion3's way in became a wall. Not seen yet: two teams inside at once, and the corrupted field (stage 2).
- **Shared.** The side-path nodes (`sidePath.stages`: the field, CavePath1/2 and the Wyvern Nest; second stage: the corrupted field) have **one copy each for the whole match** (`PVPLanes.AcquireShared`, lane slot 7 = +32,000 X), walked by every lane of both teams. No Ambush lock there: inside, every player gets `InAmbush = "SidePath"`, so enemies can fight anywhere in it.
- **One fight per path.** The first lane in pulls a shared path's fight. A later lane walks in on it as it is and may leave whenever it likes (it has no fight of its own there). Once beaten, that path stays empty for the rest of the match.
- **Counts for progression** in lanes (detours don't count in Story).
- **One life inside.** A player dying in the side path throws their whole lane out onto a normal path (per your call). They can walk back in while it's open.
- **The miniboss** (the Wyvern / the Abominable Akavish). The side whose player lands the **killing blow** (the enemy's `LastHitUserId`) owns it: that stage locks, every enemy lane inside is thrown out, enemy lanes on their way in (Path version 5) are turned onto normal paths, and its entrance becomes a wall for everyone. The slayers go back to the field and the lake works for **their side only** (per your call: lake first, then lock).
- Fix: a copied field carried the template's lake prompts without their connection; the lake now clears those and makes its own.

**Step 5, the lobby (in progress, 2026-11-19, Servitude).** Decided with Lukas: lanes are picked **in the lobby** and remembered; teams are drawn **per lane**; enemy lanes stay hidden until the match starts; the Soul is picked **in the match** (fog-styled screen, mock-up on the "PVP Lanes – Role Pick" canvas); when its timer runs out you go in as the Soul you have selected. Lukas prefers **sharp corners** in UI.
- **Open PVP lobby, now 5v5** (`SessionConfig.PVPLobby`: `maxPlayers` 10, `laneSeats` Vanguard 2 / Rearguard 4 / Scout 4 for both teams together). `LaneFavourites` (DataStore `PVPLaneFavourite`) puts you straight back in your lane if it has a seat.
- **Lane cards** (`ScreenGui.LaneCardsFrame`, beside the lobby window): the whole card is the button. Idle cards use the house see-through panel (`PVPLobbyFrame`'s gradient) with the Hotbar plate's corner filigree; hover grows a card a touch; your lane pops ~7% bigger, turns solid plum with a gold edge. Cards come in one after another when the window opens. A full lane can't be picked; Ready needs a lane.
- **In-server invites:** "Invite" in the lobby window lists the players on this server; the invited player gets an Accept / Decline pop-up (`ScreenGui.PVPInvitePrompt`, 30 s). Accepting joins the lobby **in your group**: a group (max 5, lanes must fit one team) is always drawn onto the same team.
- **The draw:** groups first (each whole, onto the smaller team it fits), then everyone else per lane (never more than half a lane's seats on one team). The session gets `teleportData.lanes = true` and `.roles`.
- **Custom Match** (2026-11-19; was "PVP Test", mode id still `PVPTest`, open to everyone). Hosted / joined as before, but in its own window (`ScreenGui.CustomMatchFrame`, `CustomMatchScript`): **two sides** (Embracers / Navats), each list ending in "+ Join this side" to switch and back; the **host's rules switch**: *Free battle* (the old PVP Test: shared start, item shop, levelling, fight anywhere) or *PVP match* (exactly the PVP mode: lanes, separate entrances, the final fight). Under PVP rules the lane cards appear (`CustomLaneCardsFrame`), seats per side (1 / 2 / 2), and the other side's lanes stay hidden; changing the rules un-readies everyone. **Invite** works here too (accepting puts you on the inviter's side). Server: SessionManager's CUSTOM MATCH section.
- Shared pieces: `ReplicatedStorage.LaneCards` (the cards' look and animation, used by both windows) and `ScreenGui.PVPInvitePrompt.InvitePromptScript` (one pop-up for both kinds of invite).
- Still to build: the in-match Soul pick. Not yet tested with 2+ players: invites, groups and the draw.

## Build order
1. **Lanes:** per-lane path copies and the lane track system (no PVP yet).
2. **Roles and matchups:** Vanguard duel rhythm and mirrored first events; Rearguard–Scout Ambushes; Rearguard reinforcement; the bounty.
3. **Team fights and the ending:** waiting-room barrier and line, boss contests and rewards, the final Safe Haven fight.
4. **Side path:** contested field and cave, one life inside, lock on the miniboss's death.
5. **Lobby:** 10-player lobby, role pick UI (mock-up first), Soul pick before the match.
