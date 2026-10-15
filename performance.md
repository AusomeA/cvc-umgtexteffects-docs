# Performance

Every number on this page is **measured** on our test PC unless it says otherwise. Your numbers depend on your
hardware, so the plugin ships the same benchmark: run `UTE.Bench` in a packaged or `-game` build (see the end of this
page).

## How the text is drawn

- **No widget per letter.** Effects run inside the text's own run objects. Layout, line breaking, kerning and shaping are
  Epic's code, so animation never changes where lines break.
- **Finished or idle text is drawn by Epic's own code path**, exactly like a stock Rich Text Block: zero extra cost.
  Two exceptions keep our path because their resting look is not plain text: a finished Marker or Underline (the box
  stays) and a sentence with letters that vanished on their own (`after=`, for example `<damagenumber>`). They are
  drawn only when the widget repaints, with no repaint timer.
- **Letters that only move** (wave, bounce, shake, wiggle, orbit, repel, the typewriter's settled letters) are drawn
  together as **one element per text block**, the same draw count as stock text.
- **Letters that rotate, scale, recolor or fade** are drawn one element per letter. Slate still merges them into the same
  draw calls (same layer, same font atlas); the extra cost is CPU time on the game thread, about 0.4 µs per letter on our
  PC.
- **Ticking stops when nothing moves.** A text whose effects have finished, or that is collapsed or off screen, costs
  nothing per frame. Static loops (a Tint or Glow with `f=0`, a Gradient with `scroll=0`) also stop repainting.
- **No per-frame memory allocations.** The animation path allocates nothing once a block has been drawn the first
  time (see below).

## Desktop numbers

Test PC: AMD Ryzen 9 7950X3D, NVIDIA RTX 4090, Windows 11, UE 5.7 Development editor binaries in `-game`, rendered
off-screen at 1920x1080, uncapped frame rate. Text: 20 Rich Text Blocks x 100 characters (2,000 characters, 1,644
visible letters), 15 pt with a drop shadow, one style tag per block. Metric: Slate time on the game thread (tick, paint,
batching), median of per-frame values over 300 frames after 90 warm-up frames. The PC was running other work, so the
table gives the **difference to stock text measured in the same run** (median of 3 runs).

| Scenario (2,000 characters, every letter) | Extra Slate time vs stock text |
|---|---|
| Decorator on the block, no effects (idle) | +0.02 ms |
| Typewriter running in all 20 blocks | -0.11 ms (letters not typed yet are not drawn) |
| Wave (decorator / UTE Text Block) | +0.07 / +0.08 ms |
| Shake / Shine / Flicker | +0.10 / +0.13 / +0.36 ms |
| Spin / Pulse / Jello / Float | +0.72 / +0.75 / +0.77 / +0.76 ms |
| Every letter spinning and pulsing | +0.74 ms |
| Rainbow / Swing | +0.76 / +0.72 ms |
| Gradient (static colors) | +0.92 ms |
| Glow halo / Glitch / Wave + Ghost (3 echoes) | +0.67 / +0.41 / +0.80 ms |
| Blur appear cascading over the text | +0.78 ms |
| For comparison: one Text Block widget per letter, static / wave | +4.24 / +4.35 ms |

Stock text itself measured 0.43 to 0.70 ms in these runs.

**10,000 characters** (100 blocks x 100, 11 pt): stock text 2.59 ms; idle -0.04 ms; wave +0.59 ms; every letter spinning and pulsing +5.64 ms; rainbow +5.81 ms; glow +5.38 ms; wave + ghost +5.96 ms (Slate time, 1 run).

**Global Invalidation** (`Slate.EnableGlobalInvalidation 1`): idle text stays cached and costs nothing. A block that
animates repaints the whole text block every frame, while cached stock text costs almost nothing, so the difference to
stock text is larger than without Global Invalidation: idle +0.00 ms, typewriter +0.35 ms, wave +0.74 ms, every letter spinning and pulsing +1.98 ms, rainbow +2.08 ms, glow +1.80 ms, wave + ghost +1.83 ms, and a static gradient +0.09 ms (2,000 characters, difference to cached stock text, 2 runs).

## Clipped letters (Wipe, Roll, Glitch)

These effects draw part of a letter through a clip rectangle. Slate starts a new draw call for every different clip
rectangle, so the cost depends on how many letters are being clipped **at the same moment**:

- **Roll** and a **vertical Wipe** (`dir=up` or `down`) share one clip rectangle per line for letters at the same stage,
  so even 2,000 letters rolling at once stay cheap.
- **Horizontal Wipe** (the default `dir=right`) needs one clip rectangle per letter: one extra draw call per letter
  while it is being wiped (two with the soft edge, `soft=0` removes the second), times two for text with a shadow.
- **Glitch** clips the few letters that glitch at a given moment.

Measured on the same PC (UE 5.7), 2,000 characters with a drop shadow. Render thread and GPU are the frame's render-thread
and GPU time; draw calls are the RHI draw calls of the whole frame. Stock text: 27 draw calls.

| 2,000 characters, every letter at the same moment | Draw calls | Render thread | GPU | Slate (game thread) |
|---|---|---|---|---|
| Stock text (reference) | 27 | | | |
| Roll | 225 | +0.56 ms | +0.00 ms | +1.41 ms |
| Glitch (loop) | 556 | +0.58 ms | +0.01 ms | +0.57 ms |
| Wipe up (vertical) | 425 | +3.39 ms | +0.09 ms | +2.85 ms |
| Wipe right, hard edge (`soft=0`) | 3,313 | +14.6 ms | +2.0 ms | +4.4 ms |
| Wipe right, soft edge (default) | 6,601 | +57.9 ms | +11.0 ms | +14.9 ms |
| Typewriter with Fade, 20 blocks typing | 27 | +0.44 ms | +0.00 ms | -0.04 ms |
| Typewriter with Roll, 20 blocks typing | 67 | +0.51 ms | +0.00 ms | +0.06 ms |
| Typewriter with Wipe, 20 blocks typing | 861 | +0.87 ms | +0.00 ms | +0.36 ms |

Median of 3 runs; differences are to stock text in the same run.

What this means in practice: a typed dialogue line or a title that wipes in costs a fraction of a millisecond. Wiping in
a full screen of text with every letter at the same moment is expensive; use Roll, a vertical Wipe, Fade, Slide or the
typewriter for large amounts of text.

## Memory allocations

Automation test `UMGTextEffects.Perf.NoPerFrameAllocations` counts every memory allocation made on the game thread
while text is painted, for 120 frames after 30 warm-up frames, under a parent clip as in a real game viewport. Measured
on UE 5.7 (2026-10-15):

| Case (one multi-line text, 120 measured frames) | Allocations per frame |
|---|---|
| Stock Rich Text Block (reference) | 0 |
| Decorator, no effects | 0 |
| Wave (moved letters) | 0 |
| Shake + Bounce | 0 |
| Rainbow, Spin + Pulse, Jello + Sway, static Gradient (one element per letter) | 0 |
| Wipe, Roll and Decoder appearing | 0 |
| Blur, Wave + Ghost, Glow, Lift | 0 |
| Repel with a moving pointer | 0 |
| Typewriter typing, Hide in progress | 0 |
| UTE Text Block with Wave and Rainbow | 0 |
| Shine + Flicker | 0.01 (1 in 120 frames) |
| Glitch | 0.01 (1 in 120 frames) |
| Marker + Underline loops | 0.01 (1 in 120 frames) |

The single allocation in three cases is Slate's own draw-element list growing the first time a frame draws more
elements than any frame before (a random number of letters glitch or flicker at once); it stops once the largest frame
has been seen. Text without effects allocates exactly what stock text allocates.

## Android

Measured on 2026-10-15 with the demo app built with UE 5.6 (Development, x86_64) on the Android emulator (Android 16
system image, 2560x1600, 4 virtual cores, no window) on the test PC. **An emulator is not a phone**: use these numbers as a
smoke test. One physical phone was also read (below); no other devices.

**One phone, measured 2026-10-15:** Samsung Galaxy S24+ (SM-S926U, Android 17), demo app built from the shipped source, effects page with all 50 effects animating. The demo's frame-time log read avg 16.69 ms, p50 16.67-16.75 ms, p95 18.08-18.46 ms, max 19.39-21.23 ms over 4 windows of 120 frames: a steady 60 fps (the display's vsync cap). This is one device; the per-2,000-letter `UTE.Bench` numbers below were not taken on the phone.

- Every effect card, every preset and the dialogue page draw and animate, with the same layout as on Windows.
- Repel follows a finger with no setup: touching and holding pushes the letters away, and they spring back on release.
- `UTE.Bench` (20 blocks x 100 characters, 300 frames, 2 runs; a first run while the app was still warming up was
  noisier and is not used). The frame time stayed at the
  emulator's 60 Hz display limit (16.6-16.7 ms) in every mode.

| 2,000 characters, every letter | Slate ms (p50) on the emulator | vs stock text |
|---|---|---|
| Stock text | 1.07-1.13 | |
| Decorator, no effects | 1.00-1.01 | -0.1 ms |
| Typewriter running in all 20 blocks | 1.27-1.72 | +0.2 to +0.6 ms |
| Wave (decorator / UTE Text Block) | 3.53-3.59 / 3.52-3.74 | +2.4 to +2.6 ms |
| Rainbow | 3.38-4.30 | +2.2 to +3.2 ms |
| Every letter spinning and pulsing | 4.22-5.17 | +3.1 to +4.0 ms |

On the emulator our own paint code costs about 0.9 µs per moving letter (about 0.1 µs on the desktop PC),
so a full screen of moving text costs a few milliseconds there, while a typed dialogue line costs a fraction of a
millisecond.

## Measure it yourself

`UTE.Bench` ships in the `UMGTextEffectsBench` module. In a Development or `-game` build, open the console and run:

```
UTE.Bench Blocks=20 Glyphs=100 Frames=300 Warmup=90 Modes=stock+ute_idle+ute_wave+ute_rainbow+fx_spin_pulse+ute_type Quit=1
```

Modes: `stock` (Epic's block), `ute_idle` (decorator, no effects), `ute_wave`, `ute_rainbow`, `ute_swing`, `ute_type`
(typewriter), `ute_widget_wave` (UTE Text Block), `fx_<effect>_<effect>` (any effects on every letter, for example
`fx_spin_pulse`), `clip_wipe`, `clip_roll`, `clip_glitch`, `type_<effect>` (typewriter with that appear effect),
`naive_static` and `naive_wave` (one Text Block widget per letter, for comparison). Results go to the log and to
`Saved/UTEBench/bench_<tag>.json` and `.csv`. Add `Shots=1` for two screenshots per mode.
