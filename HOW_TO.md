# How-Tos

This contains a quick list of tutorials for some features that may be harder to understand on first glance.

## Variations

Custom games now support *variations*, which allow you to create your own alterations of gameplay.

### Custom Games

In custom games, you can use [`api.getVariation()`](#apiReference-getVariation) to get the current variation. For the base game, this will be `default`.

You can use [`api.matchmakeToVariation(playerId, variation)`](#apiReference-matchmakeToVariation) to connect a player to a different variation of your custom game. Use `default` to matchmake back to the original

Variation names must be between 1 and 32 characters. They can only contain letters, numbers, hyphens and underscores.

You can set the variation title using `api.setClientOption(playerId, "customVariationTitle", "My Variation")`. View the docs for the [`api.setClientOption`](#apiReference-setClientOption) or the [`customVariationTitle`](#clientOptions-customVariationTitle) for more information.

Player database values (set with [`api.setPlayerDbValue`](#apiReference-setPlayerDbValue), including persistent currencies) are **shared across all variations** of your custom game, so progress a player earns in the hub carries into `1v1`, `2v2`, etc. If you want a value to differ per variation, include the variation name in the key (e.g. `` `wins_${api.getVariation()}` ``).

### Example

Here's an example inspired by the lobby arenas of games such as Clutch and Containment Breach. It creates a hub variation (`default`) and then variations for two different modes: Solos (`1v1`) and Duos (`2v2`)

```js

let initialised = false
const variation = api.getVariation()

tick = () => {
    if (initialised) {
        return
    }

    if (api.getBlock(0, 0, 0) === "Unloaded") {
        // Wait for chunk to load
        return
    }


    if (variation === "default") {
        // Create platforms to matchmake players
        api.setBlockRect([0, 0, 0], [2, 0, 2], "Red Concrete") // 1v1
        api.setBlockRect([4, 0, 0], [6, 0, 2], "Blue Concrete") // 2v2
    }
    initialised = true
}

onPlayerJoin = (playerId) => {
    if (variation === "default") {
        api.setPosition(playerId, 3, 5, -3)
        api.setCameraDirection(playerId, [0, 0, 1])
    }
    else if (variation === "1v1") {
        api.setClientOption(playerId, "customVariationTitle", "Solos")
    } else if (variation === "2v2") {
        api.setClientOption(playerId, "customVariationTitle", "Duos")
    }
}

// Don't matchmake people if they've already been matchmade
/** @type {Record<PlayerId, boolean>} */
const playerHasMatchmade = {}

onBlockStandStart = (playerId, _x, _y, _z, blockName) => {
    if (variation !== "default") {
        return
    }
    if (playerHasMatchmade[playerId]) {
        return
    }

    if (blockName === "Red Concrete") {
        api.matchmakeToVariation(playerId, "1v1")
        playerHasMatchmade[playerId] = true
    } else if (blockName === "Blue Concrete") {
        api.matchmakeToVariation(playerId, "2v2")
        playerHasMatchmade[playerId] = true
    }
}
```

### Worlds

Due to worlds being their own lobby, there is no possibility for variations. However, to test out custom games, you can reload lobby code as a specific variation. There are two main ways of doing this, however both require you to have at least Coder permissions or higher.

- In the Code Editor, click the three dots on the right to open the `More Options` menu. Here, you can type in a variation and reload the lobby code to test as that variation
- If your code runs `api.matchmakeToVariation`, it will ask you if you want to reload to that variation.

## Plugins

First-party plugins provide reusable World Code features. Import a plugin from `@plugins/<name>` in your entry file or another project file:

```ts
import { setTimeout, getRandomItem } from "@plugins/helpers"
import { debugLog } from "@plugins/debug"
```

Importing a plugin also loads its callbacks. Plugin callbacks run before callbacks with the same name in your own code, so both of these `tick` callbacks run:

```ts
import { setTimeout } from "@plugins/helpers"

tick = () => {
    /* Your tick code */
}
```

If callbacks return a value, your callback's return value takes precedence over the plugin's. Avoid replacing a useful plugin return by accident, particularly in callbacks such as `onPlayerChat` and `onRespawnRequest`.

Plugins may import other plugins. Their dependencies are loaded automatically, so importing `@plugins/sessionBasedGame` also loads the helpers it needs.

### Available plugins

- `@plugins/helpers` exports `setTimeout`, `setInterval`, `clearTimeout`, `clearInterval`, `forceLoadChunk`, `randomInt`, `shuffleArray`, and `getRandomItem`.
- `@plugins/dialogue` exports `sayDialogue`, which shows a player a line of dialogue for a while, and `clearDialogue`. See [Dialogue](#howTo-Dialogue).
- `@plugins/debug` exports `debugLog`.
- `@plugins/sessionBasedGame` provides a lobby-to-game lifecycle, teams, spectators, map loading, optional map voting, optional team choosing, and automatic game resets.

The editor's autocomplete and the read-only plugin source files are the reference for the latest exports and option types.

## Building a SessionBasedGame

Use `@plugins/sessionBasedGame` for a team game that follows this loop:

* Load a lobby and wait for players.
* Optionally let players choose teams and vote for a map.
* Copy the selected map into the play area and assign teams.
* Run the game until one team remains or your code chooses a winner.
* Show the result, reset the lobby, and start the next countdown.

### World layout

Build the lobby and each map in unused parts of the world. The plugin treats these areas as templates and copies one of them into `playAreaRect`.

- `playAreaRect` is the destination where the current lobby or map is placed.
- `lobby.source.rect` and each `map.source.rect` are absolute world coordinates for template builds.
- Lobby spawns, map team spawns, and team-pad rectangles are offsets from the lowest corner of `playAreaRect`, not absolute world coordinates.
- Source rectangles are rounded outwards to chunk boundaries. Keep template builds aligned to 32-block chunk boundaries so the copied area is predictable.
- Every lobby and map source must fit inside `playAreaRect`.
- At least two teams and one map are required.

### Minimal setup

Create the game once, at the top level of your entry file:

```js
import { createGame } from "@plugins/sessionBasedGame"

const teams = [
    { name: "Red", maxPlayers: 4, colour: "#ff5555" },
    { name: "Blue", maxPlayers: 4, colour: "#5555ff" },
]

const game = createGame({
    teams,
    maps: [
        {
            name: "Castle",
            icon: "Stone Bricks",
            source: {
                rect: [
                    [320, 0, 0],
                    [351, 31, 31],
                ],
            },
            teams: {
                Red: {
                    spawn: [6, 2, 16],
                    spawnFacing: [1, 0, 0],
                },
                Blue: {
                    spawn: [26, 2, 16],
                    spawnFacing: [-1, 0, 0],
                },
            },
        },
    ],
    lobby: {
        source: {
            rect: [
                [256, 0, 0],
                [287, 31, 31],
            ],
        },
        spawn: [16, 2, 16],
        spawnFacing: [0, 0, 1],
    },
    playAreaRect: [
        [0, 0, 0],
        [31, 31, 31],
    ],
    emptyLobbyStartInSecs: 60,
    fullLobbyStartInSecs: 10,
    gameResetInSecs: 10,
    onStart: () => {
        game.giveColouredArmourToEveryone()
    },
})
```

The plugin supplies default values for the optional settings. With the setup above, players are assigned to the smallest available team when the game starts.

### Eliminating players and ending the game

By default, players remain on their team after dying. For an elimination game, turn the victim into a spectator:

```ts
onPlayerKilledOtherPlayer = (_killerId, victimId) => {
    game.getPlayer(victimId).startSpectating()
}

onMobKilledPlayer = (_mobId, playerId) => {
    game.getPlayer(playerId).startSpectating()
}
```

`startSpectating()` checks whether only one team remains and finishes the game automatically. For games with another win condition, call `game.makeTeamWin(team)` when that condition is met.

### Team choosing

Add `teamPads` to the lobby to let players request a team before the game starts:

```ts
lobby: {
    source: {
        rect: [
            [256, 0, 0],
            [287, 31, 31],
        ],
    },
    spawn: [16, 2, 16],
    spawnFacing: [0, 0, 1],
    teamPads: {
        Red: {
            rect: [
                [4, 1, 14],
                [6, 1, 16],
            ],
            emptyBlock: "Red Concrete",
            fullBlock: "Red Ceramic",
        },
        Blue: {
            rect: [
                [25, 1, 14],
                [27, 1, 16],
            ],
            emptyBlock: "Blue Concrete",
            fullBlock: "Blue Ceramic",
        },
    },
}
```

Each key must exactly match a team name. The plugin changes a pad from `emptyBlock` to `fullBlock` when that team has enough requests.

### Map voting

Set `mapVotingEnabled: true` and provide multiple maps. Before each game, the plugin opens a shop containing up to four randomly selected maps. The `icon` on each map is used as its shop image.

### Custom player state and behaviour

Extend `SessionBasedPlayer` when each player needs additional state or customised lifecycle behaviour, then pass the class as `playerConstructor`. This example assumes `teams`, `maps`, `lobby`, and `playAreaRect` are top-level constants:

```ts
import { createGame, SessionBasedPlayer } from "@plugins/sessionBasedGame"

class GamePlayer extends SessionBasedPlayer<(typeof teams)[number], (typeof maps)[number]> {
    kills = 0

    startPlaying() {
        super.startPlaying()
        this.kills = 0
    }
}

const game = createGame({
    teams,
    maps,
    lobby,
    playAreaRect,
    playerConstructor: GamePlayer,
})
```

Call `super.startPlaying()` or `super.startSpectating()` when overriding those methods so the plugin still updates inventory, health, visibility, building permissions, and win detection.

### Main session-based exports

- `createGame(options)` creates and returns the single game instance.
- `SessionBasedGame` exposes the current `gamePhase`, selected `map`, `getPlayer`, `getPlayers`, `getStartingInText`, `giveColouredArmourToEveryone`, `checkWin`, and `makeTeamWin`.
- `SessionBasedPlayer` exposes `playerId`, `game`, `team`, `requestedTeam`, `isSpectating`, `startPlaying`, `startSpectating`, `onMessage`, `onRespawnRequest`, and `getPositionInPlayArea`.
- `BaseTeamInfo`, `BaseMapInfo`, and `SessionBasedGameSetupOptions` are exported TypeScript types.


### Complete example: team deathmatch

This combines team choosing, two-map voting, a custom player class, respawning equipment, personal kill counts, team scores, a round timer, sudden death, and automatic resets. Each enemy kill earns one team point. The first team to 10 points wins; after three minutes, the leading team wins. A tied round continues until a team takes the lead. Players who join mid-round spectate until the next round.

#### Build the templates first

Use these absolute world coordinates, or change the corresponding rectangles in the code:

| Area | Lowest block | Highest block |
| --- | --- | --- |
| Play area (overwritten each round) | `[0, 0, 0]` | `[31, 31, 31]` |
| Lobby template | `[256, 0, 0]` | `[287, 31, 31]` |
| Castle template | `[320, 0, 0]` | `[351, 31, 31]` |
| Courtyard template | `[384, 0, 0]` | `[415, 31, 31]` |

Build a floor at `y = 1` in each template, with walls to keep players inside and clear space above each spawn. In the lobby, leave room for the spawn at `[272, 2, 16]` and the team pads at `[260, 1, 14]`–`[262, 1, 16]` and `[281, 1, 14]`–`[283, 1, 16]`. The plugin colours the pads in the copied lobby. Castle's spawns are `[326, 2, 16]` and `[346, 2, 16]`; Courtyard's are `[400, 2, 6]` and `[400, 2, 26]`. Add cover between the opposing spawns.

Enable PvP and health, disable spawn protection, and save the builds in your world. The code copies your templates; it does not build them. All spawn and pad coordinates **inside the code** are offsets within the copied area, which is why Castle's Red spawn is `[6, 2, 16]`, not `[326, 2, 16]`.

#### Paste the whole entry file

Use `index.js` in the Code Editor. Replace the earlier examples with this entire block: it includes every constant and creates the game exactly once. Test with at least two players on opposing teams; team-pad requests are honoured, so choose different pads when testing.

```js
import { createGame, SessionBasedPlayer } from "@plugins/sessionBasedGame"

/* Settings */
const SCORE_TO_WIN = 10
const ROUND_SECONDS = 180
const PLAYERS_PER_TEAM = 4

/* World layout */
/* This example's scoring and HUD are written for exactly two teams: Red and Blue. */
const teams = [
    { name: "Red", maxPlayers: PLAYERS_PER_TEAM, colour: "#ff5555" },
    { name: "Blue", maxPlayers: PLAYERS_PER_TEAM, colour: "#5555ff" },
]

const maps = [
    {
        name: "Castle",
        icon: "Stone Bricks",
        source: { rect: [[320, 0, 0], [351, 31, 31]] },
        teams: {
            Red: { spawn: [6, 2, 16], spawnFacing: [1, 0, 0] },
            Blue: { spawn: [26, 2, 16], spawnFacing: [-1, 0, 0] },
        },
    },
    {
        name: "Courtyard",
        icon: "Grass Block",
        source: { rect: [[384, 0, 0], [415, 31, 31]] },
        teams: {
            Red: { spawn: [16, 2, 6], spawnFacing: [0, 0, 1] },
            Blue: { spawn: [16, 2, 26], spawnFacing: [0, 0, -1] },
        },
    },
]

/* Round state */
const scores = { Red: 0, Blue: 0 }
let roundEndsAt = 0
let resultText = ""

class ArenaPlayer extends SessionBasedPlayer {
    kills = 0

    startPlaying() {
        super.startPlaying()
        this.kills = 0
        api.setClientOptions(this.playerId, {
            secsToRespawn: 3,
            respawnButtonText: "Respawn",
            usePlayAgainButton: false,
        })
    }

    onRespawnRequest() {
        const position = super.onRespawnRequest()
        if (this.team !== null) {
            /* Also runs at round start. Set slots so respawns cannot duplicate equipment. */
            api.setItemSlot(this.playerId, 0, "Iron Sword")
            api.setItemSlot(this.playerId, 46, `${this.team.name} Wood Helmet`)
            api.setItemSlot(this.playerId, 47, `${this.team.name} Wood Chestplate`)
            api.setItemSlot(this.playerId, 48, `${this.team.name} Wood Gauntlets`)
            api.setItemSlot(this.playerId, 49, `${this.team.name} Wood Leggings`)
            api.setItemSlot(this.playerId, 50, `${this.team.name} Wood Boots`)
        }
        return position
    }
}

const game = createGame({
    teams,
    maps,
    playerConstructor: ArenaPlayer,
    playAreaRect: [[0, 0, 0], [31, 31, 31]],
    lobby: {
        source: { rect: [[256, 0, 0], [287, 31, 31]] },
        spawn: [16, 2, 16],
        spawnFacing: [0, 0, 1],
        teamPads: {
            Red: {
                rect: [[4, 1, 14], [6, 1, 16]],
                emptyBlock: "Red Concrete",
                fullBlock: "Red Ceramic",
            },
            Blue: {
                rect: [[25, 1, 14], [27, 1, 16]],
                emptyBlock: "Blue Concrete",
                fullBlock: "Blue Ceramic",
            },
        },
    },
    mapVotingEnabled: true,
    maxSpectators: 4,
    emptyLobbyStartInSecs: 30,
    fullLobbyStartInSecs: 5,
    gameResetInSecs: 10,
    onStart: () => {
        scores.Red = 0
        scores.Blue = 0
        resultText = ""
        roundEndsAt = api.now() + ROUND_SECONDS * 1000
        api.broadcastMessage(`First team to ${SCORE_TO_WIN} kills wins!`)
        game.checkWin()
    },
    onFinish: (winningTeam) => {
        resultText = `${winningTeam.name} wins! Red ${scores.Red} - Blue ${scores.Blue}`
        api.broadcastMessage(resultText)
        /* Disable combat and clear equipment during the result screen. */
        for (const player of game.getPlayers()) {
            player.startSpectating()
        }
    },
    getRightInfoText: (player) => {
        if (game.gamePhase === "beforeGame") {
            return [
                "TEAM DEATHMATCH\nChoose a team on the coloured pads.\nVote for a map in the shop.\n",
                ...game.getStartingInText(),
            ]
        }
        if (game.gamePhase === "gameFinished") {
            return `${resultText}\nNext round soon!`
        }
        const secondsLeft = Math.max(0, Math.ceil((roundEndsAt - api.now()) / 1000))
        const clock = secondsLeft > 0 ? `${secondsLeft}s left` : "Sudden death: next team lead wins!"
        const role = player.team === null ? "Spectating until next round" : `${player.team.name} team`
        return `TEAM DEATHMATCH\n${role}\nRed ${scores.Red} - Blue ${scores.Blue}\nTarget: ${SCORE_TO_WIN}\n${clock}\nYour kills: ${player.kills}`
    },
})

function checkScoreWin() {
    if (game.gamePhase !== "inGame") {
        return
    }
    for (const team of teams) {
        const otherName = team.name === "Red" ? "Blue" : "Red"
        const reachedTarget = scores[team.name] >= SCORE_TO_WIN
        const leadsAfterTime = api.now() >= roundEndsAt && scores[team.name] > scores[otherName]
        if (reachedTarget || leadsAfterTime) {
            game.makeTeamWin(team)
            return
        }
    }
}

tick = () => {
    checkScoreWin()
}

onPlayerKilledOtherPlayer = (killerId, victimId) => {
    if (game.gamePhase === "inGame") {
        const killer = game.getPlayer(killerId)
        const victim = game.getPlayer(victimId)
        if (killer.team !== null && victim.team !== null && killer.team !== victim.team) {
            killer.kills++
            scores[killer.team.name]++
            checkScoreWin()
        }
    }
    return "keepInventory"
}

onMobKilledPlayer = () => {
    return "keepInventory"
}

onPlayerDropItem = () => {
    return "preventDrop"
}
```

#### How the pieces connect

- Importing the plugin registers its join, leave, tick, team-pad, shop-voting, chat, and respawn callbacks. Do not call those plugin callbacks manually: your `tick` and kill callbacks run after theirs automatically.
- `startPlaying()` runs once per round for each participant. It resets personal kills and calls the base implementation to restore health, team visibility, friendly-fire rules, and the spawn position. `onStart` resets shared scores and the deadline after players have started.
- Deaths leave players on their teams, so the plugin respawns them at their map spawn. The player method `onRespawnRequest()` restores equipment and returns the base spawn position. Returning that position is essential; a separate global respawn callback could accidentally override it. Mob and environmental deaths do not award points.
- `getRightInfoText(player)` builds the scoreboard for that player, including their personal `kills` count. The plugin refreshes this HUD once a second.
- `makeTeamWin` changes the phase before calling `onFinish`, so converting everyone to spectators there cannot trigger another win. Scores remain visible during the result screen; the plugin reloads the lobby after ten seconds, then `onStart` clears them for the next match. The plugin also ends a round when only one team remains after disconnects.

To check the full loop, choose opposite pads, vote, score a kill, and confirm the victim respawns with a sword while the killer's team gains a point. Temporarily set `SCORE_TO_WIN` to `2` and `ROUND_SECONDS` to `15` to check a score victory, a timed victory, a tied round continuing into sudden death, and the next round starting with cleared scores. Join mid-round to check spectating.

## Interactable NPCs

An `interactable` entity is one the player aims at and interacts with rather than attacks. Interacting fires `onPlayerAltAction` with the entity as the target, and the crosshair tells the player which key to press.

* `interactable` makes the entity pickable on its own, so leave `canAttack` off unless being hit is meant to do something. Aim still lands on the entity; swinging or shooting at it does nothing.
* The alt action is consumed, so a held block is not placed through the NPC and a held throwable is not spent.
* `interactionPrompt` replaces the label beside the key. Leave it unset for the default `Interact`. Describe what interacting does rather than which button to press, since the engine draws the key itself.
* It takes plain text or `CustomTextStyling`, so use a `translationKey` if your world is translated.
* Works on mobs as well as mesh entities.
* Nothing is drawn on touchscreens, where a tap needs no prompt. The interaction itself still works there.

### A shopkeeper

```js
let shopkeeperId = null

tick = () => {
    if (shopkeeperId !== null) {
        return
    }

    if (api.getBlock(8, 4, 8) === "Unloaded") {
        // Wait for the chunk to load before placing the NPC
        return
    }

    shopkeeperId = api.attemptCreateMeshEntity("Person", {
        size: 1,
        pose: "standing",
        textures: { head: "trader_black" },
    }, "Blacksmith")

    if (shopkeeperId === null) {
        // The mesh entity limit was reached
        return
    }

    api.setPosition(shopkeeperId, 8, 5, 8)

    // interactable is what keeps the NPC pickable, so canAttack can stay off
    api.setTargetedPlayerSettingForEveryone(shopkeeperId, "canAttack", false)
    api.setTargetedPlayerSettingForEveryone(shopkeeperId, "interactable", true)
    api.setTargetedPlayerSettingForEveryone(shopkeeperId, "interactionPrompt", "Buy Gear")
}

onPlayerAltAction = (playerId, x, y, z, block, targetEId) => {
    if (targetEId === shopkeeperId) {
        // Toggle off, or holding the key would flicker the shop open and closed
        api.openShop(playerId, false)
    }
}
```

Players now see the interact key and `Buy Gear` at the crosshair whenever they look at the Blacksmith, and pressing it opens the shop. Hitting the NPC does nothing at all.

### Guarding a held key

`onPlayerAltAction` keeps firing while the interact key is held down, roughly every 50-150ms. Reopening an already open shop does nothing, so the example above needs no guard. An interaction that grants an item, charges a currency or advances a conversation is not idempotent, and has to swallow the repeats itself:

```js
const INTERACT_COOLDOWN_MS = 800
const lastInteractAt = {}

onPlayerAltAction = (playerId, x, y, z, block, targetEId) => {
    if (targetEId === questGiverId) {
        const now = api.now()
        if (now - (lastInteractAt[playerId] ?? 0) >= INTERACT_COOLDOWN_MS) {
            lastInteractAt[playerId] = now
            sayNextLine(playerId)
        }
    }
}

onPlayerLeave = (playerId) => {
    delete lastInteractAt[playerId]
}
```

800ms is long enough to swallow a held key and short enough that two deliberate presses still count twice. [A quest giver](#howTo-A quest giver) fills in `sayNextLine`.

## Dialogue

`@plugins/dialogue` shows a player a line of dialogue in the panel below the crosshair. The line is revealed word by word, and each syllable blips in the speaker's voice as its word appears.

```js
import { sayDialogue, clearDialogue } from "@plugins/dialogue"

onPlayerJoin = (playerId) => {
    sayDialogue(playerId, "Welcome to the forge. Mind the sparks!", {
        speakerName: "Blacksmith",
        voice: "gruff",
    })
}
```

* `sayDialogue(playerId, text, opts)` shows the line and hides it after `opts.durationMs`, 6 seconds by default, counted from when the line starts to appear. A new line replaces the one already up, and saying the same line again replays it.
* `clearDialogue(playerId)` hides the line early.
* `text` is plain text or a `TranslatedText`. `[[Item Name]]` in it shows that item's icon inline, e.g. `"Bring me 3 [[Iron Bar]]"`.
* `speakerName` is shown above the line, as plain text or `CustomTextStyling`, so it can carry a portrait icon.
* `voice` is one of the presets `"soft"`, `"regular"`, `"dark"`, `"high-pitched"`, `"gruff"`, `"lively"`, `"posh"` and `"mechanical"`, or a `SpeechSynth` that sets every parameter, such as its waveform, pitch and speaking rate. Leave it out for a silent line. Hear the presets and make a voice of your own in [Dialogue Voices](#dialogueVoices).

The plugin is built on the [`dialogue`](#clientOptions-dialogue) client option, which you can also set yourself with `api.setClientOption`, e.g. from a code block. A line set that way stays up until you set the option to `null`, and setting the same line again does not replay it. Set `animateIn: false` on it to show the line whole, without its reveal or voice.

### A quest giver

Create the quest giver as in [A shopkeeper](#howTo-A shopkeeper), keeping its ID in `questGiverId`, with `interactionPrompt` set to `"Talk"` and no shop. Each interaction says the next line, guarded as in [Guarding a held key](#howTo-Guarding a held key):

```js
import { sayDialogue } from "@plugins/dialogue"

const QUEST_LINES = [
    "Ah, a new face! The mine has been quiet for days.",
    "Bring me 3 [[Iron Bar]] and I'll make it worth your while.",
    "Off you go, then!",
]
const INTERACT_COOLDOWN_MS = 800
const lastInteractAt = {}
const nextLineIndex = {}

function sayNextLine(playerId) {
    const index = nextLineIndex[playerId] ?? 0
    sayDialogue(playerId, QUEST_LINES[index], { speakerName: "Miner", voice: "lively" })
    nextLineIndex[playerId] = (index + 1) % QUEST_LINES.length
}

onPlayerAltAction = (playerId, x, y, z, block, targetEId) => {
    if (targetEId === questGiverId) {
        const now = api.now()
        if (now - (lastInteractAt[playerId] ?? 0) >= INTERACT_COOLDOWN_MS) {
            lastInteractAt[playerId] = now
            sayNextLine(playerId)
        }
    }
}

onPlayerLeave = (playerId) => {
    delete lastInteractAt[playerId]
    delete nextLineIndex[playerId]
}
```

The plugin has its own `onPlayerLeave`, which runs before yours, so both forget the player.

## Project schematics

In the Code Editor, click the `+` beside Schematics, enter a project path, then upload a `.bloxdschem` file or choose a snapshot from your profile. A project can contain up to five schematics. Open an existing schematic to rename or delete it. To replace it, delete it and add it again at the same path. Profile edits do not change an existing snapshot.

Call `api.setChunkAsSchematic(schematicPath, srcCoords, destCoords)` to replace one complete destination chunk, including air and block data. Both positions are block coordinates selecting their containing chunk, like `copyChunk`. Source coordinates use the stored chunk grid, including padding around the original selection; the Code Editor shows its dimensions.

```js
// Place two neighbouring chunks, spreading the work over ticks.
let nextChunk = 0
tick = () => {
    if (nextChunk < 2) {
        api.setChunkAsSchematic("maps/arena.bloxdschem", [nextChunk * 32, 0, 0], [nextChunk * 32, 64, 0])
        nextChunk++
    }
}
```

The example requires an arena containing source chunks `[0, 0, 0]` and `[32, 0, 0]`. Placement uses the normal chunk-load and block-data budgets, so spread larger builds over time. Missing assets or source chunks throw an error. Placement replaces chunks without block-break drops or callbacks.

Use `api.getSchematicDimensions("maps/arena.bloxdschem")` to get its stored `[x, y, z]` dimensions, including selection padding. Like placement, this reads the active project snapshot.

The `@plugins/sessionBasedGame` plugin accepts `source: { schematic: "maps/arena.bloxdschem" }` for maps and the lobby, alongside `source: { rect: [pos1, pos2] }`. Dimensions are read automatically and rounded outwards to chunks when checking the play area. Spawn positions and team pads use offsets from the stored chunk grid's lowest corner. Schematics are pasted directly through the plugin's normal map-loading flow and must fit within the available placement and block-data budgets; large metadata-heavy maps need a custom loader that spreads work over ticks.

### Loading an entire schematic

```js
function loadSchematic(schematicPath, destination) {
    const [width, height, depth] = api.getSchematicDimensions(schematicPath)
    const [destX, destY, destZ] = destination

    for (let x = 0; x < width; x += 32) {
        for (let y = 0; y < height; y += 32) {
            for (let z = 0; z < depth; z += 32) {
                api.setChunkAsSchematic(
                    schematicPath,
                    [x, y, z],
                    [destX + x, destY + y, destZ + z],
                )
            }
        }
    }
}

loadSchematic("maps/arena.bloxdschem", [0, 64, 0])
```

## More coming soon!

If you think there are any guides we should add to this page, let us know in Discord!