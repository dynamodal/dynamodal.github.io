# Dynamodal website

Quarto website for dynamodal.com, Thomas Wells's data and AI consulting business.
A GitHub Actions workflow renders and publishes it to the `gh-pages` branch on every push to `main`.

Read `DESIGN.md` before changing styles, layout, navigation or copy.

## Conventions

- Prose in `.qmd` files: one sentence per line.
- Never name clients or past employers' clients. Say "a national consulting firm".
- Colors live in `html/styles.scss` (light, `!default`) and `html/dark.scss` (dark). No hard-coded colors in rules.
- Booking links go to `/schedule/` (a redirect page in `schedule/index.html`).
- Work on a branch and open a PR. The **Check site** workflow must pass.

## Commands

- Preview: `quarto preview`
- Build: `quarto render`
- Links and structured data: `python3 .github/scripts/check_site.py _site https://dynamodal.com https://www.dynamodal.com`
- Mobile and large-text layout: `node .claude/skills/check-layout/check_layout.js`
