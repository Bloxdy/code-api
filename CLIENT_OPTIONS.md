# Client Options

A player's "Client Options" impact what they are capable of doing in the world. For example you can give them a double jump by setting the player's `airJumpCount` to `1`. Alternatively their health bar can be increased by setting their `maxHealth` to `200`. These API methods allow you to modify the client options:

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

/**
 * Returns the current value of a client option
 *
 * @param {PlayerId} playerId
 * @param {PassedOption} option
 * @returns {ClientOptions[PassedOption]}
 */
getClientOption(playerId, option)

/**
 * Modify client options at runtime
 *
 * @param {PlayerId} playerId
 * @param {Partial<ClientOptions>} optionsObj - An object which contains key value pairs of new settings. E.g {canChange: true, speedMultiplier: false}
 * @returns {void}
 */
setClientOptions(playerId, optionsObj)

/**
 * Sets a client option to its default value. This will be the value stored in your game's defaultClientOptions, otherwise Bloxd's default.
 *
 * @param {PlayerId} playerId
 * @param {ClientOption} option
 * @returns {void}
 */
setClientOptionToDefault(playerId, option)
```

Here is the full list of available client options:

```js
/**
 * Whether the player can change blocks
 * @type {boolean}
 */
canChange = true

/**
 * Speed multiplier for the player.
 * Players are used to the default bloxd movement behaviour and speed,
 * and may be put off from your game if different muscle memory is required.
 * We suggest applying speed or slowness effects instead, using api.applyEffect.
 * @type {number}
 */
speedMultiplier = 1

/**
 * Speed multiplier for the player when crouching.
 * Players are used to the default bloxd movement behaviour and speed,
 * and may be put off from your game if different muscle memory is required.
 * We suggest applying speed or slowness effects instead, using api.applyEffect.
 * @type {number}
 */
crouchingSpeed = 2

/**
 * Amount of jump power the player has
 * @type {number}
 */
jumpAmount = 8

/**
 * Amount of air jumps the player has
 * @type {number}
 */
airJumpCount = 0

/**
 * Maximum multiplier for jump height when bunnyhopping
 * @type {number}
 */
bunnyhopMaxMultiplier = 1.3

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

/**
 * Not recommended to use anything other than "default" as client FPS can drop while loading the skybox
 * @type {string | EarthSkyBox}
 */
skyBox = "default"

/**
 * Whether to show the player in unloaded chunks
 * @type {boolean}
 */
showPlayersInUnloadedChunks = false

/**
 * Whether to allow the player to use the inventory
 * Disabling this will also disable the hotbar
 * @type {boolean}
 */
useInventory = true

/**
 * For now just enables the UI of the full inventory
 * @type {boolean}
 */
useFullInventory = true

/**
 * Whether to allow the player to craft items
 * useFullInventory must be true for this to work
 * @type {boolean}
 */
canCraft = true

/**
 * Whether to allow the player to pick up items
 * @type {boolean}
 */
canPickUpItems = true

/**
 * Default camera zoom level for the player
 * @type {number}
 */
playerZoom = 0

/**
 * Distance to zoom the camera out to
 * @type {number}
 */
zoomOutDistance = 3

/**
 * Maximum camera zoom level for the player
 * @type {number}
 */
maxPlayerZoom = 15

/**
 * Columns of the lobby leaderboard
 * @type {LobbyLeaderboardInfo}
 */
lobbyLeaderboardInfo = {
        name: {
            displayName: "Name",
            sortPriority: 0,
        },
    }

/**
 * Whether the player can customise their character
 * @type {boolean}
 */
canCustomiseChar = true

/**
 * The default block the player can change blocks to, used if canChange is true but useInventory is false
 * @type {string}
 */
defaultBlock = "Block of Gold"

/**
 * Error message for when the player fails to change a block
 * @type {string | CustomTextStyling}
 */
cantChangeError = "You cannot modify this block"

/**
 * Error message for when the player fails to break a block
 * @type {string | CustomTextStyling}
 */
cantBreakError = null

/**
 * Error message for when the player fails to place a block
 * @type {string | CustomTextStyling}
 */
cantBuildError = null

/**
 * The contents of the action button. Supports custom text styling. onTouchscreenActionButton will be called when button pressed.
 * @type {string | CustomTextStyling}
 */
touchscreenActionButton = null

/**
 * Whether a player can place fluid when canChange is false
 * @type {boolean}
 */
strictFluidBuckets = true

/**
 * Whether the player can use the zoom key
 * @type {boolean}
 */
canUseZoomKey = true

/**
 * Whether the player can use the alt action key (right click on PC)
 * @type {boolean}
 */
canAltAction = true

/**
 * Whether the player can see name tags through walls
 * @type {boolean}
 */
canSeeNametagsThroughWalls = true

/**
 * Whether to show basic movement controls
 * @type {boolean}
 */
showBasicMovementControls = true

/**
 * Large text to display in the middle of the screen
 * @type {string | CustomTextStyling}
 */
middleTextUpper = ""

/**
 * Small text to display in the middle of the screen
 * @type {string | CustomTextStyling}
 */
middleTextLower = ""

/**
 * Text to display by the crosshair
 * @type {string | CustomTextStyling}
 */
crosshairText = ""

/**
 * Text to display in the right info box
 * @type {string | CustomTextStyling}
 */
RightInfoText = ""

/**
 * If set, clients will only be able to see the closest x players (good for client perf in games with many players)
 * @type {number}
 */
numClosestPlayersVisible = null

/**
 * Whether to show the progress bar
 * @type {boolean}
 */
showProgressBar = false

/**
 * Whether to show the killfeed
 * @type {boolean}
 */
showKillfeed = true

/**
 * Allows player to select a channel that is passed as argument to onPlayerChat. See SharedTypes.ts for expected format
 * @type { { channelName: string; elementContent: string | CustomTextStyling; elementBgColor: string; }[] }
 */
chatChannels = null

/**
 * Whether the player is in creative mode
 * @type {boolean}
 */
creative = false

/**
 * Multiplier for the flying speed in creative mode
 * @type {number}
 */
flySpeedMultiplier = 1.5

/**
 * Whether the player can pick blocks (middle mouse click on PC), ignored if creative is false
 * @type {boolean}
 */
canPickBlocks = true

/**
 * The target the compass will point towards
 * @type {string | number | number[]}
 */
compassTarget = [0, 0, 0]

/**
 * Multiplier for the time to break any block
 * @type {number}
 */
ttbMultiplier = 1

/**
 * Whether the player can move items in their inventory, only applicable if useInventory is true
 * @type {boolean}
 */
inventoryItemsMoveable = true

/**
 * Whether the player is invincible
 * @type {boolean}
 */
invincible = false

/**
 * Maximum shield the player can have
 * @type {number}
 */
maxShield = 100

/**
 * Shield upon joining or respawning
 * @type {number}
 */
initialShield = 0

/**
 * Maximum health the player can have
 * @type {number}
 */
maxHealth = 100

/**
 * Health upon joining or respawning. Can be null for the player to not have health
 * @type {number}
 */
initialHealth = 100

/**
 * Fraction of max health that regens each regen tick
 * @type {number}
 */
healthRegenAmount = 0.05

/**
 * How often health regen is ticked
 * @type {number}
 */
healthRegenInterval = 4000

/**
 * How long after a player receives damage to start regen again
 * @type {number}
 */
healthRegenStartAfter = 5000

/**
 * Duration of the +damage effect from plum
 * @type {number}
 */
effectDamageDuration = 8000

/**
 * Duration of +speed effect from cracked coconut
 * @type {number}
 */
effectSpeedDuration = 8000

/**
 * Duration of +damage reduction effect from pear
 * @type {number}
 */
effectDamageReductionDuration = 13000

/**
 * Duration of +health regen effect from cherry
 * @type {number}
 */
effectHealthRegenDuration = 5000

/**
 * Duration of potion effects
 * @type {number}
 */
potionEffectDuration = 12000

/**
 * Duration of splash potion effects
 * @type {number}
 */
splashPotionEffectDuration = 8000

/**
 * Duration of arrow potion effects
 * @type {number}
 */
arrowPotionEffectDuration = 6000

/**
 * RGBA array [r, g, b, a] for camera screen tint effect. Values fall between 0 and 1.
 * @type {[number, number, number, number]}
 */
cameraTint = null

/**
 * After dying the player can respawn after this many seconds
 * @type {number}
 */
secsToRespawn = 3

/**
 * When player is dead, also show a play again button to matchmake player into a new lobby. Mostly useful for sessionBased games
 * @type {boolean}
 */
usePlayAgainButton = false

/**
 * If true, player will respawn automatically after secsToRespawn seconds
 * @type {boolean}
 */
autoRespawn = false

/**
 * Text to show on respawn button. (E.g. "Spectate")
 * @type {string}
 */
respawnButtonText = "general:respawn"

/**
 * Duration before a killstreak expires. (defaults to never expiring)
 * @type {number}
 */
killstreakDuration = 200000000

/**
 * Damage multiplier for all types of damage
 * @type {number}
 */
dealingDamageMultiplier = 1

/**
 * Damage multiplier for when the player hits a head. Only applies to guns
 * @type {number}
 */
dealingDamageHeadMultiplier = 1.75

/**
 * Damage multiplier for when the player hits a leg. Only applies to guns
 * @type {number}
 */
dealingDamageLegMultiplier = 1

/**
 * Mult for when the player hits neither a leg or a head. Only applies to guns
 * @type {number}
 */
dealingDamageDefaultMultiplier = 1

/**
 * Damage multiplier for all types of incoming damage
 * @type {number}
 */
receivingDamageMultiplier = 1

/**
 * Scale factor to use for dropped item meshes
 * @type {number}
 */
droppedItemScale = 1

/**
 * Amount that player camera is affected by movement based fov
 * @type {number}
 */
movementBasedFovScale = 1

/**
 * Amount of friction to apply to airborne players.
 * Only change if absolutely necessary i.e. Rocket Obby uses 0.
 * Players are used to the default bloxd movement behaviour and speed,
 * and may be put off from your game if different muscle memory is required.
 * We suggest applying speed or slowness effects instead, using api.applyEffect.
 * @type {number}
 */
airFrictionScale = 1

/**
 * Amount of friction to apply to grounded players.
 * Only change if absolutely necessary i.e. Rocket Obby uses 3.
 * Players are used to the default bloxd movement behaviour and speed,
 * and may be put off from your game if different muscle memory is required.
 * We suggest applying speed or slowness effects instead, using api.applyEffect.
 * @type {number}
 */
groundFrictionScale = 1

/**
 * Amount of acceleration to apply to airborne players.
 * Only change if absolutely necessary i.e. Rocket Obby uses 0.25.
 * Players are used to the default bloxd movement behaviour and speed,
 * and may be put off from your game if different muscle memory is required.
 * We suggest applying speed or slowness effects instead, using api.applyEffect.
 * @type {number}
 */
airAccScale = 1

/**
 * Whether to allow the player to strafe and conserve momentum while airborne.
 * Only change if absolutely necessary i.e. only Rocket Obby uses true.
 * Players are used to the default bloxd movement behaviour and speed,
 * and may be put off from your game if different muscle memory is required.
 * We suggest applying speed or slowness effects instead, using api.applyEffect.
 * @type {boolean}
 */
airMomentumConservation = false

/**
 * Whether to deal damage to the player when they fall
 * @type {boolean}
 */
fallDamage = false

/**
 * Normally only the world owner can edit code blocks and lobby code, and you can use this option to override that.
 * Remember as owner of the world you are responsible for any code anyone writes that could break Bloxd rules.
 * @type {boolean}
 */
canEditCode = false

/**
 * How much Aura XP is required per level.
 * @type {number}
 */
auraPerLevel = 100

/**
 * The maximum Aura Level attainable - Set to 0 to disable Aura XP
 * @type {number}
 */
maxAuraLevel = 0

/**
 * Fog distance which overrides graphic settings. Uses graphic settings if null.
 * @type {number}
 */
fogChunkDistanceOverride = null

/**
 * HEX string for fog colour override. e.g. #ffffff
 * @type {string}
 */
fogColourOverride = null

/**
 * Multiplier for horizontal knockback when dealing damage
 * @type {number}
 */
horizontalKnockbackMultiplier = 1

/**
 * Multiplier for vertical knockback when dealing damage
 * @type {number}
 */
verticalKnockbackMultiplier = 1

/**
 * Mult for the damage done by "stomping" on a lifeform, i.e.: falling on them wearing Spiked Boots.
 * @type {number}
 */
stompDamageMultiplier = 0

/**
 * Radius around the player that will be affected by the stomp damage.
 * @type {number}
 */
stompDamageRadius = 0

/**
 * Mult for the radius within which mobs can detect the player when crouching.
 * If a player's mult is 2, then mobs will think they are twice as far away.
 * @type {number}
 */
crouchMobDetectionRadiusMultiplier = 2

/**
 * How much the player bounces off of solid blocks.
 * A value of 1 is equivalent to every block acting as a mushroom.
 * @type {number}
 */
bounciness = 0

/**
 * Whether the player can climb walls
 * @type {boolean}
 */
canClimbWalls = false
```
