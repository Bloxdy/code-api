# API Code
> [!NOTE]
> This document has been reorganized compared to the original documentation.

## Credits
Thank you to Tom for creating this.

Thank you to Sheriff-Unit-3 for making this documentation, I will take good care of it
## Global Variables
`myId` & `playerId`: Store the player ID of who is running the code.  
`thisPos`: Stores the position of the currently executing code block or press to code board. 
`ownerDbId`: Stores the lobby owner's DbId.

## Built-in Functions
```js
/**
 * Runs a string as if it were a script.
 * Can be used to read code blocks from World Code or from another Code Block.
 * @param {string} ""
 * @returns The completed code.
 */
eval(string)
```
Example of `eval` being used to read Code Block's code
```js
// Code Block or in callback in World Code
let code = api.getBlockData(0, 0, 0) // (0, 0, 0) must be the location of the Code Block.
eval(code) // Note that this will not work as expected if that block's data is not JavaScript code.
```

## API functions  
Global object `api` has the following methods:

### Entity functions
> [!NOTE]
> Entities can be Players, Mobs, dropped items, and thrown items.  
```js
/**
 * Get position of a player / entity.
 * @param {EntityId} entityId
 * @returns {[number, number, number]}
 */
getPosition(entityId)
```

```js
/**
 * Set position of a player / entity.
 * @param {EntityId} entityId
 * @param {number | number[]} x - Can also be an array, in which case y and z shouldn't be passed
 * @param {number} [y]
 * @param {number} [z]
 * @returns {void}
 */
setPosition(entityId, x, y, z)
```
```js
/**
 * Get the velocity of an entity
 * Will return [0, 0, 0] if the entity doesn't have a physics body
 *
 * @param {EntityId} eId
 * @returns {[number, number, number]}
 */
getVelocity(eId)
```
```js
/**
 * Get the up to 12 unit co-ordinates the lifeform is located within
 * (A lifeform is modelled as having four corners and can be in up to 3 blocks vertically)
 *
 * @param {LifeformId} lifeformId
 * @returns {number[][]} - List of x, y, z positions e.g. [[-1, 0, 0], [-1, 1, 0], [-1, 2, 0]]
 */
getUnitCoordinatesLifeformWithin(lifeformId)
```

```js
/**
 * Get the current shield of an entity.
 * @param {EntityId} entityId
 * @returns {number}
 */
getShieldAmount(entityId)
```

```js
/**
 * Set the current shield of a lifeform.
 *
 * @param {LifeformId} lifeformId
 * @param {number} newShieldAmount
 * @returns {void}
 */
setShieldAmount(lifeformId, newShieldAmount)
```

```js
/**
 * Get the current health of an entity.
 * @param {PlayerId} entityId
 * @returns {number}
 */
getHealth(entityId)
```

```js
/**
 * @param {LifeformId} lifeformId
 * @param {number} changeAmount - Must be an integer. A positive amount will increase the entity's health. A negative amount will decrease the entity's shield first, then their health.
 * @param { LifeformId | { lifeformId: LifeformId; withItem: string } } [whoDidDamage] - Optional - If damage done by another player
 * @param {boolean} [broadcastLifeformHurt]
 * @returns {boolean} - Whether the entity was killed
 */
applyHealthChange(lifeformId, changeAmount, whoDidDamage, broadcastLifeformHurt)
```

```js
/**
 * Set the current health of an entity.
 * If you want to set their health to more than their current max health, the optional increaseMaxHealthIfNeeded must be true.
 *
 * @param {EntityId} entityId
 * @param {number} newHealth - Can be null to make the player not have health
 * @param { LifeformId | { lifeformId: LifeformId; withItem: string } } [whoDidDamage] - Optional
 * @param {boolean} [increaseMaxHealthIfNeeded] - Optional
 * @returns {boolean} - Whether this change in health killed the player
 */
setHealth(entityId, newHealth, whoDidDamage, increaseMaxHealthIfNeeded)
```

```js
/**
 * Make it as if hittingEId hit hitEId
 *
 * @param {LifeformId} hittingEId
 * @param {LifeformId} hitEId
 * @param {number[]} dirFacing
 * @param {<PlayerBodyPart>} [bodyPartHit]
 * @param { {
 *    damage?: number
 *    heldItemName?:<string>
 *    horizontalKbMultiplier?: number
 *    verticalKbMultiplier?: number
 * } } [overrides]
 * @returns {boolean}
 */
applyMeleeHit(hittingEId, hitEId, dirFacing, bodyPartHit, overrides)
```

```js
/**
 * Apply damage to a lifeform.
 * eId is the player initiating the damage, hitEId is the lifeform being hit.
 *
 * It is recommended to self-inflict damage when the game code wants to apply damage to a lifeform.
 *
 * @param {PlayerAttemptDamageOtherPlayerOpts} {
 *     eId,
 *     hitEId,
 *     attemptedDmgAmt,
 *     withItem,
 *     bodyPartHit = undefined,
 *     attackDir = undefined,
 *     showCritParticles = false,
 *     reduceVerticalKbVelocity = true,
 *     horizontalKbMultiplier = 1,
 *     verticalKbMultiplier = 1,
 *     broadcastEntityHurt = true,
 *     attackCooldownSettings = null,
 *     hittingSoundOverride = null,
 *     ignoreOtherEntitySettingCanAttack = false,
 *     isTrueDamage = false,
 *     damagerDbId = null,
 * }
 * @returns {boolean} - whether the attack damaged the lifeform
 */
attemptApplyDamage({
    eId,
    hitEId,
    attemptedDmgAmt,
    withItem,
    bodyPartHit = undefined,
    attackDir = undefined,
    showCritParticles = false,
    reduceVerticalKbVelocity = true,
    horizontalKbMultiplier = 1,
    verticalKbMultiplier = 1,
    broadcastEntityHurt = true,
    attackCooldownSettings = null,
    hittingSoundOverride = null,
    ignoreOtherEntitySettingCanAttack = false,
    isTrueDamage = false,
    damagerDbId = null,
    })
```

```js
/**
 * Kill a lifeform.
 * @param {LifeformId} lifeformId
 * @param { LifeformId | { lifeformId: LifeformId; withItem: string } } [whoKilled] - Optional
 * @returns {void}
 */
killLifeform(lifeformId, whoKilled)
```

```js
/**
 * Whether a lifeform is alive or dead (or on the respawn screen, in a player's case).
 *
 * @param {LifeformId} lifeformId
 * @returns {boolean}
 */
isAlive(lifeformId)
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

```js
/**
 * Get the in game name of an entity.
 * @param {EntityId} entityId
 * @returns {string}
 */
getEntityName(entityId)
```

```js
/**
 * Check your game (and, optionally, a entity) is still valid and executing.
 * Useful if you're using async functions and await within your game.
 * If you use await/async or promises and do not check this, your game could have closed and then the rest of your
 * async code executes.
 *
 * @param {<EntityId>} [entityId]
 * @returns {boolean}
 */
checkValid(entityId)
```

```js
/**
 * Get the entities in the rect between [minX, minY, minZ] and [maxX, maxY, maxZ]
 *
 * @param {number[]} minCoords
 * @param {number[]} maxCoords
 * @returns {EntityId[]}
 */
getEntitiesInRect(minCoords, maxCoords)
```

```js
/**
 * @param {EntityId} entityId
 * @returns {EntityType}
 */
getEntityType(entityId)
```

```js
/**
 * Apply an impulse to an entity
 *
 * @param {EntityId} eId
 * @param {number} xImpulse
 * @param {number} yImpulse
 * @param {number} zImpulse
 * @returns {void}
 */
applyImpulse(eId, xImpulse, yImpulse, zImpulse)
```

```js
/**
 * Set the velocity of an entity
 *
 * @param {EntityId} eId
 * @param {number} x
 * @param {number} y
 * @param {number} z
 * @returns {void}
 */
setVelocity(eId, x, y, z)
```

```js
/**
 * Set the heading for a server-auth entity.
 *
 * @param {EntityId} entityId
 * @param {number} newHeading
 * @returns {void}
 */
setEntityHeading(entityId, newHeading)
```

```js
/**
 * Apply an effect to a lifeform.
 * Can be an inbuilt effect E.g. "Speed" (speed boost), "Damage" (damage boost).
 * For inbuilt just pass the name of the effect and the functionality is handled in-engine.
 * For custom effect, you pass customEffectInfo. The icon can be an icon from "IngameIcons.ts" or a bloxd item name.
 * The custom effect onEndCb is an optional helper within which you can undo the effect you applied.
 * Note that onEndCb will not work for press to code boards, code blocks or world code.
 *
 * @param {LifeformId} lifeformId
 * @param {string} effectName
 * @param {number | null} duration
 * @param { { icon?: IngameIconName | ItemName; onEndCb?: () => void; displayName?: string | TranslatedText } & Partial<InbuiltEffectInfo> } customEffectInfo
 * @returns {void}
 */
applyEffect(lifeformId, effectName, duration, customEffectInfo)
```

```js
/**
 * Get all the effects currently applied to a lifeform.
 *
 * @param {LifeformId} lifeformId
 * @returns {string[]}
 */
getEffects(lifeformId)
```

```js
/**
 * Remove an effect from a lifeform.
 *
 * @param {LifeformId} lifeformId
 * @param {string} name
 * @returns {void}
 */
removeEffect(lifeformId, name)
```

```js
/**
 * Attach/detach mesh instances to/from an entity
 * @param {EntityId} eId
 * @param {EntityNamedNode} node - node to attach to
 * @param {<MeshType>} type - if null, detaches mesh from this node
 * @param {MeshEntityOpts[MeshType]} [opts]
 * @param {[number, number, number]} [offset]
 * @param {[number, number, number]} [rotation]
 * @returns {void}
 */
updateEntityNodeMeshAttachment(eId, node, type, opts, offset, rotation)
```

```js
/**
 * Add following entity to player
 * @param {PlayerId} playerId
 * @param {EntityId} eId
 * @param {number[]} [offset]
 * @returns {void}
 */
addFollowingEntityToPlayer(playerId, eId, offset)
```

```js
/**
 * Remove following entity from player
 * @param {PlayerId} playerId
 * @param {EntityId} entityEId
 * @returns {void}
 */
removeFollowingEntityFromPlayer(playerId, entityEId)
```

```js
/**
 * @param {EntityId} eId
 * @param {ExplosionType} explosionType
 * @param {number} knockbackFactor
 * @param {number} explosionRadius
 * @param {number[]} explosionPos
 * @param {boolean} ignoreProjectiles
 * @returns { { force: Pos; forceFrac: number; } }
 */
calcExplosionForce(eId, explosionType, knockbackFactor, explosionRadius, explosionPos, ignoreProjectiles)
```

### Player functions
> [!NOTE]
> This contains functions that use playerId, or affect only players. For example `broadcastMessage` is included in here cause only players read chat.
 
> [!NOTE]
> These fuctions work only on players, but entity functions will work on them too.  
```js
/**
 * Get all the player ids.
 * This calls getEntitiesInRect for all loaded chunks
 * @returns {PlayerId[]}
 */
getPlayerIds()
```

```js
/**
 * Whether a player is currently in the game
 *
 * @param {PlayerId} playerId
 * @returns {boolean}
 */
playerIsInGame(playerId)
```

```js
/**
 * @param {PlayerId} playerId
 * @returns {boolean}
 */
playerIsLoggedIn(playerId)
```

```js
/**
 * Returns the party that the player was in when they joined the game. The returned object contains the playerDbIds, as well
 * as the playerIds if available, of the party leader and members.
 *
 * @param {PlayerId} playerId
 * @returns {<{ playerDbIds: PlayerDbId[] }>}
 */
getPlayerPartyWhenJoined(playerId)
```

```js
/**
 * Get the number of players in the room
 * @returns {number}
 */
getNumPlayers()
```

```js
/**
 * Update the max players and soft max players matchmaking will use
 *
 * softMaxPlayers is the number of players that matchmaking will route to using "Quick Play".
 * Once the softMaxPlayers limit is reached, this lobby can only be joined by requesting the lobby name or joining a friend.
 *
 * maxPlayers is the absolute maximum: a lobby will not have more players than this.
 * Tip: softMaxPlayers should be around 90% of maxPlayers
 *
 * WARNING: This change is not immediate, as it takes a while for matchmaking to find out.
 * Also, this will not kick players out of the lobby if set to a lower value than the current player count.
 *
 * @param {number} softMaxPlayers
 * @param {number} maxPlayers
 * @returns {void}
 */
setMaxPlayers(softMaxPlayers, maxPlayers)
```

```js
/**
 * Get the co-ordinates of the blocks the player is standing on as a list. For example, if the center of the player is at 0,0,0
 * this function will return [[0, -1, 0], [-1, -1, 0], [0, -1, -1], [-1, -1, -1]]
 * If the player is just standing on one block, the function would return e.g. [[0, 0, 0]]
 * If the player is middair then returns an empty list [].
 *
 * @param {PlayerId} playerId
 * @returns {number[][]}
 */
getBlockCoordinatesPlayerStandingOn(playerId)
```

```js
/**
 * Get the types of block the player is standing on
 * For example, if a player is standing on 4 dirt blocks, this will return ["Dirt", "Dirt", "Dirt", "Dirt"]
 * @param {PlayerId} playerId
 * @returns {any[]}
 */
getBlockTypesPlayerStandingOn(playerId)
```

```js
/**
 * Show the shop tutorial for a player. Will not be shown if they have ever seen the shop tutorial in your game before.
 * @param {PlayerId} playerId
 * @returns {void}
 */
showShopTutorial(playerId)
```

```js
/**
 * Force respawn a player
 * @param {PlayerId} playerId
 * @param {number[]} [respawnPos]
 * @returns {void}
 */
forceRespawn(playerId, respawnPos)
```

```js
/**
 * Gets the player's current killstreak
 *
 * @param {PlayerId} playerId
 * @returns {number}
 */
getCurrentKillstreak(playerId)
```

```js
/**
 * Clears the player's current killstreak
 *
 * @param {PlayerId} playerId
 * @returns {void}
 */
clearKillstreak(playerId)
```

```js
/**
 * Send a message to everyone
 *
 * @param {string | CustomTextStyling} message - The text contained within the message. Can use `Custom Text Styling`.
 * @param { { fontWeight?: number | string; color?: string } } [style] - An optional style argument. Can contain values for fontWeight and color of the message.
 * @returns {void}
 */
broadcastMessage(message, style)
```

```js
/**
 * Send a message to a specific player
 *
 * @param {PlayerId} playerId - Id of the player
 * @param {string | CustomTextStyling} message - The text contained within the message. Can use `Custom Text Styling`.
 * @param { { fontWeight?: number | string; color?: string } } [style] - An optional style argument. Can contain values for fontWeight and color of the message.
 * @returns {void}
 */
sendMessage(playerId, message, style)
```

```js
/**
 * Send a flying middle message to a specific player
 *
 * @param {PlayerId} playerId - Id of the player
 * @param {CustomTextStyling} message - The text contained within the message. Can use `Custom Text Styling`.
 * @param {number} distanceFromAction - The distance from the action that has caused this message to be displayed, this value will be used to determine how the message flies across the screen.
 * @returns {void}
 */
sendFlyingMiddleMessage(playerId, message, distanceFromAction)
```

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
 * Set a player's other-entity setting for every player in the game.
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
 * Play particle effect on all clients, or only on some clients if clientPredictedBy is specified
 * @param {TempParticleSystemOpts} opts
 * @param {PlayerId} [clientPredictedBy] - Play only on clients where client with playerId clientPredictedBy
 * @returns {void}
 */
playParticleEffect(opts, clientPredictedBy)
```

```js
/**
 * Given the name of a player, get their id
 * @param {string} playerName
 * @returns {<PlayerId>}
 */
getPlayerId(playerName)
```

```js
/**
 * Given a player, get their permanent identifier that doesn't change when leaving and re-entering
 *
 * @param {PlayerId} playerId
 * @returns {PlayerDbId}
 */
getPlayerDbId(playerId)
```

```js
/**
 * Returns null if player not in lobby
 *
 * @param {PlayerDbId} dbId
 * @returns {<PlayerId>}
 */
getPlayerIdFromDbId(dbId)
```

```js
/**
 * @param {PlayerId} playerId
 * @param {string} reason
 * @returns {void}
 */
kickPlayer(playerId, reason)
```

```js
/**
 * @deprecated - prefer using other UI elements
 * (this UI element hasn't been properly thought through in combination with other elements like killfeed, uirequests, etc)
 *
 * Send a player an icon in the top right corner
 *
 * @param {PlayerId} playerId
 * @param {string} icon - Can be any icon from font-awesome.
 * @param {string} text - The text to send.
 * @param { {
 *     duration?: number
 *     width?: number
 *     height?: number
 *     color?: string
 *     iconSizeMult?: number
 *     textAndIconColor?: string
 *     fontSize?: string
 * } } opts - Can include keys duration, width, height, color, iconSizeMult.
 * @returns {void}
 */
sendTopRightHelper(playerId, icon, text, opts)
```

```js
/**
 * Whether the player is on a mobile device or a computer.
 * @param {PlayerId} playerId
 * @returns {boolean}
 */
isMobile(playerId)
```

```js
/**
 * Set the direction the player is looking.
 *
 * @param {PlayerId} playerId
 * @param {number[]} direction - a vector of the direction to look, format [x, y, z]
 * @returns {void}
 */
setCameraDirection(playerId, direction)
```

```js
/**
 * Set a player's opacity
 * A simple helper that calls setTargetedPlayerSettingForEveryone
 *
 * @param {PlayerId} playerId
 * @param {number} opacity
 * @returns {void}
 */
setPlayerOpacity(playerId, opacity)
```

```js
/**
 * Set the level of viewable opacity by one player on another player
 * A simple helper that calls setOtherEntitySetting
 *
 * @param {PlayerId} playerIdWhoViewsOpacityPlayer - The player who sees that with opacity
 * @param {PlayerId} playerIdOfOpacityPlayer - The player/player model who is given opacity
 * @param {number} opacity
 * @returns {void}
 */
setPlayerOpacityForOnePlayer(playerIdWhoViewsOpacityPlayer, playerIdOfOpacityPlayer, opacity)
```

```js
/**
 * Update the progress bar in the bottom right corner.
 * Can be queued.
 *
 * @param {PlayerId} playerId
 * @param {number} toFraction - The fraction of the progress bar you want to be filled up.
 * @param {number} [toDuration] - The time it takes for the bar to reach the given toFraction in ms.
 * If this is too low and you queue multiple updates, this toFraction could be skipped. Treat 200ms as a minimum.
 * @returns {void}
 */
progressBarUpdate(playerId, toFraction, toDuration)
```

```js
/**
 * This will initiate the MiddleScreenBar, starting at empty and filling up to full over the given duration.
 * Good to represent cooldowns (eg gun reload) or charged items (eg crossbow)
 *
 * @param {PlayerId} playerId
 * @param {number} duration - ms over which the MiddleScreenBar fills up
 * @param {boolean} [chargeExpiresAutomatically] - Defaults to true. If true, the bar will disappear upon reaching full. If false, the bar will remain at full until hidden with removeMiddleScreenBar
 * @param {number} [horizontalBarRemOffset] - Offset the bar left or right (in css unit - rem)
 * @returns {void}
 */
initiateMiddleScreenBar(playerId, duration, chargeExpiresAutomatically, horizontalBarRemOffset)
```

```js
/**
 * If there is any current middle screen bar running, this will hide it
 *
 * @param {PlayerId} playerId
 * @returns {void}
 */
removeMiddleScreenBar(playerId)
```

```js
/**
 * Show a message over the shop in the same place that a shop item's onBoughtMessage is shown.
 * Displays for a couple seconds before disappearing
 * Use case is to show a dynamic message when player buys an item
 *
 * @param {PlayerId} playerId
 * @param {string | CustomTextStyling} info
 * @returns {void}
 */
sendOverShopInfo(playerId, info)
```

```js
/**
 * Open the shop UI for a player
 *
 * @param {PlayerId} playerId
 * @param {boolean} [toggle] - Whether to close the shop if it's already open
 * @param {string} [forceCategory] - If set, will change the shop to this category
 * @returns {void}
 */
openShop(playerId, toggle, forceCategory)
```

```js
/**
 * Creates a new shop item
 * 
 * @param {string} category
 * @param {string} item
 * @param {shopItem} opts
 * @return {void}
 */
createShopItem(category, item, opts)
```

```js
/**
 * updates a shop item
 * 
 * @param {string} category
 * @param {string} item
 * @param {shopItem} opts
 * @return {void}
 */
updateShopItem(category, item, opts)
```

```js
/**
 * updates a shop item
 * 
 * @param {string} category
 * @param {string} item
 * @return {void}
 */
deleteShopItem(category, item)
```

```js
/**
 * configures a shop catagory
 * 
 * @param {string} category
 * @param {Partial<CategoryOptions>} opts
 * @return {void}
 */
configureShopCategory(category, opts)
```

```js
/**
 * Creates a new shop item for player
 * 
 * @param {PlayerId} playerId
 * @param {string} category
 * @param {string} item
 * @param {shopItem} opts
 * @return {void}
 */
createShopItemForPlayer(playerId, category, item, opts)
```

```js
/**
 * updates a shop item for player
 * 
 * @param {PlayerId} playerId
 * @param {string} category
 * @param {string} item
 * @param {shopItem} opts
 * @return {void}
 */
updateShopItemForPlayer(playerId, category, item, opts)
```

```js
/**
 * resets shop item for player
 * 
 * @param {PlayerId} playerId
 * @param {string} category
 * @param {string} item
 * @return {void}
 */
resetShopItemForPlayer(playerId, category, item)
```

```js
/**
 * configures a shop catagory for player
 * 
 * @param {PlayerId} playerId
 * @param {string} category
 * @param {Partial<CategoryOptions>} opts
 * @return {void}
 */
configureShopCategoryForPlayer(playerId, shopCategory, opts)
```

```js
/**
 * Change a part of a player's skin
 * @param {PlayerId} playerId
 * @param {CustomisationPart} partType
 * @param {string} selected
 * @returns {void}
 */
changePlayerIntoSkin(playerId, partType, selected)
```

```js
/**
 * Remove gamemode-applied skin from a player
 * @param {PlayerId} playerId
 * @returns {void}
 */
removeAppliedSkin(playerId)
```

```js
/**
 * Scale node of a player's mesh by 3d vector.
 * State from prior calls to this api is lost so if you want to have multiple nodes scaled, pass in all the scales at once.
 *
 * @param {PlayerId} playerId
 * @param {EntityMeshScalingMap} nodeScales
 * @returns {void}
 */
scalePlayerMeshNodes(playerId, nodeScales)
```

```js
/**
 * Set the pose of the player
 * @param {PlayerId} playerId
 * @param {PlayerPose} pose
 * @returns {void}
 */
setPlayerPose(playerId, pose, poseOffset)
```

```js
/**
 * Set physics state of player (vehicle type and tier)
 * @param {PlayerId} playerId
 * @param {PlayerPhysicsStateData} physicsState
 * @returns {void}
 */
setPlayerPhysicsState(playerId, physicsState)
```

```js
/**
 * Get physics state for player
 * @param {PlayerId} playerId
 * @returns {PlayerPhysicsStateData}
 */
getPlayerPhysicsState(playerId)
```

```js
/**
 * Set camera zoom for a player
 * @param {PlayerId} playerId
 * @param {number} zoom
 * @returns {void}
 */
setCameraZoom(playerId, zoom)
```

```js
/**
 * @param {PlayerId} playerId - hears the sound
 * @param {string} soundName - Can also be a prefix. If so, a random sound with that prefix will be played
 * @param {number} volume - 0-1. If it's too quiet and volume is 1, normalise your sound in audacity
 * @param {number} rate - The speed of playback. Also affects pitch. 0.5-4. Lower playback = lower pitch
 * Good for varying the sound. E.g. item pickup sound has a random rate between 1 and 1.5.
 * @param { {
 *     playerIdOrPos: PlayerId | number[]
 *     maxHearDist?: number
 *     refDistance?: number
 * } } [posSettings]
 * @returns {void}
 */
playSound(playerId, soundName, volume, rate, posSettings)
```

```js
/**
 * See documentation for api.playSound
 * @param {string} soundName
 * @param {number} volume
 * @param {number} rate
 * @param { {
 *     playerIdOrPos: PlayerId | number[]
 *     maxHearDist?: number
 *     refDistance?: number
 * } } [posSettings]
 * @param {PlayerId} [exceptPlayerId]
 * @returns {void}
 */
broadcastSound(soundName, volume, rate, posSettings, exceptPlayerId)
```

```js
/**
 * See documentation for api.playSound
 * @param {string} soundName
 * @param {number} volume
 * @param {number} rate
 * @param { {
 *     playerIdOrPos: PlayerId | number[]
 *     maxHearDist?: number
 *     refDistance?: number
 * } } [posSettings]
 * @param {PlayerId} [predictedBy]
 * @returns {void}
 */
playClientPredictedSound(soundName, volume, rate, posSettings, predictedBy)
```

```js
/**
 * Get the position of a player's target block and the block adjacent to it (e.g. where a block would be placed)
 *
 *
 * Note: This position is a tick ahead of the client's block target info (noa.targetedBlock),
 * since the client updates the blocktarget before the entities tick (and since it uses the renderposition of the camera)
 *
 * This normally doesn't matter but if you are client predicting something based on noa.targetedBlock
 * (currently only applicable to in-engine code), you should not verify using this
 *
 * @param {PlayerId} playerId
 * @returns { { position: Pos; normal: Pos; adjacent: Pos } }
 */
getPlayerTargetInfo(playerId)
```

```js
/**
 * Get the position of a player's camera and the direction (both in Euclidean and spherical coordinates) they are attempting to use an item.
 * The camPos has the same limitations described in getPlayerTargetInfo
 *
 * @param {PlayerId} playerId
 * @returns { { camPos: Pos; dir: Pos; angleDir: AngleDir; moveHeading: number } }
 */
getPlayerFacingInfo(playerId)
```

```js
/**
 * Check whether a player is crouching
 *
 * @param {PlayerId} playerId
 * @returns {boolean}
 */
isPlayerCrouching(playerId)
```

```js
/**
 * Get the aura info for a player
 * @param {PlayerId} playerId
 * @returns { { level: number; totalAura: number; auraPerLevel: number } }
 */
getAuraInfo(playerId)
```

```js
/**
 * Sets the total aura for a player. Will not go over max level or under 0
 * @param {PlayerId} playerId
 * @param {number} totalAura
 * @returns {void}
 */
setTotalAura(playerId, totalAura)
```

```js
/**
 * Set the aura level for a player - shortcut for setTotalAura(level * auraPerLevel)
 * @param {PlayerId} playerId
 * @param {number} level
 * @returns {void}
 */
setAuraLevel(playerId, level)
```

```js
/**
 * Add (or remove if negative) aura to a player. Will not go over max level or under 0
 * @param {PlayerId} playerId
 * @param {number} auraDiff
 * @returns {number} - The actual change in aura
 */
applyAuraChange(playerId, auraDiff)
```

### Callback Functions
```js
/**
 * Set a default value to be returned by your callback code if it throws an error.
 *
 * @param {string} callbackName
 * @param {any} defaultValue
 */
setCallbackValueFallback(callbackName, defaultValue)
```

### Mob functions
> [!NOTE]
> These functions only work on mobs, but entity functions will also work on them too.  
```js
/**
 * Create a mob herd. A mob herd represents a collection of mobs that move together.
 * @returns {MobHerdId}
 */
createMobHerd()
```
```js
/**
 * Try to spawn a mob into the world at a given position. Returns null on failure.
 * WARNING: Either the "onPlayerAttemptSpawnMob" or the "onWorldAttemptSpawnMob" game callback will be called
 * depending on whether "spawnerId" is provided. Calling this function inside those callbacks risks infinite recursion.
 * @param {TMobType} mobType
 * @param {number} x
 * @param {number} y
 * @param {number} z
 * @param {Partial<{
 *     mobHerdId: MobHerdId
 *     spawnerId: PlayerId
 *     mobDbId: MobDbId
 *     name: string
 *     playSoundOnSpawn: boolean
 *     variation: MobVariation<TMobType>
 *     physicsOpts: Partial<{
 *         width: number
 *         height: number
 *     }>
 * }>} [opts]
 * @returns {<MobId>}
 */
attemptSpawnMob(mobType, x, y, z, opts)
```
```js
/**
 * Dispose of a mob's state and remove them from the world without triggering "on death" flows.
 * Always succeeds.
 * @param {MobId} mobId
 * @returns {void}
 */
despawnMob(mobId)
```
```js
/**
 * Returns the current default value for a mob setting.
 *
 * @param {TMobType} mobType
 * @param {TMobSetting} setting
 * @returns {MobSettings<TMobType>[TMobSetting]}
 */
getDefaultMobSetting(mobType, setting)
```
```js
/**
 * Set the default value for a mob setting.
 * @param {TMobType} mobType
 * @param {TMobSetting} setting
 * @param {MobSettings<TMobType>[TMobSetting]} value
 * @returns {void}
 */
setDefaultMobSetting(mobType, setting, value)
```
```js
/**
 * Get the current value of a mob setting for a specific mob.
 * @param {MobId} mobId
 * @param {TMobSetting} setting
 * @returns {MobSettings<MobType>[TMobSetting]}
 */
getMobSetting(mobId, setting)
```
```js
/**
 * Set the current value of a mob setting for a specific mob.
 * @param {MobId} mobId
 * @param {TMobSetting} setting
 * @param {MobSettings<MobType>[TMobSetting]} value
 * @returns {void}
 */
setMobSetting(mobId, setting, value)
```
```js
/**
 * Get the number of mobs in the world.
 * @returns {number}
 */
getNumMobs()
```
```js
/**
 * Get the mob IDs of all mobs in the world.
 * @returns {MobId[]}
 */
getMobIds()
```
### Block functions
> [!NOTE]
> This contains functions used for setting, changing, and getting blocks.
```js
/**
 * Check if the block at a specific position is in a loaded chunk.
 * @param {number} x
 * @param {number} y
 * @param {number} z
 * @returns {boolean} - boolean
 */
isBlockInLoadedChunk(x, y, z)
```
```js
/**
 * Get the name of a block.
 * @param {number | number[]} x - could be an array [x, y, z]. If so, the other params shouldn't be passed.
 * @param {number} [y]
 * @param {number} [z]
 * @returns {BlockName} - blockName - will be a name contained in blockMetadata.ts or 'Air'
 */
getBlock(x, y, z)
```
```js
/**
 * Used to get the block id at a specific position.
 * Intended only for use in hot code paths - default to getBlock for most use cases
 *
 * @param {number} x
 * @param {number} y
 * @param {number} z
 * @returns {BlockId}
 */
getBlockId(x, y, z)
```
```js
/**
 * Set a block. Valid names are those either contained in blockMetadata.ts or are 'Air'
 *
 * This function is optimised for setting broad swathes of blocks. For example, if you have a 50x50x50 area you need to turn to air, it will run performantly if you call this in double nested loops.
 *
 * IF you're only changing a few blocks, you want this to be super snappy for players, AND you're calling this outside of your _tick function, you can use api.setOptimisations(false).
 *
 * If you want the optimisations for large quantities of blocks later on, then call api.setOptimisations(true) when you're done.
 *
 *
 *
 * @param {number | number[]} x - Can be an array
 * @param {number | BlockName} y - Should be blockname if first param is array
 * @param {number} [z]
 * @param {BlockName} [blockName]
 * @returns {void}
 */
setBlock(x, y, z, blockName)
```
```js
/**
 * Initiate a block change "by the world".
 * This ends up calling the onWorldChangeBlock and only makes the change if not prevented by game/plugins.
 * initiatorDbId is null if the change was initiated by the game code.
 *
 * @param {<PlayerDbId>} initiatorDbId
 * @param {number} x
 * @param {number} y
 * @param {number} z
 * @param {BlockName} blockName
 * @param {WorldBlockChangeInfo} [extraInfo]
 * @returns {"preventChange" | "preventDrop" | void} - "preventChange" if the change was prevented, "preventDrop" if the change was allowed but without dropping any items, and undefined if the change was allowed with an item drop
 */
attemptWorldChangeBlock(initiatorDbId, x, y, z, blockName, extraInfo)
```

```js
/**
 * Returns whether a block is solid or not.
 * E.g. Grass block is solid, while water, ladder and water are not.
 * Will be true if the block is unloaded.
 *
 * @param {number | number[]} x
 * @param {number} [y]
 * @param {number} [z]
 * @returns {boolean}
 */
getBlockSolidity(x, y, z)
```

```js
/**
 * Helper function that sets all blocks in a rectangle to a specific block.
 *
 * @param {number[]} pos1 - array [x, y, z]
 * @param {number[]} pos2 - array [x, y, z]
 * @param {BlockName} blockName
 * @returns {void}
 */
setBlockRect(pos1, pos2, blockName)
```

```js
/**
 * Create walls by providing two opposite corners of the cuboid
 *
 *
 * @param {number[]} pos1 - array [x, y, z]
 * @param {number[]} pos2 - array [x, y, z]
 * @param {BlockName} blockName
 * @param {boolean} [hasFloor]
 * @param {boolean} [hasCeiling]
 * @returns {void}
 */
setBlockWalls(pos1, pos2, blockName, hasFloor, hasCeiling)
```

```js
/**
 * Only use this instead of getBlock if you REALLY need the performance (i.e. you are iterating over tens of thousands of blocks)
 * ReturnedObject.blockData is a 32x32x32 ndarray of block ids
 * (see https://www.npmjs.com/package/ndarray)
 * Each block id is a 16-bit number
 * The ndarray should only be read from, writing to it will result in desync between the server and client
 *
 * @param {number[]} pos - The returned chunk contains pos
 * @returns {<GameChunk>} - null if the chunk is not loaded in a persisted world. ReturnedObject.blockData is an ndarray that can be accessed
 */
getChunk(pos)
```

```js
/**
 * Use this to get a chunk ndarray you can edit and set in resetChunk.
 *
 * Only use chunk helpers if you REALLY need the performance (i.e. you are iterating over tens of thousands of blocks)
 * ReturnedObject.blockData is a 32x32x32 ndarray of air.
 * (see https://www.npmjs.com/package/ndarray)
 * Each block id is a 16-bit number
 * @returns {GameChunk}
 */
getEmptyChunk()
```

```js
/**
 * Splits the block name by '|'. If no meta info, metaInfo is ''
 *
 * @param {BlockName | null | undefined} blockName
 * @returns {ItemMetaInfo}
 */
getMetaInfo(blockName)
```

```js
/**
 * Get the numeric id of a block used in the ndarrays returned from getChunk
 * I.e. chunk.blockData.set(x, y, z, api.blockNameToBlockId("Dirt"))
 * or chunk.blockData.get(x, y, z) === api.blockNameToBlockId("Dirt")
 *
 * @param {string} blockName
 * @param {boolean} [allowInvalidBlock] - Don't throw an error if the block name is invalid.
 * @returns {number}
 */
blockNameToBlockId(blockName, allowInvalidBlock)
```

```js
/**
 * Goes from block id to block name. The reverse of blockNameToBlockId
 *
 * @param {BlockId} blockId
 * @returns {BlockName}
 */
blockIdToBlockName(blockId)
```

```js
/**
 * Get the unique id of the chunk containing pos in the current map
 *
 * @param {number[]} pos
 * @returns {string}
 */
blockCoordToChunkId(pos)
```

```js
/**
 * Get the co-ordinates of the block in the chunk with the lowest x, y, and z co-ordinates
 *
 * @param {string} chunkId
 * @returns {[number, number, number]}
 */
chunkIdToBotLeftCoord(chunkId)
```

```js
/**
 * Let a player change a block at a specific co-ordinate. Useful when client option canChange is false.
 * Overrides blockRect and blockType settings, so also useful when you have disallowed changing of a block type with setCantChangeBlockType.
 * Using this on 1000s of blocks will cause lag - if that is needed, find a way to use setCanChangeBlockType.
 *
 * @param {PlayerId} playerId
 * @param {number} x
 * @param {number} y
 * @param {number} z
 * @returns {void}
 */
setCanChangeBlock(playerId, x, y, z)
```

```js
/**
 * Prevents a player from changing a block at a specific co-ordinate. Useful when client option canChange is true.
 * Overrides blockRect and blockType settings, so also useful when you have allowed changing of a block type with setCantChangeBlockType.
 * Using this on 1000s of blocks will cause lag - if that is needed, find a way to use setCantChangeBlockType.
 *
 * @param {PlayerId} playerId
 * @param {number} x
 * @param {number} y
 * @param {number} z
 * @returns {void}
 */
setCantChangeBlock(playerId, x, y, z)
```

```js
/**
 * Lets a player Change a block type. Valid names are those contained within blockMetadata.ts and 'Air'
 * Less priority than cant change block pos/can change block rect
 *
 * @param {PlayerId} playerId
 * @param {BlockName} blockName
 * @returns {void}
 */
setCanChangeBlockType(playerId, blockName)
```

```js
/**
 * Stops a player from Changeing a block type. Valid names are those contained within blockMetadata.ts and 'Air'
 * Less priority than can change block pos/can change block rect
 *
 * @param {PlayerId} playerId
 * @param {BlockName} blockName
 * @returns {void}
 */
setCantChangeBlockType(playerId, blockName)
```

```js
/**
 * Remove any previous can/cant change block type settings for a player
 *
 * @param {PlayerId} playerId
 * @param {BlockName} blockName
 * @returns {void}
 */
resetCanChangeBlockType(playerId, blockName)
```

```js
/**
 * Make it so a player can Change blocks within two points. Coordinates are inclusive. E.g. if [0, 0, 0] is pos1
 * and [1, 1, 1] is pos2 then the 8 blocks contained within low and high will be able to be broken.
 * Overrides setCantChangeBlockType
 *
 *
 * @param {PlayerId} playerId
 * @param {number[]} pos1 - Arg as [x, y, z]
 * @param {number[]} pos2 - Arg as [x, y, z]
 * @returns {void}
 */
setCanChangeBlockRect(playerId, pos1, pos2)
```

```js
/**
 * Make it so a player cant Change blocks within two points. Coordinates are inclusive. E.g. if [0, 0, 0] is pos1
 * and [1, 1, 1] is pos2 then the 8 blocks contained within pos1 and pos2 won't be able to be broken.
 * Overrides setCanChangeBlockType
 *
 *
 * @param {PlayerId} playerId
 * @param {number[]} pos1 - Arg as [x, y, z]
 * @param {number[]} pos2 - Arg as [x, y, z]
 * @returns {void}
 */
setCantChangeBlockRect(playerId, pos1, pos2)
```

```js
/**
 * Remove any previous can/cant change block rect settings for a player
 *
 * @param {PlayerId} playerId
 * @param {number[]} pos1
 * @param {number[]} pos2
 * @returns {void}
 */
resetCanChangeBlockRect(playerId, pos1, pos2)
```

```js
/**
 * Allow a player to walk through a type of block. For blocks that are normally solid and not seethrough, the player will experience slight visual glitches while inside the block.
 *
 *
 * @param {PlayerId} playerId
 * @param {BlockName} blockName
 * @param {boolean} [disable] - If you've enabled a player to walk through a block and want to make the block solid for them again, pass this with true. Otherwise you only need to pass playerId and blockName
 * @returns {void}
 */
setWalkThroughType(playerId, blockName, disable)
```

```js
/**
 * Allow a player to walk through (or not walk through) voxels that are located within a given rectangle.
 * For blocks that are normally solid and not seethrough, the player will experience slight visual glitches while inside the block.
 *
 * You could set both pos1 and pos2 to [0, 0, 0] to make only 0, 0, 0 walkthrough, for example.
 *
 * @param {PlayerId} playerId
 * @param {number[]} pos1 - The one corner of the cuboid. Format [x, y, z]
 * @param {number[]} pos2 - The top right corner of the cuboid. Format [x, y, z]
 * @param {WalkThroughType} updateType - The type of update. Whether to make a rect solid, or able to be walked through.
 * @returns {void}
 */
setWalkThroughRect(playerId, pos1, pos2, updateType)
```

```js
/**
 * Raycast for a block in the world.
 * Given a position and a direction, find the first block that the "ray" hits.
 *
 * @param {number[]} fromPos
 * @param {number[]} dirVec
 * @returns {BlockRaycastResult}
 */
raycastForBlock(fromPos, dirVec)
```

```js
/**
 * Store data about a block in a performant manner. Data is cleared when block changes.
 * E.g. chest
 * Works well with blocks marked tickable (e.g. wheat)
 *
 * @param {number} x
 * @param {number} y
 * @param {number} z
 * @param {unknown} data
 * @returns {void}
 */
setBlockData(x, y, z, data)
```

```js
/**
 * Get stored data about a block in a performant manner. Data is cleared when block changes.
 * E.g. chest
 * Works well with blocks marked tickable (e.g. wheat)
 *
 * @param {number} x
 * @param {number} y
 * @param {number} z
 * @returns {any}
 */
getBlockData(x, y, z)
```

### Item/Inventory Functions
> [!NOTE]
> Some of these functions require/affect playerId/mobId/entityId, but to keep them all together they have been placed here.
```js
/**
 * Create a dropped item.
 * @param {number} x
 * @param {number} y
 * @param {number} z
 * @param {string} itemName - Name of the item. Valid names can be found in blockMetadata.ts and itemMetadata.ts
 * @param {number} [amount] - The amount of the item to include in the drop - so when the player picks up the item drop, they get this many of the item.
 * @param {boolean} [mergeItems] - Whether to merge the item into an nearby item of same type, if one exists. Defaults to false.
 * @param {ItemAttributes} [attributes] - Attributes of the item being dropped
 * @pram {number} [timeTillDespawn] - Time till the item automatically despawns in millisecounds. Max of 5 mins.
 * @returns {<EntityId>} - the id you can pass to setCantPickUpItem, or null if the item drop limit was reached
 */
createItemDrop(x, y, z, itemName, amount, mergeItems, attributes, timeTillDespawn)
```
```js
/**
 * Prevent a player from picking up an item. itemId returned by createItemDrop
 *
 * @param {PlayerId} playerId
 * @param {EntityId} itemId
 * @returns {void}
 */
setCantPickUpItem(playerId, itemId)
```
```js
/**
 * Delete an item drop by item drop entity ID
 *
 * @param {EntityId} itemId
 * @returns {void}
 */
deleteItemDrop(itemId)
```
```js
/**
 * Get the metadata about a block or item before stats have been modified by any client options
 * (i.e. its entry in either blockMetadata.ts or nonBlockMetadata in itemMetadata.ts)
 *
 * @param {string} itemName
 * @returns {Partial<BlockMetadataItem & NonBlockMetadataItem>}
 */
getInitialItemMetadata(itemName)
```
```js
/**
 * Get stat info about a block or item
 * Either based on a client option for a player: (e.g. `DirtTtb`)
 * or its entry in blockMetadata.ts or nonBlockMetadata in itemMetadata.ts if no client option is set.
 *
 * If null is passed for lifeformId, this is simply its entry in blockMetadata etc.
 *
 *
 * @param {<LifeformId>} LifeformId
 * @param {string} itemName
 * @param {K} stat
 * @returns {AnyMetadataItem[K]}
 */
getItemStat(LifeformId, itemName, stat)
```
```js
/**
 * Give a player an item and a certain amount of that item.
 * Returns the amount of item added to the users inventory.
 *
 * @param {PlayerId} playerId
 * @param {string} itemName
 * @param {number} [itemAmount]
 * @param {ItemAttributes} [attributes] - An optional object for certain types of item. For guns this can contain the shotsLeft field which is the amount of ammo the gun currently has.
 * @returns {number}
 */
giveItem(playerId, itemName, itemAmount, attributes)
```
```js
/**
 * Whether the player has space in their inventory to get new blocks
 * @param {PlayerId} playerId
 * @returns {boolean}
 */
inventoryIsFull(playerId)
```
```js
/**
 * Put an item in a specific index. Default hotbar is indexes 0-9
 *
 * @param {PlayerId} playerId
 * @param {number} itemSlotIndex - 0-indexed
 * @param {string} itemName - Can be 'Air', in which case itemAmount will be ignored and the slot will be cleared.
 * @param {number} [itemAmount] - -1 for infinity. Should not be set, or null, for items that are not stackable.
 * @param {ItemAttributes} [attributes] - An optional object for certain types of item. For guns this can contain the shotsLeft field which is the amount of ammo the gun currently has.
 * @param {boolean} [tellClient] - whether to tell client about it - results in desync between client and server if client doesnt locally perform the same action
 * @returns {void}
 */
setItemSlot(playerId, itemSlotIndex, itemName, itemAmount, attributes, tellClient)
```
```js
/**
 * Remove an amount of item from a player's inventory
 *
 * @param {PlayerId} playerId
 * @param {string} itemName
 * @param {number} amount
 * @returns {void}
 */
removeItemName(playerId, itemName, amount)
```
```js
/**
 * Get the item at a specific index
 * Returns null if there is no item at that index
 * If there is an item, return an object of the format { name: string; amount: number; attributes: ItemAttributes; }
 *
 * @param {PlayerId} playerId
 * @param {number} itemSlotIndex
 * @returns {<InvenItem>}
 */
getItemSlot(playerId, itemSlotIndex)
```
```js
/**
 * Whether a player has an item
 *
 * @param {PlayerId} playerId
 * @param {string} itemName
 * @returns {boolean} - bool
 */
hasItem(playerId, itemName)
```
```js
/**
 * The amount of an itemName a player has.
 * Returns 0 if the player has none, and a negative number if infinite.
 *
 * @param {PlayerId} playerId
 * @param {string} itemName
 * @returns {number} - number
 */
getInventoryItemAmount(playerId, itemName)
```
```js
/**
 * Clear the players inventory
 *
 * @param {PlayerId} playerId
 * @returns {void}
 */
clearInventory(playerId)
```
```js
/**
 * Force the player to have the ith inventory slot selected. E.g. newI 0 makes the player have the 0th inventory slot selected
 *
 * @param {PlayerId} playerId
 * @param {number} newI - integer from 0-9
 * @returns {void}
 */
setSelectedInventorySlotI(playerId, newI)
```
```js
/**
 * Get a player's currently selected inventory slot
 * @param {PlayerId} playerId
 * @returns {number}
 */
getSelectedInventorySlotI(playerId)
```
```js
/**
 * Get the currently held item of a player
 * Returns null if no item is being held
 * If an item is held, return an object of the format {name: itemName, amount: amountOfItem}
 *
 * @param {PlayerId} playerId
 * @returns {<InvenItem>}
 */
getHeldItem(playerId)
```
```js
/**
 * Get the amount of free slots in a player's inventory.
 *
 * @param {PlayerId} playerId
 * @returns {number} - number
 */
getInventoryFreeSlotCount(playerId)
```
```js
/**
 * Checks if a player is able to open a chest at a given location,
 * as per the rules laid out by the "onPlayerAttemptOpenChest" game callback.
 * Returns true if the player can open the chest, false if they cannot, and void if the chest does not exist.
 *
 * @param {PlayerId} playerId
 * @param {number} chestX
 * @param {number} chestY
 * @param {number} chestZ
 * @returns {<boolean>}
 */
canOpenStandardChest(playerId, chestX, chestY, chestZ)
```
```js
/**
 * Give a standard chest an item and a certain amount of that item.
 * Returns the amount of item added to the chest.
 *
 * @param {number[]} chestPos
 * @param {string} itemName
 * @param {number} [itemAmount]
 * @param {PlayerId} [playerId] - The player who is interacting with the chest.
 * @param {ItemAttributes} [attributes] - An optional object for certain types of item. For guns this can contain the shotsLeft field which is the amount of ammo the gun currently has.
 * @returns {number}
 */
giveStandardChestItem(chestPos, itemName, itemAmount, playerId, attributes)
```
```js
/**
 * Get the amount of free slots in a standard chest
 * Returns null for non-chests
 *
 * @param {number[]} chestPos
 * @returns {number} - number
 */
getStandardChestFreeSlotCount(chestPos)
```
```js
/**
 * The amount of an itemName a standard chest has.
 * Returns 0 if the standard chest has none, and a negative number if infinite.
 *
 * @param {number[]} chestPos
 * @param {string} itemName
 * @returns {number} - number
 */
getStandardChestItemAmount(chestPos, itemName)
```
```js
/**
 * Get the item at a chest slot. Null if empty otherwise format {name: itemName, amount: amountOfItem}
 *
 * @param {number[]} chestPos
 * @param {number} idx
 * @returns {<InvenItem>}
 */
getStandardChestItemSlot(chestPos, idx)
```
```js
/**
 * Get all the items from a standard chest in order. Use this instead of repetitive calls to getStandardChestItemSlot
 *
 * @param {number[]} chestPos
 * @returns {<InvenItem>[]}
 */
getStandardChestItems(chestPos)
```
```js
/**
 * @param {number[]} chestPos
 * @param {number} idx - 0-indexed
 * @param {string} itemName - Can be 'Air', in which case itemAmount will be ignored and the slot will be cleared.
 * @param {number} [itemAmount] - -1 for infinity. Should not be set, or null, for items that are not stackable.
 * @param {PlayerId} [playerId] - The player who is interacting with the chest.
 * @param {ItemAttributes} [attributes] - An optional object for certain types of item. For guns this can contain the shotsLeft field which is the amount of ammo the gun currently has.
 * @returns {void}
 */
setStandardChestItemSlot(chestPos, idx, itemName, itemAmount, playerId, attributes)
```
```js
/**
 * Get the item in a player's moonstone chest slot. Null if empty
 *
 * Moonstone chests are a type of chest where a player accesses the same contents no matter the location of the moonstone chest
 *
 * @param {PlayerId} playerId
 * @param {number} idx
 * @returns {<InvenItem>}
 */
getMoonstoneChestItemSlot(playerId, idx)
```
```js
/**
 * Get all the items from a moonstone chest in order. Use this instead of repetitive calls to getMoonstoneChestItemSlot
 *
 * Moonstone chests are a type of chest where a player accesses the same contents no matter the location of the moonstone chest
 *
 * @param {PlayerId} playerId
 * @returns {<InvenItem>[]}
 */
getMoonstoneChestItems(playerId)
```
```js
/**
 * Moonstone chests are a type of chest where a player accesses the same contents no matter the location of the moonstone chest
 *
 * @param {PlayerId} playerId
 * @param {number} idx - 0-indexed
 * @param {string} itemName - Can be 'Air', in which case itemAmount will be ignored and the slot will be cleared.
 * @param {number} [itemAmount] - -1 for infinity. Should not be set, or null, for items that are not stackable.
 * @param {ItemAttributes} [metadata] - An optional object for certain types of item. For guns this can contain the shotsLeft field which is the amount of ammo the gun currently has.
 * @returns {void}
 */
setMoonstoneChestItemSlot(playerId, idx, itemName, itemAmount, metadata)
```
```js
/**
 * Edit the crafting recipes for a player
 *
 * @param {PlayerId} playerId
 * @param {string} itemName
 * @param {RecipesForItem} recipesForItem
 * @returns {void}
 */
editItemCraftingRecipes(playerId, itemName, [{
        requires: [{ items: []; amt: number }],
        produces: number,
        station?: string | string[],
        onCraftedAura?: number,
        isStarterRecipe?: boolean
    }]
)
```
```js
/**
 * Reset the crafting recipes for a given back to its original bloxd state
 *
 * @param {PlayerId} playerId
 * @param {string} itemName - Resets all crafting recipes for the given player if null, otherwise restes the crafting recipes for the given  item.
 * @returns {void}
 */
resetItemCraftingRecipes(playerId, itemName)
```

```js
/**
 * Removes crafting recipes
 *
 * @param {PlayerId} playerId
 * @param {string} itemName - Removes all the crafting recipes for the given player if null, otherwise removes the crafting reipes for the given item.
 * @returns {void}
 */
removeItemCraftingRecipes(playerId, itemName)
```

```js
/**
 * Set the amount of an item in an item entity
 *
 * @param {EntityId} itemId
 * @param {number} newAmount
 * @returns {void}
 */
setItemAmount(itemId, newAmount)
```

### Lobby related functions
```js
/**
 * Obtain Date.now() value saved at start of current game tick
 * @returns {number}
 */
now()
```
```js
/**
 * Get the name of the lobby this game is running in.
 * @returns {string}
 */
getLobbyName()
```
```js
/**
 * Integer lobby names are public
 * @returns {boolean} - boolean
 */
isPublicLobby()
```

```js
/**
 * Returns if the current lobby the game is running in is special - e.g. a discord guild or dm, or simply a standard lobby
 * @returns {LobbyType}
 */
getLobbyType()
```

```js
/**
 * Check if a position is within a cubic rectangle
 *
 * @param {number[]} coordsToCheck
 * @param {number[]} pos1 - position of one corner
 * @param {number[]} pos2 - position of opposite corner
 * @param {boolean} [addOneToMax]
 * @returns {boolean}
 */
isInsideRect(coordsToCheck, pos1, pos2, addOneToMax)
```

## Glossary of Referenced Types
> [!NOTE]
> These 'types' can't be referenced by your code, but they help explain some of the parameters in the API.

```js
type CustomTextStyling = (string | EntityName | TranslatedText | StyledIcon | StyledText)[]
```

```js
type EntityMeshScalingMap = { [key in "TorsoNode" | "HeadMesh" | "ArmRightMesh" | "ArmLeftMesh" | "LegLeftMesh" | "LegRightMesh"]?: [number] }
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
type IngameIconName = "Damage" | "Damage Reduction" | "Speed" | "VoidJump" | "Fist" | "Frozen" | "Hydrated" | "Invisible" | "Jump Boost" | "Poisoned" | "Slowness" | "Weakness" | "Health Regen" | "Haste" | "Double Jump" | "Heat Resistance" | "Gliding" | "Boating" | "Obsidian Boating" | "Riding" | "Bunny Hop" | "FallDamage" | "Feather Falling" | "Thief" | "X-Ray Vision" | "Mining Yield" | "Brain Rot" | "Rested Damage" | "Rested Haste" | "Rested Speed" | "Rested Farming Yield" | "Rested Aura" | "Blindness" | "Pickpocketer" | "Lifesteal" | "Bounciness" | "Air Walk" | "Wall Climbing" | "Thorns" | "Draugr Knight Head" | "Damage Enchantment" | "Critical Damage Enchantment" | "Attack Speed Enchantment" | "Protection Enchantment" | "Health Enchantment" | "Health Regen Enchantment" | "Stomp Damage Enchantment" | "Knockback Resist Enchantment" | "Arrow Speed Enchantment" | "Arrow Damage Enchantment" | "Quick Charge Enchantment" | "Break Speed Enchantment" | "Momentum Enchantment" | "Mining Yield Enchantment" | "Farming Yield Enchantment" | "Mining Aura Enchantment" | "Digging Aura Enchantment" | "Lumber Aura Enchantment" | "Farming Aura Enchantment" | "Vertical Knockback Enchantment" | "Horizontal Knockback Enchantment" | "Self Yield" | "Friends" | "Riding Speed" | "Feed Aura" | "Double Poop" | "Mob Slayer" | "Rainbow Wool" | "Pack Leader" | "Max Health" | "Poison Claws" | "Mob Yield" | "Antlers Bonus" | "Health" | "HealthShield" | "Cross" | "Friendship" | "Dotted Friendship" | "Hunger" | "Empty Hunger" | "Pixelated Heart" | "Question Mark"
```

```js
enum ParticleSystemBlendMode {
    // Source color is added to the destination color without alpha affecting the result
    OneOne = 0,
    // Blend current color and particle color using particle's alpha
    Standard = 1,
    // Add current color and particle color multiplied by particle's alpha
    Add,
    // Multiply current color with particle color
    Multiply,
    // Multiply current color with particle color then add current color and particle color multiplied by particle's alpha
    MultiplyAdd,
}
```

```js
type RecipesForItem = [{
        requires: [{ items: string[]; amt: number }]
        produces: number
        station?: string | string[]
        onCraftedAura?: number
        isStarterRecipe?: boolean
    }]
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
type TempParticleSystemOpts = {
    texture: string
    minLifeTime: number
    maxLifeTime: number
    minEmitPower: number
    maxEmitPower: number
    minSize: number
    maxSize: number
    gravity: number[]
    velocityGradients: {
        timeFraction: number
        factor: number
        factor2: number
    }[]
    colorGradients: {
        timeFraction: number
        minColor: [number, number, number, number]
        maxColor?: [number, number, number, number]
    }[] | {
        color: [number, number, number]
    }[]
    blendMode: ParticleSystemBlendMode
    dir1: number[]
    dir2: number[]
    pos1: number[]
    pos2: number[]
    manualEmitCount: number
    hideDist?: number
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
    infiniteDistance?: boolean
    luminance?: number
    yCameraOffset?: number
    azimuth?: number
    // Not part of sky model by default; heavily tint to a vertex color
    vertexTint?: [number, number, number]
}
```

```js
type ItemAttributes = { customDisplayName?: string; customDescription?: string; customAttributes?: Record<string, any> }
```

```js
enum WalkThroughType {
    CANT_WALK_THROUGH = 0,
    CAN_WALK_THROUGH = 1,
    DEFAULT_WALK_THROUGH = 2,
}
```

```js
type WorldBlockChangedInfo = {
    cause: <"Paintball" | "FloorCreator" | "Sapling" | "StemFruit" | "MeltingIce" | "Explosion">
}
```

```js
type SkyBoxOptions = [
        default,
        earth,
        interstellar,
        space_lightblue,
        space_blue,
        space_red,
        underwater
]
```

```js
type shopItem = {
  image: string
  cost: number (optional)
  currency: string (optional)
  amount: number (optional)
  imageColour: string (optional)
  canBuy: boolean (optional)
  isSelected: boolean (optional)
  buyButtonText: string | CustomTextStyling (optional)
  customTitle: string | CustomTextStyling (optional)
  description: string | CustomTextStyling (optional)
  onBoughtMessage: string | CustomTextStyling (optional)
  redDot: boolean (optional)
  forceRemoveRedDot: boolean (optional)
  badge: { text: string | CustomTextStyling; type: "new" | "lucky" } (optional)
  userInput: ShopItemUserInput (optional)
  sell: boolean (optional)
  sortPriority: number (optional)
  hidden: boolean (optional)
}
```

```js
type CategoryOptions = {
  autoSelectCategory: boolean
  customTitle: string
  redDot: boolean
  forceRemoveRedDot: boolean
  sortPriority: number
  description: string | CustomTextStyling
}
```

## Enchants
> [!NOTE]
> Thank you to the_ccccc for sharing this!

```js
enchants = ['Protection', 'Health', 'Health Regen', 'Damage', 'Critical Damage', 'Attack Speed', 'Momentum', 'Break Speed', 'Yield', 'Aura', 'Stomp', 'Protection', 'Knockback', 'Arrow Speed', 'Arrow Damage', 'Quick Charge', 'Knockback Resist', 'Horizontal Knockback', 'Vertical Knockback']
```

```js
item_enchants = {
    "armor": ['Protection', 'Health', 'Health Regen'],
    "sword": ['Damage', 'Critical Damage', 'Attack Speed'],
    "pickaxe": ['Momentum', 'Break speed', 'Yield', 'Aura'],
    "hoe": ['Momentum', 'Break Speed', 'Yield', 'Aura'],
    "axe": ['Momentum', 'Break Speed', 'Aura'],
    "spade": ['Momentum', 'Break Speed', 'Aura'],
    "fur boots": ['Stomp', 'Protection', 'Health', 'Health Regen'],
    "knight sword": ['Damage', 'Critical Damage', 'Attack Speed', 'Knockback'],
    "moonstone pickaxe": ['Momentum', 'Break Speed', 'Aura'],
    "bow": ['Arrow Speed', 'Arrow Damage', 'Quick Charge'],
    "fur chestplate": ['Knockback Resist', 'Protection', 'Health', 'Health Regen'],
    "stick": ['Horizontal Knockback', 'Vertical Knockback', 'Damage']
```
