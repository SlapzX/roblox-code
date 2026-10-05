# AWAKENING: a 60-second Roblox cinematic

A single LocalScript builds an entire scene and then plays a 60-second cutscene in it. The scene is a floating island at night, eight rune pillars, a crystal, a lightning storm, an explosion with bullet time, a rising orb, a sunrise and a supernova. It doesn't use any models or uploads. Everything is made from parts, lights, beams, particles, post-processing and built-in engine sounds, and it's all removed when the cutscene ends.

File: [`AwakeningCinematic.client.lua`](AwakeningCinematic.client.lua)

---

## Setup

1. **Open Roblox Studio** and open any place. A fresh *Baseplate* works.
2. In the **Explorer**, expand **StarterPlayer** and select **StarterPlayerScripts**.
3. Hover over it, click **+**, and insert a **LocalScript**. It has to be a LocalScript, not a Script or ModuleScript.
4. Rename it to `AwakeningCinematic` (the name is up to you).
5. Open it, delete the default `print("Hello world!")`, and paste in the whole contents of `AwakeningCinematic.client.lua`.
6. **Recommended:** select **Lighting** in the Explorer and, in Properties, set **Technology** to **Future**. Neon glow, point-light shadows and the bloom look much better with it.
7. Press **Play** (F5). About 3 seconds after you spawn, the cinematic starts.
8. Controls:
   - **X**, or the on-screen **SKIP** button, skips it.
   - **P** replays it.

To see it on a real server, publish the place (File → Publish to Roblox) and join it. The scene is built on each player's own client, so every player gets their own copy and nobody else is affected.

---

## Optional extras

### Play it from a part instead of on join
1. In the script's `CONFIG`, set `PLAY_ON_JOIN = false`.
2. Insert a **Part** in Workspace and name it exactly **`CinematicTrigger`**. Make it Anchored and give it a glowing Neon pad look.
3. Walking onto the part plays the cinematic. There's a 3-second cooldown afterwards.

### Add music
1. Open the **Creator Store** (Toolbox → Audio) and find a track, for example something epic or orchestral.
2. Right-click it → **Copy Asset ID**.
3. In `CONFIG`, set `MUSIC_ID = "rbxassetid://PASTE_ID_HERE"`.

The music fades in, slows down and drops in pitch during bullet time, speeds back up afterwards, and fades out under the title card.

### Swap the sound effects
Each sound in `CONFIG.SOUNDS` points at a sound that ships with Roblox, so they work without uploading anything. Replace any of them with a Creator Store id (`"rbxassetid://..."`) for a heavier, more cinematic sound, for example a real thunder crack or glass shatter.

### Other things to tweak
| Setting | What it does |
|---|---|
| `CONFIG.ORIGIN` | Where the island is built. Move it if your map is huge and reaches (6000, 900, 6000). |
| `TITLE_TEXT` | The text on the end title card. |
| `VIOLET`, `CYAN`, `GOLD` | The colour palette. |
| `JOIN_DELAY` | How many seconds after spawning it starts. |

---

## What happens, second by second

| Time | Beat | Effects used |
|---|---|---|
| 0–5 | **Cold open** | Letterbox bars slide in. Narration is typed out with `MaxVisibleGraphemes`. A low drone sound starts. |
| 5–13 | **Establishing shot** | The camera flies over the island on a 4-point **Bézier curve**. Also: depth of field, a night sky with 5000 stars and a big moon, atmosphere haze, fireflies, orbiting rocks. |
| 13–21 | **The runes** | Eight pillars ignite one at a time. Each one gets a neon flash, a particle burst, a point light and a rising-pitch "ping", then fires **curved Beams** into the crystal. The camera does a low dolly with pillars passing in the foreground, then a worm's-eye shot. |
| 21–28 | **Awakening** | The crystal spins faster and faster. **Inward sphere particles** suck energy into it, the magic circles light up dash by dash, and the rocks lift up. The camera spirals 1.5 turns inward while the FOV narrows. Bloom and colour grading build up. |
| 28–34 | **The storm** | The sky goes blood red. **Procedural lightning** strikes, each bolt a jagged chain of neon segments with forks that flickers, flashes the screen and booms. Then a rotating god's-eye view. |
| 34–37 | **Silence** | The image goes almost black-and-white. A slow push-in on the crystal while heartbeats speed up, each one pulsing the FOV, vignette, blur and the crystal's glow. "...it's awake." |
| 37 | **Shatter** | White flash, explosion, two **shockwave rings**, 46 crystal shards with trails, dust and sparks, heavy camera shake, and the camera is blown back. Glowing cracks spread across the ground. |
| 37–45 | **Bullet time** | A global time scale ramps down to 3%, freezing the shards, particles and shockwaves in mid-air while the camera orbits them **Matrix-style**. Then it snaps back to full speed with a whoosh. |
| 45–53 | **Ascension** | A golden orb with three **spinning gyroscope rings** rises out of the crater. The pillars re-ignite in gold and feed it, a pillar of light shoots into the sky, and **dawn breaks** as `ClockTime` sweeps from midnight to sunrise. Hero and crane camera shots follow it up. |
| 53–60 | **Supernova** | A huge pull-back reveal, then the orb goes supernova. White-out, then fade to black. The title card **decodes from random glyphs**, with red and cyan **chromatic ghost** copies jittering behind it. |

### Techniques worth stealing
- **Camera director:** each shot is a function of an eased 0→1 alpha that returns a camera CFrame and FOV. Bézier paths, orbits and tracking shots are all just functions.
- **Trauma-based shake:** Perlin noise (`math.noise`) scaled by trauma², which decays every frame, plus a separate constant "rumble" layer.
- **Global time scale:** shards use hand-written physics and shockwaves use a time-scaled animator, so one `NumberValue` can freeze the whole scene. `ParticleEmitter.TimeScale` slows the particles too.
- **Save and restore:** your Lighting, sky, atmosphere, post effects, clouds, camera, CoreGui, mouse icon and movement controls are saved before the cutscene and restored afterwards.

---

## Troubleshooting

- **Nothing happens:** make sure it's a **LocalScript** inside **StarterPlayerScripts**, then check the **Output** window (View → Output) for errors.
- **No sound, or "Failed to load sound" warnings:** one of the built-in sound paths may have changed. Replace it in `CONFIG.SOUNDS` with any Creator Store audio id. The cinematic still plays without it.
- **The glow looks flat:** set Lighting → Technology to **Future** and turn up graphics quality (Esc → Settings → Graphics Quality).
- **I can see my map in the background:** move `CONFIG.ORIGIN` further away.
