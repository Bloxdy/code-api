# Callbacks

Players can use World Code in custom worlds get functions they've written to run when game events happen. These special functions are called callbacks. The world code can be viewed by pressing F8 by default. Initially the world code will have this comment, which can be removed:

```text
tick onClose onPlayerJoin onPlayerLeave onPlayerJump onRespawnRequest
playerCommand onPlayerChat onPlayerChangeBlock onPlayerDropItem
onPlayerPickedUpItem onPlayerSelectInventorySlot onBlockStand
onPlayerAttemptCraft onPlayerCraft onPlayerAttemptOpenChest
onPlayerOpenedChest onPlayerMoveItemOutOfInventory onPlayerMoveInvenItem
onPlayerMoveItemIntoIdxs onPlayerSwapInvenSlots onPlayerMoveInvenItemWithAmt
onPlayerAttemptAltAction onPlayerAltAction onPlayerClick
onClientOptionUpdated onMobSettingUpdated onInventoryUpdated onChestUpdated
onWorldChangeBlock onCreateBloxdMeshEntity onEntityCollision
onPlayerAttemptSpawnMob onWorldAttemptSpawnMob onPlayerSpawnMob
onWorldSpawnMob onWorldAttemptDespawnMob onMobDespawned onPlayerAttack
onPlayerDamagingOtherPlayer onPlayerDamagingMob onMobDamagingPlayer
onMobDamagingOtherMob onAttemptKillPlayer onPlayerKilledOtherPlayer
onMobKilledPlayer onPlayerKilledMob onMobKilledOtherMob onPlayerPotionEffect
onPlayerDamagingMeshEntity onPlayerBreakMeshEntity onPlayerUsedThrowable
onPlayerThrowableHitTerrain onTouchscreenActionButton onTaskClaimed
onChunkLoaded onPlayerRequestChunk onItemDropCreated
onPlayerStartChargingItem onPlayerFinishChargingItem doPeriodicSave

To use a callback, just assign a function to it in the world code!
tick = () => {}			 or			 function tick() {}
```

You can use `api.setCallbackValueFallback("callbackName", defaultValue)` to set a default value to be returned by your callback code if it throws an error.

```js
/**
 * Called every tick, 20 times per second
 * @param ms
 */
tick = (ms) => {}

/**
 * Called when the lobby is shutting down
 * @param serverIsShuttingDown - Whether the server is shutting down
 */
onClose = (serverIsShuttingDown) => {}

/**
 * Called when a player joins the lobby
 * @param playerId - The id of the player that joined
 * @param fromGameReset - Whether this call is from a game reset (used by SessionBasedGame)
 */
onPlayerJoin = (playerId, fromGameReset) => {}

/**
 * Called when a player leaves the lobby
 * @param playerId - The id of the player that left
 * @param serverIsShuttingDown - Whether the server is shutting down
 */
onPlayerLeave = (playerId, serverIsShuttingDown) => {}

/**
 * Called when a player jumps
 * @param playerId - The id of the player that jumped
 */
onPlayerJump = (playerId) => {}

/**
 * Called when a player requests to respawn.
 * Optionally return the respawn location. Defaults to [0, 0, 0].
 * Return true to handle yourself (good for async,
 * but be careful that the player isn't at the place they died,
 * as they could pick up their old items or hit the player they were fighting).
 * @param playerId - The id of the player that requested to respawn
 */
onRespawnRequest = (playerId) => {}

/**
 * Called when a player sends a command
 * @param playerId - The id of the player that sent the command
 * @param command - The command that the player sent
 */
playerCommand = (playerId, command) => {
    return false
}

/**
 * Called when a player sends a chat message
 * Return false or null to prevent the broadcast of the message.
 * Return a string or CustomTextStyling to add a prefix to message.
 * Return for most flexibility: an object where keys are playerIds -
 * the value for a playerId being false means that player won't receive the message.
 * Otherwise playerId values should be an object with (optional) keys
 * prefixContent and chatContent to modify the prefix and the chat.
 * World code is not permitted to specify chatContent, it will be ignored.
 * @param {PlayerId} playerId - The id of the player that sent the message
 * @param {string} chatMessage - The message that the player sent
 * @param {string} [channelName] - The name of the channel that the message was sent in
 */
onPlayerChat = (playerId, chatMessage, channelName) => {
    return true
}

/**
 * Called when a player changes a block
 * Return "preventChange" to prevent the change.
 * If player places block, fromBlock will be Air (and toBlock the block).
 * If a player breaks a block, toBlock will be Air.
 * Return "preventDrop" to prevent a block item from dropping.
 * Return an array to set the dropped item position.
 * @param {PlayerId} playerId - The id of the player that changed the block
 * @param {number} x - The x coordinate of the block that was changed
 * @param {number} y - The y coordinate of the block that was changed
 * @param {number} z - The z coordinate of the block that was changed
 * @param {BlockName} fromBlock - The old block that was replaced
 * @param {BlockName} toBlock - The new block that was placed
 * @param {BlockName | null} droppedItem - The item that was dropped
 * @param {MultiBlockInfo} fromBlockInfo - The info of the old block that was replaced
 * @param {MultiBlockInfo} toBlockInfo - The info of the new block that was placed
 */
onPlayerChangeBlock = (playerId, x, y, z, fromBlock, toBlock, droppedItem, fromBlockInfo, toBlockInfo) => {}

/**
 * Called when a player drops an item
 * Return "preventDrop" to prevent the player from dropping the item at all.
 * Return "allowButNoDroppedItemCreated" to allow discarding items without dropping them.
 * @param {PlayerId} playerId - The id of the player that dropped the item
 * @param {number} x - The x coordinate of the item that was dropped
 * @param {number} y - The y coordinate of the item that was dropped
 * @param {number} z - The z coordinate of the item that was dropped
 * @param {ItemName} itemName - The name of the item that was dropped
 * @param {number} itemAmount - The amount of the item that was dropped
 * @param {number} fromIdx - The index of the item that was dropped from the player's inventory
 */
onPlayerDropItem = (playerId, x, y, z, itemName, itemAmount, fromIdx) => {}

/**
 * Called when a player picks up an item
 * @param {PlayerId} playerId - The id of the player that picked up the item
 * @param {string} itemName - The name of the item that was picked up
 * @param {number} itemAmount - The amount of the item that was picked up
 */
onPlayerPickedUpItem = (playerId, itemName, itemAmount) => {}

/**
 * Called when a player selects a different inventory slot.
 * This will be called eventually when you have already set the slot using
 * api.setSelectedInventorySlotI so be careful not to cause an infinite loop doing this.
 * @param {PlayerId} playerId - The id of the player that selected the inventory slot
 * @param {number} slotIndex - The index of the inventory slot that was selected
 */
onPlayerSelectInventorySlot = (playerId, slotIndex) => {}

/**
 * Called when a player stands on a block
 * @param {PlayerId} playerId - The id of the player that stood on the block
 * @param {number} x - The x coordinate of the block that was stood on
 * @param {number} y - The y coordinate of the block that was stood on
 * @param {number} z - The z coordinate of the block that was stood on
 * @param {BlockName} blockName - The name of the block that was stood on
 */
onBlockStand = (playerId, x, y, z, blockName) => {}

/**
 * Called when a player attempts to craft an item
 * Return "preventCraft" to prevent a craft from happening
 * @param {PlayerId} playerId - The id of the player that is attempting to craft the item
 * @param {string} itemName - The name of the item that is being crafted
 * @param {number} craftingIdx - The index of the used recipe in the item's recipe list
 * @param {number} craftTimes - The number of times the craft recipe is used at once (e.g. shift held while crafting)
 */
onPlayerAttemptCraft = (playerId, itemName, craftingIdx, craftTimes) => {}

/**
 * Called when a player crafts an item
 * @param {PlayerId} playerId - The id of the player that crafted the item
 * @param {string} itemName - The name of the item that was crafted
 * @param {number} craftingIdx - The index of the used recipe in the item's recipe list
 * @param {RecipesForItem[number]} recipe - The recipe that was used to craft the item
 * @param {number} craftTimes - The number of times the craft recipe is used at once (e.g. shift held while crafting)
 */
onPlayerCraft = (playerId, itemName, craftingIdx, recipe, craftTimes) => {}

/**
 * Called when a player attempts to open a chest
 * Return "preventOpen" to prevent the player from opening the chest
 * @param {PlayerId} playerId - The id of the player that is attempting to open the chest
 * @param {number} x - The x coordinate of the chest that the player is attempting to open
 * @param {number} y - The y coordinate of the chest that the player is attempting to open
 * @param {number} z - The z coordinate of the chest that the player is attempting to open
 * @param {boolean} isMoonstoneChest - Whether the chest is a moonstone chest
 * @param {boolean} isIronChest - Whether the chest is an iron chest
 */
onPlayerAttemptOpenChest = (playerId, x, y, z, isMoonstoneChest, isIronChest) => {}

/**
 * Called when a player opens a chest
 * @param {PlayerId} playerId - The id of the player that opened the chest
 * @param {number} x - The x coordinate of the chest that was opened
 * @param {number} y - The y coordinate of the chest that was opened
 * @param {number} z - The z coordinate of the chest that was opened
 * @param {boolean} isMoonstoneChest - Whether the chest is a moonstone chest
 * @param {boolean} isIronChest - Whether the chest is an iron chest
 */
onPlayerOpenedChest = (playerId, x, y, z, isMoonstoneChest, isIronChest) => {}

/**
 * Called when a player moves an item out of their inventory
 * Return "preventChange" to prevent the movement
 * @param {PlayerId} playerId - The id of the player moving the item
 * @param {string} itemName - The name of the item being moved
 * @param {number} itemAmount - The amount of the item being moved
 * @param {number} fromIdx - The index which the item is being moved from
 * @param {string} movementType - The type of movement that occurred
 */
onPlayerMoveItemOutOfInventory = (playerId, itemName, itemAmount, fromIdx, movementType) => {}

/**
 * Called for all types of inventory item movement.
 * Certain methods of moving item can result in splitting a stack
 * into multiple slots. (e.g. shift-click).
 * toStartIdx and toEndIdx provide the min and max idxs moved into.
 * Return "preventChange" to prevent item movement.
 * @param {PlayerId} playerId - The id of the player moving the item
 * @param {number} fromIdx - The index that the item is being moved from
 * @param {number} toStartIdx - The start index that the item is being moved into
 * @param {number} toEndIdx - The end index that the item is being moved into
 * @param {number} amt - The amount of the item being moved
 */
onPlayerMoveInvenItem = (playerId, fromIdx, toStartIdx, toEndIdx, amt) => {}

/**
 * Called when a player moves an item into an index within a range of inventory slots
 * Return "preventChange" to prevent the movement
 * @param {PlayerId} playerId - The id of the player moving the item
 * @param {number} start - The start index of the range
 * @param {number} end - The end index of the range
 * @param {number} moveIdx - The index of the item being moved
 * @param {number} itemAmount - The amount of the item being moved
 */
onPlayerMoveItemIntoIdxs = (playerId, start, end, moveIdx, itemAmount) => {}

/**
 * Return "preventChange" to prevent the swap
 * @param {PlayerId} playerId - The id of the player swapping the inventory slots
 * @param {number} i - The index of the first slot
 * @param {number} j - The index of the second slot
 */
onPlayerSwapInvenSlots = (playerId, i, j) => {}

/**
 * Return "preventChange" to prevent the movement
 * @param {PlayerId} playerId - The id of the player moving the item
 * @param {number} i - The index of the first slot
 * @param {number} j - The index of the second slot
 * @param {number} amt - The amount of the item being moved
 */
onPlayerMoveInvenItemWithAmt = (playerId, i, j, amt) => {}

/**
 * Called when player alt actions (right click on pc).
 * The co-ordinates will be undefined if there is no targeted block (and block will be "Air")
 * Some actions can be prevented by returning "preventAction",
 * but this may not work as well for certain actions which the game client predicts to succeed -
 * test it to see if it works for your use case, feel free to report any broken ones.
 * @param {PlayerId} playerId - The id of the player attempting the alt action
 * @param {number} x - The x coordinate of the targeted block
 * @param {number} y - The y coordinate of the targeted block
 * @param {number} z - The z coordinate of the targeted block
 * @param {BlockName} block - The name of the targeted block
 * @param {EntityId | null} targetEId - The id of the targeted entity
 */
onPlayerAttemptAltAction = (playerId, x, y, z, block, targetEId) => {}

/**
 * Called when player completes an alt action (right click on pc).
 * The co-ordinates will be undefined if there is no targeted block (and block will be "Air")
 * @param {PlayerId} playerId - The id of the player completing the alt action
 * @param {number} x - The x coordinate of the targeted block
 * @param {number} y - The y coordinate of the targeted block
 * @param {number} z - The z coordinate of the targeted block
 * @param {BlockName} block - The name of the targeted block
 * @param {EntityId | null} targetEId - The id of the targeted entity
 */
onPlayerAltAction = (playerId, x, y, z, block, targetEId) => {}

/**
 * Called when a player clicks
 * Don't have important functionality depending on wasAltClick,
 * as it'll always be false for touchscreen players.
 * @param {PlayerId} playerId - The id of the player clicking
 * @param {boolean} wasAltClick - Whether the click was an alt click (e.g. right click
 */
onPlayerClick = (playerId, wasAltClick) => {}

/**
 * Called when a client option is updated
 * @param {PlayerId} playerId - The id of the player whose option was updated
 * @param {ClientOption} option - The option that was updated
 * @param {any} value - The new value of the option, always null for custom code
 */
onClientOptionUpdated = (playerId, option, value) => {}

/**
 * Called when a mob setting is updated
 * @param {MobId} mobId - The id of the mob whose setting was updated
 * @param {MobSetting} setting - The setting that was updated
 * @param {any} value - The new value of the setting
 */
onMobSettingUpdated = (mobId, setting, value) => {}

/**
 * Called when a player's inventory is updated
 * @param {PlayerId} playerId - The id of the player whose inventory was updated
 */
onInventoryUpdated = (playerId) => {}

/**
 * Called when a chest is updated by a player
 * x, y, z, will be null if isMoonstoneChest is true
 * @param {PlayerId} initiatorEId - The id of the player who updated the chest
 * @param {boolean} isMoonstoneChest - Whether the chest is a moonstone chest
 * @param {number | null} x - The x coordinate of the chest
 * @param {number | null} y - The y coordinate of the chest
 * @param {number | null} z - The z coordinate of the chest
 */
onChestUpdated = (initiatorEId, isMoonstoneChest, x, y, z) => {}

/**
 * Called when a block is changed in the world
 * initiatorDbId is null if updated by game code e.g. when a sapling grows
 * Return "preventChange" to prevent change
 * Return "preventDrop" to prevent a block item from dropping
 * @param {number} x - The x coordinate of the block
 * @param {number} y - The y coordinate of the block
 * @param {number} z - The z coordinate of the block
 * @param {BlockName} fromBlock - The old block that was replaced
 * @param {BlockName} toBlock - The new block that was placed
 * @param {string | null} initiatorDbId - The id of the player who updated the block
 * @param {WorldBlockChangedInfo} extraInfo - Extra info about the block change
 */
onWorldChangeBlock = (x, y, z, fromBlock, toBlock, initiatorDbId, extraInfo) => {}

/**
 * Called when a mesh entity is created
 * @param {EntityId} eId - The id of the mesh entity
 * @param {string} type - The type of mesh entity
 * @param {EntityId | null} initiatorId - The id of the entity that created the mesh entity, if any
 */
onCreateBloxdMeshEntity = (eId, type, initiatorId) => {}

/**
 * Called when a entity collides with another entity
 * @param {EntityId} eId - The id of the entity
 * @param {EntityId} otherEId - The id of the other entity
 */
onEntityCollision = (eId, otherEId) => {}

/**
 * Called when a player attempts to spawn a mob, e.g. using a spawn orb.
 * Return "preventSpawn" to prevent the mob from spawning.
 * @param {PlayerId} playerId - The id of the player
 * @param {MobType} mobType - The type of mob
 * @param {number} x - The potential x coordinate of the mob
 * @param {number} y - The potential y coordinate of the mob
 * @param {number} z - The potential z coordinate of the mob
 */
onPlayerAttemptSpawnMob = (playerId, mobType, x, y, z) => {}

/**
 * Called when the world attempts to spawn a mob.
 * Return "preventSpawn" to prevent the mob from spawning.
 * @param {MobType} mobType - The type of mob
 * @param {number} x - The potential x coordinate of the mob
 * @param {number} y - The potential y coordinate of the mob
 * @param {number} z - The potential z coordinate of the mob
 */
onWorldAttemptSpawnMob = (mobType, x, y, z) => {}

/**
 * Called when a mob is spawned by a player
 * @param {PlayerId} playerId - The id of the player who spawned the mob
 * @param {MobId} mobId - The id of the mob
 * @param {MobType} mobType - The type of mob
 * @param {number} x - The x coordinate of the mob
 * @param {number} y - The y coordinate of the mob
 * @param {number} z - The z coordinate of the mob
 * @param {MobHerdId} mobHerdId - The herd id of the mob
 * @param {boolean} playSoundOnSpawn - Whether to play a sound on spawn
 */
onPlayerSpawnMob = (playerId, mobId, mobType, x, y, z, mobHerdId, playSoundOnSpawn) => {}

/**
 * Called when a mob is spawned by the world
 * @param {MobId} mobId - The id of the mob
 * @param {MobType} mobType - The type of mob
 * @param {number} x - The x coordinate of the mob
 * @param {number} y - The y coordinate of the mob
 * @param {number} z - The z coordinate of the mob
 * @param {MobHerdId} mobHerdId - The herd id of the mob
 * @param {boolean} playSoundOnSpawn - Whether to play a sound on spawn
 */
onWorldSpawnMob = (mobId, mobType, x, y, z, mobHerdId, playSoundOnSpawn) => {}

/**
 * Called when a mob is despawned by the world.
 * Return "preventDespawn" to prevent the mob from despawning.
 * @param {MobId} mobId - The id of the mob despawned
 */
onWorldAttemptDespawnMob = (mobId) => {}

/**
 * Called when a mob is despawned
 * @param {MobId} mobId - The id of the mob despawned
 */
onMobDespawned = (mobId) => {}

/**
 * Called when a player attacks another player
 * @param playerId - The id of the player attacking
 */
onPlayerAttack = (playerId) => {}

/**
 * Called when a player is damaging another player
 * Return "preventDamage" to prevent damage
 * Return number to change damage dealt to that amount
 * Sometimes the damager will have left the game (e.g. spikes placer);
 * in this case, attackingPlayer will be the damagedPlayer,
 * but we pass damagerDbId for use cases where it's important.
 * @param {PlayerId} attackingPlayer - The id of the player attacking
 * @param {PlayerId} damagedPlayer - The id of the player being damaged
 * @param {number} damageDealt - The amount of damage dealt
 * @param {string} withItem - The item used to attack
 * @param {LifeformBodyPart} bodyPartHit - The body part hit
 * @param {PlayerDbId} damagerDbId - The database id of the player attacking
 */
onPlayerDamagingOtherPlayer = (attackingPlayer, damagedPlayer, damageDealt, withItem, bodyPartHit, damagerDbId) => {}

/**
 * Called when a player is damaging a mob
 * Return "preventDamage" to prevent damage
 * Return number to change damage dealt to that amount
 * @param {PlayerId} playerId - The id of the player damaging the mob
 * @param {MobId} mobId - The id of the mob being damaged
 * @param {number} damageDealt - The amount of damage dealt
 * @param {string} withItem - The item used to attack
 * @param {PlayerDbId} damagerDbId - The database id of the player attacking
 */
onPlayerDamagingMob = (playerId, mobId, damageDealt, withItem, damagerDbId) => {}

/**
 * Called when a mob is damaging a player
 * Return "preventDamage" to prevent damage
 * Return number to change damage dealt to that amount
 * @param {MobId} attackingMob
 * @param {PlayerId} damagedPlayer
 * @param {number} damageDealt
 * @param {string} withItem
 */
onMobDamagingPlayer = (attackingMob, damagedPlayer, damageDealt, withItem) => {}

/**
 * Called when a mob is damaging another mob
 * Return "preventDamage" to prevent damage
 * Return number to change damage dealt to that amount
 * @param {MobId} attackingMob
 * @param {MobId} damagedMob
 * @param {number} damageDealt
 * @param {string} withItem
 */
onMobDamagingOtherMob = (attackingMob, damagedMob, damageDealt, withItem) => {}

/**
 * Called when a player is about to be killed
 * Return "preventDeath" to prevent the player from being killed
 * @param {PlayerId} killedPlayer - The id of the player being killed
 * @param {LifeformId} [attackingLifeform] - The optional id of the lifeform attacking the player
 */
onAttemptKillPlayer = (killedPlayer, attackingLifeform) => {}

/**
 * Called when a player kills another player
 * Return "keepInventory" to not drop the player's inventory
 * @param attackingPlayer - The id of the player attacking
 * @param killedPlayer - The id of the player killed
 * @param damageDealt - The amount of damage dealt
 * @param withItem - The item used to attack
 */
onPlayerKilledOtherPlayer = (attackingPlayer, killedPlayer, damageDealt, withItem) => {}

/**
 * Called when a mob kills a player
 * Return "keepInventory" to not drop the player's inventory
 * @param attackingMob - The id of the mob attacking
 * @param killedPlayer - The id of the player killed
 * @param damageDealt - The amount of damage dealt
 * @param withItem - The item used to attack
 */
onMobKilledPlayer = (attackingMob, killedPlayer, damageDealt, withItem) => {}

/**
 * Called when a player kills a mob
 * Return "preventDrop" to prevent the mob from dropping items
 * @param {PlayerId} playerId - The id of the player killed
 * @param {MobId} mobId - The id of the mob that killed the player
 * @param {number} damageDealt - The amount of damage dealt
 * @param {string} withItem - The item used to attack
 */
onPlayerKilledMob = (playerId, mobId, damageDealt, withItem) => {}

/**
 * Called when a mob kills another mob
 * Return "preventDrop" to prevent the mob from dropping items
 * @param {MobId} attackingMob - The id of the mob attacking
 * @param {MobId} killedMob - The id of the mob killed
 * @param {number} damageDealt - The amount of damage dealt
 * @param {string} withItem - The item used to attack
 */
onMobKilledOtherMob = (attackingMob, killedMob, damageDealt, withItem) => {}

/**
 * Called when a player is affected by a new potion effect
 * @param initiatorId - The id of the player who initiated the potion effect
 * @param targetId - The id of the player who has started being affected
 * @param effectName - The name of the potion effect
 */
onPlayerPotionEffect = (initiatorId, targetId, effectName) => {}

/**
 * Called when a player is damaging a mesh entity
 * @param {PlayerId} playerId - The id of the player damaging the mesh entity
 * @param {EntityId} damagedId - The id of the mesh entity being damaged
 * @param {number} damageDealt - The amount of damage dealt
 * @param {string} withItem - The item used to attack
 */
onPlayerDamagingMeshEntity = (playerId, damagedId, damageDealt, withItem) => {}

/**
 * Called when a player breaks a mesh entity
 * @param {PlayerId} playerId - The id of the player breaking the mesh entity
 * @param {EntityId} entityId - The id of the mesh entity being broken
 */
onPlayerBreakMeshEntity = (playerId, entityId) => {}

/**
 * Called when a player uses a throwable item
 * @param {PlayerId} playerId - The id of the player using the throwable item
 * @param {ThrowableItem} throwableName - The name of the throwable item
 * @param {EntityId} thrownEntityId - The id of the projectile created by the player
 */
onPlayerUsedThrowable = (playerId, throwableName, thrownEntityId) => {}

/**
 * Called when a player's thrown projectile hits the terrain
 * @param {PlayerId} playerId - The id of the player that threw the throwable item
 * @param {ThrowableItem} throwableName - The name of the throwable item
 * @param {EntityId} thrownEntityId - The id of the entity which hit the terrain
 */
onPlayerThrowableHitTerrain = (playerId, throwableName, thrownEntityId) => {}

/**
 * Set client option `touchscreenActionButton` to take effect
 * Called when a player presses the touchscreen action button
 * Called for both touchDown and touchUp
 * @param {PlayerId} playerId - The id of the player pressing the touchscreen action button
 * @param {boolean} touchDown - Whether the touchscreen action button was pressed or released
 */
onTouchscreenActionButton = (playerId, touchDown) => {}

/**
 * Called when a player claims a task
 * @param playerId - The id of the player claiming the task
 * @param taskId - The id of the task being claimed
 * @param isPromoTask - Whether the task is a promo task
 * @param claimedRewards - The rewards claimed by the player
 */
onTaskClaimed = (playerId, taskId, isPromoTask, claimedRewards) => {}

/**
 * Called when a chunk is first loaded
 * API Methods that modify the chunk like setBlock cannot be used here to make
 * persisted changes, and will introduce client-server desync most cases,
 * but might have some creative uses if you know what you're doing.
 * For most use cases, consider using another callback e.g. tick.
 * @param {string} chunkId - The id of the chunk being loaded
 * @param {LoadedChunk} chunk - The chunk being loaded, which can be modified by this callback
 * @param {boolean} wasPersistedChunk - Whether the chunk was persisted
 */
onChunkLoaded = (chunkId, chunk, wasPersistedChunk) => {}

/**
 * Called when a player requests a chunk
 * @param {PlayerId} playerId - The id of the player requesting the chunk
 * @param {number} chunkX - The x coordinate of the chunk being requested
 * @param {number} chunkY - The y coordinate of the chunk being requested
 * @param {number} chunkZ - The z coordinate of the chunk being requested
 * @param {string} chunkId - The id of the chunk being requested
 */
onPlayerRequestChunk = (playerId, chunkX, chunkY, chunkZ, chunkId) => {}

/**
 * Called when an item drop is created
 * @param {EntityId} itemEId - The id of the item drop
 * @param {string} itemName - The name of the item dropped
 * @param {number} itemAmount - The amount dropped
 * @param {number} x - The x coordinate of the item drop
 * @param {number} y - The y coordinate of the item drop
 * @param {number} z - The z coordinate of the item drop
 */
onItemDropCreated = (itemEId, itemName, itemAmount, x, y, z) => {}

/**
 * Called when a player starts charging an item
 * @param {PlayerId} playerId - The id of the player charging the item
 * @param {string} itemName - The name of the item being charged
 */
onPlayerStartChargingItem = (playerId, itemName) => {}

/**
 * Called when a player finishes charging an item
 * @param {PlayerId} playerId - The id of the player charging the item
 * @param {boolean} used - Whether the item was used
 * @param {string} itemName - The name of the charged item
 * @param {number} duration - The duration of the charge
 */
onPlayerFinishChargingItem = (playerId, used, itemName, duration) => {}

/**
 * Called every so often.
 * You should save custom db values/s3 objects here.
 * Persisted items ARE saved on graceful shutdown (e.g. uncaught error, update, etc),
 * but this helps prevent large data-loss on non-graceful shutdowns.
 */
doPeriodicSave = () => {}
```
