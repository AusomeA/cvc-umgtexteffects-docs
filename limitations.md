# Known limitations (1.0)

What 1.0 does not do, and the edges we know about. Each item says what to do instead.

## Not in 1.0

- **Custom effects** (curve assets, Blueprint or C++ effect classes) are not in 1.0. Combine the 50 built-in effects
  with parameters, and save combinations as presets (Custom Presets or UTE Text Effect Preset assets).
- **Built-in typing sounds.** `<voice=Name>` and the `typewriter` preset fire On Inline Event (Name `voice`); play your
  own sound from On Letter Revealed. See [Typewriter](typewriter.md#recipes).
- **Effects attached to existing style rows** (a `<Bold>` row that also waves) and word rules. Use Default Effects or
  preset tags.
- **Editor extras**: a preset browser, tag highlighting and warnings in the Details panel, a "Replace with UTE Text Block"
  action. An unknown tag in a UTE Text Block is left to Epic's parser (style row lookup, else shown as typed).
- **Reduce Motion** project setting. Turn effects off by clearing Default Effects or by using text without tags.
- **Pixel units** (`a=4px`). Amplitudes are in em (font size), so effects scale with the text.
- A blinking **typewriter cursor** glyph.
- World-space 3D text actors (a Widget Component with a UTE Text Block works), dialogue-system integrations, and
  replication of typewriter state.

## Rendering

- **Clipped letters cost a draw call each.** Wipe, Roll and Glitch draw part of a letter through a clip rectangle, and
  Slate starts a new batch for every clip. A letter costs one extra draw call per frame while it is being clipped (two
  with Wipe's soft edge, plus its shadow). That is cheap for a typewriter or a title, and expensive for thousands of
  letters at the same moment. Numbers: [Performance](performance.md#clipped-letters-wipe-roll-glitch).
- **Scaled letters use the font atlas at its normal size**, so letters scaled far up (Pop overshoot, Stamp, Scale
  `from=3`) look soft at their largest. Slate's distance-field fonts should stay sharper; we have not tested them.
- **Glow** draws two hard outline copies (or recolors the outline when the style has one); it is not a blur. **Blur** is
  four offset copies.
- **Decoder** scrambles with ASCII letters, digits or symbols from the run's font. A font without those characters falls
  back to Slate's fallback font for the scrambled frames. The final letter is always your font.
- **Global Invalidation**: idle text stays cached. While a block animates, the whole Rich Text Block repaints every frame.

## Text and layout

- Right-to-left text (Arabic, Hebrew) and complex scripts (Thai, Devanagari) use Slate's own shaping, so they should draw
  correctly, and typing follows reading order. We have not tested them.
- Emoji and color fonts come from Slate unchanged; not tested with every effect.

## Epic's Rich Text Block (decorator)

The UTE Text Block avoids these; they apply only when the decorator is added to a stock Rich Text Block.

- Inline images from other decorators (`<img/>`) appear at once while the typewriter types; they are not our runs.
- Text overflow ellipsis is not applied inside a block while it animates.
- On UE 5.4 and 5.5, any tag that splits a line into runs (Epic's `<Bold>` or an Epic-syntax effect tag) widens that
  line by the text shadow's offset, as Epic's own style tags do there. Brace tags and the UTE Text Block do not split runs.
- A rich-text font size multiplier set on the marshaller is not read by animated runs.

## Behavior to know

- Setting the same text again does not restart the typewriter. Call Play Typewriter.
- An automatic disappear (`after=`) fires no event. Remove the widget on a timer if you need to.
- Ghost needs a moving effect next to it (`<wave><ghost>...</></>`); a ghost alone has nothing to echo.
- Repel follows a finger that is down, then the mouse. Set Pointer Position overrides both (gamepad cursor).
- Platforms other than Windows and Android are not enabled in 1.0; see [Engines and platforms](platforms.md).

## Support

Found something not listed here? Email support@barbedwireglovegames.games with your engine version and a short description.
