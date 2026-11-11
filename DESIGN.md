# Zine: Mapi's design system

Zine is paper and ink. The ground is warm paper, every line is black, and anything you can press is outlined and casts a hard shadow that it sinks into when pressed. Colour arrives in flat floods and each colour has one job. If a screen feels dull, add a flood or enlarge the type; never add a gradient, a glow or a soft shadow.

**Where it lives:** `vibe-playground/mvp/themes/zine.css`, on top of `mvp/tokens.css` and the module stylesheets. Every rule is scoped to `html[data-theme="zine"]`. Turn it on with `?theme=zine`, or "Design direction" in the demo's panel. It isn't the default yet and it isn't in the Swift app; `porting.md` lists what's left.

## Rules for every screen

1. **Black ink on every bright fill.** Never white text on pink, yellow, teal, periwinkle or lemon. White text goes on black and on red only. *Why: white on these fills is under 3:1, and black is 6.5:1 or better on all of them.*
2. **One depth system: a 2px ink outline, plus a hard offset shadow on things you can press or pick up.** Use `var(--z-out)` and `var(--z-sh)`. Never a blurred shadow, a glow, frosted glass or a gradient. *Why: the hard edge is the system's signature. One soft shadow next to it reads as a mistake.*
3. **Pressing moves a thing into its shadow.** Translate by the shadow's size and set the shadow to `var(--z-sh-0)`. Never scale on press. *Why: the shadow says "this lifts off the page", and pressing puts it back down.*
4. **Regions that only hold things get the outline and no shadow:** lists, panels, the tab bar, headers. *Why: a shadow promises a press. A list that casts one looks tappable as a whole.*
5. **Each colour has one job** (`colors.md`): pink acts, black plays, white is quiet, red is danger, yellow is energy, teal is right or done, lemon is the highlighter. Never pick a colour because a screen "needs some colour"; use a flood.
6. **One flood per screen, at most.** A flood is a full-width block of one colour behind a header: Make is yellow, Profile is pink, All games is periwinkle. *Why: two floods on one screen compete, and the outline stops reading.*
7. **Display type is Bricolage Grotesque, text is Figtree.** Titles and numbers are condensed and heavy. No uppercase with letter-spacing, anywhere. *Why: tracked capitals over every group was one of the generic tells this system replaced.*
8. **Corners stay round:** 16, 20, 24 and 28px, or a full pill. *Why: the round corners are what keep hard outlines friendly, not brutalist.*
9. **Messages is Apple's.** Inside `#app-messages`, only Mapi's own parts take Zine (`messages.md`).
10. **No sparkle icons, no emoji as interface icons.** Game art is the one place emoji appear today, and that's logged in `porting.md`.

## Layers

```
Module CSS and JS (discover, make, share, profile)  →  shared classes (tokens.css)  →  tokens
                                    themes/zine.css restyles all three, scoped to html[data-theme="zine"]
```

- **Building a screen:** use the shared classes and `App` helpers (`App.cta`, `App.sheet`, `.list`, `.lrow`, `.tag`…). They already look right in Zine.
- **Module CSS uses the role tokens** (`--bg`, `--text`, `--card`, `--blue`, `--green`…), never a hex. Zine remaps them, so `--blue` is pink here. The map is in `colors.md`.
- **A rule that only makes sense in Zine** (an outline, a hard shadow, a flood) goes in `themes/zine.css`, scoped to `html[data-theme="zine"]`, and uses the Zine names: `--pink`, `--sun`, `--z-out`, `--z-sh`.
- **The helpers' `kind` names are legacy.** `kind: 'blue'` draws pink, `'white'` draws black, `'dark'` draws white. `buttons.md` has the table.

## Before you build

```
What are you adding?
 ├── Something to press with a word on it      → buttons.md
 ├── A list, panel, sheet, toast, input or tag → surfaces.md
 ├── Any colour, or a header flood             → colors.md
 ├── Any text                                  → typography.md
 ├── An outline, shadow, radius or press       → depth.md
 ├── The mascot, an icon, a poster or game art → character.md
 └── Anything inside Messages                  → messages.md
```

## Index

| File | Read it when you're… |
| --- | --- |
| `colors.md` | choosing a fill, a text colour or a flood |
| `typography.md` | setting any text |
| `depth.md` | adding an outline, a shadow, a radius, a press state or motion |
| `buttons.md` | adding anything tappable |
| `surfaces.md` | building lists, panels, sheets, toasts, inputs, tags or switches |
| `character.md` | touching the mascot, icons, the poster, stamps or game art |
| `messages.md` | working inside the Messages app |
| `porting.md` | wondering what isn't done: the default, renames, Swift, known gaps |

**Unchanged by Zine,** and still the rule: sound and haptics (`../design-system-docs/feedback.md`: which `App.fx` plays for which moment) and the words on screen (`../design-system-docs/voice.md`).
