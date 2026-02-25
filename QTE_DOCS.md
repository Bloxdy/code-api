# Quick Time Events (QTEs)

QTEs are interactive prompts shown to a player that require a response (clicking, holding, etc). They are triggered from server-side game code via `api.addQTE()` and the result is received via the `onPlayerFinishQTE` callback.

## Overview

- **`progressBar`**: Click rapidly to fill a progress bar before it drains to zero.
- **`timedClick`**: Click within a time window to succeed.
- **`gravityBar`**: Hold click to keep a catch zone aligned with a moving target.
- **`precisionBar`**: Click when the marker is within the success zone to succeed.
- **`rhythmClick`**: Click when a shrinking outer circle aligns with a fixed inner circle.

## `progressBar`

The player clicks repeatedly to increase a progress bar. The bar drains continuously over time. Reaching 100% completes the QTE successfully. If `canFail` is true and progress reaches 0, the QTE fails; otherwise progress is clamped at 0 and the player can keep trying.

### Parameters

```ts
type ProgressBarQteParams = {
    progressStartValue?: number // Starting progress value (0-100). default: 30
    progressDecreasePerTick: number // How much progress drains each tick while the player isn't clicking. default: 0.075
    progressPerClick: number // How much progress is gained per click. default: 5
    canFail: boolean // If true, the QTE fails when progress reaches 0; otherwise progress clamps at 0. default: false
    description: CustomTextStyling // Rich text shown as the QTE prompt. default: [{ str: "Click repeatedly to complete!" }]
    clickIcon: string // Icon displayed on the click target. default: "fa-solid fa-computer-mouse"
    scale?: number // Scale multiplier for the click icon. default: 1
    rotation?: number // Rotation in degrees for the click icon. default: 15
}
```

### Sensible Defaults

```ts
api.addQTE(playerId, {
    type: "progressBar",
    parameters: {
        progressStartValue: 30,
        progressDecreasePerTick: 0.075,
        progressPerClick: 5,
        canFail: false,
        description: [{ str: "Click repeatedly to complete!" }],
        clickIcon: "fa-solid fa-computer-mouse",
        scale: 1,
        rotation: 15,
    },
})
```

## `timedClick`

A prompt appears and the player must click before the timer runs out. Clicking within the time window succeeds; letting it expire fails the QTE.

### Parameters

```ts
type TimedClickQteParams = {
    timeWindow: number // Duration in milliseconds the player has to click. default: 3000
    icon: string // Icon displayed on the click target. default: "fa-solid fa-computer-mouse"
    label: CustomTextStyling // Rich text shown as the QTE prompt. default: [{ str: "Click to complete the QTE!" }]
    showTimer: boolean // Whether to display a countdown timer. default: true
    scale?: number // Scale multiplier for the icon. default: 1
    rotation?: number // Rotation in degrees for the icon. default: 15
    breatheCenter?: boolean // If true, the icon pulses with a breathing animation anchored to the centre. default: false
}
```

### Sensible Defaults

```ts
api.addQTE(playerId, {
    type: "timedClick",
    parameters: {
        timeWindow: 3000,
        icon: "fa-solid fa-computer-mouse",
        label: [{ str: "Click to complete the QTE!" }],
        showTimer: true,
        scale: 1,
        rotation: 15,
        breatheCenter: false,
    },
})
```

## `gravityBar`

A mover travels along a bar, periodically changing direction. The player holds click to push their catch zone upward and releases to let gravity pull it down. Progress fills while the mover overlaps the catch zone and drains when it doesn't. Reaching 100% succeeds. If `canFail` is true, hitting 0% fails the QTE.

### Parameters

```ts
type GravityBarQteParams = {
    progressStartValue?: number // Starting progress value (0-100). default: 30
    catchZoneSize: number // Size of the player's catch zone as a fraction of the bar (0-1). default: 0.25
    moverSpeed: number // Speed at which the mover travels along the bar. default: 3
    moverErraticness: number // How erratically the mover changes direction (higher = more unpredictable). default: 0.8
    gravity: number // Downward pull on the catch zone when the player isn't holding click. default: 1
    riseSpeed: number // Upward force on the catch zone while the player holds click. default: 1.5
    progressGainPerSecond: number // Progress gained per second while the mover is inside the catch zone. default: 8
    progressDrainPerSecond: number // Progress lost per second while the mover is outside the catch zone. default: 4
    canFail: boolean // If true, the QTE fails when progress reaches 0; otherwise progress clamps at 0. default: false
    description: CustomTextStyling // Rich text shown as the QTE prompt. default: [{ str: "Hold to catch!" }]
    icon?: string // Icon displayed on the mover. default: "Moonfish"
}
```

### Sensible Defaults

```ts
api.addQTE(playerId, {
    type: "gravityBar",
    parameters: {
        progressStartValue: 30,
        catchZoneSize: 0.25,
        moverSpeed: 3,
        moverErraticness: 0.8,
        gravity: 1,
        riseSpeed: 1.5,
        progressGainPerSecond: 8,
        progressDrainPerSecond: 4,
        canFail: false,
        description: [{ str: "Hold to catch!" }],
        icon: "Moonfish",
    },
})
```

## `precisionBar`

A marker oscillates back and forth along a bar. A highlighted success zone sits in the centre. The player has one click — if the marker is inside the success zone, the QTE succeeds; otherwise it fails. The marker keeps bouncing indefinitely until the player clicks.

### Parameters

```ts
type PrecisionBarQteParams = {
    speed: number // Speed of the marker in full bar-widths per second (e.g. 1.0 = one full sweep per second). default: 0.5
    successZoneSize: number // Fraction of the bar that counts as the success zone, centred in the middle (0–1, e.g. 0.15 = 15%). default: 0.15
    label: CustomTextStyling // Rich text shown as the QTE prompt. default: [{ str: "Click when the marker is within the green zone." }]
    icon?: string // Icon displayed on the marker. default: ""
    scale?: number // Scale multiplier for the icon. default: 1
    rotation?: number // Rotation in degrees for the icon. default: 0
}
```

### Sensible Defaults

```ts
api.addQTE(playerId, {
    type: "precisionBar",
    parameters: {
        speed: 0.5,
        successZoneSize: 0.15,
        label: [{ str: "Click when the marker is within the green zone." }],
        icon: "",
        scale: 1,
        rotation: 0,
    },
})
```

## `rhythmClick`

An outer circle shrinks toward a fixed inner circle. The player must click when the two overlap. Each successful click counts toward `requiredSuccesses`. Missing the window (clicking too early/late or letting the circle pass without clicking) counts as a miss. If `maxMisses` is set and exceeded, the QTE fails; otherwise missed attempts simply reset the circle for another try.

### Parameters

```ts
type RhythmClickQteParams = {
    requiredSuccesses: number // Number of successful clicks needed to complete the QTE. default: 5
    shrinkDurationMs: number // Duration in milliseconds for the outer circle to shrink from max size to centre. default: 1200
    toleranceFraction: number // Fraction of the inner circle radius that counts as a successful overlap (0–1, e.g. 0.15 = ±15%). default: 0.15
    maxMisses?: number // Max misses allowed before failing. If omitted, unlimited misses are permitted. default: null (unlimited)
    label: CustomTextStyling // Rich text shown as the QTE prompt. default: [{ str: "Click when the circles align!" }]
    icon?: string // Icon displayed in the centre of the circles. default: ""
}
```

### Sensible Defaults

```ts
api.addQTE(playerId, {
    type: "rhythmClick",
    parameters: {
        requiredSuccesses: 5,
        shrinkDurationMs: 1200,
        toleranceFraction: 0.15,
        maxMisses: null (unlimited),
        label: [{ str: "Click when the circles align!" }],
        icon: "",
    },
})
```

## Usage

### Starting a QTE

```ts
const qteId = api.addQTE(playerId, {
    type: "progressBar",
    parameters: {
        progressDecreasePerTick: 0.075,
        progressPerClick: 7,
        canFail: false,
        description: [{ str: "Click repeatedly to complete!" }],
        clickIcon: "fa-solid fa-computer-mouse",
    },
})
```

### Handling the Result

```ts
onPlayerFinishQTE(playerId, qteId, result) {
    if (result) {
        // Player succeeded
    } else {
        // Player failed
    }
}
```

### Cancelling a QTE

```ts
api.deleteQTE(playerId, qteId)
```

### Checking Active QTEs

```ts
const hasQTE = api.hasActiveQTE(playerId)
```
