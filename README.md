# UMG Text Effects

Per-letter text effects and a typewriter for Unreal Engine UMG. 50 effects, 32 presets, short tags, and a live preview
in the UMG Designer. It works on Epic's own Rich Text Block (add one decorator) and as a drop-in UTE Text Block widget.
No engine changes, no shaders, full C++ source.

**Get it on Fab:** [UMG Text Effects](https://www.fab.com/listings/b359ade4-c187-4e95-9d6b-e8bed072560a) (UE 5.4-5.8).

```
Hello <wave>traveler</>, the <angry>dragon</> is <rainbow>awake</>!
```

## Documentation

| Page | What is in it |
|---|---|
| [Quick Start](quick-start.md) | Install, first animated text in 5 minutes (UTE Text Block and Epic's Rich Text Block), the demo |
| [Tag syntax](tag-syntax.md) | The three tag forms, roles (appear, loop, disappear), common parameters, typewriter tags, effects without tags |
| [Effect reference](effects.md) | All 50 effects with their roles, defaults and parameters |
| [Preset reference](presets.md) | All 32 presets and how to make your own |
| [Typewriter and Blueprint API](typewriter.md) | Typewriter settings, functions, events, recipes (dialogue box, voice blips, damage numbers), C++ |
| [Performance](performance.md) | What each kind of effect costs, measured, and how to measure on your hardware |
| [Engines and platforms](platforms.md) | Supported engine versions and platforms, what was tested where |
| [Known limitations](limitations.md) | What 1.0 does not do, and what to use instead |
| [Changelog](CHANGELOG.md) | Versions |

## What you get

- **50 effects** in five groups: motion (wave, bounce, shake, slide, drop, scatter...), rotation (swing, spin, hinge...),
  scale and shape (pop, stamp, jello, flip...), color and opacity (fade, rainbow, gradient, shine, glow...), and special
  rendering (wipe, roll, decoder, glitch, ghost, marker, underline, repel...). Most work as appear, loop and disappear.
- **32 presets** for moods and game moments: `<angry>`, `<whisper>`, `<scared>`, `<magic>`, `<neon>`, `<robot>`,
  `<titledrop>`, `<damagenumber>`, `<pickup>` and more. Add your own without code.
- **Typewriter** with pauses, speed changes, wait-for-input, inline events, skip and hide. Styles stay applied while
  typing and words never jump lines.
- **Epic's Rich Text Block keeps working.** Add the decorator to the blocks you have; your style rows and image tags stay.
- **Live preview in the UMG Designer**, including the typewriter.
- **Cheap to draw.** Moving letters are drawn in one element per text block; finished text is drawn by Epic's own code
  path. [Measured numbers](performance.md).
- **Demo map and widget** with every effect and preset labelled, and a dialogue box example.
- **UE 5.4 to 5.8**, Windows. Android compiled on 5.4 to 5.8.

## Support

Questions and bug reports: support@barbedwireglovegames.games, or the support contact on the UMG Text Effects page on Fab.
