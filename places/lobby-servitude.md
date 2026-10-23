# Code map: Lobby "Servitude" (placeId 137556040934447)

The Lobby carries its own copies of the shared data modules (ClassDefinitions, WeaponDefinitions, WeaponMoves, ClassAbilities, ClassBaseStats, LevelDefinitions, StatusEffects, SoundEffects…). They've **drifted behind** the Session's. The Lobby needs class/weapon identity (names, tiers, models, grips) to be correct; combat numbers matter mostly in the Session.

| Area | Where |
|---|---|
| Saved data, class/weapon ownership, equip, Convoke rolls, personal grants | `ServerScriptService.Script` |
| Parties, staging, teleport to the Session | `ServerScriptService.SessionManager`; `ReplicatedStorage.SessionPlaceTemplates` |
| PVP matchmaking | `ServerScriptService.PVPQueue` (+ PVPTeamBuilder) |
| Start menu | `StarterGui.MenuUI.MenuController`, `ScreenGui.PlayButton.PlayButtonScript` |
| Intro cutscene | `ReplicatedStorage.IntroCutscene`, `ServerScriptService.IntroCutsceneTracks`, Moon saves in `ServerStorage.MoonAnimator2Saves` |
| Class selection scene | `ReplicatedStorage.ClassSelectionScene`, `StarterGui.ClassSelectCard.ClassSelectCardController` |
| Character Page (Inner Core) | `StarterGui.CharacterPageUI.CharacterPageController` |
| Convoke | `StarterGui.ConvokeUI.ConvokeController`, `ReplicatedStorage.ConvokeBanners`, `ServerScriptService.ConvokeAnimationExport` |
| Party UI | `ScreenGui.ReadyUpFrame`, `HostListFrame`, stage/mode select |
| HUD (shared look) | `HotbarUI`, `HealthBarUI`, `AbilityUI`, `BuffUI` |
| Button style | `ReplicatedStorage.EngravedButton` |
| Sound test panel (dev) | `StarterPlayerScripts.SFXTestPanel` |
