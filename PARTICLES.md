


# Particles

These are the strings you can give to functions that take a particle effect `texture` as input:

<picture>
  <source media="(prefers-color-scheme: dark)" height="15" srcset="https://github.com/user-attachments/assets/c32d0a4e-4c89-47fe-a94e-1807029a3f70">
  <source media="(prefers-color-scheme: light)" height="15" srcset="https://github.com/user-attachments/assets/9db5bb07-3793-44b1-bd07-8c3e4aeb6229">
  <img alt="bubble" height="15" src="https://github.com/user-attachments/assets/c32d0a4e-4c89-47fe-a94e-1807029a3f70">
</picture> <code>bubble</code><br>


<picture>
  <source media="(prefers-color-scheme: dark)" height="15" srcset="https://github.com/user-attachments/assets/c8921c0f-2527-440b-b805-c9d451db2b88">
  <source media="(prefers-color-scheme: light)" height="15" srcset="https://github.com/user-attachments/assets/dfcdc053-9262-4970-9b44-f9b4bca83fb0">
  <img alt="critical_hit" height="15" src="https://github.com/user-attachments/assets/c8921c0f-2527-440b-b805-c9d451db2b88">
</picture> <code>critical_hit</code><br>

<picture>
  <source media="(prefers-color-scheme: dark)" height="15" srcset="https://github.com/user-attachments/assets/a488e69c-fe0c-4eae-a12f-2c8b40cebffa">
  <source media="(prefers-color-scheme: light)" height="15" srcset="https://github.com/user-attachments/assets/bda51401-d01a-48ea-a91a-97fc26c221fc">
  <img alt="drift" height="15" src="https://github.com/user-attachments/assets/a488e69c-fe0c-4eae-a12f-2c8b40cebffa">
</picture> <code>drift</code><br>

<picture>
  <source media="(prefers-color-scheme: dark)" height="15" srcset="https://github.com/user-attachments/assets/be3dcb74-658b-456a-a1e2-5ec93d8e41fb">
  <source media="(prefers-color-scheme: light)" height="15" srcset="https://github.com/user-attachments/assets/f1c3be94-b5ac-4e9e-b15e-21b77ab4fd3b">
  <img alt="effect_5" height="15" src="https://github.com/user-attachments/assets/be3dcb74-658b-456a-a1e2-5ec93d8e41fb">
</picture> <code>effect_5</code><br>

<picture>
  <source media="(prefers-color-scheme: dark)" height="15" srcset="https://github.com/user-attachments/assets/957c7839-9311-43da-9598-9c311b7cf288">
  <source media="(prefers-color-scheme: light)" height="15" srcset="https://github.com/user-attachments/assets/0182a17f-a0e7-4f3d-8156-445cce67ea28">
  <img alt="generic_2" height="15" src="https://github.com/user-attachments/assets/957c7839-9311-43da-9598-9c311b7cf288">
</picture> <code>generic_2</code><br>


<picture>
  <source media="(prefers-color-scheme: dark)" height="15" srcset="https://github.com/user-attachments/assets/ed6849cf-4ddf-462a-af0d-7f5bc5d2833b">
  <source media="(prefers-color-scheme: light)" height="15" srcset="https://github.com/user-attachments/assets/060842b4-6eca-4fb7-a2a4-7c5f850362ac">
  <img alt="glint" height="15" src="https://github.com/user-attachments/assets/ed6849cf-4ddf-462a-af0d-7f5bc5d2833b">
</picture> <code>glint</code><br>

<picture>
  <source media="(prefers-color-scheme: dark)" height="15" srcset="https://github.com/user-attachments/assets/9ce817e1-caf6-424a-9c2a-d81924c100ed">
  <source media="(prefers-color-scheme: light)" height="15" srcset="https://github.com/user-attachments/assets/dd052b31-ce4e-4ba6-94bf-dc558ca9e629">
  <img alt="soul_0" height="15" src="https://github.com/user-attachments/assets/9ce817e1-caf6-424a-9c2a-d81924c100ed">
</picture> <code>soul_0</code><br>


<picture>
  <source media="(prefers-color-scheme: dark)" height="15" srcset="https://github.com/user-attachments/assets/08461c3a-f05b-48f1-82bb-0f046c9ae7e0">
  <source media="(prefers-color-scheme: light)" height="15" srcset="https://github.com/user-attachments/assets/fa52535b-aaf0-4d67-bb84-30abd65a110b">
  <img alt="square_particle" height="15" src="https://github.com/user-attachments/assets/08461c3a-f05b-48f1-82bb-0f046c9ae7e0">
</picture> <code>square_particle</code><br>

<br>Here's the code for an example particle effect:

```ts
let [x, y, z] = thisPos
y += 1
api.playParticleEffect({
    dir1: [-1, -1, -1],
    dir2: [1, 1, 1],
    pos1: [x, y, z],
    pos2: [x + 1, y + 1, z + 1],
    texture: "bubble",
    minLifeTime: 0.2,
    maxLifeTime: 0.6,
    minEmitPower: 2,
    maxEmitPower: 2,
    minSize: 0.25,
    maxSize: 0.35,
    manualEmitCount: 20,
    gravity: [0, -10, 0],
    colorGradients: [
        {
            timeFraction: 0,
            minColor: [60, 60, 150, 1],
            maxColor: [200, 200, 255, 1],
        },
    ],
    velocityGradients: [
        {
            timeFraction: 0,
            factor: 1,
            factor2: 1,
        },
    ],
    blendMode: 1,
})
```
