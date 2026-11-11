# Depth, shape and motion

Source: the depth tokens in `mvp/themes/zine.css` line 25–26, and their uses throughout the file. Radii and curves are in `mvp/tokens.css`.

## The tokens

| Token | Value | Use |
| --- | --- | --- |
| `--z-out` | `2px solid var(--z-ink)` | The outline on every surface |
| `--z-sh` | `4px 4px 0 var(--z-ink)` | The shadow under big pressables and cards |
| `--z-sh-s` | `2px 2px 0 var(--z-ink)` | The shadow under small pressables |
| `--z-sh-0` | `0 0 0 var(--z-ink)` | The pressed state |
| `--z-press` | transform and box-shadow over 0.09s, colours over 0.2s | The transition on every pressable |

Never write a shadow by hand. No blur radius, no rgba shadow, no `filter: drop-shadow`, no `backdrop-filter`, anywhere in Zine.

## Which treatment?

```
What is it?
 ├── It only holds other things: a list, panel, header, tab bar, stat tile, plan
 │      → border: var(--z-out). No shadow.
 ├── You press it or pick it up, and it's 44px tall or more:
 │   a button, an option, a card, a game cover, the stage, the poster, the idea box, a sheet
 │      → var(--z-out) + var(--z-sh). Press: translate(4px, 4px) + var(--z-sh-0).
 ├── You press it and it's small: close and back, a pill, the energy chip, a sheet row, a profile tile, the Make pill
 │      → var(--z-out) + var(--z-sh-s). Press: translate(2px, 2px) + var(--z-sh-0).
 ├── It's a tiny mark: a tag, an avatar, a tray slot
 │      → a 1.5px ink outline. No shadow.
 └── It floats briefly: a toast
        → var(--z-out) + 3px 3px 0 ink. It isn't pressable.
```

- **A full-width row inside a list doesn't move when pressed.** It turns lemon: `button.lrow:active { background: var(--lemon) }`. *Why: a row can't sink without dragging its neighbours' outlines.*
- **A chosen or "on" thing stays pressed in, and is lemon:** `.pill.on`, `.mk-opt.chosen`. A Home card stays pressed in while its game plays (`.dc-playing .dc-hstage`). *Why: pressed-in is the system's word for "active".*
- **A black button's shadow is drawn as an outline,** so it shows against the ink: `box-shadow: 4px 4px 0 -2px #fff, 4px 4px 0 0 var(--z-ink)` (`.cta.white`).
- **An input gains its shadow on focus:** `.input:focus { box-shadow: var(--z-sh) }`. Never a coloured focus ring on inputs. Buttons take `outline: 3px solid var(--z-ink); outline-offset: 3px` on `:focus-visible`. Today `zine.css` declares that ring only for Profile (`.pf-root`, `.pf-page`), so a new module declares the same rule for its own root (`porting.md` #14).
- **Leave room for shadows.** A scrolling container that holds shadowed things needs 4px of padding on the right and bottom, or the shadows clip (see `.sheet .sbody`).

```css
/* Correct: zine.css, an option card */
html[data-theme="zine"] .mk-opt { border: var(--z-out); box-shadow: var(--z-sh); transition: var(--z-press); }
html[data-theme="zine"] .mk-opt:active { transform: translate(4px, 4px); box-shadow: var(--z-sh-0); }

/* Incorrect: a soft shadow and a scale press */
.mk-opt { box-shadow: 0 8px 24px rgba(0,0,0,.2); }
.mk-opt:active { transform: scale(.96); }
```

## Corners

| Radius | For |
| --- | --- |
| 999px | Pills, tags, chips, the tab bar's Make button, the toast |
| `var(--r-card)` 28px | Sheets, the Home stage, the closing card, Inspire me cards |
| `var(--r-panel)` 24px | Panels, game covers |
| `var(--r-group)` 20px | Lists, stat tiles; also the notification (22px) |
| `var(--r-control)` 16px | Buttons (18 for `.btn`, 22 for `.hero`), inputs, sheet rows, speech bubbles (18px) |

Pick from this table. Nested corners: inner radius = outer radius − the gap between them. Never a square corner; never `border-radius: 0` except where two regions share an ink rule.

## Spacing

- Page inset: 16px. Shadows hang outside it, so a shadowed thing's box still sits on the 16px grid.
- Gap between two stacked buttons: 14px (`.ctas`). *Why: 10px lets one button's shadow touch the next.*
- Text that isn't inside a region (a headline, a sentence) is inset 20px.
- Between a region and a button under it: 16px.
- Gap between sheet rows: 10px. Section heading: 26px above, 10px below (`.sec`).
- Between two regions that touch, share one ink rule (`border-top: var(--z-out)`), never two.

## Motion

- **Press:** `var(--z-press)`, 0.09s. Fast, because the thing is hitting the page.
- **Everything else** keeps the base curves from `tokens.css`: `--spring` for small things arriving, `--springSoft` for sheets and cards, `--ease` for fades and progress. Nothing in the UI runs over 0.55s.
- **Sheets** slide up from below the screen and stop 20px above the bottom edge, so they can carry the outline and shadow.
- **The poster unrolls** with a clip-path wipe (`zineUnroll`), not a scale.
- **Reduced motion:** presses still happen instantly (the shadow goes); nothing slides. The block at the end of `zine.css` lists every pressable; add new ones to it.
- Never animate a shadow's colour or blur. It has neither.
