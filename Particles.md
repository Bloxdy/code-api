# Particles

These are the strings you can give to functions that take a particle effect `texture` as input:

`bubble`
`critical_hit`
`drift`
`effect_5`
`generic_2`
`glint`
`soul_0`
`square_particle`
`heart`
`z-particle`

Here's the code for an example particle effect:

```ts
let [x, y, z] = thisPos
y += 1
api.playParticleEffect({
    dir1: [-1, -1, -1],
    dir2: [1, 1, 1],
    pos1: [x, y, z],
    pos2: [x + 1, y + 1, z + 1],
    texture: "bubble",
    minLifeTime: 0.2,
    maxLifeTime: 0.6,
    minEmitPower: 2,
    maxEmitPower: 2,
    minSize: 0.25,
    maxSize: 0.35,
    manualEmitCount: 20,
    gravity: [0, -10, 0],
    colorGradients: [
        {
            timeFraction: 0,
            minColor: [60, 60, 150, 1],
            maxColor: [200, 200, 255, 1],
        },
    ],
    velocityGradients: [
        {
            timeFraction: 0,
            factor: 1,
            factor2: 1,
        },
    ],
    blendMode: 1,
})
```

You can also use a `presetId` instead to use a pre-defined particle effect, to replicate effects we use in-engine.
Here is the code for an example of using a presetId:

```ts
let [x, y, z] = thisPos
y += 1
api.playParticleEffect({
    presetId: "aura",
    pos1: [x, y, z],
    pos2: [x + 1, y + 1, z + 1],
})
```

Here is a list of the presetIds you can use:

`brainRot`
`stomp`
`fertiliser`
`bonemeal`
`mobTameSuccess`
`mobTameFailure`
`mobCatch`
`spawnCaughtMob`
`mobFeedDefault`
`mobFeedSuperliked`
`mobFeedLike`
`mobFeedNeutral`
`mobFeedDisliked`
`mobDeath`
`mobDeathSoul`
`boardShopSuccess`
`mobSpawnerBlockFail`
`mobSpawnerBlockPassive`
`mobSpawnerBlockNeutral`
`mobSpawnerBlockHostile`
`mobSpawnOrb`
`aura`
`yellowFirecrackerSmall`
`yellowFirecrackerLarge`
`whiteFirecrackerSmall`
`whiteFirecrackerLarge`
`redFirecrackerSmall`
`redFirecrackerLarge`
`purpleFirecrackerSmall`
`purpleFirecrackerLarge`
`pinkFirecrackerSmall`
`pinkFirecrackerLarge`
`orangeFirecrackerSmall`
`orangeFirecrackerLarge`
`magentaFirecrackerSmall`
`magentaFirecrackerLarge`
`limeFirecrackerSmall`
`limeFirecrackerLarge`
`lightGrayFirecrackerSmall`
`lightGrayFirecrackerLarge`
`lightBlueFirecrackerSmall`
`lightBlueFirecrackerLarge`
`greenFirecrackerSmall`
`greenFirecrackerLarge`
`grayFirecrackerSmall`
`grayFirecrackerLarge`
`cyanFirecrackerSmall`
`cyanFirecrackerLarge`
`brownFirecrackerSmall`
`brownFirecrackerLarge`
`blueFirecrackerSmall`
`blueFirecrackerLarge`
`blackFirecrackerSmall`
`blackFirecrackerLarge`
`defaultFirecrackerSmall`
`defaultFirecrackerLarge`
`mango`
`speedInner`
`speedOuter`
`damageReductionInner`
`damageReductionOuter`
`damageInner`
`damageOuter`
`invisibleInner`
`invisibleOuter`
`jumpBoostInner`
`jumpBoostOuter`
`knockbackInner`
`knockbackOuter`
`poisonedInner`
`poisonedOuter`
`slownessInner`
`slownessOuter`
`weaknessInner`
`weaknessOuter`
`cleansedInner`
`cleansedOuter`
`instantDamageInner`
`instantDamageOuter`
`healthRegenInner`
`healthRegenOuter`
`instantHealthInner`
`instantHealthOuter`
`hasteInner`
`hasteOuter`
`shieldInner`
`shieldOuter`
`doubleJumpInner`
`doubleJumpOuter`
`heatResistanceInner`
`heatResistanceOuter`
`thiefInner`
`thiefOuter`
`xRayVisionInner`
`xRayVisionOuter`
`miningYieldInner`
`miningYieldOuter`
`brainRotInner`
`brainRotOuter`
`auraInner`
`auraOuter`
`wallClimbingInner`
`wallClimbingOuter`
`airWalkInner`
`airWalkOuter`
`pickpocketerInner`
`pickpocketerOuter`
`lifestealInner`
`lifestealOuter`
`bouncinessInner`
`bouncinessOuter`
`blindnessInner`
`blindnessOuter`
`poopyInner`
`poopyOuter`
