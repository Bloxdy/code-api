# Entity Settings

An "Entity Setting" impacts how a player sees or interacts with another player or entity.
E.g. Player 1 could have an otherEntitySetting for entity 2 as opacity set to 0.5. This means player 1 sees entity 2 as partly see-through. Player1 is the relevant player, player2 is the targeted player.

## API Methods

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
```

```js
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
```

```js
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
```

```js
/**
 * Set many of a player's other-entity settings for a specific entity.
 *
 * @param {PlayerId} relevantPlayerId
 * @param {EntityId} targetedEntityId
 * @param {Partial<OtherEntitySettings>} settingsObject
 * @returns {void}
 */
setOtherEntitySettings(relevantPlayerId, targetedEntityId, settingsObject)
```

```js
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

## Entity Settings and their Values

```js
/**
 * Opacity of the entity
 * Fractional values will use dithering
 * 0 opacity will hide the entity but not its name tag
 * @type {number}
 */
opacity = 1
```

```js
/**
 * Rendering order of the entity, higher zIndex renders on top of lower ones.
 * @type {0 | 1}
 */
zIndex = 0
```

```js
/**
 * Applies a colour tint to the entity when set, like the red tint when an entity gets hurt.
 * @type {string}
 */
overlayColour = null
```

```js
/**
 * Whether the entity can attack other entities, ignored if the targeted entity is invincible
 * @type {boolean}
 */
canAttack = false
```

```js
/**
 * Whether the entity can be seen by the relevant player
 * @type {boolean}
 */
canSee = true
```

```js
/**
 * Whether you can see damage amounts when shooting the entity
 * @type {boolean}
 */
showDamageAmounts = true
```

```js
/**
 * The colour of kills in the killfeed. Defaults to blue for themselves and red for everyone else.
 * @type {string}
 */
killfeedColour = ""
```

```js
/**
 * Scaling of mesh nodes, see bottom of this page for EntityMeshScalingMap
 * @type {EntityMeshScalingMap}
 */
meshScaling = {}
```

```js
/**
 * The colour of the player in the lobby leaderboard.
 * @type {string}
 */
colorInLobbyLeaderboard = ""
```

```js
/**
 * The values of the leaderboard.
 * @type {LobbyLeaderboardValues}
 */
lobbyLeaderboardValues = {}
```

```js
/**
 * The name tag info of the player:
 * {
 *     backgroundColor?: string
 *     content?: StyledText[]
 *     subtitle?: StyledText[]
 *     subtitleBackgroundColor?: string
 * }
 * @type {NameTagInfo}
 */
nameTagInfo = null
```

```js
/**
 * Whether the player has a priority name tag
 * @type {boolean}
 */
hasPriorityNametag = false
```

```js
/**
 * The colour of the player's name.
 * @type {string}
 */
nameColour = "default"
```

## Extra Information

```js
type EntityMeshScalingMap = { [key in "TorsoNode" | "HeadMesh" | "ArmRightMesh" | "ArmLeftMesh" | "LegLeftMesh" | "LegRightMesh"]?: number[] }
```

```js
type EntityName = {
    entityName: string
    style?: {
        color?: string
        colour?: string
    }
}
```

```js
type MeshTypes = [null, BloxdBlock, ParticleEmitter]
```
```js
type BoxMeshTypes = [Person, Box, BlockBlock, ParticleEmitter]
```
