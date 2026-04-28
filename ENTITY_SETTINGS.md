# Entity Settings

An "Entity Setting" impacts how a player sees or interacts with another player or entity.
E.g. Player 1 could have an otherEntitySetting for entity 2 as opacity set to 0.5. This means player 1 sees entity 2 as partly see-through. Player1 is the relevant player, player2 is the targeted player.
These API methods allow you to modify entity settings:

```js
/**
 * Set every player's other-entity setting to a specific value for a particular player.
 * includeNewJoiners=true means that new players joining the game will also have this other player setting applied.
 *
 * @param {PlayerId} targetedPlayerId
 * @param {Setting} settingName
 * @param {OtherEntitySettings[Setting]} settingValue
 * @param {boolean} [includeNewJoiners]
 * @returns {void}
 */
setTargetedPlayerSettingForEveryone(targetedPlayerId, settingName, settingValue, includeNewJoiners)

/**
 * Set a player's other-entity setting for every lifeform in the game.
 * includeNewJoiners=true means that the player will have the setting applied to new joiners.
 *
 * @param {PlayerId} playerId
 * @param {Setting} settingName
 * @param {OtherEntitySettings[Setting]} settingValue
 * @param {boolean} [includeNewJoiners]
 * @returns {void}
 */
setEveryoneSettingForPlayer(playerId, settingName, settingValue, includeNewJoiners)

/**
 * Set a player's other-entity setting for a specific entity.
 *
 * @param {PlayerId} relevantPlayerId
 * @param {EntityId} targetedEntityId
 * @param {Setting} settingName
 * @param {OtherEntitySettings[Setting]} settingValue
 * @returns {void}
 */
setOtherEntitySetting(relevantPlayerId, targetedEntityId, settingName, settingValue)

/**
 * Set many of a player's other-entity settings for a specific entity.
 *
 * @param {PlayerId} relevantPlayerId
 * @param {EntityId} targetedEntityId
 * @param {Partial<OtherEntitySettings>} settingsObject
 * @returns {void}
 */
setOtherEntitySettings(relevantPlayerId, targetedEntityId, settingsObject)

/**
 * Get the value of a player's other-entity setting for a specific entity.
 *
 * @param {PlayerId} relevantPlayerId
 * @param {EntityId} targetedEntityId
 * @param {Setting} settingName
 * @returns {OtherEntitySettings[Setting]}
 */
getOtherEntitySetting(relevantPlayerId, targetedEntityId, settingName)
```

Here is the full list of available entity settings:


## canAttack

**Type:** `boolean`

**Default:** `false`

 

Whether the entity can attack other entities, ignored if the targeted entity is invincible

 



## canSee

**Type:** `boolean`

**Default:** `true`

 

Whether the entity can be seen by the relevant player

 



## colorInLobbyLeaderboard

**Type:** `string`

**Default:** `""`

 

The colour of the player in the lobby leaderboard.

 



## hasPriorityNametag

**Type:** `boolean`

**Default:** `false`

 

Whether the player has a priority name tag

 



## killfeedColour

**Type:** `string`

**Default:** `""`

 

The colour of kills in the killfeed. Defaults to blue for themselves and red for everyone else.

 



## lobbyLeaderboardValues

**Type:** `LobbyLeaderboardValues`

**Default:** `{}`

 

The values of the leaderboard.

 



## meshScaling

**Type:** `EntityMeshScalingMap`

**Default:** `{}`

 

Scaling of mesh nodes, see api.scalePlayerMeshNodes

 



## nameColour

**Type:** `"default" | "yellow" | "lime" | "green" | "aqua" | "cyan" | "blue" | "purple" | "pink" | "red" | "orange"`

**Default:** `"default"`

 

The colour of the entity's name.

 



## nameTagInfo

**Type:** `NameTagInfo`

**Default:** `null`

 

The name tag info of the player:

 {

     backgroundColor?: string

     content?: StyledText[]

     subtitle?: StyledText[]

     subtitleBackgroundColor?: string

 }

 



## opacity

**Type:** `number`

**Default:** `1`

 

Opacity of the entity

 Fractional values will use dithering

 0 opacity will hide the entity but not its name tag

 



## overlayColour

**Type:** `string`

**Default:** `null`

 

Applies a colour tint to the entity when set, like the red tint when an entity gets hurt.

 



## showDamageAmounts

**Type:** `boolean`

**Default:** `true`

 

Whether you can see damage amounts when shooting the entity

 



## zIndex

**Type:** `0 | 1`

**Default:** `0`

 

Rendering order of the entity, higher zIndex renders on top of lower ones.

 



