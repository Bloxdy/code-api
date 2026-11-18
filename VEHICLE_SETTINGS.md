# Vehicle Settings

A vehicle is something you get into and ride, like a boat or a kart. A physics type is a list of settings describing how something moves - its weight, speed, steering, and so on. The two are separate: riding a vehicle normally changes your physics type, but a player can be given one without riding anything.

Each setting's value is resolved in three steps:

1. The defaults, which are the same for everything.
2. The physics type (e.g. `CAR`) changes some of them.
3. The tier (e.g. `OFF_ROADER`) changes a few more.

One vehicle can also be given its own value for a setting, which beats all three - but only for whoever rides it.

A few settings (`canAutoStep`, `speedMultiplier`) can be given a value for one block at a time as well, by naming the block first - `"IceCanAutoStep"`. That applies whenever the rider is stood on that block.

These API methods spawn vehicles and change how a player moves:

```js
/**
 * Set physics state of player (vehicle type and tier).
 *
 * For types that have tiers (e.g. BOAT, GLIDER, CAR), a `tier` of `null` defaults to the first
 * tier (0). Types without tiers (e.g. DEFAULT) must be given a `null` tier.
 *
 * @param {PlayerId} playerId
 * @param {PlayerPhysicsState<PhysicsType>} physicsState
 * @param {[number, number, number]} [positionOffset] - Optional offset to adjust the player's collision box, which is at most 25 blocks wide and 90 tall
 * @returns {void}
 */
setPlayerPhysicsState(playerId, physicsState, positionOffset)

/**
 * Get physics state for player
 *
 * @param {PlayerId} playerId
 * @returns {PlayerPhysicsState<PhysicsType>}
 */
getPlayerPhysicsState(playerId)

/**
 * Seat a player in a vehicle. Seat 0 drives; other seats follow the vehicle without movement control.
 * The player leaves their current vehicle and replaces anyone occupying the requested seat.
 *
 * @param {PlayerId} playerId
 * @param {EntityId} vehicleEId - A spawned vehicle or a rideable mob.
 * @param {number} [seatIndex] - Seat index (default 0).
 * @returns {void}
 */
setPlayerVehicle(playerId, vehicleEId, seatIndex)

/**
 * Take a player off whatever they are riding, dropping them where it is. Does nothing if they are
 * not riding anything.
 *
 * @param {PlayerId} playerId
 * @returns {void}
 */
exitPlayerVehicle(playerId)

/**
 * The seat layout for a vehicle entity - at minimum one driver seat (index 0), optionally more
 * passenger seats after it. Resolved from the spawned vehicle's def (`VehicleMeshEntityDef.seats`,
 * falling back to a single driver seat at the physics type/tier's own `riderOffset` default) or a
 * rideable mob's override (`getMobVehicleSeats`, falling back to its `rideHeight`). Throws if
 * `vehicleEId` is not a vehicle.
 *
 * @param {EntityId} vehicleEId
 * @returns {VehicleSeat[]}
 */
getVehicleSeats(vehicleEId)

/**
 * The current value of a physics setting for a specific vehicle entity: the value set via
 * `setVehicleSetting` if overridden, otherwise (when `returnDefaultIfNotOverridden`) the type/tier
 * default from the vehicle's physics state. Mirrors `getMobSetting`.
 *
 * The one default not read from the type/tier is a mob's `riderOffset`, which comes from its ride height.
 *
 * A per-block setting (e.g. `"IceCanAutoStep"`) is `undefined` when that block is unlisted, which means
 * the block does not change the setting rather than that the setting is off.
 *
 * @param {EntityId} vehicleEId
 * @param {TSetting} setting
 * @param {boolean} [returnDefaultIfNotOverridden]
 * @returns {SettableVehicleSettingValue<TSetting>}
 */
getVehicleSetting(vehicleEId, setting, returnDefaultIfNotOverridden)

/**
 * Override a physics setting for a specific vehicle entity, replacing the type/tier default for that
 * vehicle's rider. The override is stored on the shared Bloxd and, when the vehicle currently has a
 * rider, replicated to that rider's client. Mirrors `setMobSetting`.
 *
 * A setting can also be overridden for one block by naming the block first, e.g.
 * `setVehicleSetting(boatId, "IceCanAutoStep", true)`. That is stored as an entry of the matching
 * `<setting>ByBlock` record, which the physics tick reads as a precomputed block lookup.
 *
 * The rider's body is at most 25 blocks wide and 90 tall: a larger `width` or `height` acts as that.
 *
 * @param {EntityId} vehicleEId
 * @param {TSetting} setting
 * @param {SettableVehicleSettingValue<TSetting>} value
 * @returns {void}
 */
setVehicleSetting(vehicleEId, setting, value)

/**
 * Try to spawn a rideable vehicle, which players can mount with an alt action.
 * There is a limit to the number of mesh entities with physics that can be created.
 * WARNING: Either the "onPlayerAttemptSpawnVehicle" or the "onWorldAttemptSpawnVehicle" game callback will be called
 * depending on whether "spawnerId" is provided. Calling this function inside those callbacks risks infinite recursion.
 *
 * @param {MeshEntityVehicleType} vehicleType
 * @param {number} x
 * @param {number} y
 * @param {number} z
 * @param {VehicleSpawnOpts} [opts] - Includes:
 * - spawnerId The ID of the player who spawned the vehicle. The vehicle faces away from them.
 * @returns {EntityId | null} - null if the vehicle could not be spawned, otherwise the entity ID of the vehicle.
 */
attemptSpawnVehicle(vehicleType, x, y, z, opts)

/**
 * Dispose of a vehicle's state and remove them from the world.
 * Always succeeds.
 *
 * @param {EntityId} vehicleId
 * @returns {void}
 */
despawnVehicle(vehicleId)
```

## Vehicle Catalogue

### Mesh-Entity Vehicles

Objects placed in the world, which a player walks up to and climbs into. Use `attemptSpawnVehicle` to make one.

| Vehicles | Physics | Placeable on | Mesh size | Place sound |
|----------|---------|--------------|-----------|-------------|
| `Boat` | `BOAT` / `WOOD` | `Water`, `Lava` | 1.2 | `splash1` |
| `Obsidian Boat` | `BOAT` / `OBSIDIAN` | `Water`, `Lava` | 1.2 | `splash1` |
| `Speedboat` | `BOAT` / `SPEED` | `Water` | 1.2 | `splash1` |
| `Hovercraft` | `HOVERCRAFT` | `solidBlocksOrFluid` | 4 | depends on the block it is put on |
| `Yellow Kart`, `White Kart`, `Red Kart`, `Purple Kart`, `Pink Kart`, `Orange Kart`, `Magenta Kart`, `Lime Kart`, `Light Gray Kart`, `Light Blue Kart`, `Green Kart`, `Gray Kart`, `Cyan Kart`, `Brown Kart`, `Blue Kart`, `Black Kart` | `CAR` / `KART` | `solidBlocks` | 4 | depends on the block it is put on |
| `Off Roader` | `CAR` / `OFF_ROADER` | `solidBlocks` | 4 | depends on the block it is put on |
| `Gold Mining Drill` | `CAR` / `GOLD_MINING_DRILL` | `solidBlocks` | 4 | depends on the block it is put on |
| `Iron Mining Drill` | `CAR` / `IRON_MINING_DRILL` | `solidBlocks` | 4 | depends on the block it is put on |
| `Diamond Mining Drill` | `CAR` / `DIAMOND_MINING_DRILL` | `solidBlocks` | 4 | depends on the block it is put on |
| `Moonstone Mining Drill` | `CAR` / `MOONSTONE_MINING_DRILL` | `solidBlocks` | 4 | depends on the block it is put on |
| `Armored Car` | `CAR` / `ARMORED_CAR` | `solidBlocks` | 4 | depends on the block it is put on |
| `Light Blue Car` | `CAR` / `CAR` | `solidBlocks` | 4 | depends on the block it is put on |

### Consumable Vehicles

Nothing is placed in the world: the player's own movement changes and the vehicle appears attached to their body. Using the item consumes one, and the ride ends once the player stops floating.

| Vehicles | Physics | Mesh attachment |
|----------|---------|-----------------|
| `Yellow Balloon` | `BALLOON` / `YELLOW` | `{ size: 0.12, offset: [0, 1.8, -0.24], rotation: [0, 0, 0] }` |
| `White Balloon` | `BALLOON` / `WHITE` | `{ size: 0.12, offset: [0, 1.8, -0.24], rotation: [0, 0, 0] }` |
| `Red Balloon` | `BALLOON` / `RED` | `{ size: 0.12, offset: [0, 1.8, -0.24], rotation: [0, 0, 0] }` |
| `Purple Balloon` | `BALLOON` / `PURPLE` | `{ size: 0.12, offset: [0, 1.8, -0.24], rotation: [0, 0, 0] }` |
| `Pink Balloon` | `BALLOON` / `PINK` | `{ size: 0.12, offset: [0, 1.8, -0.24], rotation: [0, 0, 0] }` |
| `Orange Balloon` | `BALLOON` / `ORANGE` | `{ size: 0.12, offset: [0, 1.8, -0.24], rotation: [0, 0, 0] }` |
| `Magenta Balloon` | `BALLOON` / `MAGENTA` | `{ size: 0.12, offset: [0, 1.8, -0.24], rotation: [0, 0, 0] }` |
| `Lime Balloon` | `BALLOON` / `LIME` | `{ size: 0.12, offset: [0, 1.8, -0.24], rotation: [0, 0, 0] }` |
| `Light Gray Balloon` | `BALLOON` / `LIGHT_GRAY` | `{ size: 0.12, offset: [0, 1.8, -0.24], rotation: [0, 0, 0] }` |
| `Light Blue Balloon` | `BALLOON` / `LIGHT_BLUE` | `{ size: 0.12, offset: [0, 1.8, -0.24], rotation: [0, 0, 0] }` |
| `Green Balloon` | `BALLOON` / `GREEN` | `{ size: 0.12, offset: [0, 1.8, -0.24], rotation: [0, 0, 0] }` |
| `Gray Balloon` | `BALLOON` / `GRAY` | `{ size: 0.12, offset: [0, 1.8, -0.24], rotation: [0, 0, 0] }` |
| `Cyan Balloon` | `BALLOON` / `CYAN` | `{ size: 0.12, offset: [0, 1.8, -0.24], rotation: [0, 0, 0] }` |
| `Brown Balloon` | `BALLOON` / `BROWN` | `{ size: 0.12, offset: [0, 1.8, -0.24], rotation: [0, 0, 0] }` |
| `Blue Balloon` | `BALLOON` / `BLUE` | `{ size: 0.12, offset: [0, 1.8, -0.24], rotation: [0, 0, 0] }` |
| `Black Balloon` | `BALLOON` / `BLACK` | `{ size: 0.12, offset: [0, 1.8, -0.24], rotation: [0, 0, 0] }` |

### Held Vehicles

Also attached to the player rather than placed in the world. Active for as long as the matching item is held, from waking if taken out in bed - swap to something else and the ride ends.

| Vehicles | Physics | Mesh attachment |
|----------|---------|-----------------|
| `Wood Hang Glider` | `GLIDER` / `WOOD` | `{ size: 3, offset: [0, 0.72, 0.3], rotation: [-1.5708, 3.14159, 3.14159] }` |
| `Iron Hang Glider` | `GLIDER` / `IRON` | `{ size: 3, offset: [0, 0.72, 0.3], rotation: [-1.5708, 3.14159, 3.14159] }` |
| `Gold Hang Glider` | `GLIDER` / `GOLD` | `{ size: 3, offset: [0, 0.72, 0.3], rotation: [-1.5708, 3.14159, 3.14159] }` |
| `Diamond Hang Glider` | `GLIDER` / `DIAMOND` | `{ size: 3, offset: [0, 0.72, 0.3], rotation: [-1.5708, 3.14159, 3.14159] }` |

Every vehicle setting and the default it starts from. Valid `pose` values are listed in `SKINS_AND_POSES.md`, and valid `effect.icon` values in `ICONS.md`.

Each example changes the setting on one vehicle, where the id came from `attemptSpawnVehicle`.

## airborneFallSpeedLimit

**Type:** `FallSpeedLimitOpts | null`

**Default:** `null`

Caps how fast it falls while flying. Only used when `airborneMovement` is `"heading"`.
`null` = it falls at full speed. See `FallSpeedLimitOpts`.

```js
// A flying hovercraft that drifts down at 4 blocks a second instead of dropping
api.setVehicleSetting(hovercraftId, "airborneMovement", { model: "heading" })
api.setVehicleSetting(hovercraftId, "airborneFallSpeedLimit", { maxFallSpeed: 4, slowDownSeconds: 0.15, compensateGravity: true })
```

## airborneMode

**Type:** `AirborneModeOpts | null`

**Default:** `null`

Lets it fly once mid-air and descending. Supported movement types: `GLIDING` and `FLOATING`.
`null` = it cannot fly. See `AirborneModeOpts`.

```js
// Give a hovercraft a glider mode, so driving off a cliff becomes a flight
api.setVehicleSetting(hovercraftId, "airborneMode", { activeMovementType: MovementType.GLIDING, durationMs: null, exitsVehicleOnEnd: false })
```

## airborneMovement

**Type:** `AirborneMovementOpts | null`

**Default:** `null`

How it moves while flying. `null` = it steers the same way it does on the ground. See `AirborneMovementOpts`.

```js
// Make a flying hovercraft follow the camera, like a glider
api.setVehicleSetting(hovercraftId, "airborneMovement", {
    model: "cameraDirection",
    maxSpeed: 13,
    minSpeed: 0.5,
    pitchAcceleration: 0.0125,
    friction: 0,
    impulseDecayTicks: 100,
    biasMagnitude: 1.5,
    excessVelocityBleedPerTick: 0.5,
})
```

## airSpeedMultiplier

**Type:** `number`

**Default:** `1`

An extra speed multiplier used while in the air.

```js
// A kart that barely steers while mid-jump
api.setVehicleSetting(kartId, "airSpeedMultiplier", 0.2)
```

## canAutoStep

**Type:** `boolean`

**Default:** `true`

Can also be set for a single block - see `{BlockName}CanAutoStep`.

Whether it climbs one-block steps by itself, rather than the rider jumping them.

```js
// Make riders jump every kerb instead of rolling over it
api.setVehicleSetting(carId, "canAutoStep", false)
```

## {BlockName}CanAutoStep

**Type:** `boolean`

**Default:** unset - only the blocks a physics type lists in its `canAutoStepByBlock` below start with a value of their own.

Named as the block followed by the setting: `"IceCanAutoStep"`, `"Red ConcreteCanAutoStep"`.

Overrides `canAutoStep` for particular blocks, by block name. Unlisted blocks use `canAutoStep`.

```js
// A boat that rides up onto ice, but nothing else
api.setVehicleSetting(boatId, "canAutoStep", false)
api.setVehicleSetting(boatId, "IceCanAutoStep", true)
```

## canRun

**Type:** `boolean`

**Default:** `true`

Whether the rider is allowed to sprint.

```js
// Let riders sprint a kart, on top of its own speed
api.setVehicleSetting(kartId, "canRun", true)
```

## disableBaseMovement

**Type:** `boolean`

**Default:** `false`

Turns off the usual steering-driven movement, as for a sleeping player.

```js
// A ghost train: riders cannot steer at all
api.setVehicleSetting(kartId, "disableBaseMovement", true)
```

## disableFootstepSounds

**Type:** `boolean`

**Default:** `false`

Whether footstep sounds are silenced.

```js
// Turn footsteps back on for a kart
api.setVehicleSetting(kartId, "disableFootstepSounds", false)
```

## effect

**Type:** ` { name: string; icon: string; duration?: number } | null `

**Default:** `null`

A badge shown to the rider for as long as they ride. `null` = none. `icon` is an ingame icon or an
item name. `name` must be one that a physics type already uses, such as `"Driving"` or `"Boating"`,
because only those are cleared again when the rider gets off.

```js
// Badge a kart with the off-roader icon instead of the usual kart one
api.setVehicleSetting(kartId, "effect", { name: "Driving", icon: "Off Roader" })
```

## fixedHeading

**Type:** `number | null`

**Default:** `null`

Locks the direction it faces, in radians, ignoring the camera. `null` = it faces wherever the camera or steering points.

```js
// Pin a boat to one direction, whichever way the rider looks
api.setVehicleSetting(boatId, "fixedHeading", Math.PI / 2)
```

## fluidDrag

**Type:** `number`

**Default:** `-1`

How much water and lava slow it down. `-1` uses the world's normal drag.

```js
// A boat that wallows through water
api.setVehicleSetting(boatId, "fluidDrag", 8)
```

## fluidDragHorizontal

**Type:** `number`

**Default:** `-1`

`fluidDrag` for sideways movement only. `-1` falls back to `fluidDrag`.

```js
// Let a car skate across water instead of bogging down in it
api.setVehicleSetting(carId, "fluidDragHorizontal", 0)
```

## fluidDragVertical

**Type:** `number`

**Default:** `-1`

`fluidDrag` for up-and-down movement only. `-1` falls back to `fluidDrag`.

```js
// A boat that sinks and surfaces slowly instead of bobbing
api.setVehicleSetting(boatId, "fluidDragVertical", 6)
```

## fluidSkip

**Type:** `FluidSkipOpts | null`

**Default:** `null`

Makes it skip across water like a stone once it is fast enough. `null` = it floats normally. See `FluidSkipOpts`.

```js
// Let any boat skim across water like a stone
api.setVehicleSetting(boatId, "fluidSkip", { minSpeed: 11, launchSpeedFraction: 0.4, launchApproachRate: 25 })
```

## fluidSpeedMultiplier

**Type:** `number`

**Default:** `1`

An extra speed multiplier used while in water or lava.

```js
// An amphibious kart, as quick in water as on the road
api.setVehicleSetting(kartId, "fluidSpeedMultiplier", 1)
```

## gravityMultiplier

**Type:** `number`

**Default:** `2`

How hard gravity pulls it down, compared to a normal player. `0` = it never falls.

```js
// A moon-buggy kart that hangs in the air after every jump
api.setVehicleSetting(kartId, "gravityMultiplier", 0.5)
```

## height

**Type:** `number`

**Default:** `1.8`

How tall it is, in blocks.

```js
// A low-slung go-kart that fits under two-block tunnels
api.setVehicleSetting(kartId, "height", 0.9)
```

## horizontalBounciness

**Type:** `number`

**Default:** `0`

How bouncy walls are: `0` stops dead, `1` keeps all its speed. Stacks with the `bounciness` client
option. Rebounds have a floor speed, so slow bumps come back faster than they arrived.

```js
// A bumper car that pings off walls instead of stopping dead
api.setVehicleSetting(kartId, "horizontalBounciness", 0.8)
```

## horizontalImpactCameraShake

**Type:** `ImpactCameraShakeOpts | null`

**Default:** `null`

Shakes the camera when it hits a wall hard enough; it need not bounce. `null` = no shake. See `ImpactCameraShakeOpts`.

```js
// Rattle the screen when a car wraps itself round a wall
api.setVehicleSetting(carId, "horizontalImpactCameraShake", { minSpeed: 6, intensityPerSpeed: 0.03, maxIntensity: 0.6, durationMs: 300 })
```

## jumpMultiplier

**Type:** `number`

**Default:** `1`

How hard the rider pushes off the ground when jumping, compared to a normal jump. `0` = cannot jump.
Multiplies the `jumpAmount` client option rather than replacing it, and heavier types need a bigger
value to reach the same height.

```js
// A stunt kart with a much springier jump
api.setVehicleSetting(kartId, "jumpMultiplier", 5)
```

## landSpeedMultiplier

**Type:** `number`

**Default:** `1`

An extra speed multiplier used while on solid ground.

```js
// A boat that drags itself along dry land at a tenth speed
api.setVehicleSetting(boatId, "landSpeedMultiplier", 0.1)
```

## mass

**Type:** `number`

**Default:** `1`

How heavy it is. Heavier things resist being pushed around, and sink rather than float.

```js
// A featherweight kart that every explosion sends flying
api.setVehicleSetting(kartId, "mass", 0.3)
```

## maxPushDistance

**Type:** `number`

**Default:** `0`

Caps how hard it is pushed while far below the speed it is aiming for, so it takes longer to get going. `0` = no cap.

```js
// A heavy lorry takes its time getting up to speed; a nippy racecar has no cap at all
api.setVehicleSetting(lorryId, "maxPushDistance", 2)
api.setVehicleSetting(raceCarId, "maxPushDistance", 0)
```

## minHorizontalSpeedToBounce

**Type:** `number`

**Default:** `0`

How fast it must hit a wall, in blocks per second, before `horizontalBounciness` applies. `0` = any contact.

```js
// Only bounce off walls hit at speed, not gentle scrapes
api.setVehicleSetting(kartId, "minHorizontalSpeedToBounce", 8)
```

## minVerticalSpeedToBounce

**Type:** `number`

**Default:** `0`

How fast it must be falling, in blocks per second, before `verticalBounciness` applies. `0` bounces
off any contact, leaving a bouncy vehicle jiggling in place.

```js
// Bounce off big drops only, so a parked kart sits still
api.setVehicleSetting(kartId, "minVerticalSpeedToBounce", 5)
```

## movementForceMultiplier

**Type:** `number`

**Default:** `1`

How hard steering pushes it. Bigger = it speeds up and changes direction more sharply.

```js
// A twitchy kart that reaches top speed almost instantly
api.setVehicleSetting(kartId, "movementForceMultiplier", 2)
```

## pose

**Type:** `PlayerPose`

**Default:** `"standing"`

How the rider's body is posed while riding.

```js
// Riders lie back in this boat instead of sitting up
api.setVehicleSetting(boatId, "pose", "sleeping")
```

## riderOffset

**Type:** `[number, number, number]`

**Default:** `[0, 0, 0]`

Where the rider sits, as `[x, y, z]` blocks from the middle of the vehicle. A rideable mob defaults
to its own ride height rather than to this type's value.

```js
// Sit the rider further back, as if in the boot
api.setVehicleSetting(carId, "riderOffset", [0, 0.25, -0.8])
```

## speedMultiplier

**Type:** `number`

**Default:** `1`

Can also be set for a single block - see `{BlockName}SpeedMultiplier`.

Its top speed, as a multiplier on normal walking speed. `0` = it cannot move.

```js
// Double a kart's top speed for a boost pickup
api.setVehicleSetting(kartId, "speedMultiplier", 24)
```

## {BlockName}SpeedMultiplier

**Type:** `number`

**Default:** unset - only the blocks a physics type lists in its `speedMultiplierByBlock` below start with a value of their own.

Named as the block followed by the setting: `"IceSpeedMultiplier"`, `"Red ConcreteSpeedMultiplier"`.

Extra speed multipliers for the block underfoot, by block name. Solid ground only, and stacks with
`speedMultiplier` and the land/fluid/air multiplier. When several listed blocks are underfoot, the
value furthest from `1` wins.

```js
// A car that crawls through snow and flies down stone roads
api.setVehicleSetting(carId, "SnowSpeedMultiplier", 0.4)
api.setVehicleSetting(carId, "StoneSpeedMultiplier", 1.5)
```

## standingFriction

**Type:** `number`

**Default:** `5`

How quickly it slows to a stop once the rider stops steering. Bigger = stops sooner.

```js
// An icy kart that slides on after the rider stops steering
api.setVehicleSetting(kartId, "standingFriction", 0.2)
```

## steering

**Type:** `SteeringOpts | null`

**Default:** `null`

Makes it steer like a car: left and right turn it rather than sliding it sideways. `null` moves
freely in any direction, like a walking player. See `SteeringOpts`.

```js
// A drift kart: quick to turn, but the back end steps out on every corner
api.setVehicleSetting(kartId, "steering", { turnRate: 3.5, corneringSpeedDamping: 0.1, gripStrength: 2, gripInFluid: false })
```

## upwardImpulseOnUse

**Type:** `number`

**Default:** `0`

Cannot be changed for a single vehicle - it is always read from the physics type's own values.

An upward shove the moment the rider uses the vehicle, like a balloon lifting off. `0` = none.

```js
// In physics/settings/*.ts: a balloon that launches its rider higher
upwardImpulseOnUse: 30
```

## verticalBounciness

**Type:** `number`

**Default:** `0`

How bouncy the floor is: `0` lands flat, `1` bounces back as fast as it fell. Stacks with the
`bounciness` client option, and has the same rebound floor as `horizontalBounciness`.

```js
// A space hopper kart that bounces every time it lands
api.setVehicleSetting(kartId, "verticalBounciness", 0.7)
```

## verticalImpactCameraShake

**Type:** `ImpactCameraShakeOpts | null`

**Default:** `null`

Shakes the camera when it hits a floor or ceiling hard enough; it need not bounce. `null` = no shake. See `ImpactCameraShakeOpts`.

```js
// A bone-shaking thump when an off-roader lands a big jump
api.setVehicleSetting(offRoaderId, "verticalImpactCameraShake", { minSpeed: 3, intensityPerSpeed: 0.05, maxIntensity: 1, durationMs: 400 })
```

## width

**Type:** `number`

**Default:** `0.5`

How wide it is, in blocks.

```js
// A monster truck too wide for one-block alleys
api.setVehicleSetting(offRoaderId, "width", 1.2)
```

## Physics Type Defaults

Only the changes are shown - anything not listed keeps the value from the step before.

### DEFAULT

A player walking around on foot. Every other type starts from these values.

Uses the default values, with nothing changed.

### BOAT

Floats on water and slides over ice. The fast tier can steer, and skips along the surface like a stone.

```ts
mass: 0.25
fluidDrag: 2
fluidDragHorizontal: 0
fluidDragVertical: 2
height: 1.2
canRun: false
jumpMultiplier: 0
canAutoStep: false
canAutoStepByBlock: {
    Ice: true,
    "Ice Bricks": true,
    "Ice Bricks Slab": true,
    "Melting Ice": true,
    "Melting Ice|Breaking": true,
}
pose: "sitting"
effect: { name: "Boating", icon: "Boating" }
standingFriction: 0.4
speedMultiplier: 0.25
fluidSpeedMultiplier: 40
airSpeedMultiplier: 32
speedMultiplierByBlock: { Ice: 40, "Ice Bricks": 40, "Ice Bricks Slab": 40, "Melting Ice": 40, "Melting Ice|Breaking": 40 }
maxPushDistance: 7.3
movementForceMultiplier: 0.03
disableFootstepSounds: true
```

#### Tier `WOOD`

The same as `BOAT` above, with nothing changed.

#### Tier `OBSIDIAN`

```ts
effect: { name: "Boating", icon: "Obsidian Boating" }
```

#### Tier `SPEED`

```ts
fluidDragVertical: 1
fluidSpeedMultiplier: 60
airSpeedMultiplier: 56
movementForceMultiplier: 0.07
horizontalBounciness: 0.2
steering: { turnRate: 1.6, corneringSpeedDamping: 0.2, gripStrength: 4.5, gripInFluid: true }
fluidSkip: { minSpeed: 11, launchSpeedFraction: 0.4, launchApproachRate: 25 }
```

### GLIDER

Glides where you look while mid-air and falling, for as long as the glider is held.

```ts
gravityMultiplier: 0
height: 1.4
pose: "gliding"
effect: { name: "Gliding", icon: "Gliding" }
airborneMode: { activeMovementType: MovementType.GLIDING, durationMs: null, exitsVehicleOnEnd: false }
airborneMovement: {
    model: "cameraDirection",
    maxSpeed: 13,
    minSpeed: 0.5,
    pitchAcceleration: 0.0125,
    friction: 0,
    impulseDecayTicks: 100,
    biasMagnitude: 1.5,
    excessVelocityBleedPerTick: 0.5,
}
```

#### Tier `WOOD`

The same as `GLIDER` above, with nothing changed.

#### Tier `IRON`

```ts
airborneMovement: {
    model: "cameraDirection",
    maxSpeed: 17,
    minSpeed: 0.5,
    pitchAcceleration: 0.0125,
    friction: 0,
    impulseDecayTicks: 100,
    biasMagnitude: 1.5,
    excessVelocityBleedPerTick: 0.5,
}
```

#### Tier `GOLD`

```ts
airborneMovement: {
    model: "cameraDirection",
    maxSpeed: 21,
    minSpeed: 0.5,
    pitchAcceleration: 0.0125,
    friction: 0,
    impulseDecayTicks: 100,
    biasMagnitude: 1.5,
    excessVelocityBleedPerTick: 0.5,
}
```

#### Tier `DIAMOND`

```ts
airborneMovement: {
    model: "cameraDirection",
    maxSpeed: 25,
    minSpeed: 0.5,
    pitchAcceleration: 0.0125,
    friction: 0,
    impulseDecayTicks: 100,
    biasMagnitude: 1.5,
    excessVelocityBleedPerTick: 0.5,
}
```

### BALLOON

Shoves you upwards on use, then drifts back down until the time runs out.

```ts
gravityMultiplier: 0
upwardImpulseOnUse: 20
effect: { name: "Floating", icon: "Red Balloon", duration: 16000 }
airborneMode: { activeMovementType: MovementType.FLOATING, durationMs: 16000, exitsVehicleOnEnd: true }
airborneMovement: { model: "heading" }
airborneFallSpeedLimit: { maxFallSpeed: 3, slowDownSeconds: 0.15, compensateGravity: true }
```

#### Tier `YELLOW`

```ts
effect: { name: "Floating", icon: "Yellow Balloon", duration: 16000 }
```

#### Tier `WHITE`

```ts
effect: { name: "Floating", icon: "White Balloon", duration: 16000 }
```

#### Tier `RED`

The same as `BALLOON` above, with nothing changed.

#### Tier `PURPLE`

```ts
effect: { name: "Floating", icon: "Purple Balloon", duration: 16000 }
```

#### Tier `PINK`

```ts
effect: { name: "Floating", icon: "Pink Balloon", duration: 16000 }
```

#### Tier `ORANGE`

```ts
effect: { name: "Floating", icon: "Orange Balloon", duration: 16000 }
```

#### Tier `MAGENTA`

```ts
effect: { name: "Floating", icon: "Magenta Balloon", duration: 16000 }
```

#### Tier `LIME`

```ts
effect: { name: "Floating", icon: "Lime Balloon", duration: 16000 }
```

#### Tier `LIGHT_GRAY`

```ts
effect: { name: "Floating", icon: "Light Gray Balloon", duration: 16000 }
```

#### Tier `LIGHT_BLUE`

```ts
effect: { name: "Floating", icon: "Light Blue Balloon", duration: 16000 }
```

#### Tier `GREEN`

```ts
effect: { name: "Floating", icon: "Green Balloon", duration: 16000 }
```

#### Tier `GRAY`

```ts
effect: { name: "Floating", icon: "Gray Balloon", duration: 16000 }
```

#### Tier `CYAN`

```ts
effect: { name: "Floating", icon: "Cyan Balloon", duration: 16000 }
```

#### Tier `BROWN`

```ts
effect: { name: "Floating", icon: "Brown Balloon", duration: 16000 }
```

#### Tier `BLUE`

```ts
effect: { name: "Floating", icon: "Blue Balloon", duration: 16000 }
```

#### Tier `BLACK`

```ts
effect: { name: "Floating", icon: "Black Balloon", duration: 16000 }
```

### SLEEPING

A player lying in a bed. They face whichever way the bed points, so there is one tier per bed direction.

```ts
canRun: false
jumpMultiplier: 0
pose: "sleeping"
effect: { name: "Sleeping", icon: "Red Bed" }
speedMultiplier: 0
movementForceMultiplier: 0
disableBaseMovement: true
```

#### Tier `ROTATION_1`

```ts
fixedHeading: 0
```

#### Tier `ROTATION_2`

```ts
fixedHeading: 1.5708
```

#### Tier `ROTATION_3`

```ts
fixedHeading: 3.14159
```

#### Tier `ROTATION_4`

```ts
fixedHeading: -1.5708
```

### RIDING_MOB

A player riding an animal. The animal does the moving, so the player's own movement is switched off.

```ts
fluidDrag: 4
gravityMultiplier: 6
height: 1.5
pose: "riding"
effect: { name: "Riding", icon: "Riding" }
speedMultiplier: 2.25
movementForceMultiplier: 0.9
```

### CAR

Quick on solid ground and hopeless in water. It steers like a car instead of sliding sideways.

```ts
mass: 2.5
fluidDrag: 1
fluidDragHorizontal: 1
fluidDragVertical: 0
gravityMultiplier: 3
width: 0.25
height: 1.1
canRun: false
jumpMultiplier: 2.25
riderOffset: [0, 0.25, -0.5]
pose: "driving"
effect: { name: "Driving", icon: "Red Kart" }
standingFriction: 2
speedMultiplier: 12
fluidSpeedMultiplier: 0.0333333
maxPushDistance: 10
movementForceMultiplier: 0.5
disableFootstepSounds: true
horizontalBounciness: 0.6
steering: { turnRate: 3, corneringSpeedDamping: 0.25, gripStrength: 10, gripInFluid: false }
```

#### Tier `KART`

The same as `CAR` above, with nothing changed.

#### Tier `OFF_ROADER`

```ts
mass: 3
gravityMultiplier: 4
jumpMultiplier: 3
riderOffset: [0, 0.25, -0.2]
effect: { name: "Driving", icon: "Off Roader" }
speedMultiplier: 10
fluidSpeedMultiplier: 0.04
horizontalBounciness: 0.3
verticalBounciness: 0.4
minVerticalSpeedToBounce: 2
verticalImpactCameraShake: { minSpeed: 4, intensityPerSpeed: 0.025, maxIntensity: 0.5, durationMs: 250 }
steering: { turnRate: 2.2, corneringSpeedDamping: 0.25, gripStrength: 5, gripInFluid: false }
```

#### Tier `CAR`

```ts
mass: 2.75
gravityMultiplier: 3.5
jumpMultiplier: 2.625
riderOffset: [0, 0, -0.25]
effect: { name: "Driving", icon: "Light Blue Car" }
speedMultiplier: 11
fluidSpeedMultiplier: 0.0367
speedMultiplierByBlock: {
    Dirt: 0.6,
    "Messy Dirt": 0.6,
    "Rocky Dirt": 0.6,
    "Grass Block": 0.6,
    "Pine Grass Block": 0.6,
    "Jungle Grass Block": 0.6,
    "Overgrown Grass Block": 0.6,
    "Overgrown Pine Grass Block": 0.6,
    "Overgrown Jungle Grass Block": 0.6,
    "Dry Grass Block": 0.6,
    "Overgrown Dry Grass Block": 0.6,
    Sand: 0.6,
    "Red Sand": 0.6,
    Gravel: 0.6,
    Clay: 0.6,
    Snow: 0.6,
    "Packed Snow": 0.6,
}
horizontalBounciness: 0.45
minVerticalSpeedToBounce: 2
verticalImpactCameraShake: { minSpeed: 4, intensityPerSpeed: 0.0125, maxIntensity: 0.25, durationMs: 250 }
steering: { turnRate: 2.6, corneringSpeedDamping: 0.25, gripStrength: 7.5, gripInFluid: false }
```

#### Tier `GOLD_MINING_DRILL`

```ts
mass: 3
gravityMultiplier: 4
jumpMultiplier: 3
riderOffset: [0, 0.25, -0.2]
effect: { name: "Driving", icon: "Gold Mining Drill" }
speedMultiplier: 5
fluidSpeedMultiplier: 0.02
horizontalBounciness: 0
verticalBounciness: 0.4
minVerticalSpeedToBounce: 2
verticalImpactCameraShake: { minSpeed: 4, intensityPerSpeed: 0.025, maxIntensity: 0.5, durationMs: 250 }
steering: { turnRate: 1.65, corneringSpeedDamping: 0.25, gripStrength: 5, gripInFluid: false }
```

#### Tier `ARMORED_CAR`

```ts
mass: 4.5
gravityMultiplier: 4
jumpMultiplier: 2.5
riderOffset: [0, 0.25, -0.2]
effect: { name: "Driving", icon: "Armored Car" }
standingFriction: 1.6
speedMultiplier: 9
fluidSpeedMultiplier: 0.0444444
horizontalBounciness: 0.15
verticalBounciness: 0.15
minVerticalSpeedToBounce: 2
verticalImpactCameraShake: { minSpeed: 4, intensityPerSpeed: 0.03, maxIntensity: 0.5, durationMs: 300 }
steering: { turnRate: 1.6, corneringSpeedDamping: 0.35, gripStrength: 6, gripInFluid: false }
```

#### Tier `IRON_MINING_DRILL`

```ts
mass: 3
gravityMultiplier: 4
jumpMultiplier: 3
riderOffset: [0, 0.25, -0.2]
effect: { name: "Driving", icon: "Iron Mining Drill" }
speedMultiplier: 4
fluidSpeedMultiplier: 0.02
horizontalBounciness: 0
verticalBounciness: 0.4
minVerticalSpeedToBounce: 2
verticalImpactCameraShake: { minSpeed: 4, intensityPerSpeed: 0.025, maxIntensity: 0.5, durationMs: 250 }
steering: { turnRate: 1.65, corneringSpeedDamping: 0.25, gripStrength: 5, gripInFluid: false }
```

#### Tier `DIAMOND_MINING_DRILL`

```ts
mass: 3
gravityMultiplier: 4
jumpMultiplier: 3
riderOffset: [0, 0.25, -0.2]
effect: { name: "Driving", icon: "Diamond Mining Drill" }
speedMultiplier: 6
fluidSpeedMultiplier: 0.02
horizontalBounciness: 0
verticalBounciness: 0.4
minVerticalSpeedToBounce: 2
verticalImpactCameraShake: { minSpeed: 4, intensityPerSpeed: 0.025, maxIntensity: 0.5, durationMs: 250 }
steering: { turnRate: 1.65, corneringSpeedDamping: 0.25, gripStrength: 5, gripInFluid: false }
```

#### Tier `MOONSTONE_MINING_DRILL`

```ts
mass: 3
gravityMultiplier: 4
jumpMultiplier: 3
riderOffset: [0, 0.25, -0.2]
effect: { name: "Driving", icon: "Moonstone Mining Drill" }
speedMultiplier: 7
fluidSpeedMultiplier: 0.02
horizontalBounciness: 0
verticalBounciness: 0.4
minVerticalSpeedToBounce: 2
verticalImpactCameraShake: { minSpeed: 4, intensityPerSpeed: 0.025, maxIntensity: 0.5, durationMs: 250 }
steering: { turnRate: 1.65, corneringSpeedDamping: 0.25, gripStrength: 5, gripInFluid: false }
```

### HOVERCRAFT

Skims over both land and water, and slides sideways easily because it has no grip.

```ts
mass: 0.5
fluidDrag: 2
fluidDragHorizontal: 0
fluidDragVertical: 2
gravityMultiplier: 3
height: 1.2
canRun: false
jumpMultiplier: 0.7
riderOffset: [0, 0.25, -0.25]
pose: "driving"
effect: { name: "Hovering", icon: "Hovercraft" }
standingFriction: 0.4
speedMultiplier: 9
maxPushDistance: 6
movementForceMultiplier: 0.11
disableFootstepSounds: true
```

## Types Glossary

The object shapes taken by some of the settings above.

### `SteeringOpts`

```ts
type SteeringOpts = {
    /** How fast it turns, in radians per second. Bigger = snappier steering and tighter corners. */
    turnRate: number
    /** How much speed a hard corner costs, from `0` (none) to `1` (all of it). */
    corneringSpeedDamping: number
    /**
     * How much it grips the ground. `0` slides sideways like a hovercraft on ice; bigger values make it
     * follow its nose round corners. Solid ground only, unless `gripInFluid` is on.
     */
    gripStrength: number
    /** Whether it also grips while floating in water, so a boat can corner like a kart. */
    gripInFluid: boolean
}
```

### `FluidSkipOpts`

```ts
type FluidSkipOpts = {
    /** How fast it must be going, in blocks per second, to start skipping. */
    minSpeed: number
    /** How much of its speed above `minSpeed` becomes upward launch speed. Bigger = higher skips. */
    launchSpeedFraction: number
    /** How snappily it leaves the water. Bigger = a sharp pop; smaller = a slow heave. */
    launchApproachRate: number
}
```

### `ImpactCameraShakeOpts`

```ts
type ImpactCameraShakeOpts = {
    /** The crash speed, in blocks per second, needed to shake at all, so small bumps are ignored. */
    minSpeed: number
    /** How much shake each extra block per second adds. Shake runs from `0` (still) to `1` (violent). */
    intensityPerSpeed: number
    /** The most shake one crash can cause. */
    maxIntensity: number
    /** How long the shake lasts, in milliseconds. */
    durationMs: number
}
```

### `AirborneModeOpts`

```ts
type AirborneModeOpts = {
    /** Which movement type to switch to: `GLIDING` for gliders, `FLOATING` for balloons. */
    activeMovementType: MovementType
    /** How long the flying lasts, in milliseconds, before it drops the rider. `null` = no limit. */
    durationMs: number | null
    /** Whether the flying ending also throws the rider off, as a popping balloon does. */
    exitsVehicleOnEnd: boolean
}
```

### `AirborneMovementOpts`

```ts
type AirborneMovementOpts = HeadingAirborneMovement | CameraDirectionAirborneMovement
```

### `HeadingAirborneMovement`

```ts
type HeadingAirborneMovement = { model: "heading" }
```

### `CameraDirectionAirborneMovement`

```ts
type CameraDirectionAirborneMovement = {
    model: "cameraDirection"
    /** The fastest it can fly. Better gliders get a bigger value, and dive and climb more sharply. */
    maxSpeed: number
    /** The slowest it can fly. It never stalls below this, however steeply it climbs. */
    minSpeed: number
    /** How much looking up or down changes its speed. Bigger = dives gain speed quickly. */
    pitchAcceleration: number
    /** How much drag slows it each tick. `0` = none. */
    friction: number
    /** How many ticks an outside shove - a fuel boost, or knockback - keeps its momentum before flight takes over again. */
    impulseDecayTicks: number
    /** How strongly it is nudged forwards, then gently downwards, so a level glider sinks slowly. */
    biasMagnitude: number
    /** How much leftover speed from a shove is shed each tick. */
    excessVelocityBleedPerTick: number
}
```

### `FallSpeedLimitOpts`

```ts
type FallSpeedLimitOpts = {
    /** The fastest it may fall, in blocks per second. */
    maxFallSpeed: number
    /** Roughly how long, in seconds, it takes to slow back to the limit. Smaller = a firmer catch. */
    slowDownSeconds: number
    /** Whether to also cancel gravity while holding it back, so the limit is not slowly overpowered. */
    compensateGravity: boolean
}
```

