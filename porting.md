# Not done yet

**Nothing in this file exists in code.** Don't use a name from it. Each item says what's true today, the change, and what it costs. When one lands, move its rule into the topic file and delete it here.

## Make Zine the system, not a theme

### 1. Zine isn't the default
**Today:** the demo opens in the old look ("Now"). Zine needs `?theme=zine` or the panel's switch.
**Change:** set the default in `core.js` (`let theme = 'zine'`) and keep "Now" as an option until the team has signed off. **Cost:** one line.

### 2. Zine is an override layer
**Today:** `zine.css` is 599 lines that restyle `tokens.css` and four module stylesheets from the outside, with selectors like `html[data-theme="zine"] .cta`. Some rules fight the base (resetting `backdrop-filter`, `transform: none` on presses).
**Change:** fold Zine into the base. Move its tokens into `tokens.css`, rewrite each shared class once, and move module rules into their own stylesheets. Then delete `zine.css` and the other three themes. **Cost:** about a day; no visual change if done carefully.

### 3. Token and kind names lie
**Today:** `--blue` is pink, `--green` is teal, `--sheetInk` is white, and `App.cta` kinds are `'blue'` (pink), `'white'` (black) and `'dark'` (white).
**Change:** rename to roles: `--act`, `--done`, `--energy`, `--danger`, `--surface`, `--ground`, `--ink`, `--mute`; kinds `'act'`, `'play'`, `'quiet'`, `'danger'`. Do it together with item 2. **Cost:** a find-and-replace across five stylesheets and four modules (57 `App.cta` calls).

### 4. Rules that match icons by their path data
**Today:** Zine finds the bolt and the sparkle with `svg:has(> path[d^="M13 2 4 14"])`. Changing an icon's path in `core.js` silently breaks the styling.
**Change:** have `App.icon` add `data-icon="bolt"` to the SVG, and select on that. Remove `sparkle` from the icon set and from `make.js` (Inspire me, the Vibe role). **Cost:** five lines in `core.js`, plus the selectors.

### 5. CSS that needs a new browser
**Today:** the Vibe card's face uses the CSS `d` property, which Safari doesn't support, so Safari shows an outline sparkle. Many rules use `:has()`.
**Change:** item 4 removes the `d` trick. `:has()` is fine on iOS 15.4 and later; state the minimum. **Cost:** none after item 4.

### 14. Shared pieces every new page copies
**Today:** three things exist only inside Profile's stylesheet, so each new module copies them: the pushed-page body (`.pf-page`, `.pf-pscroll`), a row's trailing value (`.pf-val`), and the button focus ring (scoped to `.pf-root` and `.pf-page`).
**Change:** add `.page` and `.page-scroll`, `.lrow .val`, and a global `:focus-visible` ring to the base, and delete the copies. **Cost:** about 12 lines.

## Things CSS can't reach

### 6. Text drawn on canvases
**Today:** the Messages bubble's "HOP" and "YOU GOT" labels, its score, and the in-game HUD are drawn in `share.js` and `games.js` with Nunito, tracked capitals and a soft shadow.
**Change:** draw them with Bricolage Grotesque, sentence case, on a solid plate. **Cost:** about 30 lines across two files.

### 7. Colours set in JavaScript
**Today:** maker avatar colours (`Games.MAKERS`) and confetti colours come from JS, in the old palette.
**Change:** map both to the Zine palette. **Cost:** two small tables.

### 8. Game art is emoji
**Today:** every game draws emoji on a flat colour. That's the games' own art, so no theme changes it, but it's the largest thing on Home.
**Change:** a house style for built games, decided separately. Duolingo's rule is a good start: build characters from a rounded rectangle, a circle and a rounded triangle, with a black outline to match Zine. **Cost:** a design decision, then work in the game builder.

### 9. The notification's icon is an emoji
**Today:** `App.notify({ icon: '🎮' })`.
**Change:** use Mapi's app icon (the yellow outlined "M" tile). **Cost:** a few lines in `core.js`.

## Weak spots in the design itself

### 10. The empty Make screen
**Today:** below the idea box and "Inspire me", the lower half of the screen is bare paper.
**Change:** fill it with something real, not decoration: the player's last idea, or two example ideas as tappable cards. **Cost:** a product decision and about 40 lines in `make.js`.

### 11. The game plan is a long white form
**Today:** the editable plan from Inspire me has little colour and no hierarchy below its name.
**Change:** give each line its card's role colour as a small tag, the way the cards had it. **Cost:** about 15 lines.

### 12. Profile shows a stats row of zeros
**Today:** a new player sees "0 games made, 0 plays".
**Change:** hide the row until one number is above zero. **Cost:** three lines in `profile.js`.

## The Swift app

### 13. Nothing is ported
**Today:** `apps/ios/MapierGames/DesignSystem` is the old system, and its docs (`design-system-docs/`, pull request 69) describe that.
**Change:** port in this order:
1. `DSColor`: the Zine palette and text shades from `colors.md`.
2. `DSText` and `DSFonts`: Bricolage Grotesque (variable, with the width axis) and Figtree, with the scale in `typography.md`.
3. `DSPress`: one press that offsets by the shadow's size, replacing the scale presses.
4. A `dsOutlined(shadow:)` modifier for the outline and hard shadow, then each component.
5. Rewrite `design-system-docs/` from these files.

**Cost:** several days. Do it after items 2 and 3, so the names match.
