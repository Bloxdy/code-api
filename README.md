# Code API

You can run javascript when right clicking code blocks and press to code boards.
This is only available to owners of worlds lobbies.
The javascript can interact with the Bloxd.io game api.

Please use [our discord](https://discord.gg/vwMp5y25RX) to report any issues you come across or features you'd like to see added.

## Code Blocks

- World owners can find these by searching in the creative menu
- No need to add `press to code`, this text is only needed for code boards, and will automatically be removed
- If you want to run code without opening the code editor, you can trigger the code block by right clicking an adjacent `press to code` board instead

## Boards

- You can begin a board with `press to code` to run javascript when you right click it.
- Normally you can't edit a code board after placing it, but you can currently work around this by putting a space before `press to code`.
- Boards only allow for a small amount of text, we recommend you use Code Blocks instead, or you can work around this by using multiple boards

## Notes

- Global variable `myId` stores the PlayerID of who is running the code.
- Global variable `thisPos` stores the position of the currently executing code block or press to code board.
- You can use `api.log` or `console.log` for printing and debugging (they do the same thing).
- You can use `Date.now()` instead of `api.now()` if you prefer, both return the time in milliseconds.
- Comments like `/* comment */` work, but comments like `// comment` don't work right now.

## Examples

Code Block to make the player jump:

```ts
f = api.setVelocity(myId, 0, 9, 0)
```

Push the player

```ts
api.applyImpulse(myId, 9, 0, 9)
```

Send an orange message to yourself:

```ts
api.sendMessage(myId, "text", { color: "orange" })
```

Create flying text:

```ts
const speed = 100
api.sendFlyingMiddleMessage(myId, ["Message to display"], speed)
```

Send a message to all players:

```ts
api.broadcastMessage("announcement", { color: "red" })
```

Set player health to 99, and print the old health:

```ts
const oldHealth = api.getHealth(myId)
api.setHealth(myId, 99)
api.log("Old Health:", oldHealth)
```

Define a function to get the player IDs excluding your own ID:

```ts
getOtherIds = () => {
    const ids = api.getPlayerIds()
    const otherIds = []
    for (const id of ids) {
        if (id !== myId) {
            otherIds.push(id)
        }
    }
    return otherIds
}
```

Use the function above to make other players look like zombies:

```ts
for (const otherId of getOtherIds()) {
    api.setPlayerPose(otherId, "zombie")
    for (const part of ["head", "body", "legs"]) {
        /* Notice the skin texture uses a capital Z */
        api.changePlayerIntoSkin(otherId, part, "Zombie")
    }
}
```

Make all players look like floating wizards:

```ts
for (const playerId of api.getPlayerIds()) {
    api.setPlayerPose(playerId, "driving")
    for (const part of ["head", "body", "legs"]) {
        /* Notice the skin texture uses a capital W */
        api.changePlayerIntoSkin(playerId, part, "Wizard")
    }
}
```

## API

Global object `api` has the following methods:

```ts
/**
 * Get position of a player / entity.
 * @param entityId
 */
getPosition(entityId: EntityId): [number, number, number]

/**
 * Set position of a player / entity.
 * @param entityId
 * @param x Can also be an array, in which case y and z shouldn't be passed
 * @param y
 * @param z
 */
setPosition(entityId: EntityId, x: number | number[], y?: number, z?: number): void

/**
 * Get all the player ids.
 */
getPlayerIds(): PlayerId[]

/**
 * Whether a player is currently in the game
 *
 * @param playerId
 */
playerIsInGame(playerId: PlayerId): boolean

/**
 *
 * @param playerId
 * @returns
 */
playerIsLoggedIn(playerId: PlayerId): boolean

/**
 * Returns the party that the player was in when they joined the game. The returned object contains the playerDbIds, as well
 * as the playerIds if available, of the party leader and members.
 *
 * @param playerId
 *
 * @returns
 */
getPlayerPartyWhenJoined(playerId: PlayerId): PNull<{ playerDbIds: PlayerDbId[] }>

/**
 * Get the number of players in the room
 */
getNumPlayers(): number

/**
 * Get the co-ordinates of the blocks the player is standing on as a list. For example, if the center of the player is at 0,0,0
 * this function will return [[0, -1, 0], [-1, -1, 0], [0, -1, -1], [-1, -1, -1]]
 * If the player is just standing on one block, the function would return e.g. [[0, 0, 0]]
 * If the player is middair then returns an empty list [].
 *
 * @param playerId
 */
getBlockCoordinatesPlayerStandingOn(playerId: PlayerId): number[][]

/**
 * Get the types of block the player is standing on
 * For example, if a player is standing on 4 dirt blocks, this will return ["Dirt", "Dirt", "Dirt", "Dirt"]
 * @param playerId
 */
getBlockTypesPlayerStandingOn(playerId: PlayerId): any[]

/**
 * Get the up to 12 unit co-ordinates the lifeform is located within
 * (A lifeform is modelled as having four corners and can be in up to 3 blocks vertically)
 *
 * @param lifeformId
 * @returns List of x, y, z positions e.g. [[-1, 0, 0], [-1, 1, 0], [-1, 2, 0]]
 */
getUnitCoordinatesLifeformWithin(lifeformId: LifeformId): number[][]

/**
 * Show the shop tutorial for a player. Will not be shown if they have ever seen the shop tutorial in your game before.
 * @param playerId
 */
showShopTutorial(playerId: PlayerId): void

/**
 * Get the current shield of an entity.
 * @param entityId
 */
getShieldAmount(entityId: EntityId): number

/**
 * Set the current shield of a lifeform.
 *
 * @param lifeformId
 * @param newShieldAmount
 */
setShieldAmount(lifeformId: LifeformId, newShieldAmount: number): void

/**
 * Get the current health of an entity.
 * @param entityId
 */
getHealth(entityId: PlayerId): number

/**
 *
 * @param lifeformId
 * @param changeAmount Must be an integer. A positive amount will increase the entity's health. A negative amount will decrease the entity's shield first, then their health.
 * @param whoDidDamage Optional - If damage done by another player
 * @param broadcastLifeformHurt
 *
 * @return Whether the entity was killed
 */
applyHealthChange(lifeformId: LifeformId, changeAmount: number, whoDidDamage?: LifeformId | { lifeformId: LifeformId; withItem: string }, broadcastLifeformHurt = true): boolean

/**
 * Set the current health of an entity.
 * If you want to set their health to more than their current max health, the optional increaseMaxHealthIfNeeded must be true.
 *
 * @param entityId
 * @param newHealth Can be null to make the player not have health
 * @param whoDidDamage Optional
 * @param increaseMaxHealthIfNeeded Optional
 *
 * @return Whether this change in health killed the player
 */
setHealth(entityId: EntityId, newHealth: PNull<number>, whoDidDamage?: LifeformId | { lifeformId: LifeformId; withItem: string }, increaseMaxHealthIfNeeded = false): boolean

/**
 * Make it as if hittingEId hit hitEId
 *
 * @param hittingEId
 * @param hitEId
 * @param dirFacing
 * @param bodyPartHit
 */
applyMeleeHit(hittingEId: PlayerId, hitEId: PlayerId, dirFacing: number[], bodyPartHit?: PNull<PlayerBodyPart>): void

/**
 * Apply damage to a lifeform.
 * eId is the player initiating the damage, hitEId is the lifeform being hit.
 *
 * It is recommended to self-inflict damage when the game code wants to apply damage to a lifeform.
 *
 * @param eId
 * @param hitEId
 * @param attemptedDmgAmt
 * @param withItem
 * @param bodyPartHit
 * @param attackDir
 * @param showCritParticles
 * @param reduceVerticalKbVelocity
 * @param broadcastEntityHurt
 * @param attackCooldownSettings
 * @param hittingSoundOverride
 * @param ignoreOtherEntitySettingCanAttack
 * @param isTrueDamage
 * @param damagerDbId
 *
 * @returns whether the attack damaged the lifeform
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
    broadcastEntityHurt = true,
    attackCooldownSettings = null,
    hittingSoundOverride = null,
    ignoreOtherEntitySettingCanAttack = false,
    isTrueDamage = false,
    damagerDbId = null,
}: PlayerAttemptDamageOtherPlayerOpts): boolean

/**
 * Force respawn a player
 * @param playerId
 * @param respawnPos
 */
forceRespawn(playerId: PlayerId, respawnPos?: number[]): void

/**
 * Kill a lifeform.
 * @param lifeformId
 * @param whoKilled Optional
 */
killLifeform(lifeformId: LifeformId, whoKilled?: LifeformId | { lifeformId: LifeformId; withItem: string }): void

/**
 * Gets the player's current killstreak
 *
 * @param playerId
 * @returns
 */
getCurrentKillstreak(playerId: PlayerId): any

/**
 * Clears the player's current killstreak
 *
 * @param playerId
 */
clearKillstreak(playerId: PlayerId): void

/**
 * Whether a lifeform is alive or dead (or on the respawn screen, in a player's case).
 *
 * @param lifeformId
 * @returns
 */
isAlive(lifeformId: LifeformId): boolean

/**
 * Send a message to everyone
 *
 * @param message The text contained within the message. Can use `Custom Text Styling`.
 * @param style An optional style argument. Can contain values for fontWeight and color of the message.
 *          style is ignored if message uses custom text styling (i.e. is not a string)
 */
broadcastMessage(message: string | CustomTextStyling, style?: { fontWeight?: number | string; color?: string }): void

/**
 * Send a message to a specific player
 *
 * @param playerId Id of the player
 * @param message The text contained within the message. Can use `Custom Text Styling`.
 * @param style An optional style argument. Can contain values for fontWeight and color of the message.
 *              style is ignored if message uses custom text styling (i.e. is not a string)
 */
sendMessage(playerId: PlayerId, message: string | CustomTextStyling, style?: { fontWeight?: number | string; color?: string }): void

/**
 * Send a flying middle message to a specific player
 *
 * @param playerId Id of the player
 * @param message The text contained within the message. Can use `Custom Text Styling`.
 * @param distanceFromAction The distance from the action that has caused this message to be displayed, this value
 *                           will be used to determine how the message flies across the screen
 */
sendFlyingMiddleMessage(playerId: PlayerId, message: CustomTextStyling, distanceFromAction: number): void

/**
 * Play particle effect on all clients, or only on some clients if clientPredictedBy is specified
 * @param opts
 * @param clientPredictedBy Play only on clients where client with playerId clientPredictedBy
 *                          is not invisible, transparent, or themselves
 */
playParticleEffect(opts: TempParticleSystemOpts, clientPredictedBy: PlayerId = null): void

/**
 * Get the in game name of an entity.
 * @param entityId
 */
getEntityName(entityId: EntityId): string

/**
 * Given the name of a player, get their id
 * @param playerName
 */
getPlayerId(playerName: string): PNull<PlayerId>

/**
 * Given a player, get their permanent identifier that doesn't change when leaving and re-entering
 *
 * @param playerId
 */
getPlayerDbId(playerId: PlayerId): PlayerDbId

/**
 * Returns null if player not in lobby
 *
 * @param dbId
 */
getPlayerIdFromDbId(dbId: PlayerDbId): PNull<PlayerId>


kickPlayer(playerId: PlayerId, reason: string): void

/**
 * Check if the block at a specific position is in a loaded chunk.
 * @param x
 * @param y
 * @param z
 * @return boolean
 */
isBlockInLoadedChunk(x: number, y: number, z: number): boolean

/**
 * Get the name of a block.
 * @param x could be an array [x, y, z]. If so, the other params shouldn't be passed.
 * @param y
 * @param z
 * @return blockName - will be a name contained in blockMetadata.ts or 'Air'
 */
getBlock(x: number | number[], y?: number, z?: number): BlockName

/**
 * Used to get the block id at a specific position.
 * Intended only for use in hot code paths - default to getBlock for most use cases
 *
 * @param x
 * @param y
 * @param z
 */
getBlockId(x: number, y: number, z: number): BlockId

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
 * @param x Can be an array
 * @param y Should be blockname if first param is array
 * @param z
 * @param blockName
 */
setBlock(x: number | number[], y: number | BlockName, z?: number, blockName?: BlockName): void

/**
 * Initiate a block change "by the world".
 * This ends up calling the onWorldChangeBlock and only makes the change if not prevented by game/plugins.
 * initiatorDbId is null if the change was initiated by the game code.
 *
 * @param initiatorDbId
 * @param x
 * @param y
 * @param z
 * @param blockName
 *
 * @returns "preventChange" if the change was prevented, "preventDrop" if the change was allowed but without dropping any items, and undefined if the change was allowed with an item drop
 */
attemptWorldChangeBlock(initiatorDbId: PNull<PlayerDbId>, x: number, y: number, z: number, blockName: BlockName): "preventChange" | "preventDrop" | void

/**
 * Returns whether a block is solid or not.
 * E.g. Grass block is solid, while water, ladder and water are not.
 * Will be true if the block is unloaded.
 *
 * @param x
 * @param y
 * @param z
 */
getBlockSolidity(x: number | number[], y?: number, z?: number): boolean

/**
 * Helper function that sets all blocks in a rectangle to a specific block.
 *
 * @param pos1 array [x, y, z]
 * @param pos2 array [x, y, z]
 * @param blockName
 */
setBlockRect(pos1: number[], pos2: number[], blockName: BlockName): void

/**
 * Create walls by providing two opposite corners of the cuboid
 *
 *
 * @param pos1 array [x, y, z]
 * @param pos2 array [x, y, z]
 * @param blockName
 * @param hasFloor
 * @param hasCeiling
 */
setBlockWalls(pos1: number[], pos2: number[], blockName: BlockName, hasFloor = false, hasCeiling = false): void

/**
 * Only use this instead of getBlock if you REALLY need the performance (i.e. you are iterating over tens of thousands of blocks)
 * ReturnedObject.blockData is a 32x32x32 ndarray of block ids
 * (see https://www.npmjs.com/package/ndarray)
 * Each block id is a 16-bit number
 * The ndarray should only be read from, writing to it will result in desync between the server and client
 *
 * @param pos The returned chunk contains pos
 * @returns null if the chunk is not loaded in a persisted world. ReturnedObject.blockData is an ndarray that can be accessed
 * (but modifications have to be saved with resetChunk).
 */
getChunk(pos: number[]): PNull<GameChunk>

/**
 * Use this to get a chunk ndarray you can edit and set in resetChunk.
 *
 * Only use chunk helpers if you REALLY need the performance (i.e. you are iterating over tens of thousands of blocks)
 * ReturnedObject.blockData is a 32x32x32 ndarray of air.
 * (see https://www.npmjs.com/package/ndarray)
 * Each block id is a 16-bit number
 */
getEmptyChunk(): GameChunk

/**
 * Splits the block name by '|'. If no meta info, metaInfo is ''
 *
 * @param blockName
 */
getMetaInfo(blockName: BlockName | null | undefined): ItemMetaInfo

/**
 * Get the numeric id of a block used in the ndarrays returned from getChunk
 * I.e. chunk.blockData.set(x, y, z, api.blockNameToBlockId("Dirt"))
 * or chunk.blockData.get(x, y, z) === api.blockNameToBlockId("Dirt")
 *
 * @param blockName
 * @param allowInvalidBlock Don't throw an error if the block name is invalid. Defaults false.
 *      If true and name is invalid, returns null
 * @returns
 */
blockNameToBlockId(blockName: string, allowInvalidBlock = false): PNull<number>

/**
 * Goes from block id to block name. The reverse of blockNameToBlockId
 *
 * @param blockId
 */
blockIdToBlockName(blockId: BlockId): BlockName

/**
 * Get the unique id of the chunk containing pos in the current map
 *
 * @param pos
 */
blockCoordToChunkId(pos: number[]): string

/**
 * Get the co-ordinates of the block in the chunk with the lowest x, y, and z co-ordinates
 *
 * @param chunkId
 */
chunkIdToBotLeftCoord(chunkId: string): [number, number, number]

/**
 * @deprecated - prefer using other UI elements
 * (this UI element hasn't been properly thought through in combination with other elements like killfeed, uirequests, etc)
 *
 * Send a player an icon in the top right corner
 *
 * @param playerId
 * @param icon Can be any icon from font-awesome.
 * @param text The text to send.
 * @param opts Can include keys duration, width, height, color, iconSizeMult.
 *
 * Default opts: {
 *  duration: 8, // seconds
 *  width: 400px,
 *  height: 100px,
 *  color: 'rgb(102, 102, 102)', // must be rgb in this format (hex not supported),
 *  iconSizeMult: 5,
 *  textAndIconColor: "white", // can be any colour supported by css (e.g. hex, rgb),
 *  fontSize: '17px',
 * }
 */
sendTopRightHelper(playerId: PlayerId, icon: string, text: string, opts: {
    duration?: number
    width?: number
    height?: number
    color?: string
    iconSizeMult?: number
    textAndIconColor?: string
    fontSize?: string
}): void

/**
 * Whether the player is on a mobile device or a computer.
 * @param playerId
 */
isMobile(playerId: PlayerId): boolean

/**
 * Prevent a player from picking up an item. itemId returned by createItemDrop
 *
 * @param playerId
 * @param itemId
 */
setCantPickUpItem(playerId: PlayerId, itemId: EntityId): void

/**
 * Delete an item drop by item drop entity ID
 *
 * @param itemId
 */
deleteItemDrop(itemId: EntityId): void

/**
 * Get the metadata about a block or item before stats have been modified by any client options
 * (i.e. its entry in either blockMetadata.ts or nonBlockMetadata in itemMetadata.ts)
 *
 * @param itemName
 */
getInitialItemMetadata(itemName: string): Partial<BlockMetadataItem & NonBlockMetadataItem>

/**
 * Get stat info about a block or item
 * Either based on a client option for a player: (e.g. `DirtTtb`)
 * or its entry in blockMetadata.ts or nonBlockMetadata in itemMetadata.ts if no client option is set.
 *
 * If null is passed for playerId, this is simply its entry in blockMetadata etc.
 *
 *
 * @param playerId
 * @param itemName
 * @param stat
 */
getItemStat<K extends keyof AnyMetadataItem>(playerId: PNull<PlayerId>, itemName: string, stat: K): AnyMetadataItem[K]

/**
 * Set the direction the player is looking.
 *
 * @param playerId
 * @param direction a vector of the direction to look, format [x, y, z]
 */
setCameraDirection(playerId: PlayerId, direction: number[]): void

/**
 * Set a player's opacity
 * A simple helper that calls setTargetedPlayerSettingForEveryone
 *
 * @param playerId
 * @param opacity
 */
setPlayerOpacity(playerId: PlayerId, opacity: number): void

/**
 * Set the level of viewable opacity by one player on another player
 * A simple helper that calls setOtherEntitySetting
 *
 * @param playerIdWhoViewsOpacityPlayer The player who sees that with opacity
 * @param playerIdOfOpacityPlayer The player/player model who is given opacity
 * @param opacity
 */
setPlayerOpacityForOnePlayer(playerIdWhoViewsOpacityPlayer: PlayerId, playerIdOfOpacityPlayer: PlayerId, opacity: number): void

/**
 * Obtain Date.now() value saved at start of current game tick
 */
now(): number

/**
 * Check your game (and, optionally, a entity) is still valid and executing.
 * Useful if you're using async functions and await within your game.
 * If you use await/async or promises and do not check this, your game could have closed and then the rest of your
 * async code executes.
 *
 * @param entityId
 */
checkValid(entityId?: PNull<EntityId>): boolean

/**
 * Let a player change a block at a specific co-ordinate. Useful when client option canChange is false.
 * Overrides blockRect and blockType settings, so also useful when you have disallowed changing of a block type with setCantChangeBlockType.
 * Using this on 1000s of blocks will cause lag - if that is needed, find a way to use setCanChangeBlockType.
 *
 * @param playerId
 * @param x
 * @param y
 * @param z
 */
setCanChangeBlock(playerId: PlayerId, x: number, y: number, z: number): void

/**
 * Prevents a player from changing a block at a specific co-ordinate. Useful when client option canChange is true.
 * Overrides blockRect and blockType settings, so also useful when you have allowed changing of a block type with setCantChangeBlockType.
 * Using this on 1000s of blocks will cause lag - if that is needed, find a way to use setCantChangeBlockType.
 *
 * @param playerId
 * @param x
 * @param y
 * @param z
 */
setCantChangeBlock(playerId: PlayerId, x: number, y: number, z: number): void

/**
 * Lets a player Change a block type. Valid names are those contained within blockMetadata.ts and 'Air'
 * Less priority than cant change block pos/can change block rect
 *
 * @param playerId
 * @param blockName
 */
setCanChangeBlockType(playerId: PlayerId, blockName: BlockName): void

/**
 * Stops a player from Changeing a block type. Valid names are those contained within blockMetadata.ts and 'Air'
 * Less priority than can change block pos/can change block rect
 *
 * @param playerId
 * @param blockName
 */
setCantChangeBlockType(playerId: PlayerId, blockName: BlockName): void

/**
 * Remove any previous can/cant change block type settings for a player
 *
 * @param playerId
 * @param blockName
 */
resetCanChangeBlockType(playerId: PlayerId, blockName: BlockName): void

/**
 * Make it so a player can Change blocks within two points. Coordinates are inclusive. E.g. if [0, 0, 0] is pos1
 * and [1, 1, 1] is pos2 then the 8 blocks contained within low and high will be able to be broken.
 * Overrides setCantChangeBlockType
 *
 *
 * @param playerId
 * @param pos1 Arg as [x, y, z]
 * @param pos2 Arg as [x, y, z]
 */
setCanChangeBlockRect(playerId: PlayerId, pos1: number[], pos2: number[]): void

/**
 * Make it so a player cant Change blocks within two points. Coordinates are inclusive. E.g. if [0, 0, 0] is pos1
 * and [1, 1, 1] is pos2 then the 8 blocks contained within pos1 and pos2 won't be able to be broken.
 * Overrides setCanChangeBlockType
 *
 *
 * @param playerId
 * @param pos1 Arg as [x, y, z]
 * @param pos2 Arg as [x, y, z]
 */
setCantChangeBlockRect(playerId: PlayerId, pos1: number[], pos2: number[]): void

/**
 * Remove any previous can/cant change block rect settings for a player
 *
 * @param playerId
 * @param pos1
 * @param pos2
 */
resetCanChangeBlockRect(playerId: PlayerId, pos1: number[], pos2: number[]): void

/**
 * Allow a player to walk through a type of block. For blocks that are normally solid and not seethrough, the player will experience slight visual glitches while inside the block.
 *
 *
 * @param playerId
 * @param blockName
 * @param disable If you've enabled a player to walk through a block and want to make the block solid for them again, pass this with true. Otherwise you only need to pass playerId and blockName
 */
setWalkThroughType(playerId: PlayerId, blockName: BlockName, disable = false): void

/**
 * Allow a player to walk through (or not walk through) voxels that are located within a given rectangle.
 * For blocks that are normally solid and not seethrough, the player will experience slight visual glitches while inside the block.
 *
 * You could set both pos1 and pos2 to [0, 0, 0] to make only 0, 0, 0 walkthrough, for example.
 *
 * @param playerId
 * @param pos1 The one corner of the cuboid. Format [x, y, z]
 * @param pos2 The top right corner of the cuboid. Format [x, y, z]
 * @param updateType The type of update. Whether to make a rect solid, or able to be walked through.
 * Pass DEFAULT_WALK_THROUGH with a previously passed rect to disable any walkthrough setting for that rect
 *
 */
setWalkThroughRect(playerId: PlayerId, pos1: number[], pos2: number[], updateType: WalkThroughType): void

/**
 * Give a player an item and a certain amount of that item.
 * Returns the amount of item added to the users inventory.
 *
 * @param playerId
 * @param itemName
 * @param itemAmount
 * @param attributes An optional object for certain types of item. For guns this can contain the shotsLeft field which is the amount of ammo the gun currently has.
 */
giveItem(playerId: PlayerId, itemName: string, itemAmount?: number, attributes?: ItemAttributes): number

/**
 * Whether the player has space in their inventory to get new blocks
 * @param playerId
 */
inventoryIsFull(playerId: PlayerId): boolean

/**
 * Put an item in a specific index. Default hotbar is indexes 0-9
 *
 * @param playerId
 * @param itemSlotIndex 0-indexed
 * @param itemName Can be 'Air', in which case itemAmount will be ignored and the slot will be cleared.
 * @param itemAmount -1 for infinity. Should not be set, or null, for items that are not stackable.
 * @param attributes An optional object for certain types of item. For guns this can contain the shotsLeft field which is the amount of ammo the gun currently has.
 * @param tellClient whether to tell client about it - results in desync between client and server if client doesnt locally perform the same action
 */
setItemSlot(playerId: PlayerId, itemSlotIndex: number, itemName: string, itemAmount?: PNull<number>, attributes?: ItemAttributes, tellClient = true): void

/**
 * Remove an amount of item from a player's inventory
 *
 * @param playerId
 * @param itemName
 * @param amount
 */
removeItemName(playerId: PlayerId, itemName: string, amount: number): void

/**
 * Get the item at a specific index
 * Returns null if there is no item at that index
 * If there is an item, return an object of the format {name: itemName, amount: amountOfItem}
 *
 * @param playerId
 * @param itemSlotIndex
 */
getItemSlot(playerId: PlayerId, itemSlotIndex: number): PNull<InvenItem>

/**
 * Whether a player has an item
 *
 * @param playerId
 * @param itemName
 * @returns bool
 */
hasItem(playerId: PlayerId, itemName: string): boolean

/**
 * The amount of an itemName a player has.
 * Returns 0 if the player has none, and a negative number if infinite.
 *
 * @param playerId
 * @param itemName
 * @returns number
 */
getInventoryItemAmount(playerId: PlayerId, itemName: string): number

/**
 * Clear the players inventory
 *
 * @param playerId
 */
clearInventory(playerId: PlayerId): void

/**
 * Force the player to have the ith inventory slot selected. E.g. newI 0 makes the player have the 0th inventory slot selected
 *
 * @param playerId
 * @param newI integer from 0-9
 */
setSelectedInventorySlotI(playerId: PlayerId, newI: number): void

/**
 * Get a player's currently selected inventory slot
 * @param playerId
 * @returns
 */
getSelectedInventorySlotI(playerId: PlayerId): any

/**
 * Get the currently held item of a player
 * Returns null if no item is being held
 * If an item is held, return an object of the format {name: itemName, amount: amountOfItem}
 *
 * @param playerId
 */
getHeldItem(playerId: PlayerId): InvenItem

/**
 * Get the amount of free slots in a player's inventory.
 *
 * @param playerId
 * @returns number
 */
getInventoryFreeSlotCount(playerId: PlayerId): number

/**
 * Checks if a player is able to open a chest at a given location,
 * as per the rules laid out by the "onPlayerAttemptOpenChest" game callback.
 * Returns true if the player can open the chest, false if they cannot, and void if the chest does not exist.
 *
 * @param playerId
 * @param chestX
 * @param chestY
 * @param chestZ
 */
canOpenStandardChest(playerId: PlayerId, chestX: number, chestY: number, chestZ: number): boolean | void

/**
 * Give a standard chest an item and a certain amount of that item.
 * Returns the amount of item added to the chest.
 *
 * @param chestPos
 * @param itemName
 * @param itemAmount
 * @param playerId The player who is interacting with the chest.
 * @param attributes An optional object for certain types of item. For guns this can contain the shotsLeft field which is the amount of ammo the gun currently has.
 */
giveStandardChestItem(chestPos: readonly number[], itemName: string, itemAmount = 1, playerId?: PlayerId, attributes?: ItemAttributes): number

/**
 * Get the amount of free slots in a standard chest
 *
 * @param chestPos
 * @returns number
 */
getStandardChestFreeSlotCount(chestPos: number[]): number

/**
 * The amount of an itemName a standard chest has.
 * Returns 0 if the standard chest has none, and a negative number if infinite.
 *
 * @param chestPos
 * @param itemName
 * @returns number
 */
getStandardChestItemAmount(chestPos: number[], itemName: string): number

/**
 * Get the item at a chest slot. Null if empty otherwise format {name: itemName, amount: amountOfItem}
 *
 * @param chestPos
 * @param idx
 */
getStandardChestItemSlot(chestPos: number[], idx: number): InvenItem

/**
 * Get all the items from a standard chest in order. Use this instead of repetitive calls to getStandardChestItemSlot
 *
 * @param chestPos
 */
getStandardChestItems(chestPos: number[]): readonly InvenItem[]

/**
 *
 * @param chestPos
 * @param idx 0-indexed
 * @param itemName Can be 'Air', in which case itemAmount will be ignored and the slot will be cleared.
 * @param itemAmount -1 for infinity. Should not be set, or null, for items that are not stackable.
 * @param playerId The player who is interacting with the chest.
 * @param attributes An optional object for certain types of item. For guns this can contain the shotsLeft field which is the amount of ammo the gun currently has.
 */
setStandardChestItemSlot(chestPos: readonly number[], idx: number, itemName: string, itemAmount: number = null, playerId?: PlayerId, attributes?: ItemAttributes): void

/**
 * Get the item in a player's moonstone chest slot. Null if empty
 *
 * Moonstone chests are a type of chest where a player accesses the same contents no matter the location of the moonstone chest
 *
 * @param playerId
 * @param idx
 */
getMoonstoneChestItemSlot(playerId: PlayerId, idx: number): InvenItem

/**
 * Get all the items from a moonstone chest in order. Use this instead of repetitive calls to getMoonstoneChestItemSlot
 *
 * Moonstone chests are a type of chest where a player accesses the same contents no matter the location of the moonstone chest
 *
 * @param playerId
 */
getMoonstoneChestItems(playerId: PlayerId): readonly InvenItem[]

/**
 * Moonstone chests are a type of chest where a player accesses the same contents no matter the location of the moonstone chest
 *
 * @param playerId
 * @param idx 0-indexed
 * @param itemName Can be 'Air', in which case itemAmount will be ignored and the slot will be cleared.
 * @param itemAmount -1 for infinity. Should not be set, or null, for items that are not stackable.
 * @param metadata An optional object for certain types of item. For guns this can contain the shotsLeft field which is the amount of ammo the gun currently has.
 */
setMoonstoneChestItemSlot(playerId: PlayerId, idx: number, itemName: string, itemAmount: number = null, metadata?: ItemAttributes): void

/**
 * Get the name of the lobby this game is running in.
 */
getLobbyName(): any

/**
 * Integer lobby names are public
 * @returns boolean
 */
isPublicLobby(): boolean

/**
 * Returns if the current lobby the game is running in is special - e.g. a discord guild or dm, or simply a standard lobby
 */
getLobbyType(): LobbyType

/**
 * Update the progress bar in the bottom right corner.
 * Can be queued.
 *
 * @param playerId
 * @param toFraction The fraction of the progress bar you want to be filled up.
 * @param toDuration The time it takes for the bar to reach the given toFraction in ms.
 * If this is too low and you queue multiple updates, this toFraction could be skipped. Treat 200ms as a minimum.
 */
progressBarUpdate(playerId: PlayerId, toFraction: number, toDuration = 200): void

/**
 * Edit the crafting recipes for a player. See crafting.ts for the format of recipesForItem
 *
 * @param playerId
 * @param itemName
 * @param recipesForItem
 */
editItemCraftingRecipes(playerId: PlayerId, itemName: ItemName, recipesForItem: RecipesForItem): void

/**
 * Reset the crafting recipes for a given back to its original bloxd state
 *
 * @param playerId
 * @param itemName
 */
resetItemCraftingRecipes(playerId: PlayerId, itemName: string): void

/**
 * Check if a position is within a cubic rectangle
 *
 * @param coordsToCheck
 * @param pos1 position of one corner
 * @param pos2 position of opposite corner
 * @param addOneToMax
 */
isInsideRect(coordsToCheck: number[], pos1: number[], pos2: number[], addOneToMax = false): boolean

/**
 * Get the entities in the rect between [minX, minY, minZ] and [maxX, maxY, maxZ]
 *
 * @param minCoords
 * @param maxCoords
 * @returns
 */
getEntitiesInRect(minCoords: number[], maxCoords: number[]): EntityId[]

/**
 *
 * @param entityId
 */
getEntityType(entityId: EntityId): EntityType

/**
 * Apply an impulse to an entity
 *
 * @param eId
 * @param xImpulse
 * @param yImpulse
 * @param zImpulse
 */
applyImpulse(eId: EntityId, xImpulse: number, yImpulse: number, zImpulse: number): void

/**
 * Set the velocity of an entity
 *
 * @param eId
 * @param x
 * @param y
 * @param z
 */
setVelocity(eId: EntityId, x: number, y: number, z: number): void

/**
 * Set the heading for a server-auth entity.
 *
 * @param entityId
 * @param newHeading
 */
setEntityHeading(entityId: EntityId, newHeading: number): void

/**
 * Spin player in kart
 * @param playerId
 * @param dir direction of spin, 1 for right, -1 for left
 * @param durationInTicks the number of ticks it takes to complete a spin
 */
spinKart(playerId: PlayerId, dir: number, durationInTicks: number): void

/**
 * Set the amount of an item in an item entity
 *
 * @param itemId
 * @param newAmount
 */
setItemAmount(itemId: EntityId, newAmount: number): void

/**
 * Show a message over the shop in the same place that a shop item's onBoughtMessage is shown.
 * Displays for a couple seconds before disappearing
 * Use case is to show a dynamic message when player buys an item
 *
 * @param playerId
 * @param info
 */
sendOverShopInfo(playerId: PlayerId, info: string | CustomTextStyling): void

/**
 * Open the shop UI for a player
 *
 * @param playerId
 * @param toggle Whether to close the shop if it's already open
 * @param forceCategory If set, will change the shop to this category
 */
openShop(playerId: PlayerId, toggle = false, forceCategory: string = null): void

/**
 * Apply an effect to a player.
 * Can be an inbuilt effect E.g. "Speed" (speed boost), "Damage" (damage boost).
 * For inbuilt just pass the name of the effect and the functionality is handled in-engine.
 * For custom effect, you pass customEffectInfo. The icon can be an icon from "IngameIcons.ts" or a bloxd item name.
 * The custom effect onEndCb is an optional helper within which you can undo the effect you applied.
 *
 * @param playerId
 * @param effectName
 * @param duration
 * @param customEffectInfo (onEndCb will not work for press to code boards and code blocks)
 */
applyEffect(playerId: PlayerId, effectName: string, duration: number | null, customEffectInfo: { icon?: IngameIconName | ItemName; onEndCb?: () => void; displayName?: string | TranslatedText } & Partial<InbuiltEffectInfo>): void

/**
 * Get all the effects currently applied to a player
 *
 * @param playerId
 */
getEffects(playerId: PlayerId): string[]

/**
 * Remove an effect from a player.
 *
 * @param playerId
 * @param name
 */
removeEffect(playerId: PlayerId, name: string): void

/**
 * Change a part of a player's skin
 * @param playerId
 * @param partType
 * @param selected
 */
changePlayerIntoSkin(playerId: PlayerId, partType: CustomisationPart, selected: string): void

/**
 * Remove gamemode-applied skin from a player
 * @param playerId
 */
removeAppliedSkin(playerId: PlayerId): void

/**
 * Scale node of a player's mesh by 3d vector.
 * State from prior calls to this api is lost so if you want to have multiple nodes scaled, pass in all the scales at once.
 *
 * @param playerId
 * @param nodeScales
 */
scalePlayerMeshNodes(playerId: PlayerId, nodeScales: EntityMeshScalingMap): void

/**
 *  Attach/detach mesh instances to/from an entity
 *  @param eId
 *  @param node node to attach to
 *  @param type if null, detaches mesh from this node
 *  @param opts
 *  @param offset
 *  @param rotation
 */
updateEntityNodeMeshAttachment<MeshType extends MeshEntityType>(eId: EntityId, node: EntityNamedNode, type: PNull<MeshType>, opts?: MeshEntityOpts[MeshType], offset: number[] = [0, 0, 0], rotation = [0, 0, 0]): void

/**
 * Set the pose of the player
 * @param playerId
 * @param pose
 */
setPlayerPose(playerId: PlayerId, pose: PlayerPose): void

/**
 * Set physics state of player (vehicle type and tier)
 * @param playerId
 * @param physicsState
 */
setPlayerPhysicsState(playerId: PlayerId, physicsState: PlayerPhysicsStateData): void

/**
 * Get physics state for player
 * @param playerId
 */
getPlayerPhysicsState(playerId: PlayerId): PlayerPhysicsStateData

/**
 * Add following entity to player
 * @param playerId
 * @param eId
 * @param offset
 */
addFollowingEntityToPlayer(playerId: PlayerId, eId: EntityId, offset?: number[]): void

/**
 * Remove following entity from player
 * @param playerId
 * @param entityEId
 */
removeFollowingEntityFromPlayer(playerId: PlayerId, entityEId: EntityId): void

/**
 * Set camera zoom for a player
 * @param playerId
 * @param zoom
 */
setCameraZoom(playerId: PlayerId, zoom: number): void

/**
 *
 *
 * @param playerId hears the sound
 * @param soundName Can also be a prefix. If so, a random sound with that prefix will be played
 * @param volume 0-1. If it's too quiet and volume is 1, normalise your sound in audacity
 * @param rate The speed of playback. Also affects pitch. 0.5-4. Lower playback = lower pitch
 *        Good for varying the sound. E.g. item pickup sound has a random rate between 1 and 1.5
 * @param posSettings
 * {playerIdOrPos: PlayerId | number[], maxHearDist: number, refDistance: number}
 * playerIdOrPos: The player the sound originates from, or the position of the sound
 * maxHearDist: sound is not played if player is further than this. Default 15
 * refDistance: higher means the sound decreases less in volume with distance. Default 3. Hitting is 4. Guns are 10
 *
 */
playSound(playerId: PlayerId, soundName: string, volume: number, rate: number, posSettings?: {
    playerIdOrPos: PlayerId | number[]
    maxHearDist?: number
    refDistance?: number
}): void

// See documentation for api.playSound
broadcastSound(soundName: string, volume: number, rate: number, posSettings?: {
    playerIdOrPos: PlayerId | number[]
    maxHearDist?: number
    refDistance?: number
}, exceptPlayerId: PlayerId = null): void

// See documentation for api.playSound
playClientPredictedSound(soundName: string, volume: number, rate: number, posSettings?: {
    playerIdOrPos: PlayerId | number[]
    maxHearDist?: number
    refDistance?: number
}, predictedBy?: PlayerId): void


calcExplosionForce(eId: EntityId, explosionType: ExplosionType, knockbackFactor: number, explosionRadius: number, explosionPos: number[], ignoreProjectiles: boolean): { force: Pos; forceFrac: number; }

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
 * @param playerId
 */
getPlayerTargetInfo(playerId: PlayerId): { position: Pos; normal: Pos; adjacent: Pos }

/**
 * Get the position of a player's camera and the direction (both in Euclidean and spherical coordinates) they are attempting to use an item.
 * The camPos has the same limitations described in getPlayerTargetInfo
 *
 * @param playerId
 */
getPlayerFacingInfo(playerId: PlayerId): { camPos: Pos; dir: Pos; angleDir: AngleDir; moveHeading: number }

/**
 * Raycast for a block in the world.
 * Given a position and a direction, find the first block that the "ray" hits.
 *
 * @param fromPos
 * @param dirVec
 */
raycastForBlock(fromPos: number[], dirVec: number[]): BlockRaycastResult

/**
 * Check whether a player is crouching
 *
 * @param playerId
 */
isPlayerCrouching(playerId: PlayerId): any
```

## Glossary of Referenced Types

These 'types' can't be referenced by your code, but they help explain some of the parameters in the API.

```ts
type CustomTextStyling = (string | EntityName | TranslatedText | StyledIcon | StyledText)[]

type EntityMeshScalingMap = { [key in "TorsoNode" | "HeadMesh" | "ArmRightMesh" | "ArmLeftMesh" | "LegLeftMesh" | "LegRightMesh"]?: number[] }

type EntityName = {
    entityName: string
    style?: {
        color?: string
        colour?: string
    }
}

type IngameIconName = "Damage" | "Damage Reduction" | "Speed" | "VoidJump" | "Fist" | "Frozen" | "Hydrated" | "Invisible" | "Jump Boost" | "Poisoned" | "Slowness" | "Weakness" | "Health Regen" | "Haste" | "Heat Resistance" | "Gliding" | "Boating" | "Obsidian Boating" | "Bunny Hop" | "FallDamage" | "Feather Falling"

enum ParticleSystemBlendMode {
    // Source color is added to the destination color without alpha affecting the result
    OneOne = 0,
    // Blend current color and particle color using particle’s alpha
    Standard = 1,
    // Add current color and particle color multiplied by particle’s alpha
    Add,
    // Multiply current color with particle color
    Multiply,
    // Multiply current color with particle color then add current color and particle color multiplied by particle’s alpha
    MultiplyAdd,
}

type RecipesForItem = {
    requires: { items: string[]; amt: number }[]
    produces: number
    station?: string
}[]

type StyledIcon = {
    icon: string
    style?: {
        color?: string
        colour?: string
        fontSize?: string
        opacity?: number
    }
}

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

type TranslatedText = {
    translationKey: string
    params?: Record<string, string | number | boolean | EntityName>
}

type ItemAttributes = { customDisplayName?: string; customDescription?: string; customAttributes?: Record<string, any> }

```
