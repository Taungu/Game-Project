# Charms (in-run items)

Charms go on the run's **6-slot hotbar**. They're temporary and gone when the run ends. Sources: shops (Benesh the charm seller, Ross the merchant), enemy drops, and **crafting** at Finn in the Safe Haven. Data: `ReplicatedStorage.ItemDefinitions` (Session). Every description gives the item's **history** in lore, not its looks.

## Tier rules
- **Tier 1**: weak and material-like, usually one small stat (sometimes two). Each tier-1 is *the* base for its stat; no redundant tier-1s. Grey border. Crafted from raw materials.
- **Tier 2**: two or three bigger stats, still weak alone. Crafted from tier-1s.
- **Tier 3**: those stats much larger **plus a personal passive**. Crafted from **two tier-2s**. Max one copy of each per player. Lukas designs the tier-3s; tier-2s are derived as their sensible recipe parts.
- **True damage** only exists on tier 3. **Crit Damage** only on one specific tier-3 (needs Crit Chance first). **Health Regen is never item-buildable** (it scales off Essence capacity).
- **Shop sections** group charms by their dominant stat: **Invulnerability** (tank), **Power** (physical), **Destruction** (magic), **Momentum** (mobility / all-round).
- **Crafting at Finn:** bring the ingredients and pay a **craft fee**, which is roughly how much stronger the item is than its parts (`GetCraftFee`; e.g. Ishdam's Scriptures 730, Leviathan 125, Training Sword 60). Missing materials make it cost more, up to the full buy price. Fahlon's discount applies. Ingredients come from the small inventory first, then from equipped charms.
- **Hover tooltip** (`ItemTooltip`, every item icon): name, tier, stats with icons, no lore. Tier 2/3 also show a **"Made from"** row: an icon per ingredient with its count (2x Training Sword = the sword icon and "2x"). Ingredients you have enough of **glow gold**; the rest are **greyed out** (1 of 2 shows "1/2"). Counts come from the server (`OwnedItemsService`) each time the tooltip opens. Tier 3 passives show in a **short form** (`SHORT_PASSIVES`).
- **Finn's book in Story** (`CraftingBook`, `CraftingBookDefinitions`; PVP keeps the full library): every Safe Haven visit, each player's Finn knows every tier 1, **5 random tier 2s** and **2 random tier 3s**, at least one from the player's class section (Raam/Luther/Fahlon/Authan Power, Bella Momentum, Zumin Invulnerability, Hayut Destruction; placeholders). Tier 3s you own aren't offered. The book is re-rolled at every Safe Haven.
- **Recipe pages** (Story): a tier-3 recipe kept in your book for the rest of the run. The **Rehem** offers every standing player a choice of **1 of 3** (at least one from their class section). The **Wyvern** and the **Abominable Akavish** drop a random page 50% of the time, elites (Sufferer, Dohv(Corrupted), Host) 10%.
- **Selling** gives about half the buy price (+20% for Fahlon).

Stat units: damage/resist/CDR/crit/pen/speed use 10 units = 1%. All bonuses are **flat**, added after level scaling.

## Tier 1
| Charm | Section | Price | Stats | Recipe |
|---|---|---|---|---|
| Training Sword | Power | 90 | 100 PhysDmg | 2 Wood |
| Pardian Coin | Power | 25 | 5% Crit Chance | 1 SilverOre |
| Piercing Charm | Power | 10 | 5% Armor Pen | 3 Rock |
| Royal Tailor's Thimble | Power | 35 | 5% Armor Pen, 5% Crit | 2 Rock + 1 SilverOre |
| Essence Bark | Destruction | 90 | 100 MagicDmg | 2 Wood + 1 SilverOre |
| Essence Mathakot | Destruction | 55 | 25 Essence Cap, +8% Essence Regen | 4 Rock |
| Hourglass Charm | Destruction | 30 | 5% CDR | 1 SilverOre + 1 Rock |
| Magic Piercing Charm | Destruction | 10 | 5% Magic Pen | 3 Rock |
| Leather Wrap | Invulnerability | 30 | 25 PhysRes, 25 MagRes | 2 Hide |
| Vitality Charm | Invulnerability | 90 | 25 Health | 3 Hide |
| Ankle Weight | Momentum | 40 | 25 MoveSpd, 25 AtkSpd | 1 Hide + 1 Rock |

## Tier 2
| Charm | Section | Price | Stats | Recipe |
|---|---|---|---|---|
| Bavel Blank | Power | 295 | 240 PhysDmg, 100 Essence Cap | Training Sword + 2 Essence Mathakot |
| Dohv-Hide Harness | Power | 455 | 240 PhysDmg, 150 Health | 2 Training Sword + Vitality Charm |
| Fahlon's Carve | Power | 355 | 360 PhysDmg, 5% Crit, 10% Armor Pen | 2 Training Sword + Pardian Coin |
| Forest Cairn Stone | Power | 170 | 30 PhysRes, 30 MagRes, 120 PhysDmg | 2 Leather Wrap + Training Sword |
| Kanin Field Knife | Power | 110 | 10% Crit, 10% AtkSpd | 2 Pardian Coin + Ankle Weight |
| Essence Piercer | Destruction | 235 | 240 MagicDmg, 10% Magic Pen | 2 Essence Bark + Magic Piercing Charm |
| Forest of Life Salve | Destruction | 455 | 240 MagicDmg, 150 Health | 2 Essence Bark + Vitality Charm |
| Formula of Light | Destruction | 325 | 360 MagicDmg, 5% CDR | 2 Essence Bark + Hourglass Charm |
| Pentagon Pendant | Destruction | 245 | 240 MagicDmg, +15% Essence Regen | Essence Bark + Essence Mathakot |
| Signet Ring of Isaiah | Destruction | 155 | +50% Essence Regen, 5% CDR | Hourglass Charm + Essence Mathakot |
| Cleanser's Mantle | Invulnerability | 315 | 30 MagRes, 150 Health | Leather Wrap + 2 Vitality Charm |
| Iron-Willed Plate | Invulnerability | 295 | 50 PhysRes, 150 Health | 2 Leather Wrap + Vitality Charm |
| Congregant's Combat Wear | Momentum | 290 | 240 PhysDmg, 10% MoveSpd | Training Sword + 2 Ankle Weight |

## Tier 3
| Charm | Section | Price | Stats | Passive | Recipe |
|---|---|---|---|---|---|
| **Dragon Slayer** | Power | 860 | 900 PhysDmg | +20% damage to **beasts** | Bavel Blank + Fahlon's Carve |
| **Executioner's Split** | Power | 1100 | 1000 PhysDmg, 400 Essence Cap | **Executes** enemies you leave under 4% health | Bavel Blank + Dohv-Hide Harness |
| **Leviathan** | Power | 770 | 880 PhysDmg | +30% damage to enemies **under 50%** health | Fahlon's Carve + Congregant's Combat Wear |
| **SkyShatterer** | Power | 1000 | 700 PhysDmg, 300 Health | Your **first attack on each enemy** deals +50% | Dohv-Hide Harness + Fahlon's Carve |
| **Suit of Kanins** | Power | 875 | 760 PhysDmg, 20% Crit, 10% Armor Pen, 20% AtkSpd | Each weapon hit gives the next one +5%, up to +30% (resets after 3 s). The M1-build item. | Fahlon's Carve + Kanin Field Knife |
| **Yosher's Memorial** | Power | 720 | 45 PhysRes, 45 MagRes, 450 PhysDmg | 4% of damage you take becomes concentrated essence: up to +20% damage, fading 5 s after your last hit taken | Forest Cairn Stone + Dohv-Hide Harness |
| **Angel Flames** | Destruction | 830 | 1000 MagicDmg, 10% CDR | Magic hits **burn** for 5% of their damage as **True** damage over 5 s | Formula of Light + Signet Ring of Isaiah |
| **Ishdam's Scriptures** | Destruction | 1300 | 300 MagicDmg | **All** your MagicDmg (class + items) counts 50% more | Formula of Light + Pentagon Pendant |
| **Needle and Bloom** | Destruction | 1020 | 800 MagicDmg, 250 Health | Under 50% health: +20% magic damage | Forest of Life Salve + Formula of Light |
| **Opharim's Blessing** | Destruction | 675 | 760 MagicDmg, +40% Essence Regen | Every 10 s your Essence stays full, your next magic attack deals +30% | Pentagon Pendant + Signet Ring of Isaiah |
| **Witch's Eye** | Destruction | 750 | 600 MagicDmg, 25% CDR | Every second ability cast comes back twice as fast | Signet Ring of Isaiah + Essence Piercer |
| **Ziel's Thrash** | Destruction | 710 | 750 MagicDmg, 10% Magic Pen | Your first magic hit opens a 3 s window; 10% of the magic damage dealt in it is stored and bursts out once | Essence Piercer + Formula of Light |
| **Chieftain's Marking** | Invulnerability | 1155 | 850 Health | +50 max Health per **boss** slain | Dohv-Hide Harness + Iron-Willed Plate |
| **Cursed Plates** | Invulnerability | 870 | 90 PhysRes, 30 MagRes, 500 Health | After more than 2 magic hits, MagRes doubles. After 30 s in a fight, PhysRes doubles. Both last until the fight ends. | Iron-Willed Plate + Cleanser's Mantle |
| **Divine Restraints** | Invulnerability | 770 | 100 MagRes, 500 Health | Every magic hit you take burns enemies within 12 studs for 1% of your max HP over 3 s | Cleanser's Mantle + Iron-Willed Plate |
| **Living Roots** | Invulnerability | 860 | 90 PhysRes, 450 Health | Returns 5% of physical damage taken to the attacker. Your health can't be stolen. | Iron-Willed Plate + Forest of Life Salve |
| **Sturla's Rehem** | Invulnerability | 850 | 100 PhysRes, 650 Health | A hit over 20% of your max HP deals 20% less but **roots** you for 1 s | Iron-Willed Plate + Forest Cairn Stone |
| **Cleansing's Way of Flash** | Momentum | 800 | 540 PhysDmg, 540 MagicDmg, 25% MoveSpd | After an ability, your next M1 deals +30% and gives +20% MoveSpd for 1 s (20 s cd) | Congregant's Combat Wear + Formula of Light |
| **Core Outbreak** | Momentum | 1045 | (via passive) 650 PhysDmg, 20% AtkSpd, 10% MoveSpd, 200 Health | These stats grow 10% stronger per 1000 max Essence | Bavel Blank + Congregant's Combat Wear |
| **Embracer's Contract** | Momentum | 915 | 550 adaptive Phys/Magic, 20% Health Steal | Damage goes to your dominant stat. Kills feed it: +50 per named boss, +25 per player or boss, +5 per intermediate enemy. | Dohv-Hide Harness + Congregant's Combat Wear |

## Epigraphs (the first line of each description)
Training Sword "Feel a bit weak? Then get swinging!" · Pardian Coin "Maybe today's your lucky day, flip a coin!" · Leather Wrap "Cheap coverage is better than no coverage" · Ankle Weight "A bit sluggish aren't you? Go on and put some weight on your feet" · Essence Bark "They still exist?" · Essence Mathakot "Whatever ya found. Ya found the real deal" · Bavel Blank "Steel waits for the one who will wield it." · Dohv-Hide Harness "Two cubic metres of head, and the rest was this." · Fahlon's Carve "Sharpness can come from both long distance and close quarters" · Forest Cairn Stone "Stack one for each name." · Kanin Field Knife "Speak of debts after the cut has landed." · Forest of Life Salve "Pick from where the roots have not yet reached." · Formula of Light "Write it right, and something will answer." · Pentagon Pendant "Thanks given is light received." · Signet Ring of Isaiah "Smooth as a Pearl" · Cleanser's Mantle "Charged with the horrors, and nothing else." · Iron-Willed Plate "Rank means little to those who come back." · Congregant's Combat Wear "Before the light, the body." · Dragon Slayer "Where he struck, the water came." · Executioner's Split "The sentence is passed long before the blade falls." · Leviathan "It does not have to fall today. It only has to tire." · SkyShatterer "The first blow is the one they remember." · Suit of Kanins "By the time they see us, it is already the third cut." · Yosher's Memorial "Someone has to carry them." · Angel Flames "Some fires have to be chained." · Ishdam's Scriptures "The public is given comfort. We are given instructions." · Needle and Bloom "Every wound is soil." · Opharim's Blessing "Ask and he shall answer." · Witch's Eye "It was never taught. It was remembered." · Ziel's Thrash "It was never the weight. It was the prayer behind it." · Chieftain's Marking "Stand above the beast, and the forest is yours." · Cursed Plates "It still guards what no one is left to own." · Divine Restraints "Loved, and so set loose." · Living Roots "Cut a root, and the tree remembers." · Sturla's Rehem "Endure first. Move after." · Cleansing's Way of Flash "One step, one strike. There is no second." · Core Outbreak "The heart outgrows the body that holds it." · Embracer's Contract "Half of me already walks among them."

Placeholder text still needed: Essence Piercer, Hourglass Charm, Magic Piercing Charm, Piercing Charm, Royal Tailor's Thimble, Vitality Charm.

Dev-only items (not real content): CDRDev, PDDev and similar test pickups.
