# Dialogue Voices

Each line of [dialogue](#clientOptions-dialogue) blips once for each syllable in its speaker's voice, as its words appear. Give `voice` a preset's name, or every parameter of the synth for a voice of your own. The [Dialogue](#howTo-Dialogue) how-to shows how to say lines with `@plugins/dialogue`.

```ts
api.setClientOption(playerId, "dialogue", { line: "Welcome to my shop!", speakerName: "Shopkeeper", voice: "regular" })
```

Leave out `voice` for a silent line.

## Voice presets

Give `voice` a preset's name for a voice that suits a kind of character:

- `soft`: Gentle and unhurried: the elderly, the kindly, calm animals.
- `regular`: An everyday adult.
- `dark`: Low, slow and falling: the sinister and the imperious.
- `high-pitched`: Small and quick: children and little creatures.
- `gruff`: Deep and rough: dwarves, sailors and anyone built like a wall.
- `lively`: Bright, bouncy and fast: the eager, the cheeky and the hurried.
- `posh`: A rising drawl with grand swoops: nobility and showmen.
- `mechanical`: Flat and even: constructs and guardians.

## Custom voices

Give `voice` every parameter of the synth instead, to make a voice of your own. Characters that share a preset sound apart with a different `pitchHz` or `syllableMs`. The voice playground on the docs site's Dialogue Voices page starts from any preset and writes the code for what you hear.

```ts
type SpeechSynth = {
    waveform: "sine" | "triangle" | "square" | "sawtooth" // The tone of each blip, from the purest (sine) to the buzziest (sawtooth).
    pitchHz: number // The voice's pitch, which intonation moves each word away from.
    glideSemitones: number // How far the pitch slides over each blip; negative falls.
    durationMs: number // How long each blip sounds.
    attackMs: number // The fade in at the start of the blip.
    releaseMs: number // The fade at the end of the blip, within `durationMs`.
    lowpassHz: number | null // Darkens a bright waveform; null leaves it unfiltered.
    volume: number // 1 is the engine's speech level.
    intonation: SpeechIntonation
    syllableMs: number // The speaking rate: each syllable takes this long, and sets when the next word appears.
}

type SpeechIntonation = {
    variationSemitones: number // How far each word sits from the voice's pitch, either way.
    declineSemitones: number // How far the pitch falls from the first word of a sentence to its last.
    questionRiseSemitones: number // How far the last word of a question rises.
    exclaimLiftSemitones: number // How far a whole exclamation is lifted.
}
```

The game keeps each number within what it can play: a blip's attack and release fit within its length, for instance.

### Example

A gruff voice, pitched down and slowed for a giant:

```ts
const giantVoice = {
    waveform: "sawtooth",
    pitchHz: 80,
    glideSemitones: -0.8,
    durationMs: 75,
    attackMs: 4,
    releaseMs: 14,
    lowpassHz: 900,
    volume: 0.85,
    intonation: {
        variationSemitones: 1.2,
        declineSemitones: 3,
        questionRiseSemitones: 3,
        exclaimLiftSemitones: 2,
    },
    syllableMs: 70,
}

api.setClientOption(playerId, "dialogue", { line: "Who goes there?", speakerName: "Giant", voice: giantVoice })
```
