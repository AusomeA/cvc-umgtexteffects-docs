# Effect reference (50 effects)

Generated from the effect table in the plugin source (`UTEEffects.cpp`), so it matches the code. Every tag works in all three forms (see [Tag syntax](tag-syntax.md)): `<wave>` in a UTE Text Block, `{wave}...{/}` anywhere, and `<wave a="2">...</>` on Epic's Rich Text Block with the UTE decorator.

**Roles.** *Loop* plays all the time, *appear* plays once when a letter is revealed, *disappear* plays once when the text is hidden (or by itself with `after=`). The natural role is used when a tag names no role. Pick another role with a prefix: `<in.pop>`, `<out.fade>`, `<loop.fade>` (short forms `{+pop}`, `{-fade}`, `{~fade}`). A role an effect does not have falls back to its natural role.

**Common parameters** work on every effect: `a` amplitude, `f` speed, `w` phase step between letters, `t` duration, `d` delay, `ease`, `unit`, `steps`, `seed`, `dir`, `color`/`colors`, `after`. See [Tag syntax](tag-syntax.md#common-parameters). The tables below list the defaults and the parameters that only one effect has.

## Contents

- [Motion (12)](#motion): `wave`, `bounce`, `shake`, `wiggle`, `float`, `orbit`, `jump`, `erratic`, `slide`, `drop`, `scatter`, `converge`
- [Rotation (7)](#rotation): `swing`, `pendulum`, `dangle`, `spin`, `hinge`, `spiral`, `tada`
- [Scale and shape (12)](#scale-and-shape): `pulse`, `heartbeat`, `pop`, `scale`, `stamp`, `rise`, `unfold`, `flip`, `jello`, `rubber`, `sway`, `ripple`
- [Color and opacity (9)](#color-and-opacity): `fade`, `rainbow`, `gradient`, `shine`, `blink`, `flicker`, `tint`, `glow`, `damage`
- [Special rendering (10)](#special-rendering): `wipe`, `roll`, `blur`, `decoder`, `glitch`, `ghost`, `marker`, `underline`, `lift`, `repel`

## Motion

### 1. wave

Letters ride a sine wave (dir=side for sideways).

- Tag: `<wave>...</>`; roles: loop / appear / disappear (natural: **loop**)
- Defaults: duration `t=0.5` s, phase step `w=1`, direction `dir=up`

### 2. bounce

Letters hop; as appear they drop in and settle with a bounce.

- Tag: `<bounce>...</>`; roles: loop / appear / disappear (natural: **loop**)
- Defaults: duration `t=0.5` s, phase step `w=1`
- Parameters:
  - `height` (default `1`): Hop height multiplier.

### 3. shake

Random jumps (anger, fear, cold).

- Tag: `<shake>...</>`; roles: loop / appear / disappear (natural: **loop**)
- Defaults: duration `t=0.4` s, phase step `w=1`
- Parameters:
  - `rate` (default `12`): Jumps per second.

### 4. wiggle

Smooth random drift per letter.

- Tag: `<wiggle>...</>`; roles: loop / appear / disappear (natural: **loop**)
- Defaults: duration `t=0.4` s, phase step `w=1`

### 5. float

Slow bob with random phase and a slight tilt (buoy, ghost).

- Tag: `<float>...</>`; roles: loop / appear / disappear (natural: **loop**)
- Defaults: duration `t=0.6` s, phase step `w=1`
- Parameters:
  - `tilt` (default `1`): Tilt while floating (0 = no tilt).

### 6. orbit

Letters circle around their rest point (tornado).

- Tag: `<orbit>...</>`; roles: loop / appear / disappear (natural: **loop**)
- Defaults: duration `t=0.6` s, phase step `w=1`
- Parameters:
  - `radius` (default `1`): Circle radius multiplier.
  - `cw` (default `1`): 1 = clockwise, 0 = counter-clockwise.

### 7. jump

Squash-and-stretch hop (cartoon jump).

- Tag: `<jump>...</>`; roles: loop / appear / disappear (natural: **loop**)
- Defaults: duration `t=0.6` s, phase step `w=1`
- Parameters:
  - `squash` (default `1`): Squash-and-stretch amount (0 = none).

### 8. erratic

Random position and rotation snaps at a low rate.

- Tag: `<erratic>...</>`; roles: loop / appear / disappear (natural: **loop**)
- Defaults: duration `t=0.5` s, phase step `w=1`
- Parameters:
  - `rate` (default `4`): Snaps per second.

### 9. slide

Slides in from a direction with a fade; loop = drift back and forth.

- Tag: `<slide>...</>`; roles: appear / loop / disappear (natural: **appear**)
- Defaults: duration `t=0.3` s, phase step `w=1`, direction `dir=up`
- Parameters:
  - `dist` (default `1`): Travel distance multiplier.
- Note: `dir=up|down|left|right|x,y` sets the direction.

### 10. drop

Falls from above and bounces to rest; disappear = falls away with gravity and spin.

- Tag: `<drop>...</>`; roles: appear / disappear (natural: **appear**)
- Defaults: duration `t=0.7` s, phase step `w=1`
- Parameters:
  - `height` (default `1`): Fall height multiplier.
  - `bounces` (default `2`): Bounces after landing (0-6).

### 11. scatter

Letters fly in from random directions (assemble) or explode out.

- Tag: `<scatter>...</>`; roles: appear / disappear (natural: **appear**)
- Defaults: duration `t=0.6` s, phase step `w=1`
- Parameters:
  - `radius` (default `1`): How far letters fly (multiplier).
  - `spin` (default `1`): Spin while flying (0 = none).

### 12. converge

Letters slide in from the word's edges (tracking) or center, or spread out.

- Tag: `<converge>...</>`; roles: appear / disappear (natural: **appear**)
- Defaults: duration `t=0.5` s, phase step `w=0`
- Parameters:
  - `from` (default `edges`): `edges` (from the word's edges) or `center`.

## Rotation

### 13. swing

Rocks around its baseline.

- Tag: `<swing>...</>`; roles: loop / appear / disappear (natural: **loop**)
- Defaults: duration `t=0.5` s, phase step `w=1`
- Parameters:
  - `pivot` (default `0.85`): Pivot height in the letter cell (0 = top, 1 = bottom).

### 14. pendulum

Synchronized swing around a point above the letter (hanging).

- Tag: `<pendulum>...</>`; roles: loop / appear / disappear (natural: **loop**)
- Defaults: duration `t=0.8` s, phase step `w=0`
- Parameters:
  - `length` (default `1`): Distance of the pivot above the letter (multiplier).

### 15. dangle

Hangs from its top edge and sways with noise, like a sign in wind.

- Tag: `<dangle>...</>`; roles: loop / appear / disappear (natural: **loop**)
- Defaults: duration `t=0.8` s, phase step `w=1`

### 16. spin

Continuous rotation; as appear, spins into place.

- Tag: `<spin>...</>`; roles: loop / appear / disappear (natural: **loop**)
- Defaults: duration `t=0.5` s, phase step `w=1`
- Parameters:
  - `turns` (default `1`): Full turns per cycle (loop) or during the transition.

### 17. hinge

Swings on its top corner, then falls; as appear, swings into place.

- Tag: `<hinge>...</>`; roles: disappear / appear (natural: **disappear**)
- Defaults: duration `t=1` s, phase step `w=1`
- Parameters:
  - `corner` (default `left`): `left` or `right` top corner.

### 18. spiral

Spirals in while the radius shrinks.

- Tag: `<spiral>...</>`; roles: appear / disappear (natural: **appear**)
- Defaults: duration `t=0.7` s, phase step `w=1`
- Parameters:
  - `radius` (default `1`): Start radius multiplier.
  - `turns` (default `1`): Turns while spiralling in or out.

### 19. tada

Attention seeker: scale up plus a quick rotation wiggle, repeating.

- Tag: `<tada>...</>`; roles: loop / appear (natural: **loop**)
- Defaults: duration `t=0.9` s, phase step `w=0`

## Scale and shape

### 20. pulse

Breathes in scale.

- Tag: `<pulse>...</>`; roles: loop / appear / disappear (natural: **loop**)
- Defaults: duration `t=0.4` s, phase step `w=1`

### 21. heartbeat

Double lub-dub pulse.

- Tag: `<heartbeat>...</>`; roles: loop (natural: **loop**)
- Defaults: phase step `w=0`

### 22. pop

Scales from 0 with overshoot; loop = periodic bump.

- Tag: `<pop>...</>`; roles: appear / loop / disappear (natural: **appear**)
- Defaults: duration `t=0.35` s, phase step `w=1`
- Parameters:
  - `overshoot` (default `1`): Overshoot past full size (0 = none).

### 23. scale

Grow, shrink or zoom with easing, no overshoot (from=0 grow, from=3 zoom).

- Tag: `<scale>...</>`; roles: appear / disappear / loop (natural: **appear**)
- Defaults: duration `t=0.35` s, phase step `w=1`
- Parameters:
  - `from` (default `0`): Start scale: 0 = grow from nothing, 3 = zoom in from three times the size.

### 24. stamp

Slams down from large with a small impact shake.

- Tag: `<stamp>...</>`; roles: appear (natural: **appear**)
- Defaults: duration `t=0.45` s, phase step `w=1`
- Parameters:
  - `from` (default `3`): Start scale of the slam (1 or more).
  - `impact` (default `1`): Shake strength when it lands.

### 25. rise

Grows up from the baseline (sprout).

- Tag: `<rise>...</>`; roles: appear / disappear (natural: **appear**)
- Defaults: duration `t=0.35` s, phase step `w=1`

### 26. unfold

Opens from the center line like a blind (compress).

- Tag: `<unfold>...</>`; roles: appear / disappear (natural: **appear**)
- Defaults: duration `t=0.35` s, phase step `w=1`
- Parameters:
  - `axis` (default `y`): `y` (opens vertically) or `x`.

### 27. flip

Card-turn flip (scale through zero on one axis).

- Tag: `<flip>...</>`; roles: appear / loop / disappear (natural: **appear**)
- Defaults: duration `t=0.5` s, phase step `w=1`
- Parameters:
  - `axis` (default `y`): `y` (turns around the vertical axis) or `x`.
  - `turns` (default `1`): Half turns count (at least 0.5).

### 28. jello

Wobbly squash and shear.

- Tag: `<jello>...</>`; roles: loop / appear (natural: **loop**)
- Defaults: duration `t=0.8` s, phase step `w=1`

### 29. rubber

Stretches X while compressing Y, then back (rubber band).

- Tag: `<rubber>...</>`; roles: loop / appear (natural: **loop**)
- Defaults: duration `t=0.8` s, phase step `w=1`

### 30. sway

Leans back and forth like grass in wind (shear).

- Tag: `<sway>...</>`; roles: loop / appear / disappear (natural: **loop**)
- Defaults: duration `t=0.6` s, phase step `w=1`
- Parameters:
  - `shear` (default `1`): Lean amount.

### 31. ripple

A swelling wave of scale travels through the text.

- Tag: `<ripple>...</>`; roles: loop (natural: **loop**)
- Defaults: phase step `w=1`

## Color and opacity

### 32. fade

Fades in or out; loop = breathe.

- Tag: `<fade>...</>`; roles: appear / loop / disappear (natural: **appear**)
- Defaults: duration `t=0.25` s, phase step `w=1`
- Parameters:
  - `min` (default `0.25`): Loop only: the lowest opacity of the breathing fade.

### 33. rainbow

Hue cycles across the letters.

- Tag: `<rainbow>...</>`; roles: loop / appear (natural: **loop**)
- Defaults: duration `t=0.6` s, phase step `w=1`
- Parameters:
  - `sat` (default `0.85`): Color saturation (0-1).

### 34. gradient

Color gradient across the letters, optionally scrolling.

- Tag: `<gradient>...</>`; roles: loop (natural: **loop**)
- Defaults: phase step `w=1`, color `#ff5f6d`, `#ffc371`
- Parameters:
  - `scroll` (default `0`): Scroll speed of the gradient (0 = static and cheap).
- Note: `colors=a,b[,c]` sets up to three colors.

### 35. shine

A highlight band sweeps across the text.

- Tag: `<shine>...</>`; roles: loop (natural: **loop**)
- Defaults: phase step `w=1`, color `#ffc83d`
- Parameters:
  - `width` (default `3`): Width of the highlight band, in letters.
- Note: `color=` sets the band color.

### 36. blink

On/off strobe.

- Tag: `<blink>...</>`; roles: loop (natural: **loop**)
- Defaults: phase step `w=0`
- Parameters:
  - `duty` (default `0.5`): Fraction of each cycle the text is visible (0-1).

### 37. flicker

Neon-style random dimming.

- Tag: `<flicker>...</>`; roles: loop (natural: **loop**)
- Defaults: phase step `w=1`

### 38. tint

Flashes to a color and back; loop = pulse to the color (f=0 holds it).

- Tag: `<tint>...</>`; roles: appear / loop / disappear (natural: **appear**)
- Defaults: duration `t=0.5` s, phase step `w=1`, color `#ff3b30`
- Note: `color=` sets the color; `f=0` holds the color without pulsing (no repaint cost).

### 39. glow

Pulses the outline color, or a halo when the style has no outline.

- Tag: `<glow>...</>`; roles: loop (natural: **loop**)
- Defaults: phase step `w=0`, color `#ffcf5a`
- Parameters:
  - `size` (default `1`): Halo or outline thickness multiplier.
- Note: `color=` sets the glow color; `f=0` holds it.

### 40. damage

Red flash plus a small shake (hit reaction), one-shot by default.

- Tag: `<damage>...</>`; roles: appear / loop (natural: **appear**)
- Defaults: duration `t=0.45` s, phase step `w=0`, color `#ff2a2a`
- Note: `color=` sets the flash color.

## Special rendering

### 41. wipe

Each letter is revealed by a moving clip edge.

- Tag: `<wipe>...</>`; roles: appear / disappear (natural: **appear**)
- Defaults: duration `t=0.35` s, phase step `w=1`, direction `dir=right`
- Parameters:
  - `soft` (default `1`): Soft edge width (0 = hard edge).
- Note: `dir=right|left|up|down` sets the direction of the moving edge.

### 42. roll

The letter rolls in from below inside its own cell (odometer).

- Tag: `<roll>...</>`; roles: appear / disappear (natural: **appear**)
- Defaults: duration `t=0.35` s, phase step `w=1`, direction `dir=up`
- Note: `dir=up|down|left|right` sets where the letter rolls in from.

### 43. blur

Enters blurred and sharpens (offset copies).

- Tag: `<blur>...</>`; roles: appear / disappear / loop (natural: **appear**)
- Defaults: duration `t=0.45` s, phase step `w=1`
- Parameters:
  - `radius` (default `1`): Spread of the blur copies (multiplier).
- Note: An approximation drawn with four offset copies, not a true blur.

### 44. decoder

Random characters cycle, then resolve to the real letter (scramble, hacker).

- Tag: `<decoder>...</>`; roles: appear (natural: **appear**)
- Defaults: duration `t=0.6` s, phase step `w=1`
- Parameters:
  - `charset` (default `letters`): `letters`, `digits`, `symbols`, `binary` or `mixed`.
  - `order` (default `forward`): `forward` (all letters resolve together) or `random` (each at its own moment).
- Note: Scrambled characters come from the run's font; a font without them falls back to Slate's fallback font.

### 45. glitch

Random slice jumps with red/cyan ghost copies.

- Tag: `<glitch>...</>`; roles: loop / appear / disappear (natural: **loop**)
- Defaults: duration `t=0.5` s, phase step `w=1`
- Parameters:
  - `split` (default `1`): Distance of the red and cyan copies (multiplier).

### 46. ghost

Trailing echoes behind moving letters (combine with a motion effect).

- Tag: `<ghost>...</>`; roles: loop (natural: **loop**)
- Defaults: phase step `w=1`
- Parameters:
  - `count` (default `3`): Number of echoes (1-8).
  - `lag` (default `1`): Delay between echoes (multiplier).
- Note: Draws echoes of the other effects on the same letters, so pair it with a motion effect: `<wave><ghost>...</></>`.

### 47. marker

A highlighter stroke draws in behind the text and stays; disappear = wipes it away.

- Tag: `<marker>...</>`; roles: appear / loop / disappear (natural: **appear**)
- Defaults: duration `t=0.5` s, phase step `w=1`, color `#ffdb1a70`
- Parameters:
  - `height` (default `1`): Marker height in em.
- Note: `color=#RRGGBBAA` sets the marker color (alpha included).

### 48. underline

An underline or strike line draws in and stays; disappear = wipes it away.

- Tag: `<underline>...</>`; roles: appear / disappear / loop (natural: **appear**)
- Defaults: duration `t=0.4` s, phase step `w=1`
- Parameters:
  - `pos` (default `under`): `under` (underline) or `strike` (strike-through).
  - `thick` (default `1`): Line thickness multiplier.
- Note: `color=` sets the line color (default: the text color).

### 49. lift

The letter rises while its drop shadow grows (paper lift).

- Tag: `<lift>...</>`; roles: loop / appear (natural: **loop**)
- Defaults: duration `t=0.6` s, phase step `w=1`

### 50. repel

Letters push away from the mouse or touch point and spring back.

- Tag: `<repel>...</>`; roles: loop (natural: **loop**)
- Defaults: phase step `w=1`
- Parameters:
  - `radius` (default `1`): Reach of the push, in units of 2.5 em.
- Note: Follows the mouse and touches automatically. Set Pointer Position overrides it (gamepad cursor, scripted pointer).
