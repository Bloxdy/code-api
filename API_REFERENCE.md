# API Reference

## addFollowingEntityToPlayer
Add following entity to player

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| eId | `EntityId` |  |
| offset | `number[]` |  |
| followsPlayerRotation | `boolean` |  |





 

## addQTE
Create and register the UI for the requested quicktime event (QTE) to the screen.

Handle the result via the onPlayerFinishQTE engine callback.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| qteParameters | `QTEClientParameters<T>` | includes type and parameters |



### Returns:
`QTERequestId`

 

an id that can be passed to deleteQTE

 

## animateEntity
Animates the given entity.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| entityId | `EntityId` |  |
| animationSchema | `AnimationSchema \| BlockbenchAnimationSchema` |  |
| initialTimeFraction | `number` |  |
| animationSpeed | `number` |  |





 

## applyAuraChange
Add (or remove if negative) aura to a player. Will not go over max level or under 0

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| auraDiff | `number` |  |



### Returns:
`number`

 

The actual change in aura

 

## applyEffect
Apply an effect to a lifeform.

Can be an inbuilt effect E.g. "Speed" (speed boost), "Damage" (damage boost).

For inbuilt just pass the name of the effect and the functionality is handled in-engine.

For custom effect, you pass customEffectInfo. The icon can be an InGameIconName or a bloxd item name.

The custom effect onEndCb is an optional helper within which you can undo the effect you applied.

Note that onEndCb will not work for press to code boards, code blocks or world code.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| lifeformId | `LifeformId` |  |
| effectName | `string` |  |
| duration | `number \| null` |  |
| customEffectInfo | ` { icon?: IngameIconName \| ItemName; onEndCb?: () => void; displayName?: string \| TranslatedText } & Partial<InbuiltEffectInfo> ` |  |





 

## applyHealthChange


### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| lifeformId | `LifeformId` |  |
| changeAmount | `number` | Must be an integer. A positive amount will increase the entity's health. A negative amount will decrease the entity's shield first, then their health. |
| whoDidDamage | ` LifeformId \| { lifeformId: LifeformId; withItem: string } ` | Optional - If damage done by another player |
| broadcastLifeformHurt | `boolean` |  |



### Returns:
`boolean`

 



 

## applyImpulse
Apply an impulse to an entity

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| eId | `EntityId` |  |
| xImpulse | `number` |  |
| yImpulse | `number` |  |
| zImpulse | `number` |  |





 

## applyMeleeHit
Make it as if hittingEId hit hitEId

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| hittingEId | `LifeformId` |  |
| hitEId | `LifeformId` |  |
| dirFacing | `number[]` |  |
| bodyPartHit | `PNull<LifeformBodyPart>` |  |
| overrides | ` { damage?: PNull<number> heldItemName?: PNull<string> horizontalKbMultiplier?: number verticalKbMultiplier?: number } ` |  |



### Returns:
`boolean`

 



 

## attemptApplyDamage
Apply damage to a lifeform.

eId is the player initiating the damage, hitEId is the lifeform being hit.



It is recommended to self-inflict damage when the game code wants to apply damage to a lifeform.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| { eId, hitEId, attemptedDmgAmt, withItem, bodyPartHit = undefined, attackDir = undefined, showCritParticles = false, reduceVerticalKbVelocity = true, horizontalKbMultiplier = 1, verticalKbMultiplier = 1, broadcastEntityHurt = true, attackCooldownSettings = null, hittingSoundOverride = null, ignoreOtherEntitySettingCanAttack = false, isTrueDamage = false, damagerDbId = null, } | `PlayerAttemptDamageOtherPlayerOpts` |  |



### Returns:
`boolean`

 

whether the attack damaged the lifeform

 

## attemptCreateMeshEntity
Try to create a mesh entity. This creates an entity whose mesh position is synced with clients.

Set entity position using setPosition

There is a limit to the number of mesh entities and throwables that can be created, with an even smaller limit for mesh entities with physics.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| type | `MeshType` |  |
| opts | `MeshEntityOpts[MeshType]` |  |
| name | `string` | The default name for the nametag |
| physicsOptions | `MeshEntityPhysicsOpts` | Physics Options |
| initiatorId | `EntityId` | The entity that initiated the creation of the mesh entity. |



### Returns:
`PNull<EntityId>`

 

null if the entity creation failed, otherwise the entity ID.

 

## attemptCreateThrowable
Try to create a throwable entity.

Similar to creating a mesh entity and uses the same rate limiting.

However, this uses the predefined throwables system and physics used by throwable items with the game

Each throwable item has its own behaviour already, including default velocity, damage and gravity multipliers.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| throwerEId | `EntityId` |  |
| itemName | `ThrowableItem` | Must be an Item that is usually throwable in-engine |
| position | `[number, number, number]` | Starting position |
| direction | `[number, number, number]` |  |
| velocityMult | `number` | Multiplier for the default velocity of the throwable item |
| damageMult | `number` | Multiplier for the default damage of the throwable item |
| gravityMult | `number` | Multiplier for the default gravity of the throwable item |
| attributes | `ItemAttributes` | item attributes (currently used only for the "Boomerag" item) |



### Returns:
`string`

 

null if throwable creation failed, otherwise the entity ID.

 

## attemptSpawnMob
Try to spawn a mob into the world at a given position. Returns null on failure.

WARNING: Either the "onPlayerAttemptSpawnMob" or the "onWorldAttemptSpawnMob" game callback will be called

depending on whether "spawnerId" is provided. Calling this function inside those callbacks risks infinite recursion.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| mobType | `TMobType` |  |
| x | `number` |  |
| y | `number` |  |
| z | `number` |  |
| opts | `MobSpawnOpts<TMobType>` | Includes: - mobHerdId The ID of this mob's herd. (A mob herd represents a collection of mobs that move together.) - spawnerId The ID of the player who tried to spawn this mob. - mobDbId A persistent ID for the mob. This can be useful when loading mob data from the database. If the DB ID is already taken, null will be returned. - name If set, gives the mob a name that will be displayed as a nametag above their head. - playSoundOnSpawn - variation - physicsOpts { width: number; height: number; collidesEntities: boolean } |



### Returns:
`PNull<MobId>`

 

null if the mob could not be spawned.
This can happen when there are too many mobs in the world for the current number
of players in the lobby, or if the area is protected e.g. by spawn area protection.

 

## attemptWorldChangeBlock
Initiate a block change "by the world".

This ends up calling the onWorldChangeBlock and only makes the change if not prevented by game/plugins.

initiatorDbId is null if the change was initiated by the game code.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| initiatorDbId | `PNull<PlayerDbId>` |  |
| x | `number` |  |
| y | `number` |  |
| z | `number` |  |
| blockName | `BlockName` |  |
| extraInfo | `WorldBlockChangedInfo` |  |



### Returns:
`"preventChange" | "preventDrop" | void`

 

"preventChange" if the change was prevented, "preventDrop" if the change was allowed but without dropping any items, and undefined if the change was allowed with an item drop

 

## blockCoordToChunkId
Get the unique id of the chunk containing pos in the current map

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| pos | `number[]` |  |



### Returns:
`string`

 



 

## blockIdToBlockName
Goes from block id to block name. The reverse of blockNameToBlockId

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| blockId | `BlockId` |  |



### Returns:
`BlockName`

 



 

## blockNameToBlockId
Get the numeric id of a block used in the ndarrays returned from getChunk

I.e. chunk.blockData.set(x, y, z, api.blockNameToBlockId("Dirt"))

or chunk.blockData.get(x, y, z) === api.blockNameToBlockId("Dirt")

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| blockName | `BlockName` |  |
| allowInvalidBlock | `boolean` | Don't throw an error if the block name is invalid. Defaults false. If true and name is invalid, returns null. |



### Returns:
`PNull<number>`

 



 

## broadcastMessage
Send a message to everyone

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| message | `string \| CustomTextStyling` | The text contained within the message. Can use `Custom Text Styling`. |
| style | ` { fontWeight?: number \| string; color?: string } ` | An optional style argument. Can contain values for fontWeight and color of the message. style is ignored if message uses custom text styling (i.e. is not a string). |





 

## broadcastSound
See documentation for api.playSound

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| soundName | `string` |  |
| volume | `number` |  |
| rate | `number` |  |
| posSettings | ` { playerIdOrPos: PlayerId \| number[] maxHearDist?: number refDistance?: number } ` |  |
| exceptPlayerId | `PlayerId` |  |





 

## calcExplosionForce


### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| eId | `EntityId` |  |
| explosionType | `ExplosionType` |  |
| knockbackFactor | `number` |  |
| explosionRadius | `number` |  |
| explosionPos | `number[]` |  |
| ignoreProjectiles | `boolean` |  |



### Returns:
` { force: [number, number, number]; forceFrac: number; } `

 



 

## canOpenStandardChest
Checks if a player is able to open a chest at a given location,

as per the rules laid out by the "onPlayerAttemptOpenChest" game callback.

Returns true if the player can open the chest, false if they cannot, and void if the chest does not exist.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| chestX | `number` |  |
| chestY | `number` |  |
| chestZ | `number` |  |



### Returns:
`PNull<boolean>`

 



 

## changePlayerIntoSkin
Change a part of a player's skin.

UGC code is restricted to cosmetics from packs with ugcSelectable; internal code can use any cosmetics.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | Player to change |
| cosmeticType | `CosmeticType` | Type of cosmetic |
| cosmeticName | `CosmeticName` | Chosen cosmetic, will be made lowercase automatically |





 

## checkValid
Check your game (and, optionally, a entity) is still valid and executing.

Useful if you're using async functions and await within your game.

If you use await/async or promises and do not check this, your game could have closed and then the rest of your

async code executes.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| entityId | `PNull<EntityId>` |  |



### Returns:
`boolean`

 



 

## chunkIdToBotLeftCoord
Get the co-ordinates of the block in the chunk with the lowest x, y, and z co-ordinates

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| chunkId | `string` |  |



### Returns:
`[number, number, number]`

 



 

## clearDirectionArrow
Clear a directional arrow from the player's screen.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The player to clear the arrow for |
| id | `PNull<string>` | The arrow identifier to clear. If null, clears all arrows for this player. |





 

## clearInventory
Clear the players inventory

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |





 

## clearKillstreak
Clears the player's current killstreak

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |





 

## closeChestForPlayer
Close a chest for a player.

If the player does not have a chest open, do nothing.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |





 

## configureShopCategory
Set properties of a shop category.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| categoryKey | `ShopCategoryKey` | The key of the category to configure |
| config | `ShopCategoryConfig` | Category configuration properties |





 

## configureShopCategoryForPlayer
Configure a shop category for a specific player.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The player to configure the category for |
| categoryKey | `ShopCategoryKey` | The key of the category to configure |
| config | `ShopCategoryConfig` | Category configuration properties |





 

## createItemDrop
Create a dropped item.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| x | `number` |  |
| y | `number` |  |
| z | `number` |  |
| itemName | `ItemName` | Name of the item. Any item name, including blocks and 'Air' |
| amount | `PNull<number>` | The amount of the item to include in the drop - so when the player picks up the item drop, they get this many of the item. |
| mergeItems | `boolean` | Whether to merge the item into a nearby item of same type, if one exists. Defaults to false. |
| attributes | `ItemAttributes` | Attributes of the item being dropped |
| timeTillDespawn | `number` | Time till the item automatically despawns in milliseconds. Max of 5 mins. |
| dropperId | `PNull<LifeformId>` | Who dropped the item. |
| options | `ItemDropOptions` | Additional options, such as doPhysics and size. |



### Returns:
`PNull<EntityId>`

 

the id you can pass to setCantPickUpItem, or null if the item drop limit was reached

 

## createMobHerd
Create a mob herd. A mob herd represents a collection of mobs that move together.

### Returns:
`MobHerdId`

 



 

## createShopItem
Create a new shop item under the given category.

Will create a new category if it does not exist.

If the shop item already exists then it will be replaced.

If any per-player overrides exist under the same categoryKey and itemKey then they will be deleted.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| categoryKey | `ShopCategoryKey` | The key of the category to create the item in |
| itemKey | `ShopItemKey` | The unique key for the item |
| item | `ShopItem` | The shop item to create (will be mutated) |





 

## createShopItemForPlayer
Create a new shop item for a specific player.

Will create a new category if it does not exist.

Will replace any overrides this player already has for the same item.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The player to create the item for |
| categoryKey | `ShopCategoryKey` | The key of the category to create the item in |
| itemKey | `ShopItemKey` | The unique key for the item |
| item | `ShopItem` | The shop item to create (will be mutated) |





 

## deleteAllLobbyDbValues
Deletes all database values that are saved per lobby.



 

## deleteAllPlayerDbValues
Deletes all database values that are saved per player.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |





 

## deleteItemDrop
Delete an item drop by item drop entity ID

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| itemId | `EntityId` |  |





 

## deleteLobbyDbValue
Deletes a database value that is saved per lobby.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| key | `string` |  |





 

## deleteMeshEntity
Delete a mesh entity

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| eId | `EntityId` | Returns whether the api successfully deleted the meshEntity |



### Returns:
`boolean`

 



 

## deletePlayerDbValue
Deletes a database value that is saved per player.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| key | `string` |  |





 

## deleteQTE
Delete a quicktime event from the screen

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| id | `QTERequestId` | Returned from the addQTE request you want to cancel |





 

## deleteShopItem
Delete an existing shop item.

Throws an error if the item does not exist.

Will also delete all per-player overrides for the shop item.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| categoryKey | `ShopCategoryKey` | The key of the category containing the item |
| itemKey | `ShopItemKey` | The unique key for the item |





 

## deleteThrowable
Delete a throwable entity before it automatically removes itself.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| eId | `EntityId` |  |



### Returns:
`boolean`

 

true if the entity was deleted, false if it was not a throwable entity

 

## despawnMob
Dispose of a mob's state and remove them from the world without triggering "on death" flows.

Always succeeds.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| mobId | `MobId` |  |





 

## editItemCraftingRecipes
Edit the crafting recipes for a player.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| itemName | `ItemName` |  |
| recipesForItem | `RecipesForItem` |  |





 

## forceRespawn
Force respawn a player

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| respawnPos | `number[]` |  |





 

## getAuraInfo
Get the aura info for a player

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |



### Returns:
` { level: number; totalAura: number; auraPerLevel: number } `

 



 

## getBlock
Get the name of a block.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| x | `number \| number[]` | could be an array [x, y, z]. If so, the other params shouldn't be passed. |
| y | `number` |  |
| z | `number` |  |



### Returns:
`BlockName`

 



 

## getBlockCoordinatesPlayerStandingOn
Get the co-ordinates of the blocks the player is standing on as a list. For example, if the center of the player is at 0,0,0

this function will return [[0, -1, 0], [-1, -1, 0], [0, -1, -1], [-1, -1, -1]]

If the player is just standing on one block, the function would return e.g. [[0, 0, 0]]

If the player is middair then returns an empty list [].

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |



### Returns:
`number[][]`

 



 

## getBlockData
Get stored data about a block in a performant manner. Data is cleared when block changes.

E.g. chest

Works well with blocks marked tickable (e.g. wheat)

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| x | `number` |  |
| y | `number` |  |
| z | `number` |  |



### Returns:
`any`

 



 

## getBlockId
Used to get the block id at a specific position.

Intended only for use in hot code paths - default to getBlock for most use cases

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| x | `number` |  |
| y | `number` |  |
| z | `number` |  |



### Returns:
`BlockId`

 



 

## getBlockSolidity
Returns whether a block is solid or not.

E.g. Grass block is solid, while water, ladder and water are not.

Will be true if the block is unloaded.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| x | `number \| number[]` |  |
| y | `number` |  |
| z | `number` |  |



### Returns:
`boolean`

 



 

## getBlockTypesPlayerStandingOn
Get the types of block the player is standing on

For example, if a player is standing on 4 dirt blocks, this will return ["Dirt", "Dirt", "Dirt", "Dirt"]

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |



### Returns:
`any[]`

 



 

## getChunk
Only use this instead of getBlock if you REALLY need the performance (i.e. you are iterating over tens of thousands of blocks)

ReturnedObject.blockData is a 32x32x32 ndarray of block ids

(see https://www.npmjs.com/package/ndarray)

Each block id is a 16-bit number

The ndarray should only be read from, writing to it will result in desync between the server and client

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| pos | `number[]` | The returned chunk contains pos |



### Returns:
`PNull<GameChunk>`

 

null if the chunk is not loaded in a persisted world. ReturnedObject.blockData is an ndarray that can be accessed
(but modifications have to be saved with resetChunk).

 

## getClientOption
Returns the current value of a client option

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| option | `PassedOption` |  |



### Returns:
`ClientOptions[PassedOption]`

 



 

## getCurrentKillstreak
Gets the player's current killstreak

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |



### Returns:
`number`

 



 

## getDefaultMobSetting
Returns the current default value for a mob setting.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| mobType | `TMobType` |  |
| setting | `TMobSetting` |  |



### Returns:
`MobSettings<TMobType>[TMobSetting]`

 



 

## getEffects
Get all the effects currently applied to a lifeform.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| lifeformId | `LifeformId` |  |



### Returns:
`string[]`

 



 

## getEmptyChunk
Use this to get a chunk ndarray you can edit and set in resetChunk.



Only use chunk helpers if you REALLY need the performance (i.e. you are iterating over tens of thousands of blocks)

ReturnedObject.blockData is a 32x32x32 ndarray of air.

(see https://www.npmjs.com/package/ndarray)

Each block id is a 16-bit number

### Returns:
`GameChunk`

 



 

## getEntitiesInRect
Get the entities in the rect between [minX, minY, minZ] and [maxX, maxY, maxZ]

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| minCoords | `number[]` |  |
| maxCoords | `number[]` |  |



### Returns:
`EntityId[]`

 



 

## getEntityHeading


### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| entityId | `EntityId` |  |



### Returns:
`number`

 



 

## getEntityName
Get the in game name of an entity.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| entityId | `EntityId` |  |



### Returns:
`string`

 



 

## getEntityRotation
Get the rotation for a server-auth entity.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| entityId | `EntityId` |  |



### Returns:
`[number, number, number]`

 



 

## getEntityType


### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| entityId | `EntityId` |  |



### Returns:
`EntityType`

 



 

## getHealth
Get the current health of an entity.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| entityId | `PlayerId` |  |



### Returns:
`number`

 



 

## getHeldItem
Get the currently held item of a player

Returns null if no item is being held

If an item is held, return an object of the format {name: itemName, amount: amountOfItem}

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |



### Returns:
`PNull<InvenItem>`

 



 

## getInitialItemMetadata
Get the metadata about a block or item before stats have been modified by any client options

(i.e. its entry in the initial metadata object)

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| itemName | `string` |  |



### Returns:
`Partial<BlockMetadataItem & NonBlockMetadataItem>`

 



 

## getInventoryFreeSlotCount
Get the amount of free slots in a player's inventory.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |



### Returns:
`number`

 

number

 

## getInventoryItemAmount
The amount of an itemName a player has.

Returns 0 if the player has none, and a negative number if infinite.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| itemName | `ItemName` |  |



### Returns:
`number`

 

number

 

## getItemSlot
Get the item at a specific index

Returns null if there is no item at that index

If there is an item, return an object of the format { name: string; amount: PNull<number>; attributes: ItemAttributes; }

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| itemSlotIndex | `number` |  |



### Returns:
`PNull<InvenItem>`

 



 

## getItemStat
Get stat info about a block or item

Either based on a client option for a player: (e.g. `DirtTtb`)

or its entry in the initial metadata object if no client option is set.



If null is passed for lifeformId, this is simply its entry in blockMetadata etc.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| lifeformId | `PNull<LifeformId>` |  |
| itemName | `ItemName` |  |
| stat | `K` |  |



### Returns:
`AnyMetadataItem[K]`

 



 

## getLobbyDbValue
Gets a database value that is saved per lobby.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| key | `string` |  |



### Returns:
`PNull<string | number>`

 



 

## getLobbyName
Get the name of the lobby this game is running in.

### Returns:
`string`

 



 

## getLobbyType
Returns if the current lobby the game is running in is special - e.g. a discord guild or dm, or simply a standard lobby

### Returns:
`LobbyType`

 



 

## getMetaInfo
Splits the block name by '|'. If no meta info, metaInfo is ''

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| blockName | `BlockName \| null \| undefined` |  |



### Returns:
`ItemMetaInfo`

 



 

## getMobAiState
Gets the current AI state for the given mob.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| mobId | `MobId` |  |



### Returns:
` { state: MobAiState; params: MobAiStateParams<MobAiState> } `

 



 

## getMobIds
Get the mob IDs of all mobs in the world.

### Returns:
`MobId[]`

 



 

## getMobSetting
Get the current value of a mob setting for a specific mob.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| mobId | `MobId` |  |
| setting | `TMobSetting` |  |
| returnDefaultIfNotOverridden | `boolean` | If true, return the default setting if not overridden. |



### Returns:
`MobSettings<MobType>[TMobSetting]`

 



 

## getMoonstoneChestItems
Get all the items from a moonstone chest in order. Use this instead of repetitive calls to getMoonstoneChestItemSlot



Moonstone chests are a type of chest where a player accesses the same contents no matter the location of the moonstone chest

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |



### Returns:
`PNull<InvenItem>[]`

 



 

## getMoonstoneChestItemSlot
Get the item in a player's moonstone chest slot. Null if empty



Moonstone chests are a type of chest where a player accesses the same contents no matter the location of the moonstone chest

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| idx | `number` |  |



### Returns:
`PNull<InvenItem>`

 



 

## getNumMobs
Get the number of mobs in the world.

### Returns:
`number`

 



 

## getNumPlayers
Get the number of players in the room

### Returns:
`number`

 



 

## getOtherEntitySetting
Get the value of a player's other-entity setting for a specific entity.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| relevantPlayerId | `PlayerId` |  |
| targetedEntityId | `EntityId` |  |
| settingName | `Setting` |  |



### Returns:
`OtherEntitySettings[Setting]`

 



 

## getPlayerCosmetic
Get a single equipped cosmetic for a player.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| cosmeticType | `CosmeticType` | Type of cosmetic |



### Returns:
`CosmeticName`

 



 

## getPlayerDbId
Given a player, get their permanent identifier that doesn't change when leaving and re-entering

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |



### Returns:
`PlayerDbId`

 



 

## getPlayerDbValue
Gets a database value that is saved per player.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| key | `string` |  |



### Returns:
`PNull<string | number>`

 



 

## getPlayerFacingInfo
Get the position of a player's camera and the direction (both in Euclidean and spherical coordinates) they are attempting to use an item.

The camPos has the same limitations described in getPlayerTargetInfo

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |



### Returns:
` { camPos: [number, number, number]; dir: [number, number, number]; angleDir: AngleDir; moveHeading: number } `

 



 

## getPlayerId
Given the name of a player, get their id

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerName | `string` |  |



### Returns:
`PNull<PlayerId>`

 



 

## getPlayerIdFromDbId
Returns null if player not in lobby

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| dbId | `PlayerDbId` |  |



### Returns:
`PNull<PlayerId>`

 



 

## getPlayerIds
Get all the player ids.

### Returns:
`PlayerId[]`

 



 

## getPlayerPartyWhenJoined
Returns the party that the player was in when they joined the game. The returned object contains the playerDbIds, as well

as the playerIds if available, of the party leader and members.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |



### Returns:
`PNull<{ partyCode: string; playerDbIds: PlayerDbId[] }>`

 



 

## getPlayerPhysicsState
Get physics state for player

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |



### Returns:
`PlayerPhysicsStateData`

 



 

## getPlayerTargetInfo
Get the position of a player's target block and the block adjacent to it (e.g. where a block would be placed)





Note: This position is a tick ahead of the client's block target info (noa.targetedBlock),

since the client updates the blocktarget before the entities tick (and since it uses the renderposition of the camera)



This normally doesn't matter but if you are client predicting something based on noa.targetedBlock

(currently only applicable to in-engine code), you should not verify using this

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |



### Returns:
` { position: [number, number, number]; normal: [number, number, number]; adjacent: [number, number, number] } `

 



 

## getPosition
Get position of a player / entity.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| entityId | `EntityId` |  |



### Returns:
`[number, number, number]`

 



 

## getSelectedInventorySlotI
Get a player's currently selected inventory slot

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |



### Returns:
`number`

 



 

## getShieldAmount
Get the current shield of an entity.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| entityId | `EntityId` |  |



### Returns:
`number`

 



 

## getStandardChestFreeSlotCount
Get the amount of free slots in a standard chest

Returns null for non-chests

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| chestPos | `number[]` |  |



### Returns:
`PNull<number>`

 

number

 

## getStandardChestItemAmount
The amount of an itemName a standard chest has.

Returns 0 if the standard chest has none, and a negative number if infinite.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| chestPos | `number[]` |  |
| itemName | `ItemName` |  |



### Returns:
`number`

 

number

 

## getStandardChestItems
Get all the items from a standard chest in order. Use this instead of repetitive calls to getStandardChestItemSlot

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| chestPos | `number[]` |  |



### Returns:
`PNull<InvenItem>[]`

 



 

## getStandardChestItemSlot
Get the item at a chest slot. Null if empty otherwise format {name: itemName, amount: amountOfItem}

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| chestPos | `number[]` |  |
| idx | `number` |  |



### Returns:
`PNull<InvenItem>`

 



 

## getUnitCoordinatesLifeformWithin
Get the up to 12 unit co-ordinates the lifeform is located within

(A lifeform is modelled as having four corners and can be in up to 3 blocks vertically)

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| lifeformId | `LifeformId` |  |



### Returns:
`number[][]`

 

List of x, y, z positions e.g. [[-1, 0, 0], [-1, 1, 0], [-1, 2, 0]]

 

## getVelocity
Get the velocity of an entity

Will return [0, 0, 0] if the entity doesn't have a physics body

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| eId | `EntityId` |  |



### Returns:
`[number, number, number]`

 



 

## giveItem




Inventory stuff

Give a player an item and a certain amount of that item.

Returns the amount of item added to the users inventory.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| itemName | `ItemName` |  |
| itemAmount | `number` |  |
| attributes | `ItemAttributes` | An optional object for certain types of item. For guns this can contain the shotsLeft field which is the amount of ammo the gun currently has. |



### Returns:
`number`

 



 

## giveStandardChestItem
Give a standard chest an item and a certain amount of that item.

Returns the amount of item added to the chest.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| chestPos | `number[]` |  |
| itemName | `ItemName` |  |
| itemAmount | `number` |  |
| playerId | `PlayerId` | The player who is interacting with the chest. |
| attributes | `ItemAttributes` | An optional object for certain types of item. For guns this can contain the shotsLeft field which is the amount of ammo the gun currently has. |



### Returns:
`number`

 



 

## hasActiveQTE
Returns whether the player has any qteRequests

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |



### Returns:
`boolean`

 



 

## hasItem
Whether a player has an item

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| itemName | `ItemName` |  |



### Returns:
`boolean`

 

bool

 

## initiateMiddleScreenBar
This will initiate the MiddleScreenBar, starting at empty and filling up to full over the given duration.

Good to represent cooldowns (eg gun reload) or charged items (eg crossbow)

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| duration | `number` | ms over which the MiddleScreenBar fills up |
| chargeExpiresAutomatically | `boolean` | Defaults to true. If true, the bar will disappear upon reaching full. If false, the bar will remain at full until hidden with removeMiddleScreenBar |
| horizontalBarRemOffset | `number` | Offset the bar left or right (in css unit - rem) |





 

## inventoryIsFull
Whether the player has space in their inventory to get new blocks

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |



### Returns:
`boolean`

 



 

## isAlive
Whether a lifeform is alive or dead (or on the respawn screen, in a player's case).

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| lifeformId | `LifeformId` |  |



### Returns:
`boolean`

 



 

## isBlockInLoadedChunk
Check if the block at a specific position is in a loaded chunk.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| x | `number` |  |
| y | `number` |  |
| z | `number` |  |



### Returns:
`boolean`

 



 

## isInsideRect
Check if a position is within a cubic rectangle

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| coordsToCheck | `number[]` |  |
| pos1 | `number[]` | position of one corner |
| pos2 | `number[]` | position of opposite corner |
| addOneToMax | `boolean` |  |



### Returns:
`boolean`

 



 

## isMobile
Whether the player is on a mobile device or a computer.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |



### Returns:
`boolean`

 



 

## isPlayerCrouching
Check whether a player is crouching

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |



### Returns:
`boolean`

 



 

## isPublicLobby
Integer lobby names are public

### Returns:
`boolean`

 

boolean

 

## kickPlayer


### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| reason | `string` |  |





 

## killLifeform
Kill a lifeform.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| lifeformId | `LifeformId` |  |
| whoKilled | ` LifeformId \| { lifeformId: LifeformId; withItem: string } ` | Optional |





 

## matchmakePlayer
Tell a player to disconnect from the current lobby and join a new one.



To connect to a specific variation, format is `gamename_variation`.

For Custom Games, this will be `classic_playerSchematic|XXXXXXXXXX`.



NOTE: Players won't disconnect immediately (they may play an ad before being redirected).

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| game | `string` | Defaults to the current game. |
| lobbyName | `string` | Defaults to "Quick Play" |





 

## now
Obtain Date.now() value saved at start of current game tick

### Returns:
`number`

 



 

## openChestForPlayer
Open a chest for a player.

If there is no chest, or the player cannot open it, do nothing.

WARNING: This may call "onPlayerAttemptOpenChest" to determine if the player has permission to open it. Using this function inside that callback risks infinite recursion.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| x | `number` |  |
| y | `number` |  |
| z | `number` |  |





 

## openShop
Open the shop UI for a player

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| toggle | `boolean` | Whether to close the shop if it's already open |
| forceCategoryKey | `PNull<ShopCategoryKey>` | If set, will change the shop to this category |
| onlyIfNonEmpty | `boolean` | If true, will only open the shop if the category (or shop, if no category is provided) is non-empty |





 

## playClientPredictedSound
See documentation for api.playSound

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| soundName | `string` |  |
| volume | `number` |  |
| rate | `number` |  |
| posSettings | ` { playerIdOrPos: PlayerId \| number[] maxHearDist?: number refDistance?: number } ` |  |
| predictedBy | `PlayerId` |  |





 

## playerIsInGame
Whether a player is currently in the game

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |



### Returns:
`boolean`

 



 

## playerIsLoggedIn


### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |



### Returns:
`boolean`

 



 

## playParticleEffect
Play particle effect on all clients, or only on some clients if clientPredictedBy is specified

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| opts | `TempParticleSystemOpts \| ParticlePresetOpts` |  |
| clientPredictedBy | `PlayerId` | Play only on clients where client with playerId clientPredictedBy is not invisible, transparent, or themselves |





 

## playSound


### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | hears the sound |
| soundName | `string` | Can also be a prefix. If so, a random sound with that prefix will be played |
| volume | `number` | 0-1. If it's too quiet and volume is 1, normalise your sound in audacity |
| rate | `number` | The speed of playback. Also affects pitch. 0.5-4. Lower playback = lower pitch Good for varying the sound. E.g. item pickup sound has a random rate between 1 and 1.5. |
| posSettings | ` { playerIdOrPos: PlayerId \| number[] maxHearDist?: number refDistance?: number } ` | : PlayerId \| number[], maxHearDist: number, refDistance: number} playerIdOrPos: The player the sound originates from, or the position of the sound maxHearDist: sound is not played if player is further than this. Default 15 refDistance: higher means the sound decreases less in volume with distance. Default 3. Hitting is 4. Guns are 10 |





 

## progressBarUpdate
Update the progress bar in the bottom right corner.

Can be queued.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| toFraction | `number` | The fraction of the progress bar you want to be filled up. |
| toDuration | `number` | The time it takes for the bar to reach the given toFraction in ms. If this is too low and you queue multiple updates, this toFraction could be skipped. Treat 200ms as a minimum. |





 

## raycastForBlock
Raycast for a block in the world.

Given a position and a direction, find the first block that the "ray" hits.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| fromPos | `number[]` |  |
| dirVec | `number[]` |  |



### Returns:
`BlockRaycastResult`

 



 

## removeAppliedSkin
Remove gamemode-applied skin from a player

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |





 

## removeEffect
Remove an effect from a lifeform.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| lifeformId | `LifeformId` |  |
| name | `string` |  |





 

## removeFollowingEntityFromPlayer
Remove following entity from player

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| entityEId | `EntityId` |  |





 

## removeItemCraftingRecipes
Removes crafting recipes

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| itemName | `PNull<string>` | Removes all crafting recipes for the given player if null, otherwise removes the crafting recipes for the given item. |





 

## removeItemName
Remove an amount of item from a player's inventory

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| itemName | `ItemName` |  |
| amount | `number` |  |





 

## removeMiddleScreenBar
If there is any current middle screen bar running, this will hide it

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |





 

## resetCanChangeBlockRect
Remove any previous can/cant change block rect settings for a player

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| pos1 | `number[]` |  |
| pos2 | `number[]` |  |





 

## resetCanChangeBlockType
Remove any previous can/cant change block type settings for a player

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| blockName | `BlockName` |  |





 

## resetItemCraftingRecipes
Reset the crafting recipes for a given back to its original bloxd state

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| itemName | `PNull<string>` | Resets all crafting recipes for the given player if null, otherwise resets the crafting recipes for the given item. |





 

## resetShopItemForPlayer
Delete a specific player's overrides for a shop item.

Like other methods, it doesn't matter whether the overrides were created

using createShopItemForPlayer or by using updateShopItemForPlayer instead.

This method does nothing if the overrides don't exist or are defined internally by the engine.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The player to reset the item for |
| categoryKey | `ShopCategoryKey` | The key of the category containing the item |
| itemKey | `ShopItemKey` | The unique key for the item |





 

## scalePlayerMeshNodes
Scale node of a player's mesh by 3d vector.

State from prior calls to this api is lost so if you want to have multiple nodes scaled, pass in all the scales at once.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| nodeScales | `EntityMeshScalingMap` |  |





 

## sendFlyingMiddleMessage
Send a flying middle message to a specific player

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | Id of the player |
| message | `string \| CustomTextStyling` | The text contained within the message. Can be either a string or use `Custom Text Styling`. |
| distanceFromAction | `number` | The distance from the action that has caused this message to be displayed, this value will be used to determine how the message flies across the screen. |
| lifetimeMs | `number` | How long the message will be visible in milliseconds. Defaults to 1000ms. |





 

## sendHitmarker
Show a hitmarker on the player's screen (the X-shaped crosshair flash indicating a successful hit).

Useful for custom weapons or things that need visual hit feedback.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The player to show the hitmarker to |
| isCrit | `boolean` | If true, shows an enhanced critical-hit hitmarker with a longer, more dramatic animation |
| directionVector | `PNull<number[]>` | Optional [x, y, z] direction vector. When provided, the hitmarker appears at the projected screen position of that direction rather than at the centre of the screen. Same flow as mobile melee attacks where the tap point differs from screen centre. |





 

## sendMessage
Send a message to a specific player

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | Id of the player |
| message | `string \| CustomTextStyling` | The text contained within the message. Can use `Custom Text Styling`. |
| style | ` { fontWeight?: number \| string; color?: string } ` | An optional style argument. Can contain values for fontWeight and color of the message. style is ignored if message uses custom text styling (i.e. is not a string). |





 

## sendOverShopInfo
Show a message over the shop in the same place that a shop item's onBoughtMessage is shown.

Displays for a couple seconds before disappearing

Use case is to show a dynamic message when player buys an item

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| info | `string \| CustomTextStyling` |  |





 

## sendTopRightHelper


### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| icon | `string` | Can be any icon from font-awesome. |
| text | `string` | The text to send. |
| opts | ` { duration?: number width?: number height?: number color?: string iconSizeMult?: number textAndIconColor?: string fontSize?: string } ` | Can include keys duration, width, height, color, iconSizeMult.  Default opts: { duration: 8, // seconds width: 400px, height: 100px, color: 'rgb(102, 102, 102)', // must be rgb in this format (hex not supported), iconSizeMult: 5, textAndIconColor: "white", // can be any colour supported by css (e.g. hex, rgb), fontSize: '17px', } |





 

## setAuraLevel
Set the aura level for a player - shortcut for setTotalAura(level * auraPerLevel)

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| level | `number` |  |





 

## setBlock
Set a block. Valid names are any block name, including 'Air'



This function is optimised for setting broad swathes of blocks. For example, if you have a 50x50x50 area you need to turn to air, it will run performantly if you call this in double nested loops.



IF you're only changing a few blocks, you want this to be super snappy for players, AND you're calling this outside of your _tick function, you can use api.setOptimisations(false).



If you want the optimisations for large quantities of blocks later on, then call api.setOptimisations(true) when you're done.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| x | `number \| number[]` | Can be an array |
| y | `number \| BlockName` | Should be blockname if first param is array |
| z | `number` |  |
| blockName | `BlockName` |  |





 

## setBlockData
Store data about a block in a performant manner. Data is cleared when block changes.

E.g. chest

Works well with blocks marked tickable (e.g. wheat)

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| x | `number` |  |
| y | `number` |  |
| z | `number` |  |
| data | `object` |  |





 

## setBlockRect
Helper function that sets all blocks in a rectangle to a specific block.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| pos1 | `number[]` | array [x, y, z] |
| pos2 | `number[]` | array [x, y, z] |
| blockName | `BlockName` |  |





 

## setBlockWalls
Create walls by providing two opposite corners of the cuboid

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| pos1 | `number[]` | array [x, y, z] |
| pos2 | `number[]` | array [x, y, z] |
| blockName | `BlockName` |  |
| hasFloor | `boolean` |  |
| hasCeiling | `boolean` |  |





 

## setCallbackValueFallback
Set a default value to be returned by your callback code if it throws an error.


### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| callbackName | string | The name of the callback to set the default value for |
| defaultValue | any | The default value to return if the callback throws an error |
        

 

## setCameraDirection
Set the direction the player is looking.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| direction | `number[]` | a vector of the direction to look, format [x, y, z] |





 

## setCameraZoom
Set camera zoom for a player

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| zoom | `number` |  |





 

## setCanChangeBlock
Let a player change a block at a specific co-ordinate. Useful when client option canChange is false.

Overrides blockRect and blockType settings, so also useful when you have disallowed changing of a block type with setCantChangeBlockType.

Using this on 1000s of blocks will cause lag - if that is needed, find a way to use setCanChangeBlockType.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| x | `number` |  |
| y | `number` |  |
| z | `number` |  |





 

## setCanChangeBlockRect
Make it so a player can Change blocks within two points. Coordinates are inclusive. E.g. if [0, 0, 0] is pos1

and [1, 1, 1] is pos2 then the 8 blocks contained within low and high will be able to be broken.

Overrides setCantChangeBlockType

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| pos1 | `number[]` | Arg as [x, y, z] |
| pos2 | `number[]` | Arg as [x, y, z] |





 

## setCanChangeBlockType
Lets a player Change a block type. Valid names are any block name, including 'Air'

Less priority than cant change block pos/can change block rect

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| blockName | `BlockName` |  |





 

## setCantChangeBlock
Prevents a player from changing a block at a specific co-ordinate. Useful when client option canChange is true.

Overrides blockRect and blockType settings, so also useful when you have allowed changing of a block type with setCantChangeBlockType.

Using this on 1000s of blocks will cause lag - if that is needed, find a way to use setCantChangeBlockType.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| x | `number` |  |
| y | `number` |  |
| z | `number` |  |





 

## setCantChangeBlockRect
Make it so a player cant Change blocks within two points. Coordinates are inclusive. E.g. if [0, 0, 0] is pos1

and [1, 1, 1] is pos2 then the 8 blocks contained within pos1 and pos2 won't be able to be broken.

Overrides setCanChangeBlockType

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| pos1 | `number[]` | Arg as [x, y, z] |
| pos2 | `number[]` | Arg as [x, y, z] |





 

## setCantChangeBlockType
Stops a player from changing a block type. Valid names are any block name, including 'Air'

Less priority than can change block pos/can change block rect

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| blockName | `BlockName` |  |





 

## setCantPickUpItem
Prevent a player from picking up an item. itemId returned by createItemDrop

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| itemId | `EntityId` |  |





 

## setClientOption
Modify a client option at runtime and send to the client if it changed

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| option | `PassedOption` | The name of the option |
| value | `ClientOptions[PassedOption]` | The new value of the option |





 

## setClientOptions
Modify client options at runtime

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| optionsObj | `Partial<ClientOptions>` | An object which contains key value pairs of new settings. E.g {canChange: true, speedMultiplier: false} |





 

## setClientOptionToDefault
Sets a client option to its default value. This will be the value stored in your game's defaultClientOptions, otherwise Bloxd's default.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| option | `ClientOption` |  |





 

## setDefaultMobSetting
Set the default value for a mob setting.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| mobType | `TMobType` |  |
| setting | `TMobSetting` |  |
| value | `MobSettings<TMobType>[TMobSetting]` |  |





 

## setDirectionArrow
Show a directional arrow indicator on the player's screen pointing toward a world position.

When the position is off-screen the indicator is a rotating chevron at the screen edge.

When the position is on-screen it becomes a small marker dot.



The arrow persists until explicitly cleared via `clearDirectionArrow`.

Calling again with the same `id` updates the existing arrow in-place.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The player to show the arrow to |
| id | `string` | Unique identifier for this arrow (allows multiple concurrent arrows) |
| position | `number[]` | [x, y, z] world position the arrow should point toward |
| text | `PNull<string \| CustomTextStyling>` | Optional label rendered below the indicator. Supports CustomTextStyling for rich text with icons/colours. |
| showDistance | `boolean` | If true, displays the distance (in blocks) from the player to the arrow position. |
| style | `PNull<TextStyle>` | Optional style object (same format as CustomTextStyling's StyledText `style`). Controls chevron/marker colour, label typography, and opacity. |





 

## setEntityHeading


### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| entityId | `EntityId` |  |
| newHeading | `number` |  |





 

## setEntityRotation
Set the rotation for a server-auth entity.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| entityId | `EntityId` |  |
| xRotation | `number` |  |
| yRotation | `number` |  |
| zRotation | `number` |  |





 

## setEveryoneSettingForPlayer
Set a player's other-entity setting for every lifeform in the game.

includeNewJoiners=true means that the player will have the setting applied to new joiners.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| settingName | `Setting` |  |
| settingValue | `OtherEntitySettings[Setting]` |  |
| includeNewJoiners | `boolean` |  |





 

## setHealth
Set the current health of an entity.

If you want to set their health to more than their current max health, the optional increaseMaxHealthIfNeeded must be true.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| entityId | `EntityId` |  |
| newHealth | `PNull<number>` | Can be null to make the player not have health |
| whoDidDamage | ` LifeformId \| { lifeformId: LifeformId; withItem: string } ` | Optional |
| increaseMaxHealthIfNeeded | `boolean` | Optional |



### Returns:
`boolean`

 



 

## setItemAmount
Set the amount of an item in an item entity

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| itemId | `EntityId` |  |
| newAmount | `number` |  |





 

## setItemSlot
Put an item in a specific index. Default hotbar is indexes 0-9

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| itemSlotIndex | `number` | 0-indexed |
| itemName | `ItemName` | Can be 'Air', in which case itemAmount will be ignored and the slot will be cleared. |
| itemAmount | `PNull<number>` | -1 for infinity. Should not be set, or null, for items that are not stackable. |
| attributes | `ItemAttributes` | An optional object for certain types of item. For guns this can contain the shotsLeft field which is the amount of ammo the gun currently has. |
| tellClient | `boolean` | whether to tell client about it - results in desync between client and server if client doesnt locally perform the same action |





 

## setItemStat
Set a stat attribute for a block or item



NOTE: Only a subset of stats are customisable this way.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| itemName | `ItemName` |  |
| stat | `K` |  |
| value | `AnyMetadataItem[K]` |  |





 

## setLobbyDbValue
Sets a database value that is saved per lobby. This persists between sessions.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| key | `string` |  |
| value | `string \| number` |  |





 

## setMaxPlayers
Update the max players and soft max players matchmaking will use



softMaxPlayers is the number of players that matchmaking will route to using "Quick Play".

Once the softMaxPlayers limit is reached, this lobby can only be joined by requesting the lobby name or joining a friend.



maxPlayers is the absolute maximum: a lobby will not have more players than this.

Tip: softMaxPlayers should be around 90% of maxPlayers



WARNING: This change is not immediate, as it takes a while for matchmaking to find out.

Also, this will not kick players out of the lobby if set to a lower value than the current player count.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| softMaxPlayers | `number` |  |
| maxPlayers | `number` |  |





 

## setMobAiState
Sets the current AI state for the given mob.

Some AI states will require context such as the ID of the lifeform being chased.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| mobId | `MobId` |  |
| state | `TState` |  |
| params | `MobAiStateParams<TState>` |  |





 

## setMobSetting
Set the current value of a mob setting for a specific mob.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| mobId | `MobId` |  |
| setting | `TMobSetting` |  |
| value | `MobSettings<MobType>[TMobSetting]` |  |





 

## setMoonstoneChestItemSlot
Moonstone chests are a type of chest where a player accesses the same contents no matter the location of the moonstone chest

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| idx | `number` | 0-indexed |
| itemName | `ItemName` | Can be 'Air', in which case itemAmount will be ignored and the slot will be cleared. |
| itemAmount | `number` | -1 for infinity. Should not be set, or null, for items that are not stackable. |
| metadata | `ItemAttributes` | An optional object for certain types of item. For guns this can contain the shotsLeft field which is the amount of ammo the gun currently has. |





 

## setOtherEntitySetting
Set a player's other-entity setting for a specific entity.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| relevantPlayerId | `PlayerId` |  |
| targetedEntityId | `EntityId` |  |
| settingName | `Setting` |  |
| settingValue | `OtherEntitySettings[Setting]` |  |





 

## setOtherEntitySettings
Set many of a player's other-entity settings for a specific entity.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| relevantPlayerId | `PlayerId` |  |
| targetedEntityId | `EntityId` |  |
| settingsObject | `Partial<OtherEntitySettings>` |  |





 

## setPlayerDbValue
Sets a database value that is saved per player. This persists between sessions and between lobbies for custom games.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| key | `string` |  |
| value | `string \| number` |  |





 

## setPlayerOpacity
Set a player's opacity

A simple helper that calls setTargetedPlayerSettingForEveryone

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| opacity | `number` |  |





 

## setPlayerOpacityForOnePlayer
Set the level of viewable opacity by one player on another player

A simple helper that calls setOtherEntitySetting

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerIdWhoViewsOpacityPlayer | `PlayerId` | The player who sees that with opacity |
| playerIdOfOpacityPlayer | `PlayerId` | The player/player model who is given opacity |
| opacity | `number` |  |





 

## setPlayerPhysicsState
Set physics state of player (vehicle type and tier)

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| physicsState | `PlayerPhysicsStateData` |  |
| positionOffset | `[number, number, number]` | Optional offset to adjust the player's collision box |





 

## setPlayerPose
Set the pose of the player

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| pose | `PlayerPose` |  |
| poseOffset | `[number, number, number]` |  |





 

## setPosition
Set position of a player / entity.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| entityId | `EntityId` |  |
| x | `number \| number[]` | Can also be an array, in which case y and z shouldn't be passed |
| y | `number` |  |
| z | `number` |  |





 

## setSelectedInventorySlotI
Force the player to have the ith inventory slot selected. E.g. newI 0 makes the player have the 0th inventory slot selected

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| newI | `number` | integer from 0-9 |





 

## setShieldAmount
Set the current shield of a lifeform.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| lifeformId | `LifeformId` |  |
| newShieldAmount | `number` |  |





 

## setStandardChestItemSlot


### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| chestPos | `number[]` |  |
| idx | `number` | 0-indexed |
| itemName | `ItemName` | Can be 'Air', in which case itemAmount will be ignored and the slot will be cleared. |
| itemAmount | `number` | -1 for infinity. Should not be set, or null, for items that are not stackable. |
| playerId | `PlayerId` | The player who is interacting with the chest. |
| attributes | `ItemAttributes` | An optional object for certain types of item. For guns this can contain the shotsLeft field which is the amount of ammo the gun currently has. |





 

## setTargetedPlayerSettingForEveryone
Set every player's other-entity setting to a specific value for a particular player.

includeNewJoiners=true means that new players joining the game will also have this other player setting applied.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| targetedPlayerId | `PlayerId` |  |
| settingName | `Setting` |  |
| settingValue | `OtherEntitySettings[Setting]` |  |
| includeNewJoiners | `boolean` |  |





 

## setTotalAura
Sets the total aura for a player. Will not go over max level or under 0

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| totalAura | `number` |  |





 

## setVelocity
Set the velocity of an entity

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| eId | `EntityId` |  |
| x | `number` |  |
| y | `number` |  |
| z | `number` |  |





 

## setWalkThroughRect
Allow a player to walk through (or not walk through) voxels that are located within a given rectangle.

For blocks that are normally solid and not seethrough, the player will experience slight visual glitches while inside the block.



You could set both pos1 and pos2 to [0, 0, 0] to make only 0, 0, 0 walkthrough, for example.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| pos1 | `number[]` | The one corner of the cuboid. Format [x, y, z] |
| pos2 | `number[]` | The top right corner of the cuboid. Format [x, y, z] |
| updateType | `WalkThroughType` | The type of update. Whether to make a rect solid, or able to be walked through. Pass DEFAULT_WALK_THROUGH with a previously passed rect to disable any walkthrough setting for that rect. |





 

## setWalkThroughType
Allow a player to walk through a type of block. For blocks that are normally solid and not seethrough, the player will experience slight visual glitches while inside the block.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| blockName | `BlockName` |  |
| disable | `boolean` | If you've enabled a player to walk through a block and want to make the block solid for them again, pass this with true. Otherwise you only need to pass playerId and blockName |





 

## showShopTutorial
Show the shop tutorial for a player. Will not be shown if they have ever seen the shop tutorial in your game before.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |





 

## updateEntityNodeMeshAttachment
Attach/detach mesh instances to/from an entity

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| eId | `EntityId` |  |
| node | `EntityNamedNode` | node to attach to |
| type | `PNull<MeshType>` | if null, detaches mesh from this node |
| opts | `MeshEntityOpts[MeshType]` |  |
| offset | `[number, number, number]` |  |
| rotation | `[number, number, number]` |  |





 

## updateMeshEntity
Update a mesh entity. If used on a non-mesh entity, will do nothing.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| eId | `EntityId` |  |
| type | `MeshType` |  |
| opts | `MeshEntityOpts[MeshType]` |  |





 

## updateShopItem
Update selected properties of an existing shop item.

For example, { canBuy: true } to allow players to purchase the item.

Throws an error if the item does not exist.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| categoryKey | `ShopCategoryKey` | The key of the category containing the item |
| itemKey | `ShopItemKey` | The unique key for the item |
| changes | `Partial<ShopItem>` | Partial shop item properties to update |





 

## updateShopItemForPlayer
Update selected properties of an existing shop item for a specific player.

For example, { canBuy: true } to allow this player to purchase the item.

Throws an error if the item does not exist.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The player to update the item for |
| categoryKey | `ShopCategoryKey` | The key of the category containing the item |
| itemKey | `ShopItemKey` | The unique key for the item |
| changes | `Partial<ShopItem>` | Partial shop item properties to update |




