# Run structure (Session place)

Driver: `ServerScriptService.PathProgressionScript`.

## Flow
1. The party arrives (teleported from the Lobby into a **private server per party**). Party roster comes from TeleportData (`SessionPartyState`). Intro: `SessionIntro` plays the class pick pose and essence colour.
2. Start at **EntranceToNorthForest** (fixed, no forced event).
3. Reaching a path's **EndOfPath** fades to black, picks the next node and moves everyone to its **StartOfPath**.
4. Each new path pulls one **event** at its `EventAnchor!`.
5. Repeat. A **Safe Haven** every 5 paths. At **path 27** the route leads to the **Rehem**; after it comes the corrupted **second stage**. At **path 50** (second stage only) it leads to **The Chastity**, the North Forest's final boss. Killing her ends the run: ending scene (placeholder), results, back to the Lobby. Every run pays Essence ([economy](economy.md)).

## Path nodes
- **Arriving on a path** (`movePlayerToStart`): players are set down 40 studs past the start, side by side in lanes. A lane is shortened if a wall or rock is in the way. The floor is found with a ray from just above the entrance's own floor level that skips characters, enemies, VFX and anything non-solid or invisible (falls back to the node's `Path` model). Players are set at their real feet height plus 0.75 studs.
- **Off-path guard:** every 0.5 s, a standing player more than 25 studs outside the current path's box, or more than 20 studs below its bottom (fell through), is put back at the path's start.
- **Reset = unstuck (Story runs):** Roblox's Reset Character button no longer kills you; it puts you back at the start of the path you're on (8 s cooldown; not while grabbed, downed or between paths). Server: `UnstuckEvent` in PathProgressionScript; client: `StarterPlayerScripts.ResetToPath`. The Lobby keeps reset disabled.
- **Dash collision:** dashes/leaps sweep a body-wide sphere (radius 1.2) along each step and ignore non-colliding parts, so they stop at trunks and walls instead of tunnelling in (`clampDashStep` in AbilityScript).
- **Pick weight:** a node's `PickWeight` attribute (default 1) sets how often it comes up in the random pool. **PathVersion3** and **PathVersion3 (corrupted)** (the ones with the Rejuvenating Field exit) are **0.6**: about 13% of picks each instead of 20%.
- Hand-built walkable segments tagged `PathNode` (PathVersion1–4, "Path version 5", cave paths…). Inactive nodes are parked in ServerStorage; only the active one is in Workspace.
- **Party gate:** the exit wall stays solid until **every** party member has touched it *and* the combat event is cleared. Then it opens and everyone walks through as the screen fades.
- PathVersion2 has two exits (a fork); for now both lead to a random next node.
- Special routes:
  - **Rejuvenating Field** (a RouteNode via a SpecialExit) → **CaveEntrance** → cave paths in **random order** → **Wyvern Nest** (boss). Players leave through the same opening they came in by. Killing the Wyvern **clears the cave**: the party goes **back to the field they came from** (not the random paths), still on the route (no safe haven, doesn't count as a path). After that, the field's CaveEntrance is a solid wall for that team. The field's own EndOfPath leads back to the forest paths.
  - **The lake** (`RejuvenatingLake`): press **G** at the field's water ("Touch the water") to be **healed fully** (health and Essence) and **levelled up once**. Only once your team has cleared the cave. **Twice per run per player**: once before the Rehem and once after it. Lines are placeholders.
  - **Soul whispers** (`SoulWhisperLines` data, `SoulWhisper` server helper, `SoulWhisperClient` UI): the player is a vessel, and the equipped Soul speaks up on the right side of the screen to hint at what the game doesn't spell out. Presentation: a soft plum haze swells in from the right edge (strips fading left, top and bottom), a lavender accent line grows from its middle with a sheen drifting along it, the Soul's name drifts down into place, then the line (italic Accanthis) writes itself in letter by letter, each letter fading and rising in, overlapping. It holds 4.5 s, then fades out: the letters dissolve left to right over 1.4 s (`fadeTime`) as the block drifts up, and the haze lingers a moment longer. Nothing pops.

    | When | Line |
    |---|---|
    | First time a team enters a Rejuvenating Field with its cave uncleared (first half only), 3 s after | "I feel a formidable presence in the cave." |
    | The Wyvern dies (2.5 s after) | "Well fought. You seem like you could use a sip, the lake seems clean enough." |
    | A player first comes within 110 studs of the Abominable Akavish | "Good luck with that." |
    | The Abominable Akavish dies (2.5 s after) | "Wash yourself, you've earned it." |

    Lines queue, never overlap. `PER_SOUL` can give a Soul its own wording later; for now every Soul says the same.
  - **After the Rehem** the same detour (PathVersion3 (corrupted) → Path version 5 (corrupted)) leads to **RejuvenatingField(Corrupted)** instead. Its cave entrance is blocked by the Corrupted Roots; the **Abominable Akavish** waits there instead (the second stage's Wyvern). The field's exit stays shut until it's dead, and killing it unlocks that field's lake (`LakeUnlockedBy = "Event"`), which gives the after-Rehem claim.
  - **SecondHalfBossPath(TheRehem)**: the Rehem arena.
  - **FinalBossPath(TheChastity)**: The Chastity's arena (not built yet). Scheduled like the Rehem, but counted **overall** (every path left counts, detours and Safe Havens too, unlike the Rehem's counter): once per run, the first normal pick after 50 paths, never random; a due Safe Haven comes first and a detour isn't interrupted. Needs `LockedEvent = ChastityEncounter`, the `PathNode` tag and an `EventAnchor!` in its middle (arena centre; optional `ArenaRadius`, default 135). Until it exists she appears on **whatever second-stage path comes up at path 50** (her encounter overrides that path's own locked event; the arena centre is its `EventAnchor!`).
  - **"Secound Stage"**: copies of the paths with some grass turned dark red (corrupted), matching the Lobby's maroon Tree of Knowledge.

## Path guide (`PathGuideClient`, tuning in `PathGuideSettings`)
Quiet guidance that doesn't look like guidance. Each player only sees their own.
- **Pebbles:** on arriving at a path, a trail of small pale stones (ground colour mixed 55% toward pale stone, Slate) marks the last 45 studs of the real walking route (client-side pathfinding) up to every exit, about every 5 studs (`pebbleLastStretch`; nil = the whole route). The stones cover the **middle half of the path's width** (`pebbleWidthShare`), leaving the edges bare: at each cluster the width is measured by casting for the walls/side barriers either side (up to 110 studs, band capped at 100), with ~0.25 stones per stud of band (2–24 a cluster). Open ground with no walls found: ±4 studs. Always there. Where pathfinding finds no route (e.g. the Rehem's open arena), it falls back to a straight line along the ground.
- **Guide wind:** once the path's fight is over (its exits open), streaks of warm-gold wind (the ambient wind is white), about 60% solid at their thickest (the ambient ~25%), 1.8× as wide and brighter, start just behind you and blow past you, riding the route all the way to the exit (`windToExit`, 15 studs/s). 8 at once, so a new one passes you every few seconds. They stop within 14 studs of the exit. On a fork, the wind alternates between the routes.
- **Testing note:** while `FORCED_ENTRANCE_EVENT_NAME` (Studio only) puts a fight at the entrance, the entrance counts as an uncleared path, so there's no guide wind there until that fight is won.
- **Server side:** `PathProgressionScript` (PATH GUIDE block) tags each team's current exits `GuideExit` (attributes `GuideTrack`, `GuideVisit`) and sets the players' `GuideTrack` / `GuideVisit` / `GuideOpen` every 0.5 s. An exit part with `NoGuide = true` is never guided to.

## Events (`ServerStorage.PathEvents`)
- **CombatEvents**: lists of enemies with offsets from the anchor. The exit stays shut until they're all dead.
- **SafeEvents**: merchants and NPCs (Benesh, Ross).
- **Weighted pull:** every event has a `weight` (default 1); Benesh is rarer.
- Rules:
  - A SafeEvent never comes twice in a row.
  - The path after a Safe Haven or a SafeEvent is **always a combat event**.
  - The first path after an entrance is always combat.
  - **Ross** is a chance encounter in the random pool (weight 0.5; a fight is 1, Benesh 0.3). No longer guaranteed before a Safe Haven.
  - **Locked events** only appear on their own path (Totems → Path version 5, Akavish → caves, Wyvern → Nest, Rehem → its arena).
- Leaving a node destroys any leftover enemies, so a fight left mid-battle doesn't follow you.
- Full event list: [enemies/README.md](../enemies/README.md#encounters-serverstoragepathevents).

## Safe Havens
- Their own node (`SafeHavenNode` tag), guaranteed every **5** paths. There are no random ones now (`SAFE_HAVEN_CHANCE = 0`). Never two in a row.
- **Heal zone:** refills **25% of max Health and Essence per second**.
- **Finn**, the craftsman: crafts charms from your materials for a fee (see [charms](../items/charms.md)).
- PVP's second team has its own **NavatSafeHaven** with a Seedling craftsman.

## Enemy difficulty over the run (`RunProgressState`)
- Counts completed paths. Every **10 paths** is a tier: paths 0–9 spawn level-1 enemies, 10–19 level 20, 20–29 level 40, and so on (placeholders).
- Enemies also scale with party size (+10% per extra player).

## Economy
- **Silver**: run currency, **100 at the start**, resets each run. Earned from kills (Silver ≈ a multiple of the enemy's EXP), sales, and PVP kills.
- **Shops:** Benesh (charm seller, NewShopUI), Ross (merchant: buy and sell), Finn (craft). Fahlon gets −20% prices and +20% on sales.
- **Small inventory:** materials and edibles (see [materials](../items/materials.md)).
- The first half's economy is deliberately **halved**; the second half pays a bit more.

## Morale (`MoraleSystem`)
A party-wide penalty for **fleeing** a combat event: −10% to Phys/Magic Damage and Phys/Magic Res per stack, max 5 stacks (−50%). A stack comes off for winning a fight or resting at a Safe Haven. *The flee trigger isn't wired yet.*

## Death (Story) (`StoryDeathRules`)
- **Solo:** 2 lives. Dying with a life left respawns you on the current path after 3 s. Lives refill at every **Safe Haven**. Dying on your last life is defeat.
- **Group (2+):** 1 life each. A fallen player stays down (their body is cleared after 4 s and their camera follows a standing teammate) until the ones still standing reach a **Safe Haven**, where everyone fallen is revived. Nobody left standing is defeat.
- **While down you earn nothing:** no EXP, Silver or Essence (attribute `Downed`). The end-of-path gate doesn't wait for fallen players.
- **Defeat:** everyone keeps the Essence they earned (it's banked as you go), sees their results and picks **Retry** (a fresh run in a new private server with whoever picked it) or **Return to the Lobby**. Undecided after 30 s goes to the Lobby.
- **Dev revives** (developers in `DevAccess` only): a top-left toggle turns revives on for the run; every death then respawns after 3 s with no lives lost and no defeat.
- Characters are spawned by hand in Story (`CharacterAutoLoads` off).
- An infested player turns into a **Host**.

## Footsteps (`FootstepSounds` data, `FootstepClient`)
Every player character's steps sound like the ground under it (Roblox's own running sound is muted). One step per 3.6 studs walked, picked from the surface's variations (never the same one twice in a row, ±7% pitch), played at the character (3D) so other players' steps come from where they are.

| Surface | Materials | Where |
|---|---|---|
| Grass | Grass, LeafyGrass | the forest paths |
| Dirt | Ground, Mud, Sand… (and anything unlisted) | path dirt |
| Gravel | Pebble, or attribute `FootstepSurface = "Gravel"` | the entrance's grey Rock unions are tagged Gravel |
| Stone | Rock, Slate, Sandstone, Basalt, Concrete… | the caves, Wyvern Nest |
| Wood | Wood, WoodPlanks | bridges, house floors |

- No naming needed: the ground's Material decides. To override one spot, give the part (or its model) `FootstepSurface` = a surface name.
- Sounds are free Creator Store picks (PLACEHOLDER; Openverse didn't respond when this was built): grass and wood by Robloxmastermanyay, stone by YuvoNet, dirt/gravel/wood by Diman_x86. Swap ids in `SURFACES`.
