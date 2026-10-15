# Changelog

## 1.0.0 (first release)

- 50 per-letter effects in five groups (motion, rotation, scale and shape, color and opacity, special rendering), most
  of them usable as appear, loop and disappear.
- 32 presets (`<angry>`, `<shout>`, `<whisper>`, `<magic>`, `<titledrop>`, `<damagenumber>`, ...), plus your own
  through Custom Presets or UTE Text Effect Preset data assets.
- Works on Epic's Rich Text Block through the UTE Text Effects Decorator, and as the UTE Text Block widget
  (a Rich Text Block with short tags and the effects built in).
- Three tag forms: short tags (`<wave>`), brace tags (`{wave}...{/}`, also on stock blocks and across style tags) and
  Epic syntax (`<wave a="2">`). Effects without tags through Default Effects.
- Typewriter: by letter or word; forward, backward, random or center-out; pause, speed, wait-for-input, event, voice and
  skip tags; per-letter, finished, waiting, inline-event and hidden events; Hide Text plays disappear effects;
  frame-rate-independent timing; styles kept and no word jumping to the next line while typing.
- Live preview in the UMG Designer, including the typewriter.
- Repel follows the mouse and touches (no setup on phones).
- Demo content: `UTE_DemoMap` and `WBP_UTE_Demo` (every effect, every preset, a typewriter dialogue with events and a
  frame-time readout), `DT_UTE_DemoStyles`.
- Performance: no widget per letter, idle and finished text drawn by Epic's own path (a finished marker or underline
  keeps its box: one box per text block, no repaint timer), no per-frame memory allocations in the animation path,
  `UTE.Bench` console command to measure on your hardware.
- Marker and Underline keep their box after the draw-in; `out.marker` and `out.underline` wipe it away on Hide Text.
- 105 automation tests (Session Frontend > Automation, filter `UMGTextEffects`).
- Unreal Engine 5.4, 5.5, 5.6, 5.7 and 5.8. Windows and Android.
