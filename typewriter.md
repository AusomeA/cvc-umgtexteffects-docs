# Typewriter and Blueprint API

The typewriter reveals text letter by letter (or word by word). Every revealed letter plays an appear effect, styles
stay applied while typing, and the layout never changes mid-typing: a word never starts on one line and jumps to the
next, because hidden letters keep their space.

## Settings

On a **UTE Text Block**: **Details > Text Effects > Typewriter**. On a stock Rich Text Block: the **Typewriter**
property of a Blueprint subclass of **UTE Text Effects Decorator** (see [Quick Start](quick-start.md#3-option-b-epics-rich-text-block-3-minutes)).
At runtime: **Set Typewriter Settings** (takes effect on the next Play Typewriter or text change).

| Setting | Default | Meaning |
|---|---|---|
| Enabled | off | Reveal the text over time instead of showing it at once |
| Auto Play | on | Start typing whenever the text changes |
| Unit | Letter | Reveal by `Letter` or by `Word` |
| Order | Forward | `Forward` (reading order), `Backward`, `Random` (repeatable for the same text), `CenterOut` |
| Letters Per Second | 40 | Typing speed at speed 1 |
| Comma Pause | 0.08 s | Extra wait after `, ; :` |
| Sentence Pause | 0.25 s | Extra wait after `. ! ?` and line breaks |
| Appear Effect | Fade | Effect every revealed letter plays, unless its tag has its own appear effect (`<in.pop>`). `None` shows letters at once |
| Appear Duration | 0.2 s | Duration of that appear effect |
| Disappear Effect | Fade | Effect Hide Text plays on letters whose tag has no disappear effect of its own |
| Disappear Duration | 0.25 s | Duration of that disappear effect |
| Hide Stagger | 0.015 s | Delay between letters when Hide Text plays the disappear effects (in reveal order) |
| Loop For Preview | off | Replay forever (demos). The UTE Text Block replays in the Designer on its own while Preview In Designer is on |
| Loop Hold | 1.5 s | How long a replaying text stays complete before it starts again |

Orders other than Forward use a fixed time per letter (comma and sentence pauses and `<pause>` tags apply in Forward only).

## Tags inside the text

| Tag | Meaning |
|---|---|
| `<pause=0.5>` | Wait 0.5 seconds before the next letter |
| `<speed=2>...</speed>` | Type the enclosed text twice as fast (0.5 = half speed) |
| `<waitinput>` | Stop until Continue Typewriter is called; On Waiting For Input fires |
| `<event=Name:Argument>` | On Inline Event fires with Name and Argument when the typewriter gets here |
| `<voice=Robot>` | On Inline Event fires with Name `voice` and Argument `Robot` |
| `<skip>` | Reveal the rest at once |

On a stock Rich Text Block write them as `{pause=0.5}` or `<pause t="0.5"/>`. See [Tag syntax](tag-syntax.md).

## Functions

The UTE Text Block and the UTE Text Effects Decorator have the same functions (Blueprint category **UMG Text Effects**).
For a stock Rich Text Block, get the decorator with **Get Decorator By Class** and cast it to
**UTE Text Effects Decorator**.

| Function | What it does |
|---|---|
| Play Typewriter | Types from the first letter. Also shows the text again after Hide Text. With the typewriter off, it shows all text and replays the appear effects |
| Skip Typewriter | Reveals every remaining letter now (they still play their appear effect) |
| Continue Typewriter | Goes on after a `<waitinput>` tag |
| Hide Text | Plays every letter's disappear effect, staggered, then hides the text (it draws nothing until Play Typewriter) |
| Is Typewriter Playing | True while letters are still to come (also while waiting for input) |
| Is Waiting For Input | True while stopped at a `<waitinput>` tag |
| Is Text Hidden | True after Hide Text finished |
| Set Typewriter Settings | Replaces the typewriter settings |
| Set Pointer Position / Clear Pointer Position | Repel: use this screen position as the pointer (a gamepad cursor, a scripted pointer). Without it Repel follows the finger, then the mouse |

## Events

| Event | When |
|---|---|
| On Letter Revealed (Letter Index, Letter) | Every revealed letter. Use it for voice blips |
| On Typewriter Finished | The last letter was revealed |
| On Waiting For Input | The typewriter stopped at a `<waitinput>` tag |
| On Inline Event (Name, Argument) | The typewriter reached an `<event>` or `<voice>` tag |
| On Text Hidden | Hide Text finished playing the disappear effects |

The clock does not depend on drawing: once the text has been laid out, a collapsed or off-screen widget keeps typing
and its events still fire. (Text set while the widget is collapsed starts typing when the widget is first laid out.) Reveal
times are exact and independent of the frame rate (a letter revealed between two frames starts its appear effect at
its exact time).

## Recipes

**Dialogue box with one Next button.** On click: if **Is Waiting For Input**, call **Continue Typewriter**; else if
**Is Typewriter Playing**, call **Skip Typewriter**; else set the next line with **Set Text** (Auto Play starts typing).
The demo widget `WBP_UTE_Demo` does exactly this (see `UUTEDemoWidget::Next` in the `UMGTextEffectsBench` module).

**Voice blips.** Bind **On Letter Revealed**; skip spaces and play a short sound every second letter with a slightly
random pitch. Put `<voice=Robot>` in the text and switch the sound in **On Inline Event** when Name is `voice`. The
plugin has no built-in blip sounds in 1.0.

**Show the same line again.** Setting the same text again does not restart the typewriter; call **Play Typewriter**.

**Damage numbers and toasts.** `<damagenumber>-125</>` pops in, rises and fades out by itself (the preset uses
`out.slide after=0.15` and `out.fade after=0.75`). Remove or recycle the widget about 1.2 seconds later; no event fires
for an automatic disappear.

**Title cards.** `<titledrop>CHAPTER ONE</>` drops and stamps each letter in. Without the typewriter the letters cascade
on their own; add `w=0` to an effect to start all letters together.

**Clear a dialogue line, then type the next.** Call **Hide Text**; on **On Text Hidden**, call **Set Text** with the
new line.

## C++

```cpp
// MyWidget.Build.cs: PublicDependencyModuleNames.Add("UMGTextEffects");
#include "UTETextBlock.h"

void UMyDialogue::ShowLine(const FText& Line)
{
    DialogueText->SetText(Line); // a UUTETextBlock* bound with meta = (BindWidget)
    // Auto Play starts typing when the text changes.
}

void UMyDialogue::OnNextPressed()
{
    if (DialogueText->IsWaitingForInput()) { DialogueText->ContinueTypewriter(); }
    else if (DialogueText->IsTypewriterPlaying()) { DialogueText->SkipTypewriter(); }
    else { ShowNextLine(); }
}
```

The events are dynamic multicast delegates (`OnLetterRevealed`, `OnTypewriterFinished`, `OnWaitingForInput`,
`OnInlineEvent`, `OnTextHidden`); bind them with `AddDynamic`. For a stock Rich Text Block, call
`GetDecoratorByClass(UUTERichTextDecorator::StaticClass())` and cast to `UUTERichTextDecorator`.
