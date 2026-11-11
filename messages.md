# Inside Messages

Source: `mvp/themes/zine.css` lines 37–47 (Messages' tokens put back), 484–592 (Mapi's parts), and `mvp/share.js`.

Messages is Apple's app. Zine never restyles Apple's chrome; it styles only what Mapi draws inside it. *Why: a themed chat screen stops looking like the player's real Messages, and the demo stops being believable.*

## Whose is it?

```
Is the thing drawn by Messages or iOS?
 ├── The chat background, text bubbles, header, input bar, "+" drawer,
 │   the conversation list, the App Clip card, the App Store sheet
 │        → Apple's. Dark, Nunito, the base tokens. Don't touch.
 └── The game bubble, the score plate, the staged bubble, taunt chips,
     the Mapi panel and lobby, the result card, the App Clip's inside, Mapi's app icon
          → Mapi's. Zine applies.
```

`zine.css` resets the base tokens on `#app-messages`, then turns Zine back on for `.sh-mb`, `.sh-panel`, `.sh-clip` and `.sh-taunts`. A new Mapi part inside Messages must be added to that selector list (lines 18–22), or it renders with Apple's dark tokens.

## Mapi's parts

| Part | Class | In Zine |
| --- | --- | --- |
| A friend's game bubble | `.sh-mb` | A white paper card with a 2px outline. No shadow: Messages bubbles don't cast one. |
| Your game bubble | `.sh-mb.out` | The same, in pink. All its text is black. |
| The art in a bubble | `.sh-mb-img` | Closed by an ink rule underneath |
| The score on the art | `.sh-mb-score` | A yellow outlined plate, radius 14, bottom right. Never white type with a soft shadow. |
| The caption | `.sh-mb-c1`, `.sh-mb-c2` | Display 16px / 700, then Figtree 500 in `--mute` |
| Taunt chips | `.sh-taunts` | White outlined pills with a small shadow; the chosen one is lemon and pressed in |
| The Mapi panel and lobby | `.sh-panel` | Paper, with Zine buttons and tiles |
| The App Clip's inside | `.sh-clip` | Paper, Zine type; its tag is lemon |
| Mapi's app icon | `.sh-appicon` | A yellow tile with a 2px outline and a display "M" |

- **Your bubble is pink, your friend's is white.** *Why: Messages colours outgoing bubbles; pink is Mapi's "you sent this".*
- **Buttons in the result follow `buttons.md`:** "Send back" is `'blue'` (pink), "Play again" is `'white'` (black). That's the one pink-and-black pair.
- **No send sound and no confetti in the chat.** Messages plays its own send sound.
- **The App Clip card and the App Store sheet stay iOS's,** with the system sheet and its blue and white buttons, even though they're built with `App.sheet`. `zine.css` puts them back with `:has(.sh-cliparts)` and `:has(.sh-store)`; never restyle those two.

```css
/* Correct: zine.css. The score is a plate, legible on any art. */
html[data-theme="zine"] .sh-mb-img .sh-mb-score { background: var(--sun); border: var(--z-out); color: var(--z-ink); text-shadow: none; }

/* Incorrect: white type with a soft shadow straight on the art */
.sh-mb-score { color: #fff; text-shadow: 0 2px 8px rgba(0,0,0,.5); }
```

## What a theme can't reach

The bubble's art is drawn on a canvas in `share.js`, including the small "HOP" and "YOU GOT" labels (uppercase, tracked, Nunito). CSS can't restyle them. Until `share.js` draws them in Zine's type, they're the one place the old look survives (`porting.md`).
