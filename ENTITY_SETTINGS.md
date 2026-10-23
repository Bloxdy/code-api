# Entity Settings

An "Entity Setting" impacts how a player sees or interacts with another player or entity.
E.g. Player 1 could have an otherEntitySetting for entity 2 as opacity set to 0.5. This means player 1 sees entity 2 as partly see-through. Player1 is the relevant player, player2 is the targeted player.
These API methods allow you to modify entity settings:

```js
/**
 * Set every player's other-entity setting to a specific value for a particular entity.
 * includeNewJoiners=true means that new players joining the game will also have this other player setting applied.
 *
 * @param {EntityId} targetedPlayerId
 * @param {Setting} settingName
 * @param {OtherEntitySettings[Setting]} settingValue
 * @param {boolean} [includeNewJoiners]
 * @returns {void}
 */
setTargetedPlayerSettingForEveryone(targetedPlayerId, settingName, settingValue, includeNewJoiners)

/**
 * Set a player's other-entity setting for every player, mob and Person mesh in the game.
 * includeNewJoiners=true means that the player will have the setting applied to new joiners.
 *
 * @param {PlayerId} playerId
 * @param {Setting} settingName
 * @param {OtherEntitySettings[Setting]} settingValue
 * @param {boolean} [includeNewJoiners]
 * @returns {void}
 */
setEveryoneSettingForPlayer(playerId, settingName, settingValue, includeNewJoiners)

/**
 * Set a player's other-entity setting for a specific entity.
 *
 * @param {PlayerId} relevantPlayerId
 * @param {EntityId} targetedEntityId
 * @param {Setting} settingName
 * @param {OtherEntitySettings[Setting]} settingValue
 * @returns {void}
 */
setOtherEntitySetting(relevantPlayerId, targetedEntityId, settingName, settingValue)

/**
 * Set many of a player's other-entity settings for a specific entity.
 *
 * @param {PlayerId} relevantPlayerId
 * @param {EntityId} targetedEntityId
 * @param {Partial<OtherEntitySettings>} settingsObject
 * @returns {void}
 */
setOtherEntitySettings(relevantPlayerId, targetedEntityId, settingsObject)

/**
 * Get the value of a player's other-entity setting for a specific entity.
 *
 * @param {PlayerId} relevantPlayerId
 * @param {EntityId} targetedEntityId
 * @param {Setting} settingName
 * @returns {OtherEntitySettings[Setting]}
 */
getOtherEntitySetting(relevantPlayerId, targetedEntityId, settingName)

/**
 * Reset a player's other-entity setting for a specific entity to the game's default value.
 *
 * @param {PlayerId} relevantPlayerId
 * @param {EntityId} targetedEntityId
 * @param {Setting} settingName
 * @returns {void}
 */
setOtherEntitySettingToDefault(relevantPlayerId, targetedEntityId, settingName)
```

Here is the full list of available entity settings:


## alwaysInInterest

**Type:** `boolean`

**Default:** `false`

Keep sending this entity to the player at any distance, for markers or objectives they follow from afar, once players are sent only the entities near them

 



## canAttack

**Type:** `boolean`

**Default:** `false`

Whether the relevant player can attack this entity, ignored if the entity is invincible. Also makes the entity pickable, so the player's aim lands on it - as does interactable, which is the setting to use for an entity that should be aimed at without being a valid target.

 



## canSee

**Type:** `boolean`

**Default:** `true`

Whether the entity can be seen by the relevant player

 



## colorInLobbyLeaderboard

**Type:** `string`

**Default:** `""`

The colour of the player in the lobby leaderboard.

 



## hasPriorityNametag

**Type:** `boolean`

**Default:** `false`

Whether the player has a priority name tag

 



## interactable

**Type:** `boolean`

**Default:** `false`

Whether alt actioning this entity (right click / the interact bind / a mobile tap) counts as interacting with it. When true the alt action is consumed, so a held block isn't placed through the entity and a held throwable isn't used, and onPlayerAltAction fires with it as the target. Leave false on decorative entities, or they will swallow player input.

 Also makes the entity pickable, so an entity can be interactable without canAttack: the player's aim lands on it, but swinging or shooting at it does nothing.

 Works on mobs too, taking priority over held items and native harvesting, riding and pet actions. Native action packets are also rejected by the server for interactable entities.

 



## interactionPrompt

**Type:** `string | CustomTextStyling`

**Default:** `null`

Replaces the label in the interaction prompt shown by the crosshair while the relevant player is looking at this entity, e.g. "Open Shop". Left null, the prompt reads "Interact". The engine draws the player's interact key next to it, so write what interacting does rather than which button to press. Only used on entities that have interactable set, and not shown on touchscreens, where there is no key to show.

 



## killfeedColour

**Type:** `string`

**Default:** `""`

The colour of kills in the killfeed. Defaults to blue for themselves and red for everyone else.

 



## lobbyLeaderboardTags

**Type:** `ChatTags`

**Default:** `null`

The tags to the left of a player's name in the lobby leaderboard.

 



## lobbyLeaderboardValues

**Type:** `LobbyLeaderboardValues`

**Default:** `{}`

The values of the leaderboard.

 



## meshScaling

**Type:** `EntityMeshScalingMap`

**Default:** `{}`

Scaling of mesh nodes, see api.scalePlayerMeshNodes

 



## multilineTextBox

**Type:** `MultilineTextBox`

**Default:** `null`

Multiline text info for displaying text next to entities:

 ```ts
 {
     content: (CustomTextStyling[number] | RankInfo)[]    // Array of text content
     backgroundColor?: string                             // Background color
     animateIn?: boolean                                  // Whether text should animate in character-by-character
     visibleThroughWalls?: boolean                        // Overrides canSeeNametagsThroughWalls for this text: true draws it over blocks and entities, false lets them hide it
 }
 ```

 

 Hidden whenever the entity's own mesh is, so a mesh entity's hideDist applies to its text too.

 



## nameColour

**Type:** `"default" | "yellow" | "lime" | "green" | "aqua" | "cyan" | "blue" | "purple" | "pink" | "red" | "orange"`

**Default:** `"default"`

The colour of the entity's name.

 



## nameTagInfo

**Type:** `NameTagInfo`

**Default:** `null`

The name tag info of the player:

 ```ts
 {
     backgroundColor?: string
     content?: StyledText[]
     subtitle?: StyledText[]
     subtitleBackgroundColor?: string
     minLighting?: number
     healthbar?: { display?: "always" | "never" | "onDamage"; height?: FontSize; backgroundColour?: string; foregroundColour?: string | { healthFraction: number; colour: string }[] }
     border?: { colour: string; style?: "solid" | "glow" | "double"; width?: FontSize; applyTo?: "both" | "nametag" | "healthbar" }
 }
 ```

 



## opacity

**Type:** `number`

**Default:** `1`

Opacity of the entity

 Fractional values will use dithering

 0 opacity will hide the entity but not its name tag

 



## overlayColour

**Type:** `string`

**Default:** `null`

Applies a colour tint to the entity when set, like the red tint when an entity gets hurt.

 



## showDamageAmounts

**Type:** `boolean`

**Default:** `true`

Whether you can see damage amounts when shooting the entity

 



## zIndex

**Type:** `0 | 1`

**Default:** `0`

Rendering order of the entity, higher zIndex renders on top of lower ones.

 



