# Client Options

A player's "Client Options" impact what they are capable of doing in the world. For example you can give them a double jump by setting the player's `airJumpCount` to `1`. Alternatively their health bar can be increased by setting their `maxHealth` to `200`. These API methods allow you to modify the client options:

## API functions for modifying Client Options
```js
/**
 * Modify a client option at runtime and send to the client if it changed
 *
 * @param {PlayerId} playerId
 * @param {PassedOption} option - The name of the option
 * @param {ClientOptions[PassedOption]} value - The new value of the option
 * @returns {void}
 */
setClientOption(playerId, option, value)
```

```js
/**
 * Returns the current value of a client option
 *
 * @param {PlayerId} playerId
 * @param {PassedOption} option
 * @returns {ClientOptions[PassedOption]}
 */
getClientOption(playerId, option)
```

```js
/**
 * Modify client options at runtime
 *
 * @param {PlayerId} playerId
 * @param {Partial<ClientOptions>} optionsObj - An object which contains key value pairs of new settings. E.g {canChange: true, speedMultiplier: false}
 * @returns {void}
 */
setClientOptions(playerId, optionsObj)
```

```js
/**
 * Sets a client option to its default value. This will be the value stored in your game's defaultClientOptions, otherwise Bloxd's default.
 *
 * @param {PlayerId} playerId
 * @param {ClientOption} option
 * @returns {void}
 */
setClientOptionToDefault(playerId, option)
```

## Options list
Here is the full list of available client options:
> [!NOTE]
> This list has been reorganized compared to the Official documentation, but all the Client Options are here.


### Can Options
```js
/**
 * Whether the player can change blocks
 * @type {boolean}
 */
canChange = true
```

```js
/**
 * Whether to allow the player to craft items
 * useFullInventory must be true for this to work
 * @type {boolean}
 */
canCraft = true
```

```js
/**
 * Whether to allow the player to pick up items
 * @type {boolean}
 */
canPickUpItems = true
```

```js
/**
 * Whether the player can customise their character
 * @type {boolean}
 */
canCustomiseChar = true
```

```js
/**
 * Whether the player can use the zoom key
 * @type {boolean}
 */
canUseZoomKey = true
```

```js
/**
 * Whether the player can use the alt action key (right click on PC)
 * @type {boolean}
 */
canAltAction = true
```

```js
/**
 * Whether the player can see name tags through walls
 * @type {boolean}
 */
canSeeNametagsThroughWalls = true
```

```js
/**
 * Whether the player can pick blocks (middle mouse click on PC), ignored if creative is false
 * @type {boolean}
 */
canPickBlocks = true
```
```js
/**
 * Whether the player can climb walls
 * @type {boolean}
 */
canClimbWalls = false
```

```js
/**
 * Whether the player can crouch
 * @type {boolean}
 */
canCrouch = true
```

### Can't Options
```js
/**
 * Error message for when the player fails to change a block
 * @type {string | CustomTextStyling}
 */
cantChangeError = "You cannot modify this block"
```

```js
/**
 * Error message for when the player fails to break a block
 * @type {string | CustomTextStyling}
 */
cantBreakError = null
```

```js
/**
 * Error message for when the player fails to place a block
 * @type {string | CustomTextStyling}
 */
cantBuildError = null
```

### Use Options
```js
/**
 * Whether to allow the player to use the inventory
 * Disabling this will also disable the hotbar
 * @type {boolean}
 */
useInventory = true
```

```js
/**
 * For now just enables the UI of the full inventory
 * @type {boolean}
 */
useFullInventory = true
```

### Show Options
```js
/**
 * Whether to show the player in unloaded chunks
 * @type {boolean}
 */
showPlayersInUnloadedChunks = false
```

```js
/**
 * Whether to show the killfeed
 * @type {boolean}
 */
showKillfeed = true
```

```js
/**
 * Whether to show basic movement controls
 * @type {boolean}
 */
showBasicMovementControls = true
```

```js
/**
 * If set, clients will only be able to see the closest x players (good for client perf in games with many players)
 * @type {number}
 */
numClosestPlayersVisible = null
```

```js
/**
 * Whether to show the progress bar
 * @type {boolean}
 */
showProgressBar = false
```

### Speed Options
```js
/**
 * Speed multiplier for the player.
 * Players are used to the default bloxd movement behaviour and speed,
 * and may be put off from your game if different muscle memory is required.
 * We suggest applying speed or slowness effects instead, using api.applyEffect.
 * @type {number}
 */
speedMultiplier = 1
```

```js
/**
 * Speed multiplier for the player when crouching.
 * Players are used to the default bloxd movement behaviour and speed,
 * and may be put off from your game if different muscle memory is required.
 * We suggest applying speed or slowness effects instead, using api.applyEffect.
 * @type {number}
 */
crouchingSpeed = 2
```

```js
 * Walking speed for the player.
 * STRONGLY recommend using `speedMultiplier` unless you have a specific use case for this, walkingSpeed doesn't make UX sense on mobile.
 * (Walking speed ignored for mobile players, mobile player speed is determined by joystick input and the max of runningSpeed & walkingSpeed).
 *
 * Players are used to the default bloxd movement behaviour and speed,
 * and may be put off from your game if different muscle memory is required.
 * We suggest applying speed or slowness effects instead, using api.applyEffect.
 *
 * The only use case for walkingSpeed/runningSpeed over speedMultiplier or speed effects is to disable running or to inverse walking/running (so you run by default and e.g. hold shift to go slower).
 * @type {number}
 */
walkingSpeed = 4
```

```js
/**
 * Running speed for the player.
 * STRONGLY recommend using `speedMultiplier` unless you have a specific use case for this, runningSpeed doesn't make UX sense on mobile.
 * (Walking speed is ignored for mobile players, mobile player speed is determined by joystick input and the max of runningSpeed & walkingSpeed).
 *
 * Players are used to the default bloxd movement behaviour and speed,
 * and may be put off from your game if different muscle memory is required.
 * We suggest applying speed or slowness effects instead, using api.applyEffect.
 *
 * The only use case for walkingSpeed/runningSpeed over speedMultiplier or speed effects is to disable running or to inverse walking/running (so you run by default and e.g. hold shift to go slower).
 * @type {number}
 */
runningSpeed = 7
```

```js
/**
 * Multiplier for the flying speed in creative mode
 * @type {number}
 */
flySpeedMultiplier = 1.5
```

### Jump Options
```js
/**
 * Amount of jump power the player has
 * @type {number}
 */
jumpAmount = 8
```

```js
/**
 * Amount of air jumps the player has
 * @type {number}
 */
airJumpCount = 0
```

```js
/**
 * How much the player bounces off of solid blocks.
 * A value of 1 is equivalent to every block acting as a mushroom.
 * @type {number}
 */
bounciness = 0
```

### Bunnyhop Options
```js
/**
 * Maximum multiplier for jump height when bunnyhopping
 * @type {number}
 */
bunnyhopMaxMultiplier = 1.3
```

### Music Options
```js
/**
 * The music track to play in the background
 * @type {Song}
 */
music = null

/**
 * Volume level for the music
 * @type {number}
 */
musicVolumeLevel = 0.6
```

### Camera Options
```js
/**
 * RGBA array [r, g, b, a] for camera screen tint effect. Values fall between 0 and 1.
 * @type {[number, number, number, number]}
 */
cameraTint = null
```

```js
/**
 * Default camera zoom level for the player
 * @type {number}
 */
playerZoom = 0
```

```js
/**
 * Distance to zoom the camera out to
 * @type {number}
 */
zoomOutDistance = 3
```

```js
/**
 * Maximum camera zoom level for the player
 * @type {number}
 */
maxPlayerZoom = 15
```

```js
/**
 * Duration in ms to animate/transition to the forced camera direction (when you change forcedCameraDirection). 0 = instant.
 * Useful for smooth camera movements.
 * @type {number}
 */
forcedCameraDirectionTransitionMs = 0
```

```js
/**
 * Roll angle of the camera in radians. Useful for disorientation effects, death effects, etc.
 * @type {number}
 */
cameraRoll = 0
```

```js
/**
 * Duration in ms to animate/transition to the camera roll angle (when you change cameraRoll). 0 = instant.
 * Useful for smooth camera roll transitions.
 * @type {number}
 */
cameraRollTransitionMs = 0
```

```js
/**
 * When null, just use the player's graphics setting. When set, forces lighting on (true) or off (false).
 * @type {boolean}
 */
lightingOverride = null
```

```js
/**
 * Sky light colour override - hex string e.g. #ffffff
 * @type {string}
 */
skyLightColourOverride = null
```

### Fog Options
```js
/**
 * Fog distance which overrides graphic settings. Uses graphic settings if null.
 * @type {number}
 */
fogChunkDistanceOverride = null
```

```js
/**
 * RGB string for fog colour override. e.g. #ffffff
 * @type {string}
 */
fogColourOverride = null
```
### Fade Options
```js
/**
 * Distance in blocks over which we reduce the opacity of entities as they approach the camera.
 * @type {number}
 */
proximityFadeDistance = 0.625
```
```js
/**
 * Minimum opacity multiplier reachable when fading entities based on camera proximity.
 * The player's own model is always able to fade to 0, and entities being ridden stay fully opaque (as if this value was 1).
 * @type {number}
 */
proximityFadeMinOpacity = 0.5
```

### Leaderboard Options
```js
/**
 * Columns of the lobby leaderboard
 * @type {LobbyLeaderboardInfo}
 */
lobbyLeaderboardInfo = {
        pfp: {
            sortPriority: 1,
        },
        name: {
            displayName: "Name",
            sortPriority: 0,
        },
    }
```

### Text Options
```js
/**
 * Large text to display in the middle of the screen
 * @type {string | CustomTextStyling}
 */
middleTextUpper = ""
```

```js
/**
 * Small text to display in the middle of the screen
 * @type {string | CustomTextStyling}
 */
middleTextLower = ""
```

```js
/**
 * Text to display by the crosshair
 * @type {string | CustomTextStyling}
 */
crosshairText = ""
```

```js
/**
 * Text to display in the right info box
 * @type {string | CustomTextStyling}
 */
RightInfoText = ""

// RightInfoText example:

api.setClientOption(playerId, "RightInfoText", [
     {str: "text"}, // simple line of text
     "\n", // adds an extra line
     {str: "text",style:{color:"red"}}, // simple line of colored text
     "\n", // adds extra line
     {str:"text "+var,style:{color:'green'}} // colored text with a var added to it
    ])
```

```js
/**
 * The contents of the action button. Supports custom text styling. onTouchscreenActionButton will be called when button pressed.
 * @type {string | CustomTextStyling}
 */
touchscreenActionButton = null
```
```js
/**
 * Text to display at the crosshair's location. Supports custom text styling.
 * @type {string | CustomTextStyling}
 */
crosshairText = ""
```

### Inventory Options
```js
/**
 * Whether the player can move items in their inventory, only applicable if useInventory is true
 * @type {boolean}
 */
inventoryItemsMoveable = true
```

### Health and Shield Options
```js
/**
 * Whether the player is invincible
 * @type {boolean}
 */
invincible = false
```

```js
/**
 * Maximum shield the player can have
 * @type {number}
 */
maxShield = 100
```

```js
/**
 * Shield upon joining or respawning
 * @type {number}
 */
initialShield = 0
```

```js
/**
 * Maximum health the player can have
 * @type {number}
 */
maxHealth = 100
```

```js
/**
 * Health upon joining or respawning. Can be null for the player to not have health
 * @type {number}
 */
initialHealth = 100
```

```js
/**
 * Fraction of max health that regens each regen tick
 * @type {number}
 */
healthRegenAmount = 0.05
```

```js
/**
 * How often health regen is ticked
 * @type {number}
 */
healthRegenInterval = 4000
```

```js
/**
 * How long after a player receives damage to start regen again
 * @type {number}
 */
healthRegenStartAfter = 5000
```

### Effect Options
```js
/**
 * Duration of the +damage effect from plum
 * @type {number}
 */
effectDamageDuration = 8000
```

```js
/**
 * Duration of +speed effect from cracked coconut
 * @type {number}
 */
effectSpeedDuration = 8000
```

```js
/**
 * Duration of +damage reduction effect from pear
 * @type {number}
 */
effectDamageReductionDuration = 13000
```

```js
/**
 * Duration of +health regen effect from cherry
 * @type {number}
 */
effectHealthRegenDuration = 5000
```

```js
/**
 * Duration of potion effects
 * @type {number}
 */
potionEffectDuration = 12000
```

```js
/**
 * Duration of splash potion effects
 * @type {number}
 */
splashPotionEffectDuration = 8000
```

```js
/**
 * Duration of arrow potion effects
 * @type {number}
 */
arrowPotionEffectDuration = 6000
```

```js
/**
 * RGBA array [r, g, b, a] for camera screen tint effect. Values fall between 0 and 1.
 * @type {[number, number, number, number]}
 */
cameraTint = null
```

### Respawn Options
```js
/**
 * After dying the player can respawn after this many seconds
 * @type {number}
 */
secsToRespawn = 5
```

```js
/**
 * When player is dead, also show a play again button to matchmake player into a new lobby. Mostly useful for sessionBased games
 * @type {boolean}
 */
usePlayAgainButton = false
```

```js
/**
 * If true, player will respawn automatically after secsToRespawn seconds
 * @type {boolean}
 */
autoRespawn = false
```

```js
/**
 * Text to show on respawn button. (E.g. "Spectate")
 * @type {string}
 */
respawnButtonText = "general:respawn"
```

```js
/**
 * Duration before a killstreak expires. (defaults to never expiring)
 * @type {number}
 */
killstreakDuration = 200000000
```

### Damage Options
```js
/**
 * Damage multiplier for all types of damage
 * @type {number}
 */
dealingDamageMultiplier = 1
```

```js
/**
 * Damage multiplier for when the player hits a head. Only applies to guns
 * @type {number}
 */
dealingDamageHeadMultiplier = 1.75
```

```js
/**
 * Damage multiplier for when the player hits a leg. Only applies to guns
 * @type {number}
 */
dealingDamageLegMultiplier = 1
```

```js
/**
 * Mult for when the player hits neither a leg or a head. Only applies to guns
 * @type {number}
 */
dealingDamageDefaultMultiplier = 1
```

```js
/**
 * Damage multiplier for all types of incoming damage
 * @type {number}
 */
receivingDamageMultiplier = 1
```
#### Fall Damage Options
```js
/**
 * Whether to deal damage to the player when they fall
 * @type {boolean}
 */
fallDamage = false
```
#### Stomp Damage Options
```js
/**
 * Mult for the damage done by "stomping" on a lifeform, i.e.: falling on them wearing Spiked Boots.
 * @type {number}
 */
stompDamageMultiplier = 0
```
```js
/**
 * Radius around the player that will be affected by the stomp damage.
 * @type {number}
 */
stompDamageRadius = 0
```

### Knockback Options
```js
/**
 * Multiplier for horizontal knockback when dealing damage
 * @type {number}
 */
horizontalKnockbackMultiplier = 1
```

```js
/**
 * Multiplier for vertical knockback when dealing damage
 * @type {number}
 */
verticalKnockbackMultiplier = 1
```

### Kart Options
```js
/**
 * Speed multiplier for karts
 * @type {number}
 */
kartTargetSpeedMult = 1
```

```js
/**
 * Speed multiplier for speed boost effects in karts
 * @type {number}
 */
kartSpeedEffectMult = 1
```

```js
/**
 * Kart gliding height
 * @type {number}
 */
kartGliderTargetHeight = 25
```

```js
/**
 * Kart ground height
 * @type {number}
 */
kartGroundHeight = 4
```

```js
/**
 * How much to step towards target speed each movement tick (acceleration)
 * @type {number}
 */
kartApproachMaxSpeedScalar = 1
```

### Friction Options
```js
/**
 * Amount of friction to apply to airborne players.
 * Only change if absolutely necessary i.e. Rocket Obby uses 0.
 * Players are used to the default bloxd movement behaviour and speed,
 * and may be put off from your game if different muscle memory is required.
 * We suggest applying speed or slowness effects instead, using api.applyEffect.
 * @type {number}
 */
airFrictionScale = 1
```

```js
/**
 * Amount of friction to apply to grounded players.
 * Only change if absolutely necessary i.e. Rocket Obby uses 3.
 * Players are used to the default bloxd movement behaviour and speed,
 * and may be put off from your game if different muscle memory is required.
 * We suggest applying speed or slowness effects instead, using api.applyEffect.
 * @type {number}
 */
groundFrictionScale = 1
```

```js
/**
 * Amount of acceleration to apply to airborne players.
 * Only change if absolutely necessary i.e. Rocket Obby uses 0.25.
 * Players are used to the default bloxd movement behaviour and speed,
 * and may be put off from your game if different muscle memory is required.
 * We suggest applying speed or slowness effects instead, using api.applyEffect.
 * @type {number}
 */
airAccScale = 1
```

```js
/**
 * Whether to allow the player to strafe and conserve momentum while airborne.
 * Only change if absolutely necessary i.e. only Rocket Obby uses true.
 * Players are used to the default bloxd movement behaviour and speed,
 * and may be put off from your game if different muscle memory is required.
 * We suggest applying speed or slowness effects instead, using api.applyEffect.
 * @type {boolean}
 */
airMomentumConservation = false
```

### canEditCode Options
```js
/**
 * Normally only the world owner can edit code blocks and lobby code, and you can use this option to override that.
 * Remember as owner of the world you are responsible for any code anyone writes that could break Bloxd rules.
 * @type {boolean}
 */
canEditCode = false
```

### Aura Options
```js
/**
 * How much Aura XP is required per level.
 * @type {number}
 */
auraPerLevel = 100
```

```js
/**
 * The maximum Aura Level attainable - Set to 0 to disable Aura XP
 * @type {number}
 */
maxAuraLevel = 0
```

### Misc
```js
/**
 * Mult for the radius within which mobs can detect the player when crouching.
 * If a player's mult is 2, then mobs will think they are twice as far away.
 * @type {number}
 */
crouchMobDetectionRadiusMultiplier = 2
```

```js
/**
 * Allows player to select a channel that is passed as argument to onPlayerChat. See engineGameplayType.ts for expected format
 * @type { { channelName: string; elementContent: string | CustomTextStyling; elementBgColor: string; }[] }
 */
chatChannels = null
```


```js
/**
 * Scale factor to use for dropped item meshes
 * @type {number}
 */
droppedItemScale = 1
```

```js
/**
 * Amount that player camera is affected by movement based fov
 * @type {number}
 */
movementBasedFovScale = 1
```

```js
/**
 * Whether the player is in creative mode
 * @type {boolean}
 */
creative = false
```

```js
/**
 * The target the compass will point towards
 * @type {string | number | number[]}
 */
compassTarget = [0, 0, 0]
```

```js
/**
 * Not recommended to use anything other than "default" as client FPS can drop while loading the skybox
 * @type {string | EarthSkyBox}
 */
skyBox = "default"
```

```js
/**
 * Minimum size of region around player where chunks are loaded.
 * Format [horizontalMinChunkRadius, verticalMinChunkRadius].
 * Each value should be between 2 and 4.
 *
 * We recommend leaving this at the default of [2, 2] unless you have a specific reason to change it.
 * (e.g. you need players to see the bottom of a dropper)
 * This is because higher values can be laggier on low-end devices.
 * @type {[number, number]}
 */
minChunkAddDist = [2, 2]
```

```js
/**
 * The default block the player can change blocks to, used if canChange is true but useInventory is false
 * @type {string}
 */
defaultBlock = "Block of Gold"
```

```js
/**
 * Multiplier for the time to break any block
 * @type {number}
 */
ttbMultiplier = 1
```

```js
/**
 * Whether a player can place fluid when canChange is false
 * @type {boolean}
 */
strictFluidBuckets = true
```

## Extra Information
```js
type CustomTextStyling = (string | EntityName | TranslatedText | StyledIcon | StyledText)[]
```

```js
type IngameIconName = "Damage" | "Damage Reduction" | "Speed" | "VoidJump" | "Fist" | "Frozen" | "Hydrated" | "Invisible" | "Jump Boost" | "Poisoned" | "Slowness" | "Weakness" | "Health Regen" | "Haste" | "Double Jump" | "Heat Resistance" | "Gliding" | "Boating" | "Obsidian Boating" | "Riding" | "Bunny Hop" | "FallDamage" | "Feather Falling" | "Thief" | "Rested Damage" | "Rested Haste" | "Rested Speed" | "Rested Farming Yield" | "Rested Aura" | "Damage Enchantment" | "Critical Damage Enchantment" | "Attack Speed Enchantment" | "Protection Enchantment" | "Health Enchantment" | "Health Regen Enchantment" | "Stomp Damage Enchantment" | "Knockback Resist Enchantment" | "Arrow Speed Enchantment" | "Arrow Damage Enchantment" | "Quick Charge Enchantment" | "Break Speed Enchantment" | "Momentum Enchantment" | "Mining Yield Enchantment" | "Farming Yield Enchantment" | "Mining Aura Enchantment" | "Digging Aura Enchantment" | "Lumber Aura Enchantment" | "Farming Aura Enchantment" | "Vertical Knockback Enchantment" | "Horizontal Knockback Enchantment" | "Health" | "HealthShield"
```

```js
type StyledIcon = {
    icon: string
    style?: {
        color?: string
        colour?: string
        fontSize?: string
        opacity?: number
    }
}
```

```js
type StyledText = {
    str: string | EntityName | TranslatedText
    style?: {
        color?: string
        colour?: string
        fontWeight?: string
        fontSize?: string
        fontStyle?: string
        opacity?: number
    }
    clickableUrl?: string
}
```

```js
type TranslatedText = {
    translationKey: string
    params?: Record<string, string | number | boolean | EntityName>
}
```

```js
type EarthSkyBox = {
    type: "earth"
    inclination?: number
    turbidity?: number
    infiniteDistance?: number
    luminance?: number
    yCameraOffset?: number
    azimuth?: number
    // Not part of sky model by default; heavily tint to a vertex color
    vertexTint?: [number, number, number]
}
```

```js
enum WalkThroughType {
    CANT_WALK_THROUGH = 0,
    CAN_WALK_THROUGH = 1,
    DEFAULT_WALK_THROUGH = 2,
}
```

```js
SkyBoxOptions = [
        default,
        earth,
        interstellar,
        space_lightblue,
        space_blue,
        space_red,
        underwater
]
```
