# DivineServant

- **Tier:** 3★ · **Affinity:** Conjurer (the first Conjurer weapon; no Conjurer Soul exists yet) · In the Convoke pool
- **What it is:** an **Opharim**, a Cleanser's guardian angel ([lore](../lore/world.md#the-church-cleansers-and-opharim)). Worn on the **torso**, not held; it pulses its emitters. The wielder writes glyphs with their fingertips and the Opharim casts them.
- **Used by:** the Cleanser spellcaster NPC (`CleanserAI`), and Authan for testing.
- **F skill:** none yet.
- **Passive: OverflowingSynapse**: each cast takes **1 s** off every cooldown that's counting down.

## M1: written incantations
A 4-hit combo, all **Magic**: 10 / 10 / 11 / 19, swing cooldowns 0.72 / 0.62 / 0.73 / 1.1 s. Each hit is a written spell ([the Opharim's language](../lore/world.md#the-opharims-language)):

| Hits | Incantation | Reads |
|---|---|---|
| 1–3: lances | **Ranah · Velith · Akhar** | destruction, sent moving, at the other |
| 4: sky lance, right hand | **Ranah · Ithav · Lumet · Marek** | destruction, given a great form, raised high, brought down |
| 4: sky lance, left hand | **Velith · Ovin · Tzimra · Akhar** | sent moving, binding, echoing out, on the other |

The sky lance is written with both hands at once, one from each side.

- Client effects: `DivineServantClient`. Sounds: `OpharimLanceAppear`, `OpharimLanceLaunch`, `OpharimLanceImpact`, `OpharimSkyImpact`, `OpharimSkyGround`.
