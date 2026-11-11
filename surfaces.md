# Surfaces: lists, panels, sheets, toasts, inputs, tags

Source: the shared classes in `mvp/tokens.css` and the helpers in `mvp/core.js` (`App.sheet`, `App.toast`, `App.notify`, `App.navbar`), restyled in `mvp/themes/zine.css` lines 139–200.

## Telling the player something

```
What happened?
 ├── They need to choose, or see more, without leaving → App.sheet
 ├── Something they did worked; nothing to do next     → App.toast
 ├── Something finished while they were elsewhere      → App.notify (the banner)
 └── A group of settings or links on a page            → .list with .lrow rows
```

Never a system `alert()` or `confirm()`. A confirm is an `App.sheet` with a `'danger'` button, then a `'dark'` "Cancel".

## A pushed page

`App.push(el)` slides a full screen over the tabs. Build `el` as a column: `App.navbar(title, { onBack: () => App.pop() })`, then one scrolling body on paper. The navbar is the page's title; don't repeat it in the content.

There's no shared class for the page body yet. Copy `.pf-page` and `.pf-pscroll` from `profile.css` under your module's prefix (`porting.md` #14). Pushed pages have no flood.

## Lists and rows

`.list` is a white region with a 2px outline and no shadow. Rows (`.lrow`) are separated by a 2px ink rule.

- A row that goes somewhere is `button.lrow` with a `.chev`. Pressed, it turns lemon and doesn't move.
- A row that only shows something is a `div.lrow`, with no chevron.
- Rows are at least 50px tall, with 12px by 14px padding. The main line is `.t-row`, with `.t-sub` under it.
- One list per group, with a `.t-sec` heading above it (`.sec`). A page with one group has no heading, and an empty state never has one.
- A read-only value with one action, such as a link to copy, is a `div.lrow` with a `.followbtn` at the end (`buttons.md`), never a read-only `.input`.
- A value at the end of a row has no shared class yet: copy `.pf-val` from `profile.css` (15px, secondary colour, tabular digits) under your module's prefix (`porting.md` #14).
- A destructive row is the last group on the page, alone, with red text (`--redText`), no heading and no chevron.

```html
<!-- Correct: profile.js builds rows like this -->
<div class="sec"><span class="t-sec">Notifications</span></div>
<div class="list"><div class="lrow"><span class="tx"><span class="t-row">Your game is ready</span></span>…switch…</div></div>

<!-- Incorrect: a shadowed card per row. Rows live in one outlined list. -->
<div class="panel" style="box-shadow: var(--z-sh)">Your game is ready</div>
```

## Panels and stat tiles

- `.panel`: a white outlined box, radius 24, for mixed content that isn't rows.
- `.stat`: a white outlined tile, one display number (26px) over one word. Three or four in a row, never one alone; one number alone is a row with a `.num` value.
- Never show a stat row of zeros. Show the empty state instead. *Why: "0, 0, 0" is a layout with no content.*

## Sheets

`App.sheet({ title, sub, dark, body })`. In Zine every sheet is the same: a sheet of paper floating 10–20px clear of the screen's edges, outlined, with a 4px shadow, over a 45% black scrim. The `dark` flag changes nothing in Zine. `profile.js` still passes it; new code leaves it out.

- Title: display, 26px. One line. A confirm's title is the question.
- `sub`: one sentence in `--mute`.
- Inside: `.srow` rows (white, outlined, small shadow, 10px apart), or a form, then at most one pink or black button.
- At most 72% of the screen's height. Longer content is a pushed page.
- Never a sheet on a sheet.

## Toast and banner

- `App.toast(text, ms, low)`: a white outlined pill. Under 40 characters, past tense, no button. `low: true` puts it above the tab bar.
- `App.notify({ icon, title, body, onTap })`: the white outlined banner under the island, for "Your game is ready". Its icon sits on a yellow outlined tile. Only for things that finished while the player was elsewhere.

## Inputs

- `.input` (50px) and `.search` (46px): white, outlined, radius 16. On focus they gain the 4px shadow.
- Placeholder is `--mute2`; typed text is black at weight 600.
- A label goes above the field as `.t-sub`, never only as a placeholder.
- The idea box on Make (`.mk-mfield`) is the one large text area: white, outlined, always shadowed, sitting half on the yellow flood.

## Tags and chips

| Class | Looks like | Use |
| --- | --- | --- |
| `.tag` | White, 1.5px outline, 12px text | A neutral label |
| `.tag.new` | Yellow | "New" |
| `.tag.row` | Lemon | A label inside a row |
| `.tag.dim` | Black, white text | A label over game art |
| `.energy` | White pill, small shadow, display number; yellow on Home and Profile | The energy balance |
| `.tapcue` | Black pill, white display text | "Tap to play" over game art |

Tags are sentence case and 1–2 words. At most two tags on one thing.

## Switch

`.pf-switch` (in `profile.js`): an outlined white track with a black knob. On, the track is teal and the knob is white with an outline. Use it in a row's trailing slot, with the row's title as its label. Never a custom switch.

## Avatars

`.avatar` / `App.maker(id)`: a filled circle with a 1.5px ink outline and a display initial. The big profile avatar (88px) also casts a shadow, because you tap it to rename.

## Empty states

`.empty`: one line in `--mute` saying what isn't here and how it gets here, then at most one `'dark'` or `'blue'` compact button. No illustration.
