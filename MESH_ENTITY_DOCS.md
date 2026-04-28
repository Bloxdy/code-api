# Mesh Entities, Throwables & Node Mesh Attachment

Mesh entities are server-created 3D objects whose position is synced with all clients. Throwables are a specialised subset that use the engine's built-in projectile physics. Node mesh attachment lets you attach additional meshes to specific bones/nodes of an existing entity (e.g. a weapon on a player's hand).

## Limits

- There is a limit to the number of mesh entities and throwables that can be created. `attemptCreateMeshEntity` returns `null` when the limit is reached instead of throwing.
- There are two separate limits: one for mesh entities without physics and one for mesh entities with physics (which is a lot smaller).

---

## Mesh Entities

### Mesh Types

There are four mesh types, each with different options:

- **`Box`**
- **`BloxdBlock`**
- **`Person`**
- **`ParticleEmitter`**

#### `Box`

```ts
type BoxOpts = {
    hideDist?: number
    meshOffset?: number[]
    autoRotate?: boolean
    lineToEId?: EntityId // EntityId to connect to using a line
    width: number
    height: number
    depth: number
    diffuseColor?: number[]
    emissiveColor?: number[]
    backFaceCulling?: boolean // Default true
    texture?: string // Can be a blockname. Wraps every one block
    faceUV?: number[][]
}
```

#### `BloxdBlock`

```ts
type BloxdBlockOpts = {
    hideDist?: number
    meshOffset?: number[]
    autoRotate?: boolean
    lineToEId?: EntityId // EntityId to connect to using a line
    blockName: BlockNameOrId
    size: number | [number, number, number]
}
```

#### `Person`

```ts
type PersonOpts = {
    hideDist?: number
    meshOffset?: number[]
    autoRotate?: boolean
    lineToEId?: EntityId // EntityId to connect to using a line
    size?: number
    textures?: Partial<Cosmetics>
    pose?: PlayerPose
}
```

#### `ParticleEmitter`

```ts
type ParticleEmitterOpts = {
    texture: string
    minLifeTime: number
    maxLifeTime: number
    minEmitPower: number
    maxEmitPower: number
    minSize: number
    maxSize: number
    gravity: number[]
    velocityGradients: VelocityGradient[]
    colorGradients: TimeColorGradient[] | RandomColorGradient[]
    blendMode: ParticleSystemBlendMode
    hideDist?: number
    meshOffset?: number[]
    autoRotate?: boolean
    lineToEId?: EntityId // EntityId to connect to using a line
    height: number
    width: number
    depth: number
    emitRate: number
    dir1?: number[]
    dir2?: number[]
}
```

### Creating a Mesh Entity

```ts
/**
 * Try to create a mesh entity. This creates an entity whose mesh position is synced with clients.
 * Set entity position using setPosition
 * There is a limit to the number of mesh entities and throwables that can be created, with an even smaller limit for mesh entities with physics.
 *
 * @param {MeshType} type
 * @param {MeshEntityOpts[MeshType]} opts
 * @param {string} [name] - The default name for the nametag
 * @param {MeshEntityPhysicsOpts} [physicsOptions] - Physics Options
 * @param {EntityId} [initiatorId] - The entity that initiated the creation of the mesh entity.
 * @returns {PNull<EntityId>} - null if the entity creation failed, otherwise the entity ID.
 */
attemptCreateMeshEntity(type, opts, name, physicsOptions, initiatorId)
```

Use `api.setPosition()` to move the entity after creation.

#### Examples

Create a red box:

```ts
const boxId = api.attemptCreateMeshEntity("Box", {
    width: 1,
    height: 1,
    depth: 1,
    diffuseColor: [255, 0, 0],
})
if (boxId) {
    api.setPosition(boxId, 10, 50, 10)
}
```

Create a Bloxd block mesh:

```ts
const blockId = api.attemptCreateMeshEntity("BloxdBlock", {
    blockName: "Block of Diamond",
    size: 1,
})
```

Create a person mesh:

```ts
const personId = api.attemptCreateMeshEntity("Person", {
    size: 1,
    pose: "standing",
    textures: { head: "trader_black" },
}, "Bedwars Merchant")
```

Create a mesh entity with physics:

```ts
const physicsBoxId = api.attemptCreateMeshEntity(
    "Box",
    { width: 0.5, height: 0.5, depth: 0.5, diffuseColor: [0, 255, 0] },
    "",
    {
        doPhysics: true,
        collidesEntities: true,
        collideBits: 1,
        collideMask: 1,
    },
)
```

### Updating a Mesh Entity

```ts
/**
 * Update a mesh entity. If used on a non-mesh entity, will do nothing.
 *
 * @param {EntityId} eId
 * @param {MeshType} type
 * @param {MeshEntityOpts[MeshType]} opts
 * @returns {void}
 */
updateMeshEntity(eId, type, opts)
```

```ts
api.updateMeshEntity(boxId, "Box", {
    width: 2,
    height: 2,
    depth: 2,
    diffuseColor: [0, 0, 255],
})
```

### Deleting a Mesh Entity

```ts
/**
 * Delete a mesh entity
 *
 * @param {EntityId} eId
 * @returns {boolean}
 */
deleteMeshEntity(eId)
```

```ts
api.deleteMeshEntity(boxId)
```

---

## Throwables

Throwables use the engine's built-in projectile system. Each throwable item has predefined velocity, damage, and gravity. You can multiply these defaults with the `velocityMult`, `damageMult`, and `gravityMult` parameters.

### Creating a Throwable

```ts
/**
 * Try to create a throwable entity.
 * Similar to creating a mesh entity and uses the same rate limiting.
 * However, this uses the predefined throwables system and physics used by throwable items with the game
 * Each throwable item has its own behaviour already, including default velocity, damage and gravity multipliers.
 *
 * @param {EntityId} throwerEId
 * @param {ThrowableItem} itemName - Must be an Item that is usually throwable in-engine
 * @param {[number, number, number]} position - Starting position
 * @param {[number, number, number]} direction
 * @param {number} [velocityMult] - Multiplier for the default velocity of the throwable item
 * @param {number} [damageMult] - Multiplier for the default damage of the throwable item
 * @param {number} [gravityMult] - Multiplier for the default gravity of the throwable item
 * @param {ItemAttributes} [attributes] - item attributes (currently used only for the "Boomerag" item)
 * @returns {string} - null if throwable creation failed, otherwise the entity ID.
 */
attemptCreateThrowable(throwerEId, itemName, position, direction, velocityMult, damageMult, gravityMult, attributes)
```

#### Example

```ts
const pos = api.getPosition(playerId)
const { dir } = api.getPlayerFacingInfo(playerId)
const throwableId = api.attemptCreateThrowable(
    playerId,
    "Fireball",
    [pos[0], pos[1] + 1.5, pos[2]],
    dir,
    2,    // double velocity
    1.5,  // 1.5x damage
)
```

### Deleting a Throwable
Some throwables are deleted automatically when they hit a block or entity, or despawn after a certain time. You can delete them manually with this method.

```ts
/**
 * Delete a throwable entity before it automatically removes itself.
 *
 * @param {EntityId} eId
 * @returns {boolean} - true if the entity was deleted, false if it was not a throwable entity
 */
deleteThrowable(eId)
```

```ts
api.deleteThrowable(throwableId)
```

---

## Node Mesh Attachment

Attach or detach a mesh to a specific node ("bone") of an entity. This is useful for attaching items, effects, or decorations to player body parts or other entity nodes.

```ts
/**
 * Attach/detach mesh instances to/from an entity
 *
 * @param {EntityId} eId
 * @param {EntityNamedNode} node - node to attach to
 * @param {PNull<MeshType>} type - if null, detaches mesh from this node
 * @param {MeshEntityOpts[MeshType]} [opts]
 * @param {[number, number, number]} [offset]
 * @param {[number, number, number]} [rotation]
 * @returns {void}
 */
updateEntityNodeMeshAttachment(eId, node, type, opts, offset, rotation)
```

### Available Nodes

`EntityNamedNode` is one of:

- `"TorsoNode"`
- `"HeadMesh"`
- `"ArmRightMesh"`
- `"ArmLeftMesh"`
- `"LegLeftMesh"`
- `"LegRightMesh"`

### Examples

Attach a block to a player's right arm:

```ts
api.updateEntityNodeMeshAttachment(
    playerId,
    "ArmRightMesh",
    "BloxdBlock",
    { blockName: "Diamond Block", size: 0.3 },
    [0, -0.5, 0],  // offset
    [0, 0, 0],     // rotation
)
```

Attach a glowing box to a player's head:

```ts
api.updateEntityNodeMeshAttachment(
    playerId,
    "HeadMesh",
    "Box",
    { width: 0.3, height: 0.3, depth: 0.3, emissiveColor: [255, 215, 0] },
    [0, 0.5, 0],
)
```

Detach a mesh from a node:

```ts
api.updateEntityNodeMeshAttachment(playerId, "ArmRightMesh", null)
```

---

## Types Glossary

Reference types that you may find useful.

### `MeshEntityOpts`

```ts
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
```

### `CommonMeshEntityOpts`

```ts
type CommonMeshEntityOpts = {
    hideDist?: number
    meshOffset?: number[]
    autoRotate?: boolean
    lineToEId?: EntityId // EntityId to connect to using a line
}
```

### `MeshEntityPhysicsOpts`

```ts
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
```

### `MeshParticleSystemOpts`

```ts
type MeshParticleSystemOpts = ParticleSystemOpts &
    CommonMeshEntityOpts & {
        height: number
        width: number
        depth: number
        emitRate: number
        dir1?: number[]
        dir2?: number[]
    }
```

### `ParticleSystemOpts`

```ts
type ParticleSystemOpts = {
    texture: string
    minLifeTime: number
    maxLifeTime: number
    minEmitPower: number
    maxEmitPower: number
    minSize: number
    maxSize: number
    gravity: number[]
    velocityGradients: VelocityGradient[]
    colorGradients: TimeColorGradient[] | RandomColorGradient[]
    blendMode: ParticleSystemBlendMode
}
```

### `TimeColorGradient`

```ts
type TimeColorGradient = {
    timeFraction: number
    minColor: [number, number, number, number]
    maxColor?: [number, number, number, number]
}
```

### `RandomColorGradient`

```ts
type RandomColorGradient = {
    color: [number, number, number]
}
```

### `VelocityGradient`

```ts
type VelocityGradient = {
    timeFraction: number
    factor: number
    factor2: number
}
```

### `ParticleSystemBlendMode`

```ts
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

### `EntityNamedNode`

Nodes on a player/entity skeleton that meshes can be attached to.

```ts
type EntityNamedNode = "TorsoNode" | "HeadMesh" | "ArmRightMesh" | "ArmLeftMesh" | "LegLeftMesh" | "LegRightMesh"
```
