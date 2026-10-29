# Settings menu

A **Settings** button (the game's diamond frame + a sliders mark) in the **top-left corner**, in both the Lobby and runs. It opens a centred panel with lavender styling, hard edges, a Fondamento title and Accanthis rows. Click outside the panel, or the X, to close it.

| Section | Option | Default | What it does |
|---|---|---|---|
| Audio | Master volume | 100% | Scales both groups below |
| Audio | Music | 100% | SoundGroup `Music` (soundtracks: `SFXVolumeGroup`'s MUSIC list or `IsMusic` attribute) |
| Audio | Sound effects | 100% | SoundGroup `SFX` (its base 2× loudness stays) |
| Gameplay | Screen shake | On | `ScreenShakeScript` skips shakes when off |
| Gameplay | Damage numbers | On | Hides the floating damage/heal numbers for you |
| Display | Show FPS | Off | Small FPS counter next to the button |

- **Saved per account** (DataStore `PlayerSettings`, `PlayerSettingsService` in both places), so settings follow you between the Lobby and runs. "Reset to defaults" button at the bottom.
- **Data**: `ReplicatedStorage.SettingsDefinitions` (both places, identical). Add a row there and read the LocalPlayer attribute `Setting<id>` where it matters.
- **Look**: `StarterGui.SettingsUI` (restyle `SettingsButton`, `Panel` and `Panel.Templates`; every row is a copy). The panel is drawn for 1080p and scaled to the screen.
- Lobby: hidden while the Inner Core is open (its title sits in the same corner). Session: the buff icons moved down below the button.
