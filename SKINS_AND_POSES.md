# Skins and Poses

This document lists all API-selectable skins and poses that can be used with the game API.

## Usage

### Changing Player Skin

Use `api.changePlayerIntoSkin(playerId, part, skinName)` to change a player's skin:

```js
api.changePlayerIntoSkin(playerId, "body", "body_0_0")
api.changePlayerIntoSkin(playerId, "head", "wizard") // NPC skin
```

### Setting Player Pose

Use `api.setPlayerPose(playerId, pose)` to change a player's pose:

```js
api.setPlayerPose(playerId, "standing")
api.setPlayerPose(playerId, "sitting")
```

---

## Poses (7 poses)

```
standing
sitting
zombie
gliding
driving
sleeping
riding
```

---

## Skin Parts

The following skin parts can be customised: `head`, `body`, `legs`, `shoes`, `eyebrows`, `eyes`, `skin`

---

## NPC Skins (9 skins)

These are special full-body skins typically used for NPCs. Apply them via the `head` part:

```js
api.changePlayerIntoSkin(playerId, "head", "wizard")
```

```
chef
farmer
piggy_banker
portal_mage
trader
trader_black
trader_blue
wizard
zombie
```

---

## Head (46 skins)

`head_0, head_1_0, head_1_1, head_1_2, head_1_3, head_1_4, head_2_0, head_2_1, head_2_2, head_2_3, head_2_4, head_3_0, head_3_1, head_3_2, head_3_3, head_3_4, head_4_0, head_4_1, head_4_2, head_4_3, head_4_4, head_5_0, head_5_1, head_5_2, head_5_3, head_5_4, head_6_0, head_6_1, head_6_2, head_6_3, head_6_4, head_7_0, head_7_1, head_7_2, head_7_3, head_7_4, head_8_0, head_8_1, head_8_2, head_8_3, head_8_4, head_9_0, head_9_1, head_9_2, head_9_3, head_9_4`

## Body (56 skins)

`body_0_0, body_0_1, body_0_2, body_0_3, body_0_4, body_0_5, body_0_6, body_0_7, body_1_0, body_1_1, body_1_2, body_1_3, body_1_4, body_1_5, body_1_6, body_1_7, body_2_0, body_2_1, body_2_2, body_2_3, body_2_4, body_2_5, body_2_6, body_2_7, body_3_0, body_3_1, body_3_2, body_3_3, body_3_4, body_3_5, body_3_6, body_3_7, body_4_0, body_4_1, body_4_2, body_4_3, body_4_4, body_4_5, body_4_6, body_4_7, body_5_0, body_5_1, body_5_2, body_5_3, body_5_4, body_5_5, body_5_6, body_5_7, body_6_0, body_6_1, body_6_2, body_6_3, body_6_4, body_6_5, body_6_6, body_6_7`

## Legs (15 skins)

`legs_0_0, legs_0_1, legs_0_2, legs_0_3, legs_0_4, legs_1_0, legs_1_1, legs_1_2, legs_1_3, legs_1_4, legs_2_0, legs_2_1, legs_2_2, legs_2_3, legs_2_4`

## Shoes (9 skins)

`shoes_0_0, shoes_0_1, shoes_0_2, shoes_1_0, shoes_1_1, shoes_1_2, shoes_2_0, shoes_2_1, shoes_2_2`

## Eyebrows (16 skins)

`eyebrows_0, eyebrows_1_0, eyebrows_1_1, eyebrows_1_2, eyebrows_1_3, eyebrows_1_4, eyebrows_2_0, eyebrows_2_1, eyebrows_2_2, eyebrows_2_3, eyebrows_2_4, eyebrows_3_0, eyebrows_3_1, eyebrows_3_2, eyebrows_3_3, eyebrows_3_4`

## Eyes (50 skins)

`eyes_0_0, eyes_0_1, eyes_0_2, eyes_0_3, eyes_0_4, eyes_1_0, eyes_1_1, eyes_1_2, eyes_1_3, eyes_1_4, eyes_2_0, eyes_2_1, eyes_2_2, eyes_2_3, eyes_2_4, eyes_3_0, eyes_3_1, eyes_3_2, eyes_3_3, eyes_3_4, eyes_4_0, eyes_4_1, eyes_4_2, eyes_4_3, eyes_4_4, eyes_5_0, eyes_5_1, eyes_5_2, eyes_5_3, eyes_5_4, eyes_6_0, eyes_6_1, eyes_6_2, eyes_6_3, eyes_6_4, eyes_7_0, eyes_7_1, eyes_7_2, eyes_7_3, eyes_7_4, eyes_8_0, eyes_8_1, eyes_8_2, eyes_8_3, eyes_8_4, eyes_9_0, eyes_9_1, eyes_9_2, eyes_9_3, eyes_9_4`

## Skin (24 skins)

`skin_0_0, skin_0_1, skin_0_2, skin_0_3, skin_0_4, skin_0_5, skin_0_6, skin_0_7, skin_0_8, skin_0_9, skin_0_10, skin_0_11, skin_0_12, skin_0_13, skin_0_14, skin_0_15, skin_0_16, skin_0_17, skin_0_18, skin_0_19, skin_0_20, skin_0_21, skin_0_22, skin_0_23`

---

## Notes

- Skin names ending with `_none` are internal variants and cannot be used via the API.
- Skin variants follow the pattern `{part}_{style}_{colour}` (e.g., `body_0_0`, `head_1_2`).
- NPC skins apply a complete character appearance and override individual part selections.
