# UI motion (`UIMotion`)

One shared client module (`ReplicatedStorage.UIMotion`, same copy in Lobby and Session) gives every UI the Soul whisper's feel: nothing pops, everything eases. Timings live in its `MOTION` table.

| Call | What it does |
|---|---|
| `Open(obj or ScreenGui, opts)` | Shows it: fades in from invisible to its authored look, rising ~14 px and easing from 97% scale |
| `Close(obj, opts, onClosed)` | Fades out (drifting up a little), then hides it and puts the authored transparencies back |
| `Pop(obj, opts)` | A card appearing: fade + springy scale (from 86%) |
| `Stagger(list, gap, opts)` | Pops each in turn; items stay in their layout (no reflow) |
| `Swap(old, new)` | Cross-fade between two pages/tabs |
| `WriteText(label, text, opts)` | The letter-by-letter writing: each letter fades and rises in, overlapping. `FinishText` skips to the end |

- Works on plain frames: it eases each descendant's own transparency (no CanvasGroup needed), so the authored look is kept. Interrupting (reopening mid-close) continues from the current fade.
- Options: `time`, `drift` (Vector2 or false), `scale` (number or false), `skip` (sub-trees left alone), `force`, `delay`.
- An object with its own `UIScale` (screen scaling, e.g. Settings) is never scaled by it.

## Where it's used
| Place | UI | Motion |
|---|---|---|
| Session | Shop (`NewShopUIController`) | Panel eases in/out; item slots ripple in one by one on open and tab change; hover preview fades |
| Session | Merchant sell + small inventory | Both fade in/out; the inventory panel opens through its `WantOpen` attribute so the bag toggle and the merchant animate the same |
| Session | NPC dialogue | Lines write themselves in (click still skips); reply buttons pop in one after another |
| Both | Settings | Backdrop + panel fade in/out (the panel's own scale still grows it) |
| Session | Buff badge | Slides/fades in and out |
| Lobby | Convoke | Entrance pieces fade as they slide in; banner flips slide the art from that side, write the name in and fade up the subtitle; results panel fades in, cards fade + spring in one by one, then each card's name writes itself in |
| Lobby | Character Page | Tabs cross-fade; class quotes write themselves in |
| Lobby | Main lobby screen (`UIMotionAuto`) | Any panel/button on `ScreenGui` (inventory, stage select, ready up, host list, mode select…) eases in when it appears; the whole screen fades back in after leaving Convoke / the Character Page. Add names to its `WATCH` table for more |

Already animated in their own way and left as they were: RunEndUI, MatchCountdown banners, CombatTag, Menu, ClassSelectCard.
