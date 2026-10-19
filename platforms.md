# Engines and platforms

## Engine versions

UMG Text Effects 1.0 supports **Unreal Engine 5.4, 5.5, 5.6, 5.7 and 5.8**. One source tree serves all five; the
download for each engine differs only in the `.uplugin`'s engine version and in the demo assets, which are saved by that
engine.

What we ran on each engine before release:

| Engine | Built from the download (RunUAT BuildPlugin) | Automation tests | Other checks |
|---|---|---|---|
| 5.4 | Win64 (Editor, Development, Shipping), 0 warnings | 105 of 105 pass | Android arm64 (Development, Shipping), NDK r25b |
| 5.5 | Win64 (Editor, Development, Shipping) and Android arm64 (Development, Shipping), 0 warnings | 105 of 105 pass | |
| 5.6 | Win64 (Editor, Development, Shipping) and Android arm64 (Development, Shipping), 0 warnings | 105 of 105 pass | Demo app run on an Android emulator |
| 5.7 | Win64 (Editor, Development, Shipping) and Android arm64 (Development, Shipping), 0 warnings | 105 of 105 pass | Benchmarks, UMG Designer preview |
| 5.8 | Win64 (Editor, Development, Shipping) and Android arm64 (Development, Shipping), 0 warnings | 105 of 105 pass | |

Every row was run on 2026-10-15 from the same files you download. The Android arm64 builds on 5.7 and 5.8 were added on 2026-10-19 (NDK 27.2), with the 105 tests run again on both.

## Platforms

| Platform | Status in 1.0 |
|---|---|
| Windows (Win64) | Supported. Built, tested and benchmarked on every engine version |
| Android | Supported. Built for arm64 with UE 5.4, 5.5, 5.6, 5.7 and 5.8 (5.7 and 5.8 compiled 2026-10-19; the headless tests pass on both). The demo app (UE 5.6) runs on an Android emulator: every effect, preset and the typewriter draw and animate, and Repel follows a finger. Read on one physical phone, a Galaxy S24+, at a steady 60 fps (see [Performance](performance.md#android)) |
| Mac, Linux, iOS | Not enabled in 1.0. The plugin has no platform-specific code and no shaders, but we could not compile or test these platforms, so the modules' platform allow lists do not include them yet |

The plugin draws only with Slate's own text elements (the same draws a stock Rich Text Block makes), so it works with
every renderer that draws Epic's text: DirectX 11 and 12, Vulkan, OpenGL ES.

## Requirements

- No engine changes, no shaders, no third-party libraries. Full C++ source.
- Modules: `UMGTextEffects` (runtime) and `UMGTextEffectsBench` (runtime: the `UTE.Bench`, `UTE.Gallery` and
  `UTE.Capture` console commands, and the C++ classes of the demo widget and demo actor). The bench module registers
  console commands only; it does nothing until you run one.
- Blueprint-only projects can use everything. C++ projects add `"UMGTextEffects"` to their module dependencies to call
  it from code.
