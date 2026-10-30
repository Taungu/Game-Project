# Notes for Claude (and any other assistant) working on Ones Servitude

Read [README.md](README.md) first. It indexes everything. This file covers how to *work* on the game.

## The two Roblox places

| Place | Studio name | placeId | Role |
|---|---|---|---|
| Lobby | **Servitude** ("Ones Servitude") | 137556040934447 | Hub: menu, intro cutscene, class pick, Character Page (Inner Core), Convoke gacha, party staging, PVP queue, DataStores |
| Session | **Session(NorthForest)** | 97441436334476 | The run itself: paths, events, enemies, bosses, combat, items, shop, crafting, PVP matches |

- Studio instance IDs change every time Studio restarts. Always call `list_roblox_studios` first.
- `execute_luau` needs `datamodel_type` (`"Edit"` for reading modules without playing).
- Shared data modules (ClassDefinitions, WeaponDefinitions, WeaponMoves, ClassAbilities, ClassBaseStats, LevelDefinitions, StatusEffects, SoundEffects…) exist in **both** places. The original rule was "Lobby first, then mirror to Session". In practice the **Session is far ahead**. Any identity data the Lobby also shows (class/weapon names, tiers, models, grips) must be mirrored there by hand.
- Lukas also has side places: "monster animations" (rigging/animating creatures) and "Unlimited Arts" (old VFX/assets to reuse).

## Working style Lukas prefers

- **He playtests himself.** Verify with code reads and Edit-mode `execute_luau` math checks. Don't run full play-mode tests unless he asks. He has said "you don't have to test it."
- Code comments mark his decisions with **"(per your call)"**. Numbers marked **PLACEHOLDER** are tuning knobs, not final balance.
- Every tunable number lives in a **data module** (ReplicatedStorage). Scripts read from it. Keep it that way.
- Data tables are kept separate from function tables in every module, so `pairs()` over the data never hits functions.
- NPC dialogue calls the player **"wanderer"**. Brigands talk normally (no "Oi").
- Item descriptions tell the **history** behind the item, not how it looks, and hint at links to other items rather than stating them.
- He animates in **Moon Animator** at 60 fps; hit timings are written as `frame/60`.
- **Test-only things stay in Studio.** Every test override, forced event, test pickup/NPC or dev button must be gated with `RunService:IsStudio()` (or `DevAccess` when it has to work on real servers, like PVP Test). Session test content goes in `Workspace.DevTesting` or gets the `DevOnly` tag; `DevContentGuard` removes both outside Studio.

## Tool gotchas (learned the hard way)

- `multi_edit` can **silently no-op**, especially `replace_all` across a big file. After every edit, re-read or grep the script to confirm the change landed.
- Requiring modules in Edit mode can hang if they `WaitForChild` something that only exists at runtime. Read `.Source` instead in that case.
- **Edit mode counts as a client** (`RunService:IsClient()` is true there). Client-only side effects at module load (building instances, pools) must also check `RunService:IsRunning()`, or every Edit-mode require saves junk into the place. That's how ~5,300 UI sounds piled up in SoundService. Mark runtime-built helpers `Archivable = false`.
- Server-side `:Emit()` on VFX fired right after cloning often doesn't show on clients (replication race). Play one-shot VFX on the client through a RemoteEvent.
- Grip offsets: weapon models carry a `Value` CFrameValue at the **model** level. Set the `AsC1` attribute when the value is authored as a C1.

## Keeping this repo useful

- After a design change lands in Studio, update the matching doc and add a line to [CHANGELOG.md](CHANGELOG.md).
- Prefer tables and short facts over prose. A future session should get the gist in a minute.
- The repo is **public** on GitHub. Don't put secrets, account data or private info here.

## Commit attribution

End commit messages with the attribution lines the session asks for (Co-Authored-By / Claude-Session).
