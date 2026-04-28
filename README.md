# Code API

You can run javascript when right clicking code blocks and press to code boards.
This is only available to owners of worlds lobbies.
The javascript can interact with the Bloxd.io game api.

Please use [our discord](https://discord.gg/playbloxd) to report any issues you come across or features you'd like to see added.

## Code Blocks

- World owners can find these by searching in the creative menu
- No need to add `press to code`, this text is only needed for code boards, and will automatically be removed
- If you want to run code without opening the code editor, you can trigger the code block by right clicking an adjacent `press to code` board instead

## Boards

- You can begin a board with `press to code` to run javascript when you right click it.
- Normally you can't edit a code board after placing it, but you can currently work around this by putting a space before `press to code`.
- Boards only allow for a small amount of text, we recommend you use Code Blocks instead, or you can work around this by using multiple boards

## Notes

- Global variables `myId` and `playerId` store the player ID of who is running the code.
- Global variable `thisPos` stores the position of the currently executing code block or press to code board.
- `myId`, `playerId`, `thisPos` and `ownerDbId` are all defined on `api`
- You can use `api.log` or `console.log` for printing and debugging (they do the same thing).
- You can use `Date.now()` instead of `api.now()` if you prefer, both return the time in milliseconds.
- Comments like `/* comment */` work, but comments like `// comment` don't work right now.

## Examples

Code Block to make the player jump:

```js
f = api.setVelocity(myId, 0, 9, 0)
```

Push the player:

```js
api.applyImpulse(myId, 9, 0, 9)
```

Send an orange message to yourself:

```js
api.sendMessage(myId, "text", { color: "orange" })
```

Create flying text:

```js
const speed = 100
api.sendFlyingMiddleMessage(myId, ["Message to display"], speed)
```

Send a message to all players:

```js
api.broadcastMessage("announcement", { color: "red" })
```

Set player health to 99, and print the old health:

```js
const oldHealth = api.getHealth(myId)
api.setHealth(myId, 99)
api.log("Old Health:", oldHealth)
```

Define a function to get the player IDs excluding your own ID:

```js
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

```js
for (const otherId of getOtherIds()) {
    api.setPlayerPose(otherId, "zombie")
    api.changePlayerIntoSkin(otherId, "head", "zombie")
}
```

Make all players look like floating wizards:

```js
for (const playerId of api.getPlayerIds()) {
    api.setPlayerPose(playerId, "driving")
    api.changePlayerIntoSkin(playerId, "head", "wizard")
}
```

## Glossary of Referenced Types

These 'types' can't be referenced by your code, but they help explain some of the parameters in the API.

```ts
type CustomTextStyling = (string | EntityName | TranslatedText | StyledIcon | StyledText)[]

type EntityMeshScalingMap = {
    [key in "TorsoNode" | "HeadMesh" | "ArmRightMesh" | "ArmLeftMesh" | "LegLeftMesh" | "LegRightMesh"]?: number[]
}

type EntityName = {
    entityName: string
    style?: {
        color?: string
        colour?: string
    }
}

type IngameIconName = "Damage" | "Damage Reduction" | "Speed" | "VoidJump" | "Fist" | "Frozen" | "Hydrated" | "Invisible" | "Jump Boost" | "Poisoned" | "Slowness" | "Weakness" | "Health Regen" | "Haste" | "Double Jump" | "Heat Resistance" | "Gliding" | "Boating" | "Obsidian Boating" | "Riding" | "Bunny Hop" | "FallDamage" | "Feather Falling" | "Thief" | "X-Ray Vision" | "Mining Yield" | "Brain Rot" | "Rested Damage" | "Rested Haste" | "Rested Speed" | "Rested Farming Yield" | "Rested Aura" | "Blindness" | "Pickpocketer" | "Lifesteal" | "Bounciness" | "Air Walk" | "Wall Climbing" | "Thorns" | "Poopy" | "Draugr Knight Head" | "Draugr Warper Head" | "Magma Golem Head" | "Mystery Fish" | "Damage Enchantment" | "Critical Damage Enchantment" | "Attack Speed Enchantment" | "Protection Enchantment" | "Health Enchantment" | "Health Regen Enchantment" | "Stomp Damage Enchantment" | "Knockback Resist Enchantment" | "Arrow Speed Enchantment" | "Arrow Damage Enchantment" | "Quick Charge Enchantment" | "Break Speed Enchantment" | "Momentum Enchantment" | "Mining Yield Enchantment" | "Farming Yield Enchantment" | "Mining Aura Enchantment" | "Digging Aura Enchantment" | "Lumber Aura Enchantment" | "Farming Aura Enchantment" | "Vertical Knockback Enchantment" | "Horizontal Knockback Enchantment" | "Self Yield" | "Friends" | "Riding Speed" | "Feed Aura" | "Double Poop" | "Mob Slayer" | "Rainbow Wool" | "Pack Leader" | "Max Health" | "Poison Claws" | "Mob Yield" | "Antlers Bonus" | "Health" | "HealthShield" | "Cross" | "Friendship" | "Dotted Friendship" | "Hunger" | "Empty Hunger" | "Pixelated Heart" | "Question Mark" | "Trader Black" | "Trader Blue" | "Trader Piggy"

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

type RecipesForItem = 
    {
        requires: { items: ItemName[]; amt: number }[]
        produces: number
        station?: string | string[]
        onCraftedAura?: number
        isStarterRecipe?: boolean
        attributes?: ItemAttributes
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
    style?: TextStyle
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
    hideDist: number
}

type TranslatedText = {
    translationKey: string
    params?: Record<string, string | number | boolean | EntityName>
}

type ItemAttributes = { customDisplayName?: string; customDescription?: string; customAttributes?: Record<string, any> }

enum WalkThroughType {
    CANT_WALK_THROUGH = 0,
    CAN_WALK_THROUGH = 1,
    DEFAULT_WALK_THROUGH = 2,
}

type WorldBlockChangedInfo = {
    cause: PNull<"Paintball" | "FloorCreator" | "Sapling" | "StemFruit" | "MeltingIce" | "Explosion">
}

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

type ShopItem = {
    image: string
    cost?: number
    currency?: string
    amount?: number // Display amount shown on the shop tile image (0 and 1 are not displayed)
    imageColour?: string
    canBuy?: boolean
    isSelected?: boolean
    buyButtonText?: string | CustomTextStyling
    customTitle?: string | CustomTextStyling
    description?: string | CustomTextStyling
    onBoughtMessage?: string | CustomTextStyling
    redDot?: boolean
    forceRemoveRedDot?: boolean
    badge?: { text: string | CustomTextStyling; type: ShopItemBadgeType }
    userInput?: ShopItemUserInput
    sell?: boolean // Optional, defaults to false. If true, the sign of "cost" is flipped. So a "cost" of -25 would give the player 25 currency AND be displayed as "25" (instead of -25)
    sortPriority?: number // Descending, bigger number means closer to the top
    hidden?: boolean
}

type ShopItemUserInput =
    | { type: "text"; placeholderText?: string; wordCharsOnly?: boolean; initialValue?: string } // wordCharsOnly defaults to false. If true, only allows \w character (alphanumeric and _). initialValue always takes precedence as the text input value when set.
    | { type: "number"; placeholderText?: string; initialValue?: string }
    | {
            type: "dropdown"
            dropdownOptions: readonly (string | { option: string; cost: number })[]
            shouldResetSelectionOnOptionsChange?: boolean // Defaults to false. If true, the selection will reset to the first option when dropdownOptions changes.
            initialValue?: string
      }
    | { type: "player"; excludedPlayers?: PlayerId[] } // Defaults to excluding the current player
    | { type: "color"; initialValue?: string }

type ShopCategoryConfig = Partial<{
    autoSelectCategory: boolean
    customTitle: string // Supports translation keys and ordinary text
    redDot: boolean
    forceRemoveRedDot: boolean
    sortPriority: number
    description: string | CustomTextStyling
}>

type MobSpawnOpts<TMobType extends MobType> = Partial<{
    mobHerdId: MobHerdId
    spawnerId: PlayerId
    mobDbId: MobDbId
    name: string
    playSoundOnSpawn: boolean
    variation: MobVariation<TMobType>
    physicsOpts: Partial<{
        width: number
        height: number
        collidesEntities: boolean
    }>
}>

type MeshEntityOpts = {
    Box: CommonMeshEntityOpts & {
        width: number
        height: number
        depth: number
        diffuseColor?: number[]
        emissiveColor?: number[]
        backFaceCulling?: boolean // Default true
        texture?: string // Can be a blockname. Wraps every one block
        faceUV?: number[][]
    }
    BloxdBlock: CommonMeshEntityOpts & {
        blockName: BlockNameOrId
        size: number | [number, number, number]
    }
    Person: CommonMeshEntityOpts & {
        size?: number
        textures?: Partial<Cosmetics>
        pose?: PlayerPose
    }
    ParticleEmitter: MeshParticleSystemOpts
}

type CommonMeshEntityOpts = {
    hideDist?: number
    meshOffset?: number[]
    autoRotate?: boolean
    lineToEId?: EntityId // EntityId to connect to using a line
}

type MeshEntityPhysicsOpts = {
    doPhysics: boolean
    onCollideTerrain?: () => void // Unsupported for custom code
    collidesEntities?: boolean
    collideBits?: number // bitmask category of this entity
    collideMask?: number // bitmask category of entities this entity collides with
    heightExpandAmt?: number // expand hitbox height by this amount
    widthExpandAmt?: number // expand hitbox width by this amount
    vehicleOpts?: MeshEntityVehicleOpts // Unsupported for custom code
}

/**
 * ANIMATION SCHEMA TYPES
 *
 * An animation schema describes how an entity should be positioned as time passes.
 * For each node in the entity's skeleton, we define an animation timeline.
 * A timeline is sequence of "key frames".
 * A keyframe represents an important position; if this is a jumping animation,
 * then an example of a keyframe would be the peak of the jump.
 *
 * When deciding how an entity should be positioned during an animation,
 * we will usually find ourselves between two keyframes.
 * For example, if our keyframes are at time fractions 0.0, 0.5 and 1.0,
 * and the current time fraction is 0.3, then we will need to find a middle ground
 * between the first and second keyframe.
 * This process is known as interpolating, or "lerping".
 * The default way of doing this is linear lerping; drawing a straight line between two points.
 * An alternative is splining; drawing a curve.
 */
export type AnimationSchema = {
    animationDurationMs: number
    loop?: LoopModeSchema
    nodeAnimations?: NodeSkeletonAnimationSchema
}

type LoopModeSchema = boolean | "hold-on-last-frame"

type NodeSkeletonAnimationSchema = Record<NodeName, NodeAnimationSchema>

type NodeAnimationSchema = {
    timeline: AnimationTimelineSchema
}

type AnimationTimelineSchema = readonly KeyframeSchema[]

type KeyframeSchema = {
    timeFraction: number
    rotation?: LerpPointSchema // Rotations are assumed to be in radians.
}

/**
 * "pre" and "post" points exist to allow for discontinuities.
 */
export type LerpPointSchema =
    | Point
    | {
            lerpMode?: LerpModeSchema
            point: Point
      }
    | {
            lerpMode?: LerpModeSchema
            pre: Point // When lerping towards a point, we lerp towards its pre.
            post: Point // When lerping away from a point, we lerp away from its post.
      }

/**
 * "catmull-rom-spline" is a form of splining; drawing a curve between two points.
 */
export type LerpModeSchema = "linear" | "catmull-rom-spline"

/**
 * BLOCKBENCH ANIMATION SCHEMA TYPES
 *
 * We support native Blockbench animations. It should just be a case of copying and pasting
 * the specific animation from the exported JSON file.
 *
 * Notable differences:
 * - Blockbench animations do not use time fractions. Instead, they use absolute time.
 * - The unit of time is seconds, not milliseconds.
 * - The angular unit is degrees, not radians.
 * - The x-axis is mirrored.
 */
export type BlockbenchAnimationSchema = {
    animation_length: number // The duration of the animation in seconds.
    loop?: BlockbenchLoopModeSchema
    bones?: BlockbenchBonesAnimationSchema
}

type BlockbenchLoopModeSchema = boolean | "hold_on_last_frame"

type BlockbenchBonesAnimationSchema = Record<NodeName, BlockbenchBoneAnimationSchema>

type BlockbenchBoneAnimationSchema = {
    rotation?: BlockbenchAnimationTimelineSchema // Blockbench rotations are in degrees.
}

type BlockbenchAnimationTimelineSchema = Point | Record<TimestampString, BlockbenchAnimationFrameSchema>

/**
 * "pre" and "post" points exist to allow for discontinuities.
 */
export type BlockbenchAnimationFrameSchema =
    | Point
    | {
            lerp_mode?: BlockbenchLerpModeSchema
            pre?: Point // When lerping towards a point, we lerp towards its pre.
            post: Point // When lerping away from a point, we lerp away from its post.
      }

/**
 * "catmullrom" is a form of splining; drawing a curve between two points.
 */
export type BlockbenchLerpModeSchema = "linear" | "catmullrom"

/**
 * The timestamp of the keyframe in seconds.
 */
type TimestampString = string

type Point = Vec3
```
