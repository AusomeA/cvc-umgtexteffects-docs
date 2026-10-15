# Tag syntax

Effects, presets and typewriter actions are written as tags in the text. There are three forms. All three go through the
same parser, so effects nest, stack, and cross Epic's style tags in every form.

| Form | Where it works | Example |
|---|---|---|
| Short tags | UTE Text Block | `<wave>Hello</>`, `<shake a=2>no!</shake>` |
| Brace tags | Anywhere: UTE Text Block, or Epic's Rich Text Block with the UTE decorator | `{wave}Hello{/}`, `{shake a=2}no!{/shake}` |
| Epic syntax | Epic's Rich Text Block with the UTE decorator | `<wave a="2">Hello</>`, `<pause t="0.5"/>` |

Names are case-insensitive: `<Wave>` and `<wave>` are the same tag.

## Short tags (UTE Text Block)

- Open with `<name>` and close with `</>` or `</name>`: `<wave>Hello</>`, `<wave>Hello</wave>`.
- Parameters go after the name: `<shake a=2 f=1.5>`. Quotes are optional: `<shake a="2">`.
- A bare value sets the amplitude: `<wave=2>` is `<wave a=2>`.
- Nesting stacks effects: `<wave><rainbow>Treasure!</></>` waves and cycles colors.
- Style rows work as in Epic's block, and effects can wrap or cross them: `<wave>a <Bold>b</> c</>`.
- Tags that are not effects, presets or typewriter actions are left to Epic's parser, so your style rows and other
  decorators' tags (`<img id="Coin"/>`) keep working.

## Brace tags (everywhere)

- `{wave}text{/}` or `{wave}text{/wave}`. Parameters as above: `{wave a=2}`.
- Brace tags work inside a stock Rich Text Block, inside style tags, and across style tags:
  `{wave}<Bold>river spirits</> are restless{/}`.
- A brace tag with an unknown name stays as plain text, so FText placeholders such as `{0}` and `{PlayerName}` are safe.
- Write <code>&#123;&#123;</code> (two opening braces) for a literal `{`.

## Epic syntax (stock Rich Text Block)

- `<wave a="2">text</>`: Epic's own parser reads it, so values are quoted and tags are flat (one Epic tag cannot nest
  inside another; use brace tags to nest).
- Add `style="RowName"` to apply a style row to the same text: `<wave style="Bold">text</>`.
- Self-closing actions: `<pause t="0.5"/>`, `<waitinput/>`, `<event name="sfx:creak"/>`.
- A literal `<` is `&lt;`, as in any Rich Text Block.

## Roles

Each effect has up to three roles. A tag without a role uses the effect's natural role (listed per effect in the
[effect reference](effects.md)).

| Role | Prefix | Short form (brace tags) | When it plays |
|---|---|---|---|
| Appear | `in.` | `{+pop}` | Once, when a letter is revealed by the typewriter (or when the text appears, if the typewriter is off) |
| Loop | `loop.` | `{~fade}` | All the time |
| Disappear | `out.` | `{-fade}` | Once, when Hide Text is called, or by itself after `after=` seconds |

Examples: `<in.pop>Level up!</>`, `<out.fade>gone soon</>`, `<loop.fade>breathing</>`,
`<in.drop><out.drop after=2>Bye</></>` (drops in, then falls away two seconds later).

A role the effect does not have falls back to its natural role, so `<loop.stamp>` plays the stamp's appear.

Without the typewriter, appear effects cascade through their tag's letters (0.03 s x `w` per letter, squeezed to about
1.2 s for long spans), so `<drop>Title</>` drops in letter by letter. Set `w=0` to make every letter start together.

## Common parameters

Every effect accepts these. Defaults depend on the effect (see the [effect reference](effects.md)).

| Parameter | Aliases | Meaning |
|---|---|---|
| `a` | `amp`, bare value | Amplitude. Motion in em (1 em = the font size, so effects scale with the text); rotation in units of 15 degrees; color effects use it as intensity from 0 to 1. Default 1 |
| `f` | `freq`, `speed` | Speed multiplier for loops. Default 1. `f=0` freezes a loop; a frozen Tint or Glow (and a Gradient with `scroll=0`) also stops repainting, so it costs nothing per frame |
| `w` | `wave` | Phase step between letters for loops; stagger between letters for appear and disappear without a typewriter |
| `t` | `dur` | Duration of an appear or disappear in seconds |
| `d` | `delay` | Delay in seconds before an appear or disappear starts |
| `ease` | | `linear`, `in`, `out`, `inout`, `back`, `elastic`, `bounce`. Default: the effect's own curve |
| `unit` | | `letter` (default), `word` or `line`: what moves together. Words and lines also rotate and scale around their own center |
| `steps` | | `0` = smooth. For loops, the number of updates per second (`steps=8` gives a stepped, retro look); for appear and disappear, the number of stages |
| `seed` | | Changes the random pattern of random effects (shake, wiggle, scatter, glitch, ...) |
| `dir` | | `up`, `down`, `left`, `right`, `side`, or a vector `x,y` (y points down). Used by wave, slide, wipe and roll |
| `color` | `colors`, `c` | One color, or up to three for a gradient: `colors=#ff5f6d,#ffc371` |
| `after` | | Disappear only: start the disappear this many seconds after the letter was revealed (the text vanishes by itself) |

**Colors** are `#RRGGBB`, `#RRGGBBAA` (sRGB, as in a color picker) or a name: `red`, `green`, `blue`, `cyan`, `yellow`,
`orange`, `pink`, `purple`, `magenta`, `gold`, `white`, `black`, `gray`/`grey`.

Effect-specific parameters (for example `height` for bounce, `charset` for decoder) are listed per effect in the
[effect reference](effects.md). Named values can be written as words or numbers: `charset=binary` or `charset=3`.

## Typewriter actions

These tags only matter when the typewriter is on. In a UTE Text Block they are written `<pause=0.5>`; on a stock Rich
Text Block, `{pause=0.5}` or `<pause t="0.5"/>`.

| Tag | Meaning |
|---|---|
| `<pause=0.5>` (also `<wait=0.5>`) | Wait 0.5 seconds before the next letter |
| `<speed=2>...</speed>` | Type the enclosed text 2 times faster (0.5 = half speed). Close with `</speed>` or `</>` |
| `<waitinput>` | Stop until Continue Typewriter is called. On Waiting For Input fires |
| `<event=Name:Argument>` | Fires On Inline Event (Name, Argument) when the typewriter reaches this point. Also `<event name=sfx p=creak>` |
| `<voice=Robot>` | Fires On Inline Event with Name `voice` and Argument `Robot` (switch your blip sound here) |
| `<skip>` | Reveals the rest of the text at once |

See [Typewriter](typewriter.md) for the settings and Blueprint events.

## Effects without tags (localization)

Translators should not have to keep effect tags intact. Two options:

- **Default Effects** (Details panel of the UTE Text Block or the decorator): an effect list applied to all text with no
  tags at all, for example `wave a=0.5; rainbow` or `angry`.
- **Preset tags** look like Epic's style tags (`<angry>...</>`), so translators can keep them the way they keep `<Bold>`.
  Put your own look behind a short name with Custom Presets (`Spell` -> `wave a=0.6; tint color=#ff8a3c f=0`).

Attaching effects to existing style rows (so a `<Bold>` row also waves) is not in 1.0.

Effect lists use the same names and parameters as tags, separated by `;`: `shake a=1.3; tint color=#ff3b30 f=0`.

## Presets

A preset is a named stack of effects used like an effect: `<angry>...</>`. There are 32 built in (see the
[preset reference](presets.md)), and you can add your own in **Custom Presets** (tag name to effect list) or with
**UTE Text Effect Preset** data assets.
