# Honeypot trap test site

A static, one-page test site for checking how well link-visibility methods detect hidden honeypot links. It is a fictional landing page for a wizard-for-hire business, with:

- **20 trap links**, each hidden a different way, with a unique `trap/NN-…` URL so a result can be tied to the method that produced it.
- **5 control links** that a person can see or use, and that a detector must not flag: a skip link, a link in a collapsed mobile menu, a link below the fold, a lazy-loaded link and a low-contrast footer link.

It is plain HTML, CSS and JavaScript with no build step.

## Live site

Once GitHub Pages is enabled for this repo (see below), the site is served at:

`https://tonel.github.io/honeypot-trap-test/`

## Repository layout

| Path | Purpose |
|---|---|
| `index.html` | The landing page with all trap and control links |
| `styles.css` | Layout, and one class per CSS-based trap |
| `script.js` | Mobile menu, trap 16 (hidden 500 ms after load), lazy-loaded control |
| `robots.txt` | Disallows `/trap/` for trap 20 |
| `pyproject.toml` | Python dependencies for the test and benchmark tooling |

## Trap and control reference

Traps (numbers as in the test plan):

| # | Method | href |
|---|---|---|
| 1 | Inline `display:none` | `trap/01-inline-display-none` |
| 2 | `display:none` from a stylesheet | `trap/02-stylesheet-display-none` |
| 3 | `visibility:hidden` | `trap/03-visibility-hidden` |
| 4 | `opacity:0` | `trap/04-opacity-zero` |
| 5 | `opacity:0` on the parent | `trap/05-opacity-parent` |
| 6 | Zero width and height | `trap/06-zero-size` |
| 7 | `font-size:0` | `trap/07-font-size-zero` |
| 8 | `left:-9999px` | `trap/08-left-minus-9999` |
| 9 | `transform: translateX(-200vw)` | `trap/09-translate-200vw` |
| 10 | `clip-path: inset(100%)` | `trap/10-clip-path` |
| 11 | `clip: rect(0 0 0 0)` | `trap/11-clip-rect` |
| 12 | Text the same colour as the background | `trap/12-same-colour` |
| 13 | Covered by an overlay | `trap/13-covered-by-overlay` |
| 14 | `hidden` attribute | `trap/14-hidden-attribute` |
| 15 | `aria-hidden="true"` only | `trap/15-aria-hidden` |
| 16 | Hidden by JavaScript 500 ms after load | `trap/16-hidden-after-500ms` |
| 17 | Inside a `<noscript>` element | `trap/17-noscript` |
| 18 | Inside an HTML comment | `trap/18-html-comment` |
| 19 | `content-visibility:hidden` | `trap/19-content-visibility` |
| 20 | `nofollow` link to a path disallowed in `robots.txt` | `trap/20-robots-disallowed` |

Controls (must not be flagged):

| Control | href |
|---|---|
| Skip to content | `#skip-to-content` |
| Collapsed mobile menu (3 links) | `control/mobile-menu-services`, `control/mobile-menu-pricing`, `control/mobile-menu-contact` |
| Below the fold | `control/below-fold-price-list` |
| Lazy-loaded | `control/lazy-loaded-newsletter` |
| Low-contrast footer | `control/low-contrast-cookie-settings` |

Traps 17 and 18 are not in the DOM when JavaScript runs, so DOM-based checks cannot see them.

## Local preview

Open `index.html` in a browser, or serve the folder:

```bash
python -m http.server 8000
```

then visit `http://localhost:8000/`.

## Publishing on GitHub Pages

1. Push this repo to GitHub.
2. In the repo, go to **Settings → Pages**, choose **Deploy from a branch**, select `main` and the `/ (root)` folder, and save.

`robots.txt` must sit at the root of the domain to be read by crawlers. A project page at `https://<user>.github.io/<repo>/` ignores a `robots.txt` at `/<repo>/`. For crawlers to read it, the repo must be named `<user>.github.io`.

## Python setup

Requires Python 3.11 or newer.

```bash
python -m venv .venv
source .venv/bin/activate      # Windows: .venv\Scripts\activate
pip install -e .
playwright install chromium
```

For the benchmark tooling (BeautifulSoup, Selenium, requests):

```bash
pip install -e ".[benchmark]"
```

Benchmark scripts will be added to this repo later.
