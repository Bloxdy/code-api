# Callbacks

Players can use World Code in custom worlds get functions they've written to run when game events happen. These special functions are called callbacks. The world code can be viewed by pressing F8 by default. Initially the world code will have this comment, which can be removed:

```text
tick onClose onPlayerJoin onPlayerLeave onPlayerJump onRespawnRequest
playerCommand onPlayerChat onPlayerChangeBlock onPlayerDropItem
onPlayerPickedUpItem onPlayerSelectInventorySlot onBlockStand
onPlayerAttemptCraft onPlayerCraft onPlayerAttemptOpenChest
onPlayerOpenedChest onPlayerMoveItemOutOfInventory onPlayerMoveInvenItem
onPlayerMoveItemIntoIdxs onPlayerSwapInvenSlots onPlayerMoveInvenItemWithAmt
onPlayerAttemptAltAction onPlayerAltAction onPlayerClick onPlayerClickUp
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
onPlayerStartChargingItem onPlayerFinishChargingItem onPlayerFinishQTE
onPlayerBoughtShopItem doPeriodicSave

To use a callback, just assign a function to it in the world code!
tick = () => {}			 or			 function tick() {}
```

You can use `api.setCallbackValueFallback("callbackName", defaultValue)` to set a default value to be returned by your callback code if it throws an error.

## doPeriodicSave
Called every so often.

You should save custom db values/s3 objects here.

Persisted items ARE saved on graceful shutdown (e.g. uncaught error, update, etc),

but this helps prevent large data-loss on non-graceful shutdowns.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|



    

 



## onAttemptKillPlayer
Called when a player is about to be killed

Return "preventDeath" to prevent the player from being killed

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| killedPlayer | `PlayerId` | The id of the player being killed |
| attackingLifeform | `LifeformId` | The optional id of the lifeform attacking the player |


### Returns:
`void \| "preventDeath"`

 




    

 



## onBlockStand
Called when a player stands on a block

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player that stood on the block |
| x | `number` | The x coordinate of the block that was stood on |
| y | `number` | The y coordinate of the block that was stood on |
| z | `number` | The z coordinate of the block that was stood on |
| blockName | `BlockName` | The name of the block that was stood on |


    

 



## onChestUpdated
Called when a chest is updated by a player

x, y, z, will be null if isMoonstoneChest is true

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| initiatorEId | `PlayerId` | The id of the player who updated the chest |
| isMoonstoneChest | `boolean` | Whether the chest is a moonstone chest |
| x | `number \| null` | The x coordinate of the chest |
| y | `number \| null` | The y coordinate of the chest |
| z | `number \| null` | The z coordinate of the chest |


    

 



## onChunkLoaded
Called when a chunk is first loaded

API Methods that modify the chunk like setBlock cannot be used here to make

persisted changes, and will introduce client-server desync most cases,

but might have some creative uses if you know what you're doing.

For most use cases, consider using another callback e.g. tick.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| chunkId | `string` | The id of the chunk being loaded |
| chunk | `LoadedChunk` | The chunk being loaded, which can be modified by this callback  * For world code callbacks this value will always be null. |
| wasPersistedChunk | `boolean` | Whether the chunk was persisted |


    

 



## onClientOptionUpdated
Called when a client option is updated

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player whose option was updated |
| option | `ClientOption` | The option that was updated |
| value | `any` | The new value of the option, always null for custom code |


    

 



## onClose
Called when the lobby is shutting down

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| serverIsShuttingDown | `boolean` | Whether the server is shutting down |


    

 



## onCreateBloxdMeshEntity
Called when a mesh entity is created

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| eId | `EntityId` | The id of the mesh entity |
| type | `string` | The type of mesh entity |
| initiatorId | `EntityId \| null` | The id of the entity that created the mesh entity, if any |


    

 



## onEntityCollision
Called when a entity collides with another entity

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| eId | `EntityId` | The id of the entity |
| otherEId | `EntityId` | The id of the other entity |


    

 



## onInventoryUpdated
Called when a player's inventory is updated

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player whose inventory was updated |


    

 



## onItemDropCreated
Called when an item drop is created

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| itemEId | `EntityId` | The id of the item drop |
| itemName | `string` | The name of the item dropped |
| itemAmount | `number` | The amount dropped |
| x | `number` | The x coordinate of the item drop |
| y | `number` | The y coordinate of the item drop |
| z | `number` | The z coordinate of the item drop |


    

 



## onMobDamagingOtherMob
Called when a mob is damaging another mob

Return "preventDamage" to prevent damage

Return number to change damage dealt to that amount

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| attackingMob | `MobId` | the id of the mob attacking |
| damagedMob | `MobId` | the id of the mob being damaged |
| damageDealt | `number` | the amount of damage dealt |
| withItem | `string` | the item used to attack |


### Returns:
`number \| void \| "preventDamage"`

 




    

 



## onMobDamagingPlayer
Called when a mob is damaging a player

Return "preventDamage" to prevent damage

Return number to change damage dealt to that amount

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| attackingMob | `MobId` | the id of the mob damaging the player |
| damagedPlayer | `PlayerId` | the id of the player being damaged |
| damageDealt | `number` | the amount of damage dealt |
| withItem | `string` | the item used to attack |


### Returns:
`number \| void \| "preventDamage"`

 




    

 



## onMobDespawned
Called when a mob is despawned

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| mobId | `MobId` | The id of the mob despawned |


    

 



## onMobKilledOtherMob
Called when a mob kills another mob

Return "preventDrop" to prevent the mob from dropping items

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| attackingMob | `MobId` | The id of the mob attacking |
| killedMob | `MobId` | The id of the mob killed |
| damageDealt | `number` | The amount of damage dealt |
| withItem | `string` | The item used to attack |


### Returns:
`void \| "preventDrop"`

 




    

 



## onMobKilledPlayer
Called when a mob kills a player

Return "keepInventory" to not drop the player's inventory

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| attackingMob | `any` | The id of the mob attacking |
| killedPlayer | `any` | The id of the player killed |
| damageDealt | `any` | The amount of damage dealt |
| withItem | `any` | The item used to attack |


### Returns:
`void \| "keepInventory"`

 




    

 



## onMobSettingUpdated
Called when a mob setting is updated

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| mobId | `MobId` | The id of the mob whose setting was updated |
| setting | `MobSetting` | The setting that was updated |
| value | `any` | The new value of the setting |


    

 



## onPlayerAltAction
Called when player completes an alt action (right click on pc).

The co-ordinates will be undefined if there is no targeted block (and block will be "Air")

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player completing the alt action |
| x | `number` | The x coordinate of the targeted block |
| y | `number` | The y coordinate of the targeted block |
| z | `number` | The z coordinate of the targeted block |
| block | `BlockName` | The name of the targeted block |
| targetEId | `EntityId \| null` | The id of the targeted entity |


    

 



## onPlayerAttack
Called when a player attacks another player

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `string` | The id of the player attacking |


    

 



## onPlayerAttemptAltAction
Called when player alt actions (right click on pc).

The co-ordinates will be undefined if there is no targeted block (and block will be "Air")

Some actions can be prevented by returning "preventAction",

but this may not work as well for certain actions which the game client predicts to succeed -

test it to see if it works for your use case, feel free to report any broken ones.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player attempting the alt action |
| x | `number` | The x coordinate of the targeted block |
| y | `number` | The y coordinate of the targeted block |
| z | `number` | The z coordinate of the targeted block |
| block | `BlockName` | The name of the targeted block |
| targetEId | `EntityId \| null` | The id of the targeted entity |


### Returns:
`void \| "preventAction"`

 




    

 



## onPlayerAttemptCraft
Called when a player attempts to craft an item

Return "preventCraft" to prevent a craft from happening

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player that is attempting to craft the item |
| itemName | `string` | The name of the item that is being crafted |
| craftingIdx | `number` | The index of the used recipe in the item's recipe list |
| craftTimes | `number` | The number of times the craft recipe is used at once (e.g. shift held while crafting) |


### Returns:
`void \| "preventCraft"`

 




    

 



## onPlayerAttemptOpenChest
Called when a player attempts to open a chest

Return "preventOpen" to prevent the player from opening the chest

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player that is attempting to open the chest |
| x | `number` | The x coordinate of the chest that the player is attempting to open |
| y | `number` | The y coordinate of the chest that the player is attempting to open |
| z | `number` | The z coordinate of the chest that the player is attempting to open |
| isMoonstoneChest | `boolean` | Whether the chest is a moonstone chest |
| isIronChest | `boolean` | Whether the chest is an iron chest |


### Returns:
`void \| "preventOpen"`

 




    

 



## onPlayerAttemptSpawnMob
Called when a player attempts to spawn a mob, e.g. using a spawn orb.

Return "preventSpawn" to prevent the mob from spawning.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player |
| mobType | `MobType` | The type of mob |
| x | `number` | The potential x coordinate of the mob |
| y | `number` | The potential y coordinate of the mob |
| z | `number` | The potential z coordinate of the mob |


### Returns:
`void \| "preventSpawn"`

 




    

 



## onPlayerBoughtShopItem
Called after a player successfully buys a shop item

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player that bought the item |
| categoryKey | `ShopCategoryKey` | The shop category key |
| itemKey | `ShopItemKey` | The shop item key |
| item | `BoughtShopItem` | The resolved shop item (with per-player overrides applied, internal properties stripped) |
| userInput | `string` | The user input provided, if the item has a userInput config |


    

 



## onPlayerBreakMeshEntity
Called when a player breaks a mesh entity

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player breaking the mesh entity |
| entityId | `EntityId` | The id of the mesh entity being broken |


    

 



## onPlayerChangeBlock
Called when a player changes a block

Return "preventChange" to prevent the change.

If player places block, fromBlock will be Air (and toBlock the block).

If a player breaks a block, toBlock will be Air.

Return "preventDrop" to prevent a block item from dropping.

Return an array to set the dropped item position.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player that changed the block |
| x | `number` | The x coordinate of the block that was changed |
| y | `number` | The y coordinate of the block that was changed |
| z | `number` | The z coordinate of the block that was changed |
| fromBlock | `BlockName` | The old block that was replaced |
| toBlock | `BlockName` | The new block that was placed |
| droppedItem | `BlockName \| null` | The item that was dropped |
| fromBlockInfo | `MultiBlockInfo` | The info of the old block that was replaced |
| toBlockInfo | `MultiBlockInfo` | The info of the new block that was placed |


### Returns:
`void \| [number, number, number] \| "preventChange" \| "preventDrop"`

 




    

 



## onPlayerChat
Called when a player sends a chat message

Return false or null to prevent the broadcast of the message.

Return a string or CustomTextStyling to add a prefix to message.

Return for most flexibility: an object where keys are playerIds -

the value for a playerId being false means that player won't receive the message.

Otherwise playerId values should be an object with (optional) keys

prefixContent and chatContent to modify the prefix and the chat.

CustomTextStyling[] prefixContent is expected, e.g. [["prefix"]] or [[{ str: "prefix" }]].

World code is not permitted to specify chatContent, it will be ignored.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player that sent the message |
| chatMessage | `string` | The message that the player sent |
| channelName | `string` | The name of the channel that the message was sent in |


### Returns:
`boolean \| void \| ChatTags \| OnPlayerChatObjectResponse`

 




    

 



## onPlayerClick
Called when a player clicks

Don't have important functionality depending on wasAltClick,

as it'll always be false for touchscreen players.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player clicking |
| wasAltClick | `boolean` | Whether the click was an alt click (e.g. right click |
| x | `number` |  |
| y | `number` |  |
| z | `number` |  |
| block | `BlockName` |  |
| targetEId | `EntityId \| null` |  |


    

 



## onPlayerClickUp
Called when a player releases a click (mouse-up on desktop, touch-end on mobile).

Fires for both primary and secondary click releases.

Keep in mind wasAltClick will always be false for touchscreen players.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player releasing click |
| wasAltClick | `boolean` | Whether the released click was an alt click (e.g. right click |
| x | `number` |  |
| y | `number` |  |
| z | `number` |  |
| block | `BlockName` |  |
| targetEId | `EntityId \| null` |  |


    

 



## onPlayerCraft
Called when a player crafts an item

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player that crafted the item |
| itemName | `string` | The name of the item that was crafted |
| craftingIdx | `number` | The index of the used recipe in the item's recipe list |
| recipe | `RecipesForItem[number]` | The recipe that was used to craft the item |
| craftTimes | `number` | The number of times the craft recipe is used at once (e.g. shift held while crafting) |


    

 



## onPlayerDamagingMeshEntity
Called when a player is damaging a mesh entity

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player damaging the mesh entity |
| damagedId | `EntityId` | The id of the mesh entity being damaged |
| damageDealt | `number` | The amount of damage dealt |
| withItem | `string` | The item used to attack |


    

 



## onPlayerDamagingMob
Called when a player is damaging a mob

Return "preventDamage" to prevent damage

Return number to change damage dealt to that amount

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player damaging the mob |
| mobId | `MobId` | The id of the mob being damaged |
| damageDealt | `number` | The amount of damage dealt |
| withItem | `string` | The item used to attack |
| damagerDbId | `PlayerDbId` | The database id of the player attacking |


### Returns:
`number \| void \| "preventDamage"`

 




    

 



## onPlayerDamagingOtherPlayer
Called when a player is damaging another player

Return "preventDamage" to prevent damage

Return number to change damage dealt to that amount

Sometimes the damager will have left the game (e.g. spikes placer);

in this case, attackingPlayer will be the damagedPlayer,

but we pass damagerDbId for use cases where it's important.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| attackingPlayer | `PlayerId` | The id of the player attacking |
| damagedPlayer | `PlayerId` | The id of the player being damaged |
| damageDealt | `number` | The amount of damage dealt |
| withItem | `string` | The item used to attack |
| bodyPartHit | `LifeformBodyPart` | The body part hit |
| damagerDbId | `PlayerDbId` | The database id of the player attacking |


### Returns:
`number \| void \| "preventDamage"`

 




    

 



## onPlayerDropItem
Called when a player drops an item

Return "preventDrop" to prevent the player from dropping the item at all.

Return "allowButNoDroppedItemCreated" to allow discarding items without dropping them.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player that dropped the item |
| x | `number` | The x coordinate of the item that was dropped |
| y | `number` | The y coordinate of the item that was dropped |
| z | `number` | The z coordinate of the item that was dropped |
| itemName | `ItemName` | The name of the item that was dropped |
| itemAmount | `number` | The amount of the item that was dropped |
| fromIdx | `number` | The index of the item that was dropped from the player's inventory |


### Returns:
`void \| "preventDrop" \| "allowButNoDroppedItemCreated"`

 




    

 



## onPlayerFinishChargingItem
Called when a player finishes charging an item

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player charging the item |
| used | `boolean` | Whether the item was used |
| itemName | `string` | The name of the charged item |
| duration | `number` | The duration of the charge |


    

 



## onPlayerFinishQTE


### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` |  |
| qteId | `QTERequestId` |  |
| result | `boolean` |  |


    

 



## onPlayerJoin
Called when a player joins the lobby

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `string` | The id of the player that joined |
| fromGameReset | `boolean` | Whether this call is from a game reset (used by SessionBasedGame) |


    

 



## onPlayerJump
Called when a player jumps

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `string` | The id of the player that jumped |


    

 



## onPlayerKilledMob
Called when a player kills a mob

Return "preventDrop" to prevent the mob from dropping items

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player killed |
| mobId | `MobId` | The id of the mob that killed the player |
| damageDealt | `number` | The amount of damage dealt |
| withItem | `string` | The item used to attack |


### Returns:
`void \| "preventDrop"`

 




    

 



## onPlayerKilledOtherPlayer
Called when a player kills another player

Return "keepInventory" to not drop the player's inventory

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| attackingPlayer | `string` | The id of the player attacking |
| killedPlayer | `string` | The id of the player killed |
| damageDealt | `number` | The amount of damage dealt |
| withItem | `string` | The item used to attack |


### Returns:
`void \| "keepInventory"`

 




    

 



## onPlayerLeave
Called when a player leaves the lobby

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `string` | The id of the player that left |
| serverIsShuttingDown | `boolean` | Whether the server is shutting down |


    

 



## onPlayerMoveInvenItem
Called for all types of inventory item movement.

Certain methods of moving item can result in splitting a stack

into multiple slots. (e.g. shift-click).

toStartIdx and toEndIdx provide the min and max idxs moved into.

Return "preventChange" to prevent item movement.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player moving the item |
| fromIdx | `number` | The index that the item is being moved from |
| toStartIdx | `number` | The start index that the item is being moved into |
| toEndIdx | `number` | The end index that the item is being moved into |
| amt | `number` | The amount of the item being moved |


### Returns:
`void \| "preventChange"`

 




    

 



## onPlayerMoveInvenItemWithAmt
Return "preventChange" to prevent the movement

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player moving the item |
| i | `number` | The index of the first slot |
| j | `number` | The index of the second slot |
| amt | `number` | The amount of the item being moved |


### Returns:
`void \| "preventChange"`

 




    

 



## onPlayerMoveItemIntoIdxs
Called when a player moves an item into an index within a range of inventory slots

Return "preventChange" to prevent the movement

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player moving the item |
| start | `number` | The start index of the range |
| end | `number` | The end index of the range |
| moveIdx | `number` | The index of the item being moved |
| itemAmount | `number` | The amount of the item being moved |


### Returns:
`void \| "preventChange"`

 




    

 



## onPlayerMoveItemOutOfInventory
Called when a player moves an item out of their inventory

Return "preventChange" to prevent the movement

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player moving the item |
| itemName | `string` | The name of the item being moved |
| itemAmount | `number` | The amount of the item being moved |
| fromIdx | `number` | The index which the item is being moved from |
| movementType | `string` | The type of movement that occurred |


### Returns:
`void \| "preventChange"`

 




    

 



## onPlayerOpenedChest
Called when a player opens a chest

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player that opened the chest |
| x | `number` | The x coordinate of the chest that was opened |
| y | `number` | The y coordinate of the chest that was opened |
| z | `number` | The z coordinate of the chest that was opened |
| isMoonstoneChest | `boolean` | Whether the chest is a moonstone chest |
| isIronChest | `boolean` | Whether the chest is an iron chest |


    

 



## onPlayerPickedUpItem
Called when a player picks up an item

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player that picked up the item |
| itemName | `string` | The name of the item that was picked up |
| itemAmount | `number` | The amount of the item that was picked up |


    

 



## onPlayerPotionEffect
Called when a player is affected by a new potion effect

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| initiatorId | `string` | The id of the player who initiated the potion effect |
| targetId | `string` | The id of the player who has started being affected |
| effectName | `"Damage" \| "Speed" \| "Damage Reduction" \| "Invisible" \| "Jump Boost" \| "Knockback" \| "Poisoned" \| "Slowness" \| "Weakness" \| "Cleansed" \| "Instant Damage" \| "Health Regen" \| "Instant Health" \| "Haste" \| "Shield" \| "Double Jump" \| "Heat Resistance" \| "Thief" \| "X-Ray Vision" \| "Mining Yield" \| "Brain Rot" \| "Aura" \| "Wall Climbing" \| "Air Walk" \| "Pickpocketer" \| "Lifesteal" \| "Bounciness" \| "Blindness" \| "Poopy"` | The name of the potion effect |


### Returns:
`void \| "preventEffect"`

 




    

 



## onPlayerRequestChunk
Called when a player requests a chunk

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player requesting the chunk |
| chunkX | `number` | The x coordinate of the chunk being requested |
| chunkY | `number` | The y coordinate of the chunk being requested |
| chunkZ | `number` | The z coordinate of the chunk being requested |
| chunkId | `string` | The id of the chunk being requested |


    

 



## onPlayerSelectInventorySlot
Called when a player selects a different inventory slot.

This will be called eventually when you have already set the slot using

api.setSelectedInventorySlotI so be careful not to cause an infinite loop doing this.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player that selected the inventory slot |
| slotIndex | `number` | The index of the inventory slot that was selected |


    

 



## onPlayerSpawnMob
Called when a mob is spawned by a player

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player who spawned the mob |
| mobId | `MobId` | The id of the mob |
| mobType | `MobType` | The type of mob |
| x | `number` | The x coordinate of the mob |
| y | `number` | The y coordinate of the mob |
| z | `number` | The z coordinate of the mob |
| mobHerdId | `MobHerdId` | The herd id of the mob |
| playSoundOnSpawn | `boolean` | Whether to play a sound on spawn |


    

 



## onPlayerStartChargingItem
Called when a player starts charging an item

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player charging the item |
| itemName | `string` | The name of the item being charged |


### Returns:
`void \| "preventCharge"`

 




    

 



## onPlayerSwapInvenSlots
Return "preventChange" to prevent the swap

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player swapping the inventory slots |
| i | `number` | The index of the first slot |
| j | `number` | The index of the second slot |


### Returns:
`void \| "preventChange"`

 




    

 



## onPlayerThrowableHitTerrain
Called when a player's thrown projectile hits the terrain

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player that threw the throwable item |
| throwableName | `ThrowableItem` | The name of the throwable item |
| thrownEntityId | `EntityId` | The id of the entity which hit the terrain |


    

 



## onPlayerUsedThrowable
Called when a player uses a throwable item

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player using the throwable item |
| throwableName | `ThrowableItem` | The name of the throwable item |
| thrownEntityId | `EntityId` | The id of the projectile created by the player |


    

 



## onRespawnRequest
Called when a player requests to respawn.

Optionally return the respawn location. Defaults to [0, 0, 0].

Return true to handle yourself (good for async,

but be careful that the player isn't at the place they died,

as they could pick up their old items or hit the player they were fighting).

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `string` | The id of the player that requested to respawn |


### Returns:
`true \| void \| number[]`

 




    

 



## onTaskClaimed
Called when a player claims a task

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `string` | The id of the player claiming the task |
| taskId | `any` | The id of the task being claimed |
| isPromoTask | `any` | Whether the task is a promo task |
| claimedRewards | `any` | The rewards claimed by the player |


### Returns:
`any`

 




    

 



## onTouchscreenActionButton
Set client option `touchscreenActionButton` to take effect

Called when a player presses the touchscreen action button

Called for both touchDown and touchUp

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `PlayerId` | The id of the player pressing the touchscreen action button |
| touchDown | `boolean` | Whether the touchscreen action button was pressed or released |


    

 



## onWorldAttemptDespawnMob
Called when a mob is despawned by the world.

Return "preventDespawn" to prevent the mob from despawning.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| mobId | `MobId` | The id of the mob despawned |


### Returns:
`void \| "preventDespawn"`

 




    

 



## onWorldAttemptSpawnMob
Called when the world attempts to spawn a mob.

Return "preventSpawn" to prevent the mob from spawning.

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| mobType | `MobType` | The type of mob |
| x | `number` | The potential x coordinate of the mob |
| y | `number` | The potential y coordinate of the mob |
| z | `number` | The potential z coordinate of the mob |


### Returns:
`void \| "preventSpawn"`

 




    

 



## onWorldChangeBlock
Called when a block is changed in the world

initiatorDbId is null if updated by game code e.g. when a sapling grows

Return "preventChange" to prevent change

Return "preventDrop" to prevent a block item from dropping

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| x | `number` | The x coordinate of the block |
| y | `number` | The y coordinate of the block |
| z | `number` | The z coordinate of the block |
| fromBlock | `BlockName` | The old block that was replaced |
| toBlock | `BlockName` | The new block that was placed |
| initiatorDbId | `string \| null` | The id of the player who updated the block |
| extraInfo | `WorldBlockChangedInfo` | Extra info about the block change |


### Returns:
`void \| "preventChange" \| "preventDrop"`

 




    

 



## onWorldSpawnMob
Called when a mob is spawned by the world

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| mobId | `MobId` | The id of the mob |
| mobType | `MobType` | The type of mob |
| x | `number` | The x coordinate of the mob |
| y | `number` | The y coordinate of the mob |
| z | `number` | The z coordinate of the mob |
| mobHerdId | `MobHerdId` | The herd id of the mob |
| playSoundOnSpawn | `boolean` | Whether to play a sound on spawn |


    

 



## playerCommand
Called when a player sends a command

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| playerId | `string` | The id of the player that sent the command |
| command | `string` | The command that the player sent |


### Returns:
`boolean`

 




    

 



## tick
Called every tick, 20 times per second

### Parameters:
| Parameter | Type | Description |
|-----------|------|-------------|
| ms | `number` | The fixed timestep, can be used as "milliseconds since last tick" |


    

 

