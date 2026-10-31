# Dynamodal site design guide

Design rules for dynamodal.com, and the lessons behind them.
Fruita Tech Wizard (dynamodal/fruitatechwizard) follows the same technical rules with its own look and voice.

## Voice

- Professional and polished, first person ("I"), plain English.
- Most work is subcontracting for consulting firms, but direct clients are welcome; copy should never read as sub-only.
- Focus areas: analytics and modeling (especially energy efficiency and electrification), AI adoption for teams, data strategy and platforms, team efficiency.
- Never name past or current clients or firms. Say "a national consulting firm".
- No keyword stuffing. Write for people; search engines and AI assistants read the same pages.

## Look

| Token | Light | Dark | Use |
|---|---|---|---|
| `$ink` | `#12233a` | `#eef3f8` | headings |
| `$ink-soft` | `#3b4a5e` | `#b8c4d2` | body text |
| `$accent` | `#0e7c66` | `#3cc4a4` | links, buttons, icons |
| `$page` | `#ffffff` | `#0d1824` | page background |
| `$surface` | `#ffffff` | `#142131` | cards |
| `$mist` | `#f3f6f8` | `#142131` | bands, chips, footer |
| `$line` | `#dfe5ea` | `#26364a` | borders |

- Font: Inter. Headings 700.
- Icons: Bootstrap Icons, `<i class="bi bi-..." aria-hidden="true"></i>`.
- Building blocks in `html/styles.scss`: `dm-hero`, `dm-section`, `dm-band`, `dm-cta`, `dm-wrap`, `dm-grid` (`dm-grid-2`), `dm-card`, `dm-actions`, `dm-points`, `dm-chips`, `dm-eyebrow`, `dm-lead`.

## How the styles are built

- `html/styles.scss` holds every color as a `!default` variable plus all rules.
- `html/dark.scss` only overrides those variables (and a few dark-only rules). The dark theme loads it after `styles.scss`, so its values win.
- New colors go in both files as variables. Never hard-code a color in a rule, or dark mode breaks.
- `$font-size-root` is a percentage (`106.25%` = 17px at the browser default), not pixels, so the reader's own text-size setting still works.

## Mobile and large text

Most visitors are on phones, and some use very large text.
Android and iOS large text zooms the page, so a 390px phone acts like a 240px or even 200px wide screen.

- Every page must fit at 200px wide with no sideways scrolling. Check with the `check-layout` skill (`.claude/skills/check-layout/`).
- Grids use `minmax(min(240px, 100%), 1fr)`, never a bare `minmax(240px, 1fr)`, which overflows narrow screens.
- On phones, don't stack padding. Quarto's page grid already adds a 1.5em gutter, so `dm-wrap` adds none below 576px, and cards use about 1rem of padding.
- Shaded bands (`dm-hero`, `dm-band`, `dm-cta`) run edge to edge on phones by cancelling the gutter with `margin: 0 -1.5em`.
- Long words and links wrap (`overflow-wrap: anywhere` on links, cards and the footer). Headings hyphenate only below 340px.
- Below 340px the navbar hides the logo mark and lets the site name wrap instead of cutting it off.

## Navbar and menu

- The navbar starts with a "Home" link. Not everyone knows the logo goes home.
- Below 992px the navbar collapses into a menu. Search and the light/dark switch move into that menu (`html/mobile-menu.html`) so the top bar holds only the menu button and the centered site name.
- The open menu has its own background, border and shadow so it doesn't blend into the page, and the menu button becomes an X.
- Hide individual tools in the top bar, not the whole `.quarto-navbar-tools` box. That empty box keeps the site name centered.

## Dark mode

- Quarto builds a light and a dark stylesheet. `respect-user-color-scheme: true` follows the visitor's system setting; the toggle (desktop) or menu item (phones) overrides it.
- The navbar logo swaps to `files/logo-mark-white.svg` in dark mode (`.quarto-dark .navbar-logo`).
- Check every change in both modes.

## Search

- Quarto's built-in overlay search. It costs about 37 KB of compressed JavaScript per page (cached after the first visit); the index downloads only when someone searches.

## Search engines and AI assistants

- `site-url` must be the canonical domain, `https://dynamodal.com` (www redirects there). The sitemap, robots.txt and social cards are built from it.
- `html/structured-data.html` describes Dynamodal, LLC and Thomas Wells as JSON-LD on every page. Keep it in sync with the About and Services pages.
- Every page sets `description` in its front matter. It is for search results and link previews only (hidden on the page).

## Checks before a PR

1. `quarto render`
2. `python3 .github/scripts/check_site.py _site https://dynamodal.com https://www.dynamodal.com` (also runs in CI on every PR)
3. The `check-layout` skill: no sideways scrolling at 390, 300, 240 and 200px, light and dark.

## Lessons learned

- Pandoc wraps a row of button links in a `<p>`, so `dm-actions` makes both itself and its `> p` the flex container.
- The jolla About template centers text; `.quarto-about-jolla main p { text-align: left }` overrides it.
- An empty `.nav-footer-center` makes the footer columns stack; it is hidden.
- `url()` paths in SCSS resolve from the project root (`url("files/x.svg")`), not from the `html/` folder.
- Quarto's dark search highlight is too harsh; `dark.scss` softens it.
