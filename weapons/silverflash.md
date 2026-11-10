# SilverFlash

- **Tier:** 3★ (the first pullable 3★ weapon) · **Affinity:** Medium · **Type:** a Cleanser's silver blade
- Featured on the "Cleanser's Silver" Weapons banner. Used by the Cleanser swordsman NPC.
- **Hybrid** through its passive: the lucky swing rewards Physical and Magic together.
- F skill: **FlashMode** (placeholder name), see below.

## M1: 5-hit combo
One animation sliced into 5 hits; the dash hit is the strongest.

| | |
|---|---|
| Damage | 11 / 11 / 12 / 12 / 17 Physical (was 10 / 10 / 11 / 11 / 16 before the 2026-11-04 balance pass) |
| Swing cooldowns | 0.72 / 0.62 / 0.58 / 0.5 / 0.98 s |
| Stagger | 1.3 per hit |
| Hit kind | Cut; steps 3–4 (the kicks) are Blunt |

## Passive: Inner Sensation
Each swing has a **1 in 5** chance to strike true (`luckyHit` in `WeaponMoves`):
- That swing deals **True** damage. It scales off **Physical and Magic Damage added together** (`hybridTrue`), not Physical alone.
- It also lands a **second hit as Magic** with the same base damage. Magic Damage from **charms** counts **×3.5** on this hit only (`magicItemScaling`); class and level Magic stay ×1, so a physical build isn't buffed by it.
- The split into a True and a Magic hit has a lore reason, so the True hit never scales on Physical alone.

## F skill: FlashMode (`WeaponSkills`, kind `FlashMode` in `WeaponSkillScript`)
An instant mode, not a dash.

| | |
|---|---|
| Duration | **2 s** |
| Speed | **+100%** movement speed |
| M1s | **Every** swing is a lucky swing (Inner Sensation) |
| Held damage | The mode's M1 damage doesn't land during it. Hits still connect (flash, stagger, statuses), and the stored damage comes out all at once when the mode ends, at **50%** (`delayedDamage`, `FlashModeStash`). Kill credit is kept. |
| Auto dodge | Every incoming hit is dodged for the duration (`autoDodgeChance` 1) |
| Cooldown | **13 s** (was 16, then 10) |

**How it looks** (`SilverFlashModeClient`):
- The wielder's screen flashes light yellow and the world turns half-inverted (a full invert was too harsh), slowly washing lighter until a sparkling flash with a screen shake; then everything is normal again.
- On the wielder's screen every living thing fades see-through and shows its **soul**: a small flame with a lavender core (enemies ember, allies pale blue), visible through walls up to 150 studs.
- Every client sees frozen light-yellow **afterimages** of the wielder, one every 0.1 s, which all vanish when the mode ends (drawn by `ZuminKitClient`).
- Hits during the mode play a bass hit and flash the target black. The hit sounds and hit VFX that were held back all play together in the end burst.

All numbers are PLACEHOLDERS.
