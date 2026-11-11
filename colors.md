# Colors

Source: the token block at the top of `mvp/themes/zine.css` (lines 18–36). The palette comes from Gumroad's own CSS.

## The palette

| Zine token | Hex | Job | Black text on it |
| --- | --- | --- | --- |
| `--paper` | `#f4f4f0` | The ground | 19.1:1 |
| `#ffffff` | | Cards, lists, sheets' rows, quiet buttons | 21:1 |
| `--paper2` | `#e4e4dc` | A sunk or skeleton area | |
| `--z-ink` | `#000000` | Every outline, shadow, and most text | |
| `--pink` | `#ff90e8` | **Acts:** make, send, spend | 10.4:1 |
| `--sun` | `#ffc900` | **Energy**, and the Make flood | 13.6:1 |
| `--teal` | `#23a094` | **Right, done, ready, on** | 6.5:1 |
| `--lemon` | `#f1f333` | **The highlighter:** selected, editing, a pressed row | 17.6:1 |
| `--peri` | `#90a8ed` | The All games flood; a card role | 9.0:1 |
| `--coral` | `#ff7051` | A card role only | |
| `--red` | `#dc341e` | **Danger** fill, with white text (4.6:1) | |

Nothing else exists. Don't add a colour; if a new meaning needs one, add it to `zine.css` and to this table in the same change.

## Text

```
What is the text sitting on?
 ├── Paper or white
 │    ├── Primary             → var(--z-ink)
 │    ├── Secondary           → var(--mute)   #55534c  (7.0:1 on paper, 7.7:1 on white)
 │    ├── Placeholder, hint   → var(--mute2)  #66655e  (5.3:1 on paper)
 │    ├── "Done" or "ready"   → var(--tealText) #0e6b62 (5.8:1)
 │    └── Danger              → var(--redText)  #b3260f (6.0:1)
 ├── Yellow or lemon          → var(--z-ink); secondary may use var(--mute) (5.0:1 on yellow)
 ├── Pink, periwinkle, teal   → var(--z-ink) only. --mute fails here.
 ├── Black                    → #fff
 ├── Red fill                 → #fff
 └── Game art                 → #fff on a solid black plate (.tapcue), never on the bare art
```

- Never use `--teal` or `--red` as a text colour on paper: use `--tealText` and `--redText`. *Why: as text on paper the fills are 2.9:1 and 4.2:1, both under AA. The text shades are made for it.*
- Links are black with a 2px underline, offset 3px (`.textlinks button`, `.sec .trail`). Never coloured. *Why: pink is for buttons that act, and a pink link would dilute it.*

```css
/* Correct: zine.css, the result card */
html[data-theme="zine"] .dc-result { background: var(--sun); color: var(--z-ink); }

/* Incorrect: white on yellow is about 1.5:1 */
html[data-theme="zine"] .dc-result { background: var(--sun); color: #fff; }
```

## Floods

A flood is one flat colour filling a screen's header, edge to edge and under the status bar, closed by a 2px ink rule where the paper starts.

| Screen | Flood | Rule in `zine.css` |
| --- | --- | --- |
| Make (write, questions, building, ready) | `--sun` | `.mk-root:has(> .mk-write)` |
| Profile | `--pink` | `.pf-root`, `.pf-head` |
| All games | `--peri` | `.screen:has(> .dc-list)` |
| First open | `--sun`, whole screen | `.screen.dc-onbscr` |
| A result over a game | `--sun`, the whole card | `.dc-result` |
| Home, Settings, sheets | none: paper | |

- One flood per screen. A new tab root picks a palette colour no other tab uses.
- Pushed pages such as Settings stay on paper. *Why: floods mark places; detail pages are for reading.*
- Yellow as a small element is fine on any page: the energy chip, a tag, an icon tile. A yellow *panel* or header on a pushed page is a flood, so it isn't allowed there.
- Never put a pink button on the pink flood or a yellow chip on the yellow flood. On a flood, chips and buttons are white, or black for Play.

## The role tokens (what module CSS uses)

Module stylesheets were written against role tokens. Zine remaps them, so their names no longer describe their colour:

| Role token | Means | Zine value |
| --- | --- | --- |
| `--bg` | ground | `--paper` |
| `--sheetInk`, `--card` | raised surface | `#ffffff` |
| `--card2` | sunk surface | `--paper2` |
| `--text`, `--ink`, `--ink2` | text | black |
| `--grey8e`, `--greyC7` | secondary text | `--mute` |
| `--muted2` | hint | `--mute2` |
| `--line` | hairline | black |
| `--blue` | **act** | `--pink` |
| `--yellow` | energy | `--sun` |
| `--green` | right, done | `--teal` |
| `--red` | danger | `#dc341e` |
| `--toy-blue / -sun / -gum / -leaf / -tang / -grape` | card and poster colours | `--peri / --lemon / --pink / --teal / --sun / --coral` |

- In module CSS (`discover.css`, `make.css`, `share.css`, `profile.css`), use the role token. Never a hex, never a Zine name.
- In `zine.css`, use the Zine name (`--pink`), never the role name (`--blue`). *Why: `var(--blue)` in a Zine rule reads as a bug to the next person.*
- Renaming the role tokens is logged in `porting.md`.

## Card and poster colours

Inspire me cards take their role's colour through `--c`, and the poster takes one through `--poster`; both come from the `--toy-*` tokens. A card under the top one is its colour mixed 45% into white. Never invent a card colour: the eight roles are fixed in `make.js` (`ROLES`).
