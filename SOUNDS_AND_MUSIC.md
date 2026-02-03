# Sounds and Songs

This document lists all available sound effects and songs that can be used in game modes.

---

## Sound Effects

Sound effects are short audio clips (e.g., footsteps, hits, ambient sounds). Use the `playSound`, or `broadcastSound` API methods.

### API Usage

```ts
// Play a sound for a specific player
api.playSound(playerId, soundName, volume, rate, posSettings?)

// Broadcast a sound to all players (or all except one)
api.broadcastSound(soundName, volume, rate, posSettings?, exceptPlayerId?)
```

**Parameters:**
- `soundName`: One of the sound names listed below
- `volume`: 0.0 to 1.0
- `rate`: Playback rate (1.0 = normal speed, 0.5 = half speed, 2.0 = double speed)
- `posSettings` (optional): `{ playerIdOrPos: PlayerId | number[], maxHearDist?: number, refDistance?: number }`

**Tip:** If you want a random similar sound, remove the number suffix from the sound name (e.g., use `"grass"` instead of `"grass1"`).

### Available Sounds (324 total)

`bass`
`bassattack`
`bd`
`bearRoar1`
`bearRoar2`
`bearRoar3`
`bearRoar4`
`bearRoar5`
`beep`
`bigDrumSwish1`
`bigDrumSwish2`
`bigDrumSwish3`
`bow`
`bucketEmpty1`
`bucketEmpty2`
`bucketEmpty3`
`bucketFill1`
`bucketFill2`
`bucketFill3`
`bullet_shell_bounce_general_07`
`bullet_shell_bounce_general_08`
`burp`
`cannonFire1`
`cannonFire2`
`cannonFire3`
`cashRegister`
`catHiss1`
`catHiss2`
`catHiss3`
`catHiss4`
`catHurt1`
`catHurt2`
`catHurt3`
`catMeow1`
`catMeow2`
`catMeow3`
`catMeow4`
`catMeow5`
`caughtFish`
`chestClose`
`chestOpen`
`cloth1`
`cloth2`
`cloth3`
`cloth4`
`cowHurt1`
`cowMoo1`
`cowMoo2`
`cowMoo3`
`deerGrunt1`
`deerHurt1`
`dogBark1`
`dogBark2`
`dogBark3`
`dogGrowl1`
`dogGrowl2`
`dogGrowl3`
`dogHurt1`
`dogHurt2`
`doorClose`
`doorClose2`
`doorKnock`
`doorOpen-bloxd1`
`doorOpen-bloxd2`
`drink`
`drumSwish1`
`drumSwish2`
`eat1`
`equip_leather1`
`exp_collect`
`exp_levelup`
`fallsmall`
`fireBurn`
`firecracker1`
`firecracker2`
`firecracker3`
`firecracker4`
`futureStrangePulse`
`game_start_countdown_01`
`game_start_countdown_02`
`game_start_countdown_03`
`game_start_countdown_final`
`glass1`
`glass2`
`glass3`
`golemGrunt1`
`golemGrunt2`
`gorillaIdle1`
`gorillaIdle2`
`gorillaIdle3`
`gorillaIdle4`
`gorillaRoar1`
`grass1`
`grass2`
`grass3`
`grass4`
`gravel1`
`gravel2`
`gravel3`
`gravel4`
`harp_pling`
`hat`
`hauntedHorrorImpact`
`headshot_04`
`headshot_06`
`headshot_08`
`headshot_11`
`hit1`
`hit2`
`hit3`
`hoeTill1`
`hoeTill2`
`hoeTill3`
`hoeTill4`
`horrorAccent1`
`horrorFright`
`horrorMaleDrone`
`horseHurt1`
`horseHurt2`
`horseIdle1`
`horseIdle2`
`horseIdle3`
`intensiveRainLoop`
`knightGrunt1`
`knightGrunt2`
`knightGrunt3`
`laugh1`
`laugh2`
`laugh3`
`levelup`
`lowDrum1`
`lowDrum2`
`lowDrum3`
`metalDoorKnock`
`ominousBellHit`
`pickUp`
`pigHurt1`
`pigHurt2`
`pigHurt3`
`pigOink1`
`pigOink2`
`pigOink3`
`pigOink4`
`pigOink5`
`pistol_cock_01`
`pistol_cock_02`
`pistol_cock_03`
`pistol_cock_06`
`pistol_magazine_load_01`
`pistol_magazine_load_02`
`pistol_magazine_load_03`
`pistol_magazine_unload_01`
`pistol_magazine_unload_02`
`pistol_magazine_unload_03`
`pistol_shot_01`
`pistol_shot_02`
`pistol_shot_03`
`pistol_shot_04`
`pistol_shot_05`
`pulseFromTheDark`
`reel`
`rifle_cock_01`
`rifle_cock_02`
`rifle_magazine_load_01`
`rifle_magazine_load_02`
`rifle_magazine_load_03`
`rifle_magazine_unload_01`
`rifle_magazine_unload_02`
`rifle_magazine_unload_04`
`rifle_shot_01`
`rifle_shot_02`
`rifle_shot_03`
`rifle_shot_04`
`sand1`
`sand2`
`sand3`
`sand4`
`semiAuto_cock_01`
`semiAuto_cock_02`
`semiAuto_cock_03`
`semiAuto_cock_04`
`semiAuto_cock_05`
`semiAuto_first_shot_01`
`semiAuto_magazine_load_01`
`semiAuto_magazine_load_02`
`semiAuto_magazine_load_03`
`semiAuto_magazine_load_04`
`semiAuto_magazine_load_05`
`semiAuto_magazine_unload_01`
`semiAuto_magazine_unload_02`
`semiAuto_magazine_unload_03`
`semiAuto_magazine_unload_04`
`semiAuto_shot_01`
`semiAuto_shot_02`
`semiAuto_shot_03`
`semiAuto_shot_04`
`semiAuto_shot_05`
`semiAuto_shot_06`
`semiAuto_shot_07`
`semiAuto_shot_08`
`semiAuto_tail_only_shot_01`
`sheepBaa1`
`sheepBaa2`
`sheepBaa3`
`sheepBaa4`
`sheepHurt1`
`shotgun_cock_01`
`shotgun_cock_02`
`shotgun_cock_03`
`shotgun_cock_04`
`shotgun_cock_05`
`shotgun_load_bullet_01`
`shotgun_load_bullet_02`
`shotgun_load_bullet_03`
`shotgun_load_bullet_04`
`shotgun_load_bullet_05`
`shotgun_load_bullet_06`
`shotgun_load_bullet_07`
`shotgun_load_bullet_08`
`shotgun_shot_01`
`shotgun_shot_02`
`shotgun_shot_03`
`shotgun_shot_04`
`skeletonRattle1`
`skeletonRattle2`
`skeletonRattle3`
`skeletonRattle4`
`snare`
`snow1`
`snow2`
`snow3`
`snow4`
`sonarBeep`
`splash1`
`stagGrunt1`
`stagHurt1`
`step_cloth1`
`step_cloth2`
`step_cloth3`
`step_cloth4`
`step_grass1`
`step_grass2`
`step_grass3`
`step_grass4`
`step_grass5`
`step_gravel1`
`step_gravel2`
`step_gravel3`
`step_gravel4`
`step_sand2`
`step_sand3`
`step_sand4`
`step_sand5`
`step_snow1`
`step_snow2`
`step_snow3`
`step_snow4`
`step_stone1`
`step_stone2`
`step_stone3`
`step_stone4`
`step_stone5`
`step_stone6`
`step_wood1`
`step_wood2`
`step_wood3`
`step_wood4`
`step_wood5`
`step_wood6`
`stone1`
`stone2`
`stone3`
`stone4`
`submachine_cock_01`
`submachine_cock_02`
`submachine_cock_03`
`submachine_cock_04`
`submachine_first_shot_01`
`submachine_magazine_load_01`
`submachine_magazine_load_02`
`submachine_magazine_load_03`
`submachine_magazine_load_04`
`submachine_magazine_unload_01`
`submachine_magazine_unload_02`
`submachine_magazine_unload_03`
`submachine_shot_01`
`submachine_shot_02`
`submachine_shot_03`
`submachine_shot_04`
`submachine_shot_05`
`submachine_shot_06`
`submachine_shot_07`
`submachine_shot_08`
`submachine_shot_09`
`submachine_tail_only_shot_01`
`successfulBowHit`
`suspenseRiser`
`sweep6`
`thunderRain`
`trapdoorOpen`
`trumpetFlare`
`trumpetNote1`
`trumpetNote2`
`trumpetNote3`
`warperGrunt1`
`warperGrunt2`
`warperGrunt3`
`warperGrunt4`
`warperGrunt5`
`warperPhase1`
`warperPhase2`
`wood1`
`wood2`
`wood3`
`wood4`
`zapAccent1`
`zapAccent2`
`ZombieGrunt1`
`ZombieGrunt2`
`ZombieGrunt3`
`ZombieHurt1`
`ZombieHurt2`
`ZombieHurt3`
`ZombieHurt4`

---

## Music

Music tracks are longer audio for background ambiance. Use the `setClientOption` API method with the `"music"` option.

### API Usage

```ts
// Play a song for a specific player
api.setClientOption(playerId, "music", "Adigold - Dreamless Sleep")

// Stop the current song
api.setClientOption(playerId, "music", null)
```

**Note:** Unlike sound effects, songs are played via client options, not the `playSound` API. Only one song plays at a time per player; setting a new song replaces the current one.

### Available Songs (44 total)

| Song Name | Duration |
|-----------|----------|
| `Adigold - A Place To Be Free` | 2:11 |
| `Adigold - Butterfly Effect` | 1:24 |
| `Adigold - Dreamless Sleep` | 1:58 |
| `Adigold - Frozen Pulse` | 1:39 |
| `Adigold - Frozen Skies` | 2:45 |
| `Adigold - Healing Thoughts` | 1:59 |
| `Adigold - Here Forever` | 2:43 |
| `Adigold - Just a Little Hope` | 1:48 |
| `Adigold - Just Like Heaven` | 1:22 |
| `Adigold - Memories Remain` | 2:06 |
| `Adigold - Place To Be` | 2:36 |
| `Adigold - The Riverside` | 2:28 |
| `Adigold - The Wonder` | 2:09 |
| `Adigold - Vetrar (Cut B)` | 1:30 |
| `Awkward Comedy Quirky` | 0:29 |
| `battle-ship-111902` | 1:26 |
| `cdk-Silence-Await` | 3:18 |
| `corsairs-studiokolomna-main-version-23542-02-33` | 2:34 |
| `ghost-Reverie-small-theme` | 0:41 |
| `happy` | 3:01 |
| `Heroic-Demise-New` | 5:02 |
| `I-am-the-Sea-The-Room-4` | 3:21 |
| `Juhani Junkala [Retro Game Music Pack] Ending` | 0:44 |
| `Juhani Junkala [Retro Game Music Pack] Level 1` | 1:14 |
| `Juhani Junkala [Retro Game Music Pack] Level 2` | 1:12 |
| `Juhani Junkala [Retro Game Music Pack] Level 3` | 1:21 |
| `Juhani Junkala [Retro Game Music Pack] Title Screen` | 0:11 |
| `LonePeakMusic-Highway-1` | 2:27 |
| `Mojo Productions - Pirates` | 1:54 |
| `Mojo Productions - Sneaky Jazz` | 1:43 |
| `Mojo Productions - The Sneaky` | 1:50 |
| `Mojo Productions - The Sneaky Jazz` | 2:20 |
| `progress` | 2:34 |
| `raise-the-sails-152124` | 1:44 |
| `ramblinglibrarian-I-Have-Often-T` | 3:42 |
| `Slow-Motion-Bensound` | 3:27 |
| `snowflake-Ethereal-Space` | 1:16 |
| `the-epic-adventure-131399` | 2:01 |
| `TownTheme` | 1:37 |
| `The Suspense Ambient` | 1:59 |
| `Epic1` | 2:16 |
| `Epic2` | 2:23 |
| `Emotional Epic` | 1:52 |
| `Enemy Marked` | 1:29 |
