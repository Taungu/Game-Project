# Run structure (Session place)

Driver: `ServerScriptService.PathProgressionScript`.

## Flow
1. The party arrives (teleported from the Lobby into a **private server per party**). Party roster comes from TeleportData (`SessionPartyState`). Intro: `SessionIntro` plays the class pick pose and essence colour.
2. Start at **EntranceToNorthForest** (fixed, no forced event).
3. Reaching a path's **EndOfPath** fades to black, picks the next node and moves everyone to its **StartOfPath**.
4. Each new path pulls one **event** at its `EventAnchor!`.
5. Repeat. A **Safe Haven** every 5 paths. At **path 27** the route leads to the **Rehem**; after it comes the corrupted **second stage**. At **path 50** (second stage only) it leads to **The Chastity**, the North Forest's final boss.

## Path nodes
- Hand-built walkable segments tagged `PathNode` (PathVersion1–4, "Path version 5", cave paths…). Inactive nodes are parked in ServerStorage; only the active one is in Workspace.
- **Party gate:** the exit wall stays solid until **every** party member has touched it *and* the combat event is cleared. Then it opens and everyone walks through as the screen fades.
- PathVersion2 has two exits (a fork); for now both lead to a random next node.
- Special routes:
  - **Rejuvenating Field** (a RouteNode via a SpecialExit) → **CaveEntrance** → cave paths in **random order** → **Wyvern Nest** (boss). Players leave through the same opening they came in by. Killing the Wyvern **clears the cave**: the party goes **back to the field they came from** (not the random paths), still on the route (no safe haven, doesn't count as a path). After that, the field's CaveEntrance is a solid wall for that team. The field's own EndOfPath leads back to the forest paths.
  - **The lake** (`RejuvenatingLake`): press **G** at the field's water ("Touch the water") to be **healed fully** (health and Essence) and **levelled up once**. Only once your team has cleared the cave. **Twice per run per player**: once before the Rehem and once after it. Lines are placeholders.
  - **After the Rehem** the same detour (PathVersion3 (corrupted) → Path version 5 (corrupted)) leads to **RejuvenatingField(Corrupted)** instead. Its cave entrance is blocked by the Corrupted Roots; the **Abominable Akavish** waits there instead (the second stage's Wyvern). The field's exit stays shut until it's dead, and killing it unlocks that field's lake (`LakeUnlockedBy = "Event"`), which gives the after-Rehem claim.
  - **SecondHalfBossPath(TheRehem)**: the Rehem arena.
  - **FinalBossPath(TheChastity)**: The Chastity's arena (not built yet). Scheduled like the Rehem: once per run, the first normal pick after 50 completed paths, never random; a due Safe Haven comes first and a detour isn't interrupted. Needs `LockedEvent = ChastityEncounter`, the `PathNode` tag and an `EventAnchor!` in its middle (arena centre; optional `ArenaRadius`, default 135). Until it exists she appears on **whatever second-stage path comes up at path 50** (her encounter overrides that path's own locked event; the arena centre is its `EventAnchor!`).
  - **"Secound Stage"**: copies of the paths with some grass turned dark red (corrupted), matching the Lobby's maroon Tree of Knowledge.

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

## Death (Story)
Today a dead player just respawns. The planned lives rules are in [open-questions](../concept/open-questions.md). An infested player turns into a **Host**.
