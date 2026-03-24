# Mob Settings

These impact the behaviour of mobs, what they look like, and how they sound. These can either be set on a per-mob basis or the default can be set for all mobs of a particular type.

## API Methods to Get/Set Mob Settings
These API methods allow you to modify mob settings:

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
 *
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
 *
 * Get the current value of a mob setting for a specific mob.
 * @param {MobId} mobId
 * @param {TMobSetting} setting
 * @param {boolean} [returnDefaultIfNotOverridden] - If true, return the default setting if not overridden.
 * @returns {MobSettings<MobType>[TMobSetting]}
 */
getMobSetting(mobId, setting, returnDefaultIfNotOverridden)
```

```js
/**
 *
 * Set the current value of a mob setting for a specific mob.
 * @param {MobId} mobId
 * @param {TMobSetting} setting
 * @param {MobSettings<MobType>[TMobSetting]} value
 * @returns {void}
 */
setMobSetting(mobId, setting, value)
```

### Settings and Values for them
Here is the full list of available mob settings:

```js
/**
 * @type {"default"}
 */
variation = "default"
```

```js
/**
 * @type {string}
 */
name = ""
```

```js
/**
 * @type {number}
 */
maxHealth = 75
```

```js
/**
 * @type {number}
 */
initialHealth = 75
```

```js
/**
 * @type {string}
 */
idleSound = "pigOink"
```

```js
/**
 * @type {string}
 */
attackSound = null
```

```js
/**
 * @type {string}
 */
secondaryAttackSound = null
```

```js
/**
 * @type {string}
 */
hurtSound = "pigHurt"
```

```js
/**
 * @type { { itemName: string; probabilityOfDrop: number; dropMinAmount: number; dropMaxAmount: number; applyBurstImpulseToDrop?: boolean; }[] }
 */
onDeathItemDrops = [
    {
        itemName: "Raw Porkchop",
        probabilityOfDrop: 1,
        dropMinAmount: 1,
        dropMaxAmount: 3,
    },
]
```

```js
/**
 * @type {string}
 */
onDeathParticleTexture = "critical_hit"
```

```js
/**
 * @type {number}
 */
onDeathAura = 20
```

```js
/**
 * @type {number}
 */
baseWalkingSpeed = 3.5
```

```js
/**
 * @type {number}
 */
baseRunningSpeed = 4.55 * 0.85
```

```js
/**
 * @type {number}
 */
walkingSpeedMultiplier = 1
```

```js
/**
 * @type {number}
 */
runningSpeedMultiplier = 1
```

```js
/**
 * @type {number}
 */
jumpCount = 0
```

```js
/**
 * @type {number}
 */
baseJumpImpulseXZ = 0
```

```js
/**
 * @type {number}
 */
baseJumpImpulseY = 0
```

```js
/**
 * @type {number}
 */
jumpMultiplier = 1
```

```js
/**
 * @type {number}
 */
runAwayRadius = 0
```

```js
/**
 * @type {number}
 */
chaseRadius = 0
```

```js
/**
 * @type {number}
 */
territoryRadius = 0
```

```js
/**
 * @type {number}
 */
hostilityRadius = 0
```

```js
/**
 * @type {number}
 */
stoppingRadius = 0.5
```

```js
/**
 * @type {number}
 */
attackInterval = 0
```

```js
/**
 * @type {number}
 */
attackRadius = 0
```

```js
/**
 * @type {number}
 */
secondaryAttackRadius = 0
```

```js
/**
 * @type {number}
 */
attackDamage = 0
```

```js
/**
 * @type {number}
 */
secondaryAttackDamage = 0
```

```js
/**
 * @type {number}
 */
attackImpulse = 0
```

```js
/**
 * @type {number}
 */
secondaryAttackImpulse = 0
```

```js
/**
 * @type { { burstAttackIntervals: readonly number[]; } }
 */
burstAttackInfo = null
```

```js
/**
 * @type { { burstAttackIntervals: readonly number[]; } }
 */
secondaryBurstAttackInfo = null
```

```js
/**
 * @type {string}
 */
heldItemName = null
```

```js
/**
 * @type {string}
 */
attackItemName = null
```

```js
/**
 * @type {string}
 */
secondaryAttackItemName = null
```

```js
/**
 * @type {boolean}
 */
swingArmOnAttack = true
```

```js
/**
 * @type {boolean}
 */
swingArmOnSecondaryAttack = true
```

```js
/**
 * @type {string}
 */
attackEffectName = null
```

```js
/**
 * @type {number}
 */
attackEffectDuration = 0
```

```js
/**
 * @type {MobWarpTargetSpecialAttackInfo}
 */
warpTargetSpecialAttackInfo = {
        cooldown: 20_000,
        range: 32,
        sound: "warperPhase",
        delay: 1_000,
        minDestinationRadius: 5,
        maxDestinationRadius: 7,
        swingArm: false,
        particleOpts: {
            duration: 2_000,
            texture: "soul_0",
            colorGradients: [
                {
                    timeFraction: 0,
                    minColor: [70, 215, 230, 1],
                    maxColor: [75, 225, 240, 1],
                },
            ],
        },
    }
```

```js
/**
 * @type {MobCombatTetherCombatInfo}
 */
combatTetherInfo = {
        range: 11,
        particleOpts: {
            texture: "soul_0",
            colorGradients: [
                {
                    timeFraction: 0,
                    minColor: [245, 35, 25, 1],
                    maxColor: [255, 45, 35, 1],
                },
            ],
        },
    }
```

```js
/**
 * @type {MobEvadeInfo}
 */
evadeInfo = {
        probability: 0.6,
        minAngle: Math.PI * 0.35,
        maxAngle: Math.PI * 0.6,
        impulse: 8,
    }
```

```js
/**
 * @type {MobTameInfo}
 */
tameInfo = {
    tameItemName: [
        "Raw Porkchop",
        "Raw Beef",
        "Raw Mutton",
        "Raw Venison",
        "Cooked Porkchop",
        "Steak",
        "Cooked Mutton",
        "Cooked Venison"
    ],
    probabilityOfTame: 0.32,
    isSaddleable: false,
    supportsFriendship: true,
    likedFoods: [
        "Raw Porkchop",
        "Raw Beef",
        "Raw Mutton",
        "Raw Venison",
        "Cooked Porkchop",
        "Steak",
        "Cooked Mutton",
        "Cooked Venison",
        "Banana",
        "Baked Potato",
        "Rotten Brain"
    ],
    neutralFoods: [
        "Catnip",
        "Pumpkin Pie",
        "Bowl of Cranberries",
        "Watermelon Slice",
        "Gold Watermelon Slice",
        "Bread",
        "Rotten Flesh",
        "Mushroom Soup",
        "Plum",
        "Carrot",
        "Beetroot",
        "Raw Potato"
    ],
    dislikedFoods: [
        "Apple",
        "Wheat",
        "Pear",
        "Cherry",
        "Bowl of Rice",
        "Melon Slice",
        "Gold Melon Slice",
        "Chili Pepper",
        "Cracked Coconut"
    ],
    foodItemsWithEffects: [
        {
            itemName: "Catnip",
            effects: [
                {
                    name: "Speed",
                    duration: 30000,
                    level: 1
                },
                {
                    name: "Damage",
                    duration: 30000,
                    level: 1
                }
            ]
        }
    ],
    guaranteedDrop: "Caught Fish",
    commonDrops: [
        "Poop",
        "Wheat Seeds"
    ],
    levelUpBonuses: {
        1: "Renaming",
        2: "Special Drops",
        3: "Damage +",
        4: "Painting",
        5: "Poison Claws"
    }
}
```

```js
/**
 * @type {number}
 */
onTamedHealthMultiplier = 4.0
```

```js
/**
 * @type {MobPetInfo}
 */
petInfo = {
    friendshipPoints: 0,
    lastFedAt: null,
    highestFriendshipLevelReached: 0,
    superlikedFood: null,
    superlikedFoodKnown: false,
    bonusesGained: [],
}
```

```js
/**
 * @type {string}
 */
ownerDbId = null
```

```js
/**
 * @type {number}
 */
minFollowingRadius = 5
```

```js
/**
 * @type {number}
 */
maxFollowingRadius = 12
```

```js
/**
 * @type {boolean}
 */
isRideable = false
```

```js
/**
 * @type { { amount: number; interval: number; startAfter: number; } }
 */
healthRegen = null
```

```js
/**
 * @type {number}
 */
ridingSpeedMult = 1
```

```js
/**
 * @type {boolean}
 */
isRideable = false
```

```js
/**
 * @type {string}
 */
metaInfo = ""
```

## Mob Types and Variations
> [!NOTE]
> From the offical documentation:


Some mob types support variations other than just `"default"`:

```js
Pig: "default"
Cow: "default", "cream"
Sheep: "default", "black", "red", "orange", "pink", "purple", "yellow", "blue", "brown", "cyan", "gray", "green", "lightBlue", "lightGray", "lime", "magenta"
Horse: "default", "black", "brown", "cream"
Cave Golem: "default", "iron"
Draugr Zombie: "default", "longHairChestplate", "longHairClothed", "shortHairClothed"
Draugr Skeleton: "default"
Frost Golem: "default"
Frost Zombie: "default", "longHairChestplate", "shortHairClothed"
Frost Skeleton: "default"
Draugr Knight: "default"
Wolf: "default", "white", "brown", "grey", "spectral"
Bear: "default"
Deer: "default"
Stag: "default"
Gold Watermelon Stag: "default"
Gorilla: "default"
Wildcat: "default", "tabby", "grey", "black", "calico", "siamese", "leopard"
Magma Golem: "default"
Draugr Huntress: "default", "chainmail"
Spirit Golem: "default"
Spirit Wolf: "default"
Spirit Bear: "default"
Spirit Stag: "default"
Spirit Gorilla: "default"
Draugr Warper: "default"
Frost Wraith: "default"
Draugr Reaver: "default"
NPC: "default", "emma", "leo", "isabel", "sanjay", "imara", "enoch", "sara", "carmen"
```

## Mob AI

A mob's AI state determines its behaviour, e.g.: whether it is stood still, walking in a straight line, chasing someone, running towards a coordinate, etc. These API methods allow you to modify a mob's AI state:

```js
/**
 * Gets the current AI state for the given mob.
 *
 * @param {MobId} mobId
 * @returns { { state: MobAiState; params: MobAiStateParams<MobAiState> } }
 */
getMobAiState(mobId)

/**
 * Sets the current AI state for the given mob.
 * Some AI states will require context such as the ID of the lifeform being chased.
 *
 * @param {MobId} mobId
 * @param {TState} state
 * @param {MobAiStateParams<TState>} params
 * @returns {void}
 */
setMobAiState(mobId, state, params)
```

Here is the full list of available mob AI states and their parameters:

| State | Description | Parameters |
|-------|-------------|------------|
| `idle` | The mob is stood still, but it still has awareness of its environment.<br>For example: if the mob is hostile, it will still chase and attack nearby players. | `null` |
| `disabled` | The mob is stood still, and it has no awareness of its environment.<br>It will not even react if provoked. | `null` |
| `idleBeforeTurning` | The mob is stood still (idle) and is about to turn. | `null` |
| `turning` | The mob has chosen a new direction at random and is turning to face it. | `null` |
| `idleBeforeWalking` | The mob is stood still (idle) and is about to walk. | `null` |
| `walking` | The mob is walking in the direction it is facing. | `null` |
| `runningAway` | The mob is running away from the target lifeform. | <pre lang="ts"><code>{ targetId: LifeformId }</code></pre> |
| `chasing` | The mob is chasing the target lifeform. | <pre lang="ts"><code>{ targetId: LifeformId }</code></pre> |
| `following` | The mob is following the target lifeform.<br>It will stop if it is within the `minFollowingDistance` (mob setting) of the target,<br>and teleport to the target if it is outside the `maxFollowingDistance` (mob setting) of the target. | <pre lang="ts"><code>{ targetId: LifeformId }</code></pre> |
| `watching` | The mob is stood still looking at the target. | <pre lang="ts"><code>{ targetId: LifeformId }</code></pre> |
| `walkingToPosition` | The mob is walking towards the position.<br>It will stop if it is within the `stoppingRadius` (mob setting) of the position. | <pre lang="ts"><code>{ pos: Pos }</code></pre> |
| `runningToPosition` | The mob is running towards the position.<br>It will stop if it is within the `stoppingRadius` (mob setting) of the position. | <pre lang="ts"><code>{ pos: Pos }</code></pre> |

> [!WARNING]
> This is not from the offical documentation, this was put together by NlGBOB.
> Also this has not been updated with the spirt mobs.

```json
{
  "Sheep": ["default"],
  "Stag": ["default"],
  "Wildcat": [
    "default",
    "tabby",
    "grey",
    "black",
    "calico",
    "siamese",
    "leopard"
  ],
  "Wolf": [
    "default",
    "white",
    "brown",
    "grey",
    "spectral"
  ],
  "Bear": ["default"],
  "Cave Golem": ["default", "iron"],
  "Cow": ["default", "cream"],
  "Deer": ["default"],
  "Draugr Huntress": ["default", "chainmail"],
  "Draugr Knight": ["default"],
  "Draugr Skeleton": ["default"],
  "Draugr Zombie": ["default", "longHairChestplate", "longHairClothed", "shortHairClothed"],
  "Frost Golem": ["default"],
  "Frost Skeleton": ["default"],
  "Frost Zombie": ["default", "longHairChestplate", "shortHairClothed"],
  "Gold Watermelon Stag": ["default"],
  "Gorilla": ["default"],
  "Horse": ["default", "black", "brown", "cream"],
  "Iron Golem": ["default"],
  "Magma Golem": ["default"],
  "Pig": ["default"]
}
```
