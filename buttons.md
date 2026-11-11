# Buttons

Source: `App.cta` in `mvp/core.js`; `.cta`, `.pill`, `.closebtn`, `.circ`, `.followbtn`, `.textlinks` in `mvp/tokens.css`, restyled in `mvp/themes/zine.css` lines 118–137 and 165–167.

```
What are you adding?
 ├── An action with a word on it                       → App.cta(label, { kind, size, icon, onClick })
 ├── A filter or a choice among a few                  → .pill (.on when chosen)
 ├── Close or back, in a bar                           → App.navbar(title, { onClose | onBack })
 ├── A round icon button over game art                 → .circ
 ├── A small action at the end of a row (Unblock)      → .followbtn
 ├── A low-stakes action in running text (Skip, Undo)  → a button inside .textlinks
 └── A row that goes somewhere                         → button.lrow (surfaces.md)
```

## App.cta

Use it for any action with a word on it. A row that goes somewhere is a `.lrow`. A choice among options is a `.pill`.

Kinds: `'blue'`, `'white'`, `'dark'`, `'danger'`. Sizes: default, `'btn'`, `'compact'`, `'hero'`. Nothing else exists. **The kind names are legacy and don't describe the colour:**

| `kind` | Looks like in Zine | Means | Real uses |
| --- | --- | --- | --- |
| `'blue'` | **Pink**, black text | **Acts:** makes, sends or spends | "Make it", "Get my invite link", "Add to message", "Send to a friend" |
| `'white'` | **Black**, white text | **Plays, or the free next step** | "Play again", "Next", "Done", "Continue with Apple", "Inspire me" |
| `'dark'` | **White**, black text | Everything else. **Choose this when unsure.** | "Change something", "Cancel", "Not now", "All games", "Make a game" |
| `'danger'` | Red, white text | The player loses something | "Delete account" |

- **At most one pink and one black button per view.** They appear together only when the black one is Play. Everything else is white. *Why: pink and black are the two loudest things in the system. A third loud button leaves no answer to "what do I do here?"*
- **Always pass `kind`.** Left out, `App.cta` draws `'blue'` (pink), which is the wrong default for most buttons.
- **Never pink for a free step** such as Next or Done. *Why: pink means "this makes something happen or costs something".*
- Of the 57 buttons in the demo today, 25 are `'white'`, 16 `'blue'`, 15 `'dark'` and 1 `'danger'`.

```
Which kind?
 ├── Does the player lose something (an account, a game, a setting)?  → 'danger'
 ├── Does it make, send or spend?                                     → 'blue'  (pink)
 ├── Does it play a game, or is it the free next step?                → 'white' (black)
 └── Default                                                          → 'dark'  (white)
```

| `size` | Height, radius, type | Use |
| --- | --- | --- |
| default | 54px, 16px, 17px | Every full-width button |
| `'btn'` | 58px, 18px, 18px | The one main button of a full-screen step (first open, sign-in) |
| `'compact'` | 48px, 16px, 16px | Two side by side, or a button inside a card or an empty state |
| `'hero'` | 66px, 22px, 21px | "Play again" on a result. One per screen. |

```js
// Correct: make.js, the building screen. Free step, so it's 'white' (black).
App.cta('Play while it’s made', { kind: 'white', icon: 'play', onClick: () => awayToHome() })

// Incorrect: 'primary' doesn't exist, and a free step is never pink
App.cta('Play while it’s made', { kind: 'primary' })
```

**Stacking:** put buttons in `.ctas` (14px apart), or `.ctas.row` for two side by side. Never two shadowed buttons closer than 14px.

**Icons:** an optional `icon` from `App.icon`'s set. Use one only when it's the action's own symbol: `play`, `bolt` for a cost, `chat` for sending, `plus`, `grid`, `apple`. Never `sparkle`: Zine hides it on buttons.

**Disabled:** there's no disabled look. Hide the button until it can act, or keep it enabled and answer the tap with a toast that says what's missing. *Why: a grey button can't say why it's grey.*

## Pills

`.pill` is a white pill with a small shadow. `.pill.on` is lemon and pressed in. Use pills for one choice among 2–6, in a `.pills` row. More than 6 options is a list with rows.

## Round buttons

- `.closebtn` (32px): white, outlined, small shadow. `App.navbar` draws it; never build your own.
- `.followbtn` (34px tall): a white outlined pill with a small shadow, for one small action at the end of a row: "Copy", "Unblock". `.followbtn.on` is lemon, for a state that's now on ("Copied", "Following"). The row itself then has no action.
- `.circ` (38px): the same, for an icon over game art. It's a solid white plate in Zine, never frosted.
- The Inspire me deck's Skip and Keep (`.mk-round`): white, and teal for Keep. These belong to that screen.

Every icon-only button needs an `aria-label` with a verb.

## Text links

A button inside `.textlinks` is black with a 2px underline. Use it for actions that are safe to miss: "Skip", "Undo", "Deal new cards", "Show a failed make". At most two side by side. Never a pink or coloured link.

## The Make tab's button

The pink pill in the tab bar is the one pink thing that's always on screen. While a game builds, its outline becomes the progress ring and its label counts down; when ready it turns teal. Drive it with `App.makeBadge({ p, state, label, name })` and never restyle it per screen.
