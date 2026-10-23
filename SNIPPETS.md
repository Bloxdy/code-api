# Snippets

Short pieces of code to copy into a code block and change to suit your world. In a code block or board, `myId` is the player who ran it. In World Code, use the player ID a callback is given instead.

## Make the player jump

Sets the player's velocity straight up, whatever they were doing.

```js
api.setVelocity(myId, 0, 9, 0)
```

## Push the player

Adds to the player's velocity, so the push carries on from how they were already moving.

```js
api.applyImpulse(myId, 9, 0, 9)
```

## Teleport the player onto this block

`thisPos` is the position of the code block or board that is running.

```js
api.setPosition(myId, thisPos[0] + 0.5, thisPos[1] + 1, thisPos[2] + 0.5)
```

## Send yourself a coloured message

```js
api.sendMessage(myId, "Welcome to my world!", { color: "orange" })
```

## Send a message to everyone

```js
api.broadcastMessage("The game starts in 10 seconds!", { color: "red" })
```

## Show flying text

Text that flies across the middle of the player's screen. The last number is how far away whatever caused it is, which sets how the text flies.

```js
api.sendFlyingMiddleMessage(myId, ["You found the secret room!"], 100)
```

## Say a line of dialogue

Shows a line below the crosshair, revealed word by word in a voice, with the speaker's name above it. It stays up until you hide it with `api.setClientOption(myId, "dialogue", null)`. In World Code, `sayDialogue` from `@plugins/dialogue` hides it for you after a while.

```js
api.setClientOption(myId, "dialogue", { line: "Welcome to my world!", speakerName: "Guide", voice: "regular" })
```

## Set the player's health

Logs the health they had before.

```js
const oldHealth = api.getHealth(myId)
api.setHealth(myId, 50)
api.log("Old health:", oldHealth)
```

## Give the player an item

```js
api.giveItem(myId, "Diamond Sword", 1)
```

## Place a block above this block

```js
api.setBlock(thisPos[0], thisPos[1] + 1, thisPos[2], "Block of Gold")
```

## Give the player a speed boost

An inbuilt effect, for 10 seconds.

```js
api.applyEffect(myId, "Speed", 10000, { inbuiltLevel: 1 })
```

## Make the player faster

Client options change how the game behaves for one player.

```js
api.setClientOption(myId, "speedMultiplier", 2)
```

## Play a sound

```js
api.playSound(myId, "equip_leather1", 1, 1)
```

## Find every other player

A function assigned without `const` or `let` stays defined for later code blocks, until World Code is next saved or the world restarts.

```js
getOtherIds = () => {
	const otherIds = []
	for (const id of api.getPlayerIds()) {
		if (id !== myId) {
			otherIds.push(id)
		}
	}
	return otherIds
}
```

## Turn the other players into zombies

Uses `getOtherIds` from the snippet above.

```js
for (const otherId of getOtherIds()) {
	api.setPlayerPose(otherId, "zombie")
	api.changePlayerIntoSkin(otherId, "head", "zombie")
}
```

## Make everyone a floating wizard

```js
for (const playerId of api.getPlayerIds()) {
	api.setPlayerPose(playerId, "driving")
	api.changePlayerIntoSkin(playerId, "head", "wizard")
}
```

## Welcome players as they join

World Code: `onPlayerJoin` runs each time a player joins, with their ID.

```js
onPlayerJoin = (playerId) => {
	api.sendMessage(playerId, "Welcome! Type in chat if you need help.", { color: "green" })
}
```

## Run code every second

World Code: `tick` runs many times a second, and is given the time since the last one, in milliseconds.

```js
let sinceLastSecond = 0
tick = (ms) => {
	sinceLastSecond += ms
	if (sinceLastSecond >= 1000) {
		sinceLastSecond -= 1000
		api.broadcastMessage("Another second has passed")
	}
}
```