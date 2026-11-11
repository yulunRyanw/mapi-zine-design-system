# Typography

Source: `mvp/themes/zine.css` lines 1, 27–28 and 103–116. The base sizes are in `mvp/tokens.css`.

## Two faces

| Token | Face | For |
| --- | --- | --- |
| `--display` | Bricolage Grotesque (variable: weight 400–800, width 75–100%) | Titles, names, numbers, buttons, tab labels, section headings |
| `--font` | Figtree (400–800) | Everything you read: rows, body, captions, inputs, toasts |

Nothing else. The status bar keeps iOS's own face; Messages keeps its own (`messages.md`).

## The scale

Use the class. Never set `font-family`, `font-size` or `font-stretch` by hand for one of these roles.

| Role | Class | Face | Size / weight | Width |
| --- | --- | --- | --- | --- |
| A score | `.t-score` | display | 104 / 800, line-height .9 | 75% |
| A big number | `.t-huge` | display | 48 / 800 | 78% |
| Screen title | `.screen-title` | display | 46 / 800 | 78% |
| Poster name | `.mk-pname` | display | 40 / 800 | 78% |
| A headline inside a page | `.t-gtitle` | display | 32 / 800 | 82% |
| Sheet title | `.sheet .stitle` | display | 26 / 800 | 85% |
| Hero line, game name on a card | `.t-hero` | display | 20 / 700 | 100% |
| Section heading | `.t-sec` | display | 19 / 700, sentence case | 90% |
| Button | `.cta` | display | 17 / 700 (16 compact, 18 btn, 21 hero) | 100% |
| Nav bar title | `.navbar` | display | 17 / 700 | 100% |
| Row title | `.t-row` | text | 16 / 600 | |
| Body | `.t-body` | text | 15 / 500, line-height 21.75px | |
| Secondary line | `.t-sub` | text | 13 / 500, `--mute` | |
| Tag | `.t-tag`, `.tag` | text | 12 / 700, sentence case | |
| Tab label | `.tabbar .tab` | display | 12 / 700 | |

```
Which style?
 ├── Is it a number the player earned or owns?  → .t-score (a result), .t-huge (a balance), or add .num
 ├── Is it the one title of the screen?         → .screen-title on a tab root. On a pushed page, App.navbar draws the title.
 ├── Is it a headline inside a page's content?  → .t-gtitle, at most one per page
 ├── Is it a heading over a group?              → .t-sec
 ├── Is it on a button?                         → let .cta set it
 ├── Is it a name (a game, a player)?           → .t-hero
 └── Otherwise                                  → .t-row for the main line, .t-sub under it, .t-body for sentences
```

## Rules

- **The bigger the type, the narrower and heavier it is.** Display type at 26px or more is weight 800 at 75–85% width. From 17 to 20px it is weight 700 at full width. *Why: condensed heavy type is how a short word fills a header without a picture.*
- **Text weights are 500 for reading, 600 for a row's main line, 700 for tags.** Never 400, never 800 in Figtree. *Why: 400 goes thin next to 2px outlines, and 800 belongs to the display face.*
- **No uppercase, no letter-spacing above 0.** Section headings and tags are sentence case. Large display type uses −0.01 to −0.03em.
- **Numbers that matter use the display face:** add `.num` (energy, counts, times). Numbers inside a sentence stay in Figtree.
- **Floor: 12px.** Nothing smaller, including tags and tab labels.
- **A screen has one size above 40px.** If the title is 46, the next largest thing is 32 or less. *Why: two huge things cancel each other out.*
- **Line length:** body text wraps inside the 16–20px page inset. Poster and card text is 15px.

```css
/* Correct: zine.css, a section heading */
html[data-theme="zine"] .t-sec { font-family: var(--display); font-weight: 700; font-size: 19px; font-stretch: 90%; letter-spacing: 0; text-transform: none; color: var(--text); }

/* Incorrect: the old tracked label */
.t-sec { font-size: 13px; letter-spacing: 1.3px; text-transform: uppercase; color: var(--grey8e); }
```

## Loading

The `@import` on line 1 of `zine.css` loads both faces from Google Fonts, with Bricolage's `opsz`, `wdth` and `wght` axes. `font-stretch` only works because the `wdth` axis is requested; if you change the import, keep `wdth,wght@…,75..100,400..800`.
