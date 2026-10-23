# Getting started

There are three places to write JavaScript in Bloxd: code blocks, code boards and World Code. Code in any of them controls the game through `api`, in custom games and worlds. Only players who can edit code (the `canEditCode` client option) can write it, but anyone can run a code block or board.

Please use [our Discord](https://discord.gg/playbloxd) to report problems or suggest features.

## Code Blocks

- Find the Code Block by searching in the creative menu.
- Clicking a code block runs its code, unless you are crouching. Players who can edit code open its code by right clicking it.
- Running a code block or board also runs the code blocks and boards touching it, so clicking a `press to code` board next to a code block runs it without opening the code editor.
- A code block doesn't need a `press to code` line, and removes one if you add it.

## Boards

- A board whose text starts with `press to code` on its own line runs the code below that line when someone clicks it.
- Boards only hold a little text: use a code block for anything longer, or split the code across several boards.

## World Code

World Code runs for the whole world instead of when someone clicks. Open it with F8. It defines [Callbacks](#callbacks), such as `tick` and `onPlayerJoin`, which the game calls when things happen. The [How-Tos](#howTo) build whole games with it.

## Globals

| Name | Value |
|------|-------|
| `api` | The game API: every method in the [API Reference](#apiReference) |
| `myId` | The ID of the player running the code, or `null` when nobody is, as in most World Code |
| `playerId` | The same as `myId` |
| `thisPos` | The position of the code block or board running the code, as `[x, y, z]` |
| `api.ownerDbId` | The database ID of the world's owner, or `null` if it has none |

## Logs and results

- `api.log` and `console.log` do the same thing: each shows its values in chat as a `Log:` message.
- When the last line of a code block or board has a value, the player who ran it sees that value as a `Result:` message.

## Next steps

[Snippets](#snippets) has short pieces of code to copy into a code block and change.

## Glossary of Referenced Types

These 'types' can't be referenced by your code, but they help explain some of the parameters in the API.

```ts
type CustomTextStyling = (string | EntityName | TranslatedText | StyledIcon | StyledText | ProgressBar | StyledKeyBinding)[]

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

type IngameIconName = "Damage" | "Damage Reduction" | "Speed" | "VoidJump" | "Fist" | "Frozen" | "Hydrated" | "Invisible" | "Jump Boost" | "Poisoned" | "Slowness" | "Weakness" | "Health Regen" | "Healing Pulse" | "Haste" | "Double Jump" | "Heat Resistance" | "Gliding" | "Boating" | "Obsidian Boating" | "Riding" | "Bunny Hop" | "FallDamage" | "Feather Falling" | "Thief" | "X-Ray Vision" | "Mining Yield" | "Brain Rot" | "Rested Damage" | "Rested Haste" | "Rested Speed" | "Rested Farming Yield" | "Rested Aura" | "Blindness" | "Pickpocketer" | "Lifesteal" | "Bounciness" | "Air Walk" | "Wall Climbing" | "Thorns" | "Poopy" | "Glowing" | "Night Vision" | "Irradiated" | "Draugr Knight Head" | "Draugr Warper Head" | "Magma Golem Head" | "Mystery Fish" | "Damage Enchantment" | "Critical Damage Enchantment" | "Attack Speed Enchantment" | "Protection Enchantment" | "Health Enchantment" | "Health Regen Enchantment" | "Stomp Damage Enchantment" | "Knockback Resist Enchantment" | "Arrow Speed Enchantment" | "Arrow Damage Enchantment" | "Quick Charge Enchantment" | "Break Speed Enchantment" | "Momentum Enchantment" | "Mining Yield Enchantment" | "Farming Yield Enchantment" | "Mining Aura Enchantment" | "Digging Aura Enchantment" | "Lumber Aura Enchantment" | "Farming Aura Enchantment" | "Vertical Knockback Enchantment" | "Horizontal Knockback Enchantment" | "Self Yield" | "Friends" | "Riding Speed" | "Feed Aura" | "Double Poop" | "Mob Slayer" | "Rainbow Wool" | "Pack Leader" | "Max Health" | "Poison Claws" | "Mob Yield" | "Antlers Bonus" | "Health" | "HealthShield" | "Cross" | "Friendship" | "Dotted Friendship" | "Hunger" | "Empty Hunger" | "Pixelated Heart" | "Question Mark" | "Trader Black" | "Trader Blue" | "Trader Piggy"

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
    cause: PNull<"Paintball" | "FloorCreator" | "Sapling" | "StemFruit" | "MeltingIce" | "Explosion" | "AutoShape">
}

type DamageInfo = {
    cause: PNull<"Melee" | "Projectile" | "Gun" | "Explosion" | "Spikes" | "Fall" | "Stomp" | "Thorns" | "Effect">
}

type EarthSkyBox = {
    type: "earth"
    inclination?: number
    turbidity?: number
    infiniteDistance?: boolean
    luminance?: number
    // Sky appearance (light intensity); higher = more saturated sky.
    rayleigh?: number
    // Mie scattering coefficient in [0, 0.1]; affects mieDirectionalG impact.
    mieCoefficient?: number
    // Amount of haze particles per Mie scattering theory.
    mieDirectionalG?: number
    // Distance of the sun from the active scene camera.
    distance?: number
    // When `useSunPosition` is true, this overrides `inclination` + `azimuth`.
    sunPosition?: Vec3
    useSunPosition?: boolean
    xCameraOffset?: number
    yCameraOffset?: number
    zCameraOffset?: number
    // Direction the sky considers "up"; defaults to [0, 1, 0].
    up?: Vec3
    // Dither the sky to reduce visible banding.
    dithering?: boolean
    azimuth?: number
    // Not part of sky model by default; heavily tint to a vertex color
    vertexTint?: Vec3
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
    enchant?: {
        tier: EnchantmentTier
        enchantments: Partial<Record<EnchantmentPerk, number>>
        enchantmentData?: Record<string, { icon?: string; description?: string }>
    }
    sell?: boolean // Optional, defaults to false. If true, the sign of "cost" is flipped. So a "cost" of -25 would give the player 25 currency AND be displayed as "25" (instead of -25)
    sortPriority?: number // Descending, bigger number means closer to the top
    hidden?: boolean
}

type ShopItemUserInput =
    | { type: "text"; placeholderText?: string; wordCharsOnly?: boolean; initialValue?: string } // wordCharsOnly defaults to false. If true, only allows \w character (alphanumeric and _). initialValue always takes precedence as the text input value when set.
    | { type: "number"; placeholderText?: string; initialValue?: string }
    | {
            type: "dropdown"
            dropdownOptions: (string | { option: string; cost: number })[]
            shouldResetSelectionOnOptionsChange?: boolean // Defaults to false. If true, the selection will reset to the first option when dropdownOptions changes.
            initialValue?: string
            autoSubmit?: boolean // Defaults to false. If true, the dropdown will automatically submit when the user selects an option.
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
        animateTexture?: boolean // If true, `texture` must be an animated block name (e.g. "Lava", "Red Portal") and the Box cycles through its frames.
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
    Leaderboard: LeaderboardMeshOpts
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

type AnimationTimelineSchema = KeyframeSchema[]

type KeyframeSchema = {
    timeFraction: number
    rotation?: LerpPointSchema // Rotations are assumed to be in radians.
    position?: LerpPointSchema // Position offsets in mesh-local units; (0, 0, 0) means the node's rest pose.
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
    position?: BlockbenchAnimationTimelineSchema // Blockbench position offsets in mesh-local units; rest pose is (0, 0, 0).
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

type CustomButtonType = {
    id: string // Identifier for the button. Used in the callbacks onPlayerPressedButton and onPlayerReleasedButton.
    icon: string // Icon to display for the button. Can be a font awesome icon such as "house" or a bloxd icon such as "Dirt".
    keycode: string // Key to trigger the button. Use a keycode like "KeyR" for the letter R. Allowed keys are alphanumeric and arrow keys.
    description?: string // Optional description for the button explaining what it does.
}

type CustomButtonsType = CustomButtonType[]
```
