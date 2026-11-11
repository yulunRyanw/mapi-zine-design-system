# Character: the mascot, icons, the poster, game art

Source: `mvp/themes/zine.css` lines 266–278 (mascot), 84–88 and 172–175 (icons), 339–358 (poster), 300–328 (Inspire me cards), 373–395 (the bench). The mascot's SVG and poses are in `mvp/make.js` (`MASCOT`, `mascotEl`, `setPose`).

## The mascot

The mascot is one outlined character: flat pink, a black line, no shading. It's the only illustration in the interface.

- **Fill:** `--pink` on the body and ears, white on the hands. **Line:** black, 2.6px, round joins. Eyes and mouth are black. *Why: it's drawn in the same ink line as every button, so it belongs to the page.*
- **Never** give it a gradient, a highlight, a soft shadow, sparkles or a second fill colour. The `.spark` elements in its SVG are hidden in Zine.
- **Line weight scales with size.** The tiny one (`.mk-masc.tiny`) uses 5.5px in its own coordinates so the line still reads at 30px. A new size needs its own stroke width; never scale the default one down.
- **Poses** are set with `setPose(el, pose)`: `idle`, `listening`, `thinking`, `hold`, `unroll`, `working`, `cheer`. Nothing else exists. Use the pose for the state the player is in; never animate it for decoration.

```
Which pose?
 ├── The player is typing            → listening
 ├── The player paused, or we wait   → thinking
 ├── It's showing a question card    → hold
 ├── It's revealing the proposal     → unroll
 ├── A game is being made            → working
 ├── A game is ready or made         → cheer
 └── Otherwise                       → idle
```

- **It reacts to what the player does, never to which words they typed.** *Why: there's no asset library and nothing classifies the text.*
- **One mascot per screen.** It never appears on Home, in a list row, or inside Messages.
- **Its speech** is a white outlined bubble (`.mk-bubble`, `.mk-say`), Figtree 600, one short sentence, no exclamation mark.

## Icons

Icons come from `App.icon(name)` in `core.js`: `home compass plus close back chev play share chat bolt search check undo heart x dice grid up bell apple user gear clock people`. Stroke icons, black, 2.4–2.6px in Zine.

- **Never `sparkle`.** Zine hides it on buttons and redraws it as a face on the Vibe card. Don't add a new use.
- **The bolt is a sticker:** yellow fill with a black outline wherever it appears, white-filled on a yellow chip. That's automatic; never tint it yourself.
- **A selected tab's icon is filled:** yellow on Home and Make, pink on Profile.
- **No emoji as interface icons.** Rows, buttons, tags and headers use `App.icon` or no icon. The notification's icon tile and the games' own art are the two places emoji still appear (`porting.md`).
- An icon needs a reason: it's the action's own symbol, or it marks a row's kind. A list where every row has a different decorative icon gets none.

## The poster and the game plan

The proposal is a poster (`.mk-poster`): a flat colour from the toy palette (`--poster`), a 2px outline, a 4px shadow, and a white outlined rod above and below it. It unrolls with a wipe.

- The name is display type, 40px, weight 800, width 78%. It lands last, with `App.fx('stamp')`.
- The plan's lines are separated by 2px ink rules, each with a small display label.
- **The poster carries no picture.** *Why: nothing exists to draw the game until it's built.*
- The editable plan from Inspire me (`.mk-plan`) is a white outlined region. A field being edited turns lemon.

## Inspire me cards

Each card (`.mk-card`) is a flat block of its role's colour, outlined, with a 4px shadow; the cards under it are the same colour at 45%. The role label is a white outlined pill, the phrase is display type at 44px, and the watermark icon sits at 14% opacity.

- **Keep and Skip stamps** (`.mk-stamp`) are white, with a 3px outline, rotated −8° and 8°. This is the only rotated element in the system. Never rotate anything else. *Why: rotation everywhere is the neobrutalist cliché; one rotated stamp is a joke that lands.*
- Kept slots in the tray fill with the card's colour; skipped slots are dashed.

## The bench

While a game is made, the mascot works at a bench on squared paper: a white outlined region with a 22px ink grid at 9% opacity. That grid is the only pattern in Zine, and it means "work in progress". Never use it as a general background.

The current step in the list is lemon; done steps show a teal tick.

## Game art

The stage (`.stage`, `App.stage`) is the game's own picture, and Zine doesn't restyle what's inside it. Around it:

- The stage is outlined with a 4px shadow, and stays pressed in while the game plays.
- Text over art sits on a solid plate: `.tapcue` is a black pill, a score is a yellow outlined plate, a tag is `.tag.dim`. Never white text straight on art, never a gradient scrim.
- A result covers the stage with a yellow flood and one number at 104px (`.dc-result`, `.t-score`).

Today the games draw emoji. That's the games' art, not the system's, and it needs its own style (`porting.md`).
