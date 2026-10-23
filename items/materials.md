# Small inventory: materials & edibles

A separate small inventory holds crafting materials and edibles (`SmallInventoryItemDefinitions`).

| Item | Category | Buy | Sell | Use / source |
|---|---|---|---|---|
| Wood | Material | 16 | 16 | Tier-1 recipes. Dropped by Imbedded Tree Totems (1, plus a 50% chance of a second). |
| Rock | Material | 10 | 10 | Tier-1 recipes. |
| Hide | Material | 20 | 20 | Tier-1 recipes. Zev drop 1, Dohv drop 3. |
| SilverOre | Material | 50 | 50 | Tier-1 recipes. Named this way so it isn't confused with Silver (the currency). Materials themselves aren't craftable (SilverOre from Rock was removed). |
| BeastHeart | Material | — | 50 | Sell item. Zev 10%, Dohv 20%. |
| HolySupper | Edible | 30 | 15 | Eat: +150 Essence, +60 HP ("a modest, blessed meal"). |

## Drops (`EnemyDropDefinitions`)
- **Silver**: every enemy with an EXP value drops Silver worth a multiple of its EXP (e.g. Light Brigand 30–60, Heavy 68–135, Zev 23–45, Dohv 90–180, Totem 30–60). In co-op every member gets the full drop.
- **Brigands** (all variants): 30% Pardian Coin, 30% Training Sword (separate rolls).
- **Zev:** 1 Hide + 10% Beast Heart. **Dohv:** 3 Hide + 20% Beast Heart. **Totem:** 1–2 Wood.
