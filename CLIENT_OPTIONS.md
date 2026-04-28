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


## airAccScale

**Type:** `number`

**Default:** `1`

 

Amount of acceleration to apply to airborne players.

 Only change if absolutely necessary i.e. Rocket Obby uses 0.25.

 Players are used to the default bloxd movement behaviour and speed,

 and may be put off from your game if different muscle memory is required.

 We suggest applying speed or slowness effects instead, using api.applyEffect.

 



## airFrictionScale

**Type:** `number`

**Default:** `1`

 

Amount of friction to apply to airborne players.

 Only change if absolutely necessary i.e. Rocket Obby uses 0.

 Players are used to the default bloxd movement behaviour and speed,

 and may be put off from your game if different muscle memory is required.

 We suggest applying speed or slowness effects instead, using api.applyEffect.

 



## airJumpCount

**Type:** `number`

**Default:** `0`

 

Amount of air jumps the player has

 



## airMomentumConservation

**Type:** `boolean`

**Default:** `false`

 

Whether to allow the player to strafe and conserve momentum while airborne.

 Only change if absolutely necessary i.e. only Rocket Obby uses true.

 Players are used to the default bloxd movement behaviour and speed,

 and may be put off from your game if different muscle memory is required.

 We suggest applying speed or slowness effects instead, using api.applyEffect.

 



## arrowPotionEffectDuration

**Type:** `number`

**Default:** `6000`

 

Duration of arrow potion effects

 



## auraPerLevel

**Type:** `number`

**Default:** `100`

 

How much Aura XP is required per level.

 



## autoRespawn

**Type:** `boolean`

**Default:** `false`

 

If true, player will respawn automatically after secsToRespawn seconds

 



## bounciness

**Type:** `number`

**Default:** `0`

 

How much the player bounces off of solid blocks.

 A value of 1 is equivalent to every block acting as a mushroom.

 



## bunnyhopMaxMultiplier

**Type:** `number`

**Default:** `1.3`

 

Maximum multiplier for jump height when bunnyhopping

 



## cameraRoll

**Type:** `number`

**Default:** `0`

 

Roll angle of the camera in radians. Useful for disorientation effects, death effects, etc.

 



## cameraRollTransitionMs

**Type:** `number`

**Default:** `0`

 

Duration in ms to animate/transition to the camera roll angle (when you change cameraRoll). 0 = instant.

 Useful for smooth camera roll transitions.

 



## cameraTint

**Type:** `[number, number, number, number]`

**Default:** `null`

 

RGBA array [r, g, b, a] for camera screen tint effect. Values fall between 0 and 1.

 



## canAltAction

**Type:** `boolean`

**Default:** `true`

 

Whether the player can use the alt action key (right click on PC)

 



## canChange

**Type:** `boolean`

**Default:** `true`

 

Whether the player can change blocks

 



## canClimbWalls

**Type:** `boolean`

**Default:** `false`

 

Whether the player can climb walls

 



## canCraft

**Type:** `boolean`

**Default:** `true`

 

Whether to allow the player to craft items

 useFullInventory must be true for this to work

 



## canCrouch

**Type:** `boolean`

**Default:** `true`

 

Whether the player can crouch

 



## canCustomiseChar

**Type:** `boolean`

**Default:** `true`

 

Whether the player can customise their character

 



## canPickBlocks

**Type:** `boolean`

**Default:** `true`

 

Whether the player can pick blocks (middle mouse click on PC), ignored if creative is false

 



## canPickUpItems

**Type:** `boolean`

**Default:** `true`

 

Whether to allow the player to pick up items

 



## canSeeNametagsThroughWalls

**Type:** `boolean`

**Default:** `true`

 

Whether the player can see name tags through walls

 



## cantBreakError

**Type:** `string | CustomTextStyling`

**Default:** `null`

 

Error message for when the player fails to break a block

 



## cantBuildError

**Type:** `string | CustomTextStyling`

**Default:** `null`

 

Error message for when the player fails to place a block

 



## cantChangeError

**Type:** `string | CustomTextStyling`

**Default:** `"You cannot modify this block"`

 

Error message for when the player fails to change a block

 



## canUseZoomKey

**Type:** `boolean`

**Default:** `true`

 

Whether the player can use the zoom key

 



## chatChannels

**Type:** ` { channelName: string; elementContent: string | CustomTextStyling; elementBgColor: string; }[] `

**Default:** `null`

 

Allows player to select a channel that is passed as argument to onPlayerChat. See engineGameplayTypes.ts for expected format

 



## compassTarget

**Type:** `string | number | number[]`

**Default:** `[0, 0, 0]`

 

The target the compass will point towards

 



## creative

**Type:** `boolean`

**Default:** `false`

 

Whether the player is in creative mode

 



## crosshairText

**Type:** `string | CustomTextStyling`

**Default:** `""`

 

Text to display by the crosshair

 



## crouchingSpeed

**Type:** `number`

**Default:** `2`

 

Speed multiplier for the player when crouching.

 Players are used to the default bloxd movement behaviour and speed,

 and may be put off from your game if different muscle memory is required.

 We suggest applying speed or slowness effects instead, using api.applyEffect.

 



## crouchMobDetectionRadiusMultiplier

**Type:** `number`

**Default:** `2`

 

Mult for the radius within which mobs can detect the player when crouching.

 If a player's mult is 2, then mobs will think they are twice as far away.

 



## dealingDamageDefaultMultiplier

**Type:** `number`

**Default:** `1`

 

Mult for when the player hits neither a leg or a head. Only applies to guns

 



## dealingDamageHeadMultiplier

**Type:** `number`

**Default:** `1.75`

 

Damage multiplier for when the player hits a head. Only applies to guns

 



## dealingDamageLegMultiplier

**Type:** `number`

**Default:** `1`

 

Damage multiplier for when the player hits a leg. Only applies to guns

 



## dealingDamageMultiplier

**Type:** `number`

**Default:** `1`

 

Damage multiplier for all types of damage

 



## defaultBlock

**Type:** `string`

**Default:** `"Block of Gold"`

 

The default block the player can change blocks to, used if canChange is true but useInventory is false

 



## droppedItemScale

**Type:** `number`

**Default:** `1`

 

Scale factor to use for dropped item meshes

 



## effectDamageDuration

**Type:** `number`

**Default:** `8000`

 

Duration of the +damage effect from plum

 



## effectDamageReductionDuration

**Type:** `number`

**Default:** `13000`

 

Duration of +damage reduction effect from pear

 



## effectHealthRegenDuration

**Type:** `number`

**Default:** `5000`

 

Duration of +health regen effect from cherry

 



## effectSpeedDuration

**Type:** `number`

**Default:** `8000`

 

Duration of +speed effect from cracked coconut

 



## fallDamage

**Type:** `boolean`

**Default:** `false`

 

Whether to deal damage to the player when they fall

 



## flySpeedMultiplier

**Type:** `number`

**Default:** `1.5`

 

Multiplier for the flying speed in creative mode

 



## fogChunkDistanceOverride

**Type:** `number`

**Default:** `null`

 

Fog distance which overrides graphic settings. Uses graphic settings if null.

 



## fogColourOverride

**Type:** `string`

**Default:** `null`

 

RGB string for fog colour override. e.g. #ffffff

 



## forcedCameraDirection

**Type:** `[number, number, number]`

**Default:** `null`

 

Force the camera to look in a specific direction [x, y, z]. Set to null to allow free camera movement.

 



## forcedCameraDirectionTransitionMs

**Type:** `number`

**Default:** `0`

 

Duration in ms to animate/transition to the forced camera direction (when you change forcedCameraDirection). 0 = instant.

 Useful for smooth camera movements.

 



## groundFrictionScale

**Type:** `number`

**Default:** `1`

 

Amount of friction to apply to grounded players.

 Only change if absolutely necessary i.e. Rocket Obby uses 3.

 Players are used to the default bloxd movement behaviour and speed,

 and may be put off from your game if different muscle memory is required.

 We suggest applying speed or slowness effects instead, using api.applyEffect.

 



## healthRegenAmount

**Type:** `number`

**Default:** `0.05`

 

Fraction of max health that regens each regen tick

 



## healthRegenInterval

**Type:** `number`

**Default:** `4000`

 

How often health regen is ticked

 



## healthRegenStartAfter

**Type:** `number`

**Default:** `5000`

 

How long after a player receives damage to start regen again

 



## heldLightColourOverride

**Type:** `string`

**Default:** `null`

 

Held item light colour override - hex colour string e.g. #ffffff. Applied regardless of any held item.

 



## hideCoordinates

**Type:** `boolean`

**Default:** `false`

 

When true, hides world and chunk coordinates regardless of the player's setting.

 



## horizontalKnockbackMultiplier

**Type:** `number`

**Default:** `1`

 

Multiplier for horizontal knockback when dealing damage

 



## initialHealth

**Type:** `number`

**Default:** `100`

 

Health upon joining or respawning. Can be null for the player to not have health

 



## initialShield

**Type:** `number`

**Default:** `0`

 

Shield upon joining or respawning

 



## inventoryItemsMoveable

**Type:** `boolean`

**Default:** `true`

 

Whether the player can move items in their inventory, only applicable if useInventory is true

 



## invincible

**Type:** `boolean`

**Default:** `false`

 

Whether the player is invincible

 



## jumpAmount

**Type:** `number`

**Default:** `8`

 

Amount of jump power the player has

 



## killstreakDuration

**Type:** `number`

**Default:** `200000000`

 

Duration before a killstreak expires. (defaults to never expiring)

 



## lightingOverride

**Type:** `boolean`

**Default:** `null`

 

When null, just use the player's graphics setting. When set, forces lighting on (true) or off (false).

 



## lobbyLeaderboardInfo

**Type:** `LobbyLeaderboardInfo`

**Default:** ```ts
{
	name: {
		displayName: "Name",
		sortPriority: 0,
	},
}
```

 

Columns of the lobby leaderboard

 



## maxAuraLevel

**Type:** `number`

**Default:** `0`

 

The maximum Aura Level attainable - Set to 0 to disable Aura XP

 



## maxHealth

**Type:** `number`

**Default:** `100`

 

Maximum health the player can have

 



## maxPlayerZoom

**Type:** `number`

**Default:** `15`

 

Maximum camera zoom level for the player

 



## maxShield

**Type:** `number`

**Default:** `100`

 

Maximum shield the player can have

 



## middleTextLower

**Type:** `string | CustomTextStyling`

**Default:** `""`

 

Small text to display in the middle of the screen

 



## middleTextUpper

**Type:** `string | CustomTextStyling`

**Default:** `""`

 

Large text to display in the middle of the screen

 



## minChunkAddDist

**Type:** `[number, number]`

**Default:** `[2, 2]`

 

Minimum size of region around player where chunks are loaded.

 Format [horizontalMinChunkRadius, verticalMinChunkRadius].

 Each value should be between 2 and 4.

 

 We recommend leaving this at the default of [2, 2] unless you have a specific reason to change it.

 (e.g. you need players to see the bottom of a dropper)

 This is because higher values can be laggier on low-end devices.

 



## movementBasedFovScale

**Type:** `number`

**Default:** `1`

 

Amount that player camera is affected by movement based fov

 



## music

**Type:** `Song`

**Default:** `null`

 

The music track to play in the background

 



## musicVolumeLevel

**Type:** `number`

**Default:** `0.6`

 

Volume level for the music

 



## numClosestPlayersVisible

**Type:** `number`

**Default:** `null`

 

If set, clients will only be able to see the closest x players (good for client perf in games with many players)

 



## playerZoom

**Type:** `number`

**Default:** `0`

 

Default camera zoom level for the player

 



## potionEffectDuration

**Type:** `number`

**Default:** `12000`

 

Duration of potion effects

 



## proximityFadeDistance

**Type:** `number`

**Default:** `0.625`

 

Distance in blocks over which we reduce the opacity of entities as they approach the camera.

 



## proximityFadeMinOpacity

**Type:** `number`

**Default:** `0.5`

 

Minimum opacity multiplier reachable when fading entities based on camera proximity.

 The player's own model is always able to fade to 0, and entities being ridden stay fully opaque (as if this value was 1).

 



## receivingDamageMultiplier

**Type:** `number`

**Default:** `1`

 

Damage multiplier for all types of incoming damage

 



## respawnButtonText

**Type:** `string`

**Default:** `"general:respawn"`

 

Text to show on respawn button. (E.g. "Spectate")

 



## RightInfoText

**Type:** `string | CustomTextStyling`

**Default:** `""`

 

Text to display in the right info box

 



## runningSpeed

**Type:** `number`

**Default:** `7`

 

Running speed for the player.

 STRONGLY recommend using `speedMultiplier` unless you have a specific use case for this, runningSpeed doesn't make UX sense on mobile.

 (Walking speed is ignored for mobile players, mobile player speed is determined by joystick input and the max of runningSpeed & walkingSpeed).

 

 Players are used to the default bloxd movement behaviour and speed,

 and may be put off from your game if different muscle memory is required.

 We suggest applying speed or slowness effects instead, using api.applyEffect.

 

 The only use case for walkingSpeed/runningSpeed over speedMultiplier or speed effects is to disable running or to inverse walking/running (so you run by default and e.g. hold shift to go slower).

 



## secsToRespawn

**Type:** `number`

**Default:** `5`

 

After dying the player can respawn after this many seconds

 



## showBasicMovementControls

**Type:** `boolean`

**Default:** `true`

 

Whether to show basic movement controls

 



## showKillfeed

**Type:** `boolean`

**Default:** `true`

 

Whether to show the killfeed

 



## showPlayersInUnloadedChunks

**Type:** `boolean`

**Default:** `false`

 

Whether to show the player in unloaded chunks

 



## showProgressBar

**Type:** `boolean`

**Default:** `false`

 

Whether to show the progress bar

 



## skyBox

**Type:** `string | EarthSkyBox`

**Default:** `"default"`

 

Not recommended to use anything other than "default" as client FPS can drop while loading the skybox

 



## skyLightColourOverride

**Type:** `string`

**Default:** `null`

 

Sky light colour override - hex string e.g. #ffffff.

 



## speedMultiplier

**Type:** `number`

**Default:** `1`

 

Speed multiplier for the player.

 Players are used to the default bloxd movement behaviour and speed,

 and may be put off from your game if different muscle memory is required.

 We suggest applying speed or slowness effects instead, using api.applyEffect.

 



## splashPotionEffectDuration

**Type:** `number`

**Default:** `8000`

 

Duration of splash potion effects

 



## stompDamageMultiplier

**Type:** `number`

**Default:** `0`

 

Mult for the damage done by "stomping" on a lifeform, i.e.: falling on them wearing Spiked Boots.

 



## stompDamageRadius

**Type:** `number`

**Default:** `0`

 

Radius around the player that will be affected by the stomp damage.

 



## strictFluidBuckets

**Type:** `boolean`

**Default:** `true`

 

Whether a player can place fluid when canChange is false

 



## touchscreenActionButton

**Type:** `string | CustomTextStyling`

**Default:** `null`

 

The contents of the action button. Supports custom text styling. onTouchscreenActionButton will be called when button pressed.

 



## ttbMultiplier

**Type:** `number`

**Default:** `1`

 

Multiplier for the time to break any block

 



## useFullInventory

**Type:** `boolean`

**Default:** `true`

 

For now just enables the UI of the full inventory

 



## useInventory

**Type:** `boolean`

**Default:** `true`

 

Whether to allow the player to use the inventory

 Disabling this will also disable the hotbar

 



## usePlayAgainButton

**Type:** `boolean`

**Default:** `false`

 

When player is dead, also show a play again button to matchmake player into a new lobby. Mostly useful for sessionBased games

 



## verticalKnockbackMultiplier

**Type:** `number`

**Default:** `1`

 

Multiplier for vertical knockback when dealing damage

 



## walkingSpeed

**Type:** `number`

**Default:** `4`

 

Walking speed for the player.

 STRONGLY recommend using `speedMultiplier` unless you have a specific use case for this, walkingSpeed doesn't make UX sense on mobile.

 (Walking speed ignored for mobile players, mobile player speed is determined by joystick input and the max of runningSpeed & walkingSpeed).

 

 Players are used to the default bloxd movement behaviour and speed,

 and may be put off from your game if different muscle memory is required.

 We suggest applying speed or slowness effects instead, using api.applyEffect.

 

 The only use case for walkingSpeed/runningSpeed over speedMultiplier or speed effects is to disable running or to inverse walking/running (so you run by default and e.g. hold shift to go slower).

 



## zoomOutDistance

**Type:** `number`

**Default:** `3`

 

Distance to zoom the camera out to

 



