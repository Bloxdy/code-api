# Mob Settings

These impact the behaviour of mobs, what they look like, and how they sound. These can either be set on a per-mob basis or the default can be set for all mobs of a particular type.

## API Methods to Get/Set Mob Settings

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
 * Set the default value for a mob setting.
 * 
 * @param {TMobType} mobType
 * @param {TMobSetting} setting
 * @param {MobSettings<TMobType>[TMobSetting]} value
 * @returns {void}
 */
setDefaultMobSetting(mobType, setting, value)
```

```js
/**
 * Get the current value of a mob setting for a specific mob.
 *
 * @param {MobId} mobId
 * @param {TMobSetting} setting
 * @param {boolean} [returnDefaultIfNotOverriden] - If true, return the default setting if not overridden.
 * @returns {MobSettings<MobType>[TMobSetting]}
 */
getMobSetting(mobId, setting, returnDefaultIfNotOverriden)
```

```js
/**
 * Set the current value of a mob setting for a specific mob.
 *
 * @param {MobId} mobId
 * @param {TMobSetting} setting
 * @param {MobSettings<MobType>[TMobSetting]} value
 * @returns {void}
 */
setMobSetting(mobId, setting, value)
```

### Mob Settings and their Values

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
 * @type {string}
 */
metaInfo = ""
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

## Mob Types and Variations

Some mob types support variations other than just `"default"`.
```json
{
  "Pig": ["default"],
  "Cow": ["default", "cream"],
  "Sheep": [
    "default",
    "black",
    "red",
    "orange",
    "pink",
    "purple",
    "yellow",
    "blue",
    "brown",
    "cyan",
    "gray",
    "green",
    "lightBlue",
    "lightGray",
    "lime",
    "magenta"
  ],
  "Horse": ["default", "black", "brown", "cream"],
  "Cave Golem": ["default", "iron"],
  "Draugr Zombie": ["default", "longHairChestplate", "longHairClothed", "shortHairClothed"],
  "Draugr Skeleton": ["default"],
  "Frost Golem": ["default"],
  "Frost Zombie": ["default", "longHairChestplate", "shortHairClothed"],
  "Frost Skeleton": ["default"],
  "Draugr Knight": ["default"],
  "Wolf": ["default", "white", "brown", "grey", "spectral"],
  "Bear": ["default"],
  "Deer": ["default"],
  "Stag": ["default"],
  "Gold Watermelon Stag": ["default"],
  "Gorilla": ["default"],
  "Wildcat": ["default",
    "tabby",
    "grey",
    "black",
    "calico",
    "siamese",
    "leopard"
  ],
  "Magma Golem": ["default"],
  "Draugr Huntress": ["default", "chainmail"],
  "Spirit Golem": ["default"],
  "Spirit Wolf": ["default"],
  "Spirit Bear": ["default"],
  "Spirit Stag": ["default"],
  "Spirit Gorilla": ["default"]
}
```

## Extra Information

Quick helper to get all current values of mob settings for a specific mob.
```js
const mobSettingNames = [
  "variation",
  "name",
  "maxHealth",
  "initialHealth",
  "idleSound",
  "attackSound",
  "secondaryAttackSound",
  "hurtSound",
  "onDeathItemDrops",
  "onDeathParticleTexture",
  "onDeathAura",
  "baseWalkingSpeed",
  "baseRunningSpeed",
  "walkingSpeedMultiplier",
  "runningSpeedMultiplier",
  "jumpCount",
  "baseJumpImpulseXZ",
  "baseJumpImpulseY",
  "jumpMultiplier",
  "runAwayRadius",
  "chaseRadius",
  "territoryRadius",
  "hostilityRadius",
  "stoppingRadius",
  "attackInterval",
  "attackRadius",
  "secondaryAttackRadius",
  "attackDamage",
  "secondaryAttackDamage",
  "attackImpulse",
  "secondaryAttackImpulse",
  "heldItemName",
  "attackItemName",
  "secondaryAttackItemName",
  "attackEffectName",
  "attackEffectDuration",
  "tameInfo",
  "onTamedHealthMultiplier",
  "ownerDbId",
  "minFollowingRadius",
  "maxFollowingRadius",
  "isRideable",
  "healthRegen",
  "ridingSpeedMult",
  "metaInfo",
  "petInfo",
];

getMobFullSettings = (mobId) => {
  if (!api.checkValid(mobId)) {
    return null;
  }
  const out = {};
  let name;
  for (let i = 0, n = mobSettingNames.length; i < n; i++) {
    name = mobSettingNames[i];
    out[name] = api.getMobSetting(mobId, name);
  }
  return out;
};
```
