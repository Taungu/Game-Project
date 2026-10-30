# Open design questions

Things the design hasn't settled yet. Move an item out once it's decided and documented elsewhere.

## Run & progression
- **Story death/lives**: built (see [run-structure](../systems/run-structure.md#death-story-storydeathrules)). Still open: a special class/item that revives a fallen teammate.
- How do path **forks** resolve (vote? first to arrive)? Today PathVersion2's two exits are interchangeable.
- **Final boss** of the North Forest: not designed.
- What a win/loss writes to the Beacon. The meta-currency and permanent buffs are planned but not built.
- Is the Beacon itself a walkable place?
- The **Morale** flee trigger isn't wired. Nothing adds or removes stacks yet.
- **Rejuvenating Field**: a separate zone generator is planned. Today it's a route node toward the caves and the Wyvern Nest, with the lake reward.
- The lake's lines (`RejuvenatingLake` LINES) are placeholders.
- The final Story **enemy level** pacing (PATHS_PER_TIER / LEVELS_PER_TIER are placeholders).

## Characters
- Authan's real kit (he uses Fahlon's E/Q/R as a placeholder; only his passive is his own).
- Hayut's kit and weapon (dev character, Fahlon copy with test E).
- A Conjurer Soul. DivineServant exists, but no Conjurer class does.
- Fahlon's **Affinity Stat** (deliberately undecided).
- Luther's Anemic: should a naturally expiring Bleed count as a "burst"? Today it does (`forcedBurstsOnly = false`).
- PVP signature weapons per character (for now PVP locks each character to their affinity's 3★ weapon).

## Weapons
- WarHammer has no M1 combo. Bow, Crossbow, StraightSword, Dagger and BroadDagger have basic combos.
- Weapon skills for SilverFlash, Than, DivineServant, Akhzarims Tenacity and Imperial Rifle.

## Enemies & world
- Brigand (MediumVarient) has no real attacks.
- The Sufferer's AI is a placeholder until its full kit.
- Cleanser lore lines (players can talk to survivors; the text is Lukas's to write).
- Alpha Dohv and Lord Dohv (planned major enemies).
- Hemorrhage: how a character applies it, and its lifetime rules.
