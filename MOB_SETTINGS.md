# Mob Settings

These impact the behaviour of mobs, what they look like, and how they sound. These can either be set on a per-mob basis or the default can be set for all mobs of a particular type.
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

/**
 * Set the default value for a mob setting.
 * @param {TMobType} mobType
 * @param {TMobSetting} setting
 * @param {MobSettings<TMobType>[TMobSetting]} value
 * @returns {void}
 */
setDefaultMobSetting(mobType, setting, value)

/**
 * Get the current value of a mob setting for a specific mob.
 * @param {MobId} mobId
 * @param {TMobSetting} setting
 * @returns {MobSettings<MobType>[TMobSetting]}
 */
getMobSetting(mobId, setting)

/**
 * Set the current value of a mob setting for a specific mob.
 * @param {MobId} mobId
 * @param {TMobSetting} setting
 * @param {MobSettings<MobType>[TMobSetting]} value
 * @returns {void}
 */
setMobSetting(mobId, setting, value)
```

Here is the full list of available mob settings:

```js
/**
 * @type {"default"}
 */
variation = "default"

/**
 * @type {string}
 */
name = ""

/**
 * @type {number}
 */
maxHealth = 75

/**
 * @type {number}
 */
initialHealth = 75

/**
 * @type {string}
 */
idleSound = "pigOink"

/**
 * @type {string}
 */
attackSound = null

/**
 * @type {string}
 */
secondaryAttackSound = null

/**
 * @type {string}
 */
hurtSound = "pigHurt"

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

/**
 * @type {string}
 */
onDeathParticleTexture = "critical_hit"

/**
 * @type {number}
 */
onDeathAura = 20

/**
 * @type {number}
 */
baseWalkingSpeed = 3.5

/**
 * @type {number}
 */
baseRunningSpeed = 4.55 * 0.85

/**
 * @type {number}
 */
walkingSpeedMultiplier = 1

/**
 * @type {number}
 */
runningSpeedMultiplier = 1

/**
 * @type {number}
 */
jumpCount = 0

/**
 * @type {number}
 */
baseJumpImpulseXZ = 0

/**
 * @type {number}
 */
baseJumpImpulseY = 0

/**
 * @type {number}
 */
jumpMultiplier = 1

/**
 * @type {number}
 */
runAwayRadius = 0

/**
 * @type {number}
 */
chaseRadius = 0

/**
 * @type {number}
 */
territoryRadius = 0

/**
 * @type {number}
 */
hostilityRadius = 0

/**
 * @type {number}
 */
stoppingRadius = 0.5

/**
 * @type {number}
 */
attackInterval = 0

/**
 * @type {number}
 */
attackRadius = 0

/**
 * @type {number}
 */
secondaryAttackRadius = 0

/**
 * @type {number}
 */
attackDamage = 0

/**
 * @type {number}
 */
secondaryAttackDamage = 0

/**
 * @type {number}
 */
attackImpulse = 0

/**
 * @type {number}
 */
secondaryAttackImpulse = 0

/**
 * @type {string}
 */
heldItemName = null

/**
 * @type {string}
 */
attackItemName = null

/**
 * @type {string}
 */
secondaryAttackItemName = null

/**
 * @type {string}
 */
attackEffectName = null

/**
 * @type {number}
 */
attackEffectDuration = 0

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
    foodItemNames: [
        "Raw Porkchop",
        "Raw Beef",
        "Raw Mutton",
        "Raw Venison",
        "Cooked Porkchop",
        "Steak",
        "Cooked Mutton",
        "Cooked Venison",
        "Rotten Flesh"
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
    ]
}

/**
 * @type {number}
 */
onTamedHealthMultiplier = 4.0

/**
 * @type {string}
 */
ownerDbId = null

/**
 * @type {number}
 */
minFollowingRadius = 3

/**
 * @type {number}
 */
maxFollowingRadius = 12

/**
 * @type {boolean}
 */
isRideable = false

/**
 * @type {string}
 */
metaInfo = ""
```

Some mob types support variations other than just `"default"`:

```js
Pig: "default"
Cow: "default", "cream"
Sheep: "default"
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
Iron Golem: "default", "iron", "grey"
```
