# Icons

Icons can be used in `CustomTextStyling` arrays with the `StyledIcon` type:

```js
api.sendMessage(playerId, [{ icon: "fa-solid fa-heart" }, " You gained a life!"])
api.sendMessage(playerId, [{ icon: "fa-solid fa-star", style: { color: "gold" } }, " Achievement!"])
```

> **Prefer Bloxd-native icons:** For the best visual consistency with Bloxd's theme, use **item names** (see ITEM_NAMES.txt), **block names** (see BLOCK_NAMES.txt), or **ingame icons** (below) wherever possible. These are designed specifically for Bloxd and will look more cohesive. Only fall back to Font Awesome icons when no Bloxd-native option fits your use case.

## Ingame Icons (89 icons)

See the `IngameIconName` type in the Glossary.

```
Damage
Damage Reduction
Speed
VoidJump
Fist
Frozen
Hydrated
Invisible
Jump Boost
Poisoned
Slowness
Weakness
Health Regen
Haste
Double Jump
Heat Resistance
Gliding
Boating
Obsidian Boating
Riding
Bunny Hop
FallDamage
Feather Falling
Thief
X-Ray Vision
Mining Yield
Brain Rot
Rested Damage
Rested Haste
Rested Speed
Rested Farming Yield
Rested Aura
Blindness
Pickpocketer
Lifesteal
Bounciness
Air Walk
Wall Climbing
Thorns
Poopy
Draugr Knight Head
Draugr Warper Head
Magma Golem Head
Mystery Fish
Damage Enchantment
Critical Damage Enchantment
Attack Speed Enchantment
Protection Enchantment
Health Enchantment
Health Regen Enchantment
Stomp Damage Enchantment
Knockback Resist Enchantment
Arrow Speed Enchantment
Arrow Damage Enchantment
Quick Charge Enchantment
Break Speed Enchantment
Momentum Enchantment
Mining Yield Enchantment
Farming Yield Enchantment
Mining Aura Enchantment
Digging Aura Enchantment
Lumber Aura Enchantment
Farming Aura Enchantment
Vertical Knockback Enchantment
Horizontal Knockback Enchantment
Self Yield
Friends
Riding Speed
Feed Aura
Double Poop
Mob Slayer
Rainbow Wool
Pack Leader
Max Health
Poison Claws
Mob Yield
Antlers Bonus
Health
HealthShield
Cross
Friendship
Dotted Friendship
Hunger
Empty Hunger
Pixelated Heart
Question Mark
Trader Black
Trader Blue
Trader Piggy
```

---

## Font Awesome Icons (166 icons)

Use with `fa-solid`, `fa-regular`, or `fa-duotone` prefix. Only use these when no Bloxd-native icon fits.

```
fa-solid fa-add
fa-solid fa-angle-double-up
fa-solid fa-angle-down
fa-solid fa-angle-up
fa-solid fa-angles-up
fa-solid fa-arrow-up
fa-solid fa-arrow-up-right-from-square
fa-solid fa-arrows
fa-solid fa-arrows-h
fa-solid fa-arrows-left-right
fa-solid fa-arrows-rotate
fa-solid fa-arrows-up-down-left-right
fa-solid fa-award
fa-solid fa-backpack
fa-solid fa-bars
fa-solid fa-block-question
fa-solid fa-bolt
fa-solid fa-boot
fa-solid fa-caret-up
fa-solid fa-cart-shopping
fa-solid fa-check
fa-solid fa-chess-rook
fa-solid fa-circle-info
fa-solid fa-circle-plus
fa-solid fa-clock-rotate-left
fa-solid fa-cog
fa-solid fa-coins
fa-solid fa-comment-dots
fa-solid fa-commenting
fa-solid fa-compress
fa-solid fa-computer-mouse
fa-solid fa-cookie
fa-solid fa-copy
fa-solid fa-crosshairs
fa-solid fa-crown
fa-solid fa-cube
fa-solid fa-cubes
fa-solid fa-cut
fa-solid fa-dice
fa-solid fa-dizzy
fa-solid fa-door-closed
fa-solid fa-door-open
fa-solid fa-download
fa-solid fa-edit
fa-solid fa-ellipsis
fa-solid fa-ellipsis-h
fa-solid fa-exclamation
fa-solid fa-expand
fa-solid fa-external-link
fa-solid fa-eye
fa-solid fa-eye-slash
fa-solid fa-face-diagonal-mouth
fa-solid fa-face-dizzy
fa-solid fa-face-raised-eyebrow
fa-solid fa-face-smile
fa-solid fa-face-worried
fa-solid fa-feather-alt
fa-solid fa-feather-pointed
fa-solid fa-file-alt
fa-solid fa-file-clipboard
fa-solid fa-file-lines
fa-solid fa-file-text
fa-solid fa-film
fa-solid fa-fire
fa-solid fa-fist-raised
fa-solid fa-flag
fa-solid fa-folder-image
fa-solid fa-gauge-high
fa-solid fa-gear
fa-solid fa-gem
fa-solid fa-globe
fa-solid fa-hammer
fa-solid fa-hand-back-point-up
fa-solid fa-hand-fist
fa-solid fa-hand-holding-medical
fa-solid fa-hand-point-left
fa-solid fa-hat-santa
fa-solid fa-heart
fa-solid fa-heart-music-camera-bolt
fa-solid fa-history
fa-solid fa-icons
fa-solid fa-info-circle
fa-solid fa-joystick
fa-solid fa-layer-group
fa-solid fa-lightbulb
fa-solid fa-list
fa-solid fa-list-squares
fa-solid fa-location-check
fa-solid fa-location-xmark
fa-solid fa-lock
fa-solid fa-lock-open
fa-solid fa-magnifying-glass
fa-solid fa-male
fa-solid fa-map-marker-check
fa-solid fa-map-marker-times
fa-solid fa-map-marker-xmark
fa-solid fa-minus-square
fa-solid fa-mouse
fa-solid fa-music
fa-solid fa-navicon
fa-solid fa-palette
fa-solid fa-paste
fa-solid fa-pen
fa-solid fa-pen-field
fa-solid fa-pen-to-square
fa-solid fa-person
fa-solid fa-person-arrow-down-to-line
fa-solid fa-person-arrow-up-from-line
fa-solid fa-person-falling-burst
fa-solid fa-person-military-pointing
fa-solid fa-planet-ringed
fa-solid fa-plus
fa-solid fa-plus-circle
fa-solid fa-power-off
fa-solid fa-recycle
fa-solid fa-redo-alt
fa-solid fa-refresh
fa-solid fa-right-from-bracket
fa-solid fa-rocket-launch
fa-solid fa-rotate-forward
fa-solid fa-rotate-right
fa-solid fa-scissors
fa-solid fa-search
fa-solid fa-shield
fa-solid fa-shield-alt
fa-solid fa-shield-blank
fa-solid fa-shield-halved
fa-solid fa-shirt
fa-solid fa-shopping-cart
fa-solid fa-sign-out-alt
fa-solid fa-smile
fa-solid fa-snowflake
fa-solid fa-square-dashed
fa-solid fa-square-minus
fa-solid fa-star
fa-solid fa-swords
fa-solid fa-sync
fa-solid fa-t-shirt
fa-solid fa-tachometer-alt
fa-solid fa-tachometer-alt-fast
fa-solid fa-terminal
fa-solid fa-trash-alt
fa-solid fa-trash-can
fa-solid fa-trophy
fa-solid fa-tshirt
fa-solid fa-up-from-bracket
fa-solid fa-upload
fa-solid fa-user
fa-solid fa-user-friends
fa-solid fa-user-group
fa-solid fa-user-group-crown
fa-solid fa-user-minus
fa-solid fa-user-plus
fa-solid fa-user-slash
fa-solid fa-user-unlock
fa-solid fa-users-crown
fa-solid fa-video
fa-solid fa-video-camera
fa-solid fa-volume
fa-solid fa-volume-down
fa-solid fa-volume-low
fa-solid fa-volume-medium
fa-solid fa-volume-slash
fa-solid fa-wrench
fa-solid fa-x
fa-solid fa-zap
```

## Custom Kit Icons (2 icons)

```
fa-solid fa-kit fa-pants
fa-solid fa-kit fa-shoe
```

## Brand Icons (3 icons)

```
fa-solid fa-brands fa-apple
fa-solid fa-brands fa-discord
fa-solid fa-brands fa-youtube
```
