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
 *
 * @param {TMobType} mobType
 * @param {TMobSetting} setting
 * @param {MobSettings<TMobType>[TMobSetting]} value
 * @returns {void}
 */
setDefaultMobSetting(mobType, setting, value)

/**
 * Get the current value of a mob setting for a specific mob.
 *
 * @param {MobId} mobId
 * @param {TMobSetting} setting
 * @param {boolean} [returnDefaultIfNotOverridden] - If true, return the default setting if not overridden.
 * @returns {MobSettings<MobType>[TMobSetting]}
 */
getMobSetting(mobId, setting, returnDefaultIfNotOverridden)

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

Here is the full list of available mob settings:


## armour

**Type:** `Partial<Record<ArmourPart, {itemName: string, enchantmentTier?: "Tier 1" | "Tier 2" | "Tier 3" | "Tier 4" | "Tier 5" }>>`

**Example:**
```ts
{
  "Helmet": {
    itemName: "Diamond Helmet",
    enchantmentTier: "Tier 5",
  },
  "Boots": {
    itemName: "Iron Boots",
  },
}
```




## attackDamage

**Type:** `number`

**Example:** `0`





## attackEffectDuration

**Type:** `number`

**Example:** `0`





## attackEffectName

**Type:** `string`

**Example:** `null`





## attackImpulse

**Type:** `number`

**Example:** `0`





## attackInterval

**Type:** `number`

**Example:** `0`





## attackItemName

**Type:** `string`

**Example:** `null`





## attackRadius

**Type:** `number`

**Example:** `0`





## attackSound

**Type:** `string`

**Example:** `null`





## baseJumpImpulseXZ

**Type:** `number`

**Example:** `0`





## baseJumpImpulseY

**Type:** `number`

**Example:** `0`





## baseRunningSpeed

**Type:** `number`

**Example:** `4.55 * 0.85`





## baseWalkingSpeed

**Type:** `number`

**Example:** `3.5`





## burstAttackInfo

**Type:** ` { burstAttackIntervals: number[]; } `

**Example:** `null`





## chargeSpecialAttackInfo

**Type:** `MobChargeSpecialAttackInfo`

**Example:** `{ chargeSpeedMult: 1.5 }`





## chaseRadius

**Type:** `number`

**Example:** `0`





## combatTetherInfo

**Type:** `MobCombatTetherCombatInfo`

**Example:** 
```ts
{
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





## evadeInfo

**Type:** `MobEvadeInfo`

**Example:** 
```ts
{
	probability: 0.6,
	minAngle: Math.PI * 0.35,
	maxAngle: Math.PI * 0.6,
	impulse: 8,
}
```





## healthRegen

**Type:** ` { amount: number; interval: number; startAfter: number; } `

**Example:** `null`





## heldItemEnchantmentTier

**Type:** `"Tier 1" | "Tier 2" | "Tier 3" | "Tier 4" | "Tier 5"`

**Example:** `null`





## heldItemName

**Type:** `string`

**Example:** `null`





## hostilityRadius

**Type:** `number`

**Example:** `0`





## hurtSound

**Type:** `string`

**Example:** `"pigHurt"`





## idleSound

**Type:** `string`

**Example:** `"pigOink"`





## initialHealth

**Type:** `number`

**Example:** `75`





## isReceivingDamageCooldownGlobal

**Type:** `boolean`

**Example:** `true`





## isRideable

**Type:** `boolean`

**Example:** `false`





## jumpCount

**Type:** `number`

**Example:** `0`





## jumpMultiplier

**Type:** `number`

**Example:** `1`





## knockbackReceivedMultiplier

**Type:** `number`

**Example:** `1`





## maxFollowingRadius

**Type:** `number`

**Example:** `12`





## maxHealth

**Type:** `number`

**Example:** `75`





## metaInfo

**Type:** `string`

**Example:** `""`





## minFollowingRadius

**Type:** `number`

**Example:** `5`





## name

**Type:** `string`

**Example:** `""`





## onDeathAura

**Type:** `number`

**Example:** `20`





## onDeathItemDrops

**Type:** ` { itemName: string; probabilityOfDrop: number; dropMinAmount: number; dropMaxAmount: number; applyBurstImpulseToDrop?: boolean; }[] `

**Example:** 
```ts
[
	{
		itemName: "Raw Porkchop",
		probabilityOfDrop: 1,
		dropMinAmount: 1,
		dropMaxAmount: 3,
	},
]
```





## onDeathParticleTexture

**Type:** `string`

**Example:** `"critical_hit"`





## onTamedHealthMultiplier

**Type:** `number`

**Example:** `4.0`





## ownerDbId

**Type:** `string`

**Example:** `null`





## petInfo

**Type:** `MobPetInfo`

**Example:** 
```ts
{
friendshipPoints: 0,
lastFedAt: null,
highestFriendshipLevelReached: 0,
superlikedFood: null,
superlikedFoodKnown: false,
bonusesGained: [],
	}
```





## rangedAttackInaccuracy

**Type:** `number`

**Example:** `0`





## ridingSpeedMult

**Type:** `number`

**Example:** `1`





## runAwayRadius

**Type:** `number`

**Example:** `0`





## runningSpeedMultiplier

**Type:** `number`

**Example:** `1`





## secondaryAttackDamage

**Type:** `number`

**Example:** `0`





## secondaryAttackImpulse

**Type:** `number`

**Example:** `0`





## secondaryAttackItemName

**Type:** `string`

**Example:** `null`





## secondaryAttackRadius

**Type:** `number`

**Example:** `0`





## secondaryAttackSound

**Type:** `string`

**Example:** `null`





## secondaryBurstAttackInfo

**Type:** ` { burstAttackIntervals: number[]; } `

**Example:** `null`





## stoppingRadius

**Type:** `number`

**Example:** `0.5`





## swingArmOnAttack

**Type:** `boolean`

**Example:** `true`





## swingArmOnSecondaryAttack

**Type:** `boolean`

**Example:** `true`





## tameInfo

**Type:** `PNull<MobTameInfo>`

**Example:**
```ts
{
  "tameItemName": [
    "Apple"
  ],
  "probabilityOfTame": 1,
  "isSaddleable": false,
  "supportsFriendship": true,
  "likedFoods": [
    "Plum",
    "Cracked Coconut",
    "Pumpkin Pie",
    "Bowl of Rice",
    "Melon Slice",
    "Gold Melon Slice",
    "Corn",
    "Bowl of Cranberries",
    "Mushroom Soup",
    "Chili Pepper",
    "Carrot",
    "Beetroot",
    "Raw Potato",
    "Baked Potato"
  ],
  "neutralFoods": [
    "Watermelon Slice",
    "Gold Watermelon Slice",
    "Apple",
    "Wheat",
    "Pear",
    "Cherry",
    "Bread"
  ],
  "dislikedFoods": [
    "Raw Porkchop",
    "Raw Beef",
    "Raw Mutton",
    "Raw Venison",
    "Cooked Porkchop",
    "Steak",
    "Cooked Mutton",
    "Cooked Venison",
    "Rotten Flesh",
    "Banana",
    "Rotten Brain"
  ],
  "guaranteedDrop": "Truffle",
  "commonDrops": [
    "Poop",
    "Wheat Seeds"
  ],
  "levelUpBonuses": {
    "1": "Renaming",
    "2": "Special Drops",
    "3": "Double Poop",
    "4": "Painting",
    "5": "Feed Aura"
  }
}
```

Taming configuration for this mob type. Includes tame items, probability, saddle support, food preferences, drops, and level-up bonuses. See `mobTameInfoDefaults` for per-mob defaults. `null` for non-tameable mobs.




## territoryRadius

**Type:** `number`

**Example:** `0`





## variation

**Type:** `"default"`

**Example:** `"default"`





## walkingSpeedMultiplier

**Type:** `number`

**Example:** `1`





## warpTargetSpecialAttackInfo

**Type:** `MobWarpTargetSpecialAttackInfo`

**Example:** 
```ts
{
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






## Mob Variations

Some mob types support variations other than just `"default"`:

```js
67: "default"
Pig: "default"
Cow: "default", "cream"
Sheep: "default", "black", "red", "orange", "pink", "purple", "yellow", "blue", "brown", "cyan", "gray", "green", "lightBlue", "lightGray", "lime", "magenta"
Horse: "default", "black", "brown", "cream"
Slime: "default"
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
Stalker: "default", "crimson", "frost", "void"
Crone: "default"
NPC: "default", "emma", "leo", "isabel", "sanjay", "imara", "enoch", "sara", "carmen"
Bobino Musculino: "default"
Capitano Explovissimo: "default"
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
| `runningAway` | The mob is running away from the target lifeform. | `{ targetId: LifeformId }` |
| `chasing` | The mob is chasing the target lifeform. | `{ targetId: LifeformId }` |
| `turningBeforeCharging` | A charge-attack mob is stood still, rotating at its `turnRate` until it faces the target,<br>at which point it captures the target's current position and transitions to `charging`. | `{ targetId: LifeformId }` |
| `charging` | A charge-attack mob is dashing straight at the position of the target captured when it<br>entered the state, ignoring the target's live position mid-dash (so the charge is dodgeable). | `{ targetId: LifeformId }` |
| `following` | The mob is following the target lifeform.<br>It will stop if it is within the `minFollowingDistance` (mob setting) of the target,<br>and teleport to the target if it is outside the `maxFollowingDistance` (mob setting) of the target. | `{ targetId: LifeformId }` |
| `watching` | The mob is stood still looking at the target. | `{ targetId: LifeformId }` |
| `walkingToPosition` | The mob is walking towards the position.<br>It will stop if it is within the `stoppingRadius` (mob setting) of the position. | `{ pos: Pos }` |
| `runningToPosition` | The mob is running towards the position.<br>It will stop if it is within the `stoppingRadius` (mob setting) of the position. | `{ pos: Pos }` |

