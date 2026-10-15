# Quick Start (5 minutes)

## 1. Install and enable

1. Install UMG Text Effects from Fab to your engine (Epic Games Launcher > Fab Library > Install to Engine), or copy the
   `UMGTextEffects` folder into your project's `Plugins` folder.
2. Open your project, go to **Edit > Plugins**, search for **UMG Text Effects**, tick **Enabled** and restart the editor.

C++ projects that call the plugin from code add `"UMGTextEffects"` to `PublicDependencyModuleNames` in their
`.Build.cs`. Blueprint-only projects need nothing else.

## 2. Option A: the UTE Text Block (2 minutes)

The UTE Text Block is a Rich Text Block with the effects built in. It keeps every Rich Text Block property.

1. Open a Widget Blueprint. In the **Palette**, under **Common**, drag **UTE Text Block** into the hierarchy.
2. In **Details > Appearance > Text Style Set**, pick a Rich Text style data table. Any table with the row structure
   `RichTextStyleRow` works; the sample table `DT_UTE_DemoStyles` (in the plugin's `Demo` folder) is ready to use.
3. Type in **Text**:

   ```
   Hello <wave>traveler</>, the <angry>dragon</> is <rainbow>awake</>!
   ```

The Designer animates it at once, without pressing Play. To type the text letter by letter, open
**Details > Text Effects > Typewriter** and tick **Enabled**. In the Designer the typewriter replays in a loop so you
can watch it; turn **Preview In Designer** off to stop the preview.

## 3. Option B: Epic's Rich Text Block (3 minutes)

Keep the Rich Text Blocks you already have and add the decorator:

1. Select your **Rich Text Block**. In **Details > Decorator Classes**, click **+** and choose
   **UTE Text Effects Decorator**.
2. Write effects as brace tags, or in Epic's tag syntax:

   ```
   The {wave}<Bold>river spirits</> are restless{/} tonight. Do <shake a="1.5">not</> look back.
   ```

Brace tags (`{wave}...{/}`) can wrap Epic's style tags and nest inside each other. Epic-syntax tags
(`<shake a="1.5">...</>`) are read by Epic's parser, so they are flat and their values are quoted.

**Typewriter or default effects on a stock block:** in the Content Browser, create a **Blueprint Class** whose parent is
**UTE Text Effects Decorator**, set its **Typewriter** and **Default Effects** in the class defaults, and add that class
to **Decorator Classes** instead.

## 4. Drive it from Blueprint

On a **UTE Text Block**, call the functions directly:

| Node | What it does |
|---|---|
| Play Typewriter | Types the text from the first letter (also shows hidden text again) |
| Skip Typewriter | Reveals the rest at once |
| Continue Typewriter | Goes on after a `<waitinput>` tag |
| Hide Text | Plays every letter's disappear effect, then hides the text |
| Set Typewriter Settings | Changes speed, appear effect and the rest at runtime |

Events: **On Letter Revealed**, **On Typewriter Finished**, **On Waiting For Input**, **On Inline Event**,
**On Text Hidden**.

On a **stock Rich Text Block**, use **Get Decorator By Class** (class: UTE Text Effects Decorator, or your Blueprint
subclass), cast the result to **UTE Text Effects Decorator**, and call the same functions and bind the same events.

The full list is in [Typewriter](typewriter.md).

## 5. Try the demo

The plugin includes a small demo in `UMGTextEffects Content > Demo`. If you do not see it, open the Content Browser's
**Settings** and tick **Show Plugin Content** (and **Show Engine Content** when the plugin is installed to the engine).

- **UTE_DemoMap**: open it and press **Play**. Three pages: all 50 effects, all 32 presets, and a dialogue box with the
  typewriter (Next skips the typing, continues after `<waitinput>`, then shows the next line; the log lists every event).
  The top line shows the frame time.
- **WBP_UTE_Demo**: open it to see the same pages animate in the UMG Designer. Click any card to see its markup in
  **Details > Text**, and copy it into your own widgets.
- **DT_UTE_DemoStyles**: the style rows the demo uses (`Default`, `Bold`, `Cyan`, `Red`).

The demo's logic is the C++ class `UUTEDemoWidget` (module `UMGTextEffectsBench`). The pages themselves are plain
widgets with UTE Text Blocks; nothing in them needs code.

## Next

- [Tag syntax](tag-syntax.md): roles, parameters, typewriter tags, effects without tags.
- [Effect reference](effects.md) and [preset reference](presets.md).
- [Performance](performance.md): what each kind of effect costs, with measured numbers.

## Need help?

Email support@barbedwireglovegames.games.
