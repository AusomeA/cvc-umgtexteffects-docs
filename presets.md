# Preset reference (32 presets)

A preset is a named stack of effects used as a tag: `<angry>Get out!</>`, `{angry}Get out!{/}` on a stock Rich Text Block, or `angry` in Default Effects. Generated from the preset table in the plugin source (`UTEEffects.cpp`).

A style row with the same name as a preset stays a style (your style set wins). Presets can be combined with effects and with each other: `<angry><wave>...</></>`.

| Tag | What it looks like | Effects |
|---|---|---|
| `<angry>` | Shake + red tint | `shake a=1.3; loop.tint color=#ff3b30 f=0 a=0.85` |
| `<shout>` | Stamp + shake | `stamp from=2.2; shake a=0.8` |
| `<whisper>` | Soft fade + small float | `loop.fade min=0.55 f=0.5; float a=0.35 tilt=0` |
| `<scared>` | Small shake + flicker | `shake a=0.5 rate=16; flicker a=0.5` |
| `<cold>` | Fine shiver + cyan tint | `shake a=0.3 rate=30; loop.tint color=#8fdcff f=0 a=0.75` |
| `<hot>` | Wiggle + orange shine | `wiggle a=0.8; loop.tint color=#ff8a3c f=0 a=0.45; shine color=#ffd27a` |
| `<dizzy>` | Orbit + sway | `orbit a=0.8 f=0.6; sway a=0.8` |
| `<drunk>` | Slow wiggle + swing | `wiggle a=1.6 f=0.4; swing a=0.8 f=0.5` |
| `<sleepy>` | Slow float + breathing fade | `float a=0.6 f=0.5; loop.fade min=0.4 f=0.4` |
| `<happy>` | Bounce + soft rainbow | `bounce a=0.7; rainbow a=0.5` |
| `<love>` | Heartbeat + pink | `heartbeat; loop.tint color=#ff5fa2 f=0 a=0.8` |
| `<magic>` | Purple gradient + float + shine | `gradient colors=#b06cff,#6c9cff; float a=0.5 tilt=0.5; shine color=#ffffff` |
| `<ghostly>` | Float + flicker + ghost echoes | `float a=0.8; flicker a=0.4; ghost count=3 lag=1.4` |
| `<spooky>` | Dangle + breathing fade | `dangle a=1.2; loop.fade min=0.35 f=0.5` |
| `<robot>` | Decoder + stepped motion | `decoder charset=symbols; wave a=0.35 steps=5` |
| `<hacker>` | Binary decoder + green | `decoder charset=binary t=0.8; loop.tint color=#39ff7a f=0` |
| `<glitchy>` | Glitch + flicker | `glitch a=1.5; flicker a=0.4` |
| `<retro>` | Stepped bounce | `bounce steps=8` |
| `<neon>` | Cyan glow + flicker | `glow color=#3cf0ff; loop.tint color=#c8fbff f=0 a=0.6; flicker a=0.35` |
| `<royal>` | Gold + shine | `loop.tint color=#ffcf4a f=0 a=0.45; shine color=#fff1b8` |
| `<victory>` | Tada + gold shine | `tada; loop.tint color=#ffcf4a f=0 a=0.35; shine color=#fff1b8` |
| `<alarm>` | Red blink + shake | `blink duty=0.65 f=2; loop.tint color=#ff2a2a f=0; shake a=0.5` |
| `<poison>` | Wiggle + green | `wiggle a=0.7 f=0.6; loop.tint color=#8cff4a f=0 a=0.75` |
| `<frozen>` | Unfold + ice blue | `unfold; loop.tint color=#9fe6ff f=0 a=0.75` |
| `<underwater>` | Slow wave + blur | `wave a=0.7 f=0.4; loop.blur radius=0.5 a=0.8; loop.tint color=#7fb8ff f=0 a=0.35` |
| `<dream>` | Float + pastel gradient | `float a=0.6 f=0.6; gradient colors=#ffb3e6,#b3d9ff,#d9b3ff scroll=0.4` |
| `<typewriter>` | Instant letters + click voice event | `fade t=0.01; event=voice:click` |
| `<popquiz>` | Pop + tada | `pop; tada f=0.7` |
| `<titledrop>` | Drop + stamp | `drop height=1.4; stamp from=1.8 t=0.5` |
| `<creditsroll>` | Slow slide up | `slide dir=up dist=2.5 t=1.4 ease=inout` |
| `<damagenumber>` | Pop + rise + fade out | `pop overshoot=1.6; out.slide dir=up dist=1.2 after=0.15 t=1.1 ease=out; out.fade after=0.75 t=0.4` |
| `<pickup>` | Scale + shine | `scale from=0 t=0.3; shine color=#fff2a8` |

## Your own presets

- **Custom Presets** (Details panel of the UTE Text Block or the decorator): a map of tag name to effect list, for example `Spell` -> `wave a=0.6; tint color=#ff8a3c f=0; in.pop`. Then write `<Spell>Fireball</>`.
- **UTE Text Effect Preset** data asset: set Tag Name and Effects, then add the asset to Preset Assets. Share one asset between many widgets.
- Effect lists may name built-in presets (`angry; wave a=0.4`) and actions (`event=voice:click`).
