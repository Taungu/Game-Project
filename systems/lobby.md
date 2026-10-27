# Lobby (place "Servitude")

## New-player flow
Join → **Menu** (`MenuUI`, EngravedButton style: Balthazar font, lavender glow) → **Play** → first-time **Intro Cutscene** → **The Embracer's class selection** → pick a starter → wake in the Lobby (the **Embracing Flush** bar). Returning players spawn straight in with saved data. Character customization and a tutorial are low-priority placeholders.

### Intro cutscene (`IntroCutscene`)
- A chase from the monster **Valrek**, seen from the player's own head camera (the player's avatar replaces the `PlayerIntro` stand-in). Then fixed cameras, a white flash, and the player is knocked out.
- Timeline: 0–9.5 s head camera → 9.5–23 s ChaseIntroCamera1 → glide to Camera2 → hold → flash.
- Sound focuses on grass footsteps (Lukas didn't want player breathing).

### Class selection (`ClassSelectionScene`, `ClassSelectCard`)
- **The Embracer** NPC in a Moon-driven scene with subtitles. Eyelid blinks hide his teleports.
- Starter cards: **Raam "Opressed"**, **Fahlon "Lost Ruler"**, **Luther "Champion of Vonrow"**, **Bella "Substance Fanatic"**, with rarity borders and affinity icons.
- Then a push into his void and you wake in the bar.

## Character Page: "Inner Core" (`CharacterPageUI`)
- One page for loadout. The **Soul tab** lists owned classes and plays each class's pick animation with an essence sparkle in its colour. The **Weapon tab** is the next step to design.
- Opened via the CoreEssenceButton. Lavender style.

## Convoke (gacha) (`ConvokeUI`, `ConvokeBanners`, Convoke section of `ServerScriptService.Script`)
- Opening it whites out the screen and moves the camera to the **WorldInbetweenCamera** (the ConvokeAnimationPlace).
- **Two tabs:** **Souls** and **Weapons**. Every banner pulls from the **whole pool** (souls *and* weapons); a banner just has a **featured** item.
  - Souls: "Depths Delver" (featured Zumin 2★) · "Lingers of Essence" (standard)
  - Weapons: "Cleanser's Silver" (featured SilverFlash 3★) · "Lingers of Arms" (standard)
- **Call / Call 10.** Calling plays the Moon "ConvokeAnimation" scene (tree roots burst up, essence shoots skyward), then the "Called Forth" panel.
- **Odds:** relative weights 1★ 50 : 2★ 10 : 3★ 1. A featured item takes **50%** of pulls that land on its tier.
- **Pity:** a 2★+ guaranteed within every **10** pulls; a 3★ guaranteed by **70**. Counters are separate per tab and carry across single calls.
- **Duplicates** give 10 Convoke Essence (a placeholder currency with no use yet).
- The pool is every Soul and weapon with a star tier, so all 3★ weapons (SilverFlash, IllThorn, The Commandment, DivineServant, Akhzarims Tenacity, Imperial Rifle) are pullable. Personal-tier Souls (Authan, Hayut) and Than have no tier and are never in the pool.
- `FAKE_ENTRIES` in `ConvokeBanners` adds a test 3★ Soul, "UnnamedSoul", that can be pulled but is never saved.

## Parties & sessions (`SessionManager`)
- Pick a **stage** (North Forest) and a **mode** (Story / PVP).
- **Story:** host a party or join one from the host list → everyone ready → 5 s countdown → reserved private server → teleport.
- **PVP:** see [pvp.md](pvp.md).

## Saved data (Beacon)
- DataStores `PlayerClassData` (owned and equipped classes) and `PlayerWeaponData` (owned weapons, per-class equipped weapon), plus Convoke pity and Convoke Essence.
- Personal grants by userId (in `Script`): Lukas's account gets Authan, plus DivineServant, Akhzarims Tenacity and Imperial Rifle; the dev account gets Hayut.

## Visual identity
- Lavender UI accents, the Balthazar font, star-tier borders (1★ grey, 2★ violet, 3★ gold, Personal its own border).
- The Lobby's **Tree of Knowledge** is dark maroon red.
