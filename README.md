# Wallet2QR v2 — Concept C style (static prototype)

Version 2 of the [w2qr.com](https://www.w2qr.com/) redesign in the "Concept C" visual style: a light stone page, near-black rounded stages, a lime accent, Manrope ExtraBold headlines, Geist text and Geist Mono labels.

The structure, pages, copy and behaviour are the same as version 1 (the Dusk version): single-screen home with the animated hero, the same navigation, tools, settings, help and docs pages. Only the visual style changes.

- **Text:** all visible copy follows the live site word for word (checked November 2026).
- **Structure:** the app follows the live site: a single-screen home page plus the four navigation sections.

This is a **design prototype** in plain HTML, CSS and JS. There is no build step and no dependencies. Nothing is encrypted, uploaded or sent anywhere.

## Run / publish

- **Locally:** open `index.html` in a browser, or run `npx serve .`
- **GitHub Pages:** push this folder to a repo, then go to Settings → Pages → *Deploy from branch* → `/ (root)`. The `.nojekyll` file is already included.

## Pages

| Nav | File | Live original |
|---|---|---|
| Home (one screen, animated hero) | `index.html` | `/` |
| 📁 Open Wallet | `restore.html` | `/restore` |
| ↳ Scan with Camera | `restore-camera.html` | camera view |
| ↳ Scan with Mobile | `restore-bridge.html` | `/restore/bridge` |
| ↳ Upload File | `restore-upload.html` | `/restore/upload` |
| ↳ Restore from Google Drive | `restore-drive.html` | `/restore/drive` |
| ↳ Create New | `create.html` | `/create` (3 steps) |
| ↳ Import Seed Phrase | `restore-import.html` | inline panel on `/restore` |
| 🔧 Tools | `tools.html` | `/tools` |
| ↳ QR Rotation | `rotation.html` | `/wallet/rotation` |
| ↳ Plastic Card | `card.html` | `/card` |
| ↳ Strong Password | `strong-password.html` | `/wallet/strong-password` |
| ↳ Check Password | `check-password.html` | `/check-password` |
| ↳ Cloud Backup | `cloud-backup.html` | `/cloud-backup` |
| ↳ BIP39 Simulator | `accounts.html` | `/wallet/accounts` |
| ↳ Find my Assets | `scanner.html` | `/wallet/scanner` |
| ↳ Restore my Mnemonic | `recover-mnemonic.html` | `/wallet/recover-mnemonic` |
| ⚙️ Settings (General / Networks & Assets) | `settings.html` | `/settings` |
| ❔ Help (also "About" in the footer, as on the live site) | `help.html` | `/help` |
| ↳ White Paper | `whitepaper.html` | `/whitepaper` |
| ↳ How W2QR Works | `how-it-works.html` | `/how-it-works` |
| ↳ Security & Recovery | `basic-vs-advanced.html` | `/basic-vs-advanced` |
| ↳ FAQ (accordion) | `faq.html` | `/faq` |
| ↳ Plastic Card | `card-about.html` | `/card/about` |
| ↳ Documentation | `docs.html` + 9 `docs-*.html` | `/docs`, `/docs/*` |
| ↳ Honey Encryption (visual explainer) | `honey-encryption.html` | `/honey-encryption` |
| ↳ Security Model | `security.html` | `/security` |
| Activate Card | `activate.html` | `/activate` |
| Terms of Use | `terms.html` | `/terms` |
| Privacy Policy | `privacy.html` | `/privacy` |
| Contact Us | `contact.html` | footer link (`mailto:` on the live site) |
| Not found | `404.html` | GitHub Pages serves it for unknown URLs |

On every page there is a **Master Key** button in the bottom-right corner. It also opens with `Ctrl+0`.

## Content pages (generated)

Terms, Privacy, FAQ, Docs, White Paper and the other text pages are generated from `content/*.html` by `build.py`. Their copy was taken word for word from the live site in November 2026.

- To change a text page, edit its file in `content/`, then run `python3 build.py`.
- `build.py` rewrites live-site links (`/terms`, `/docs/recovery` …) to the local files.
- Styles for these pages live in `assets/css/docs.css`.
- The live site has no separate Contact page; `contact.html` uses the two e-mail addresses that appear on the site, and its subtitle was written for the prototype.

## Home hero animation

`assets/js/hero.js` contains the four flows from the live site's tabs. The text comes from the site's animation files `w2qr-anim-1…4`.

| Tab | Flow | CTA |
|---|---|---|
| QR + password | Open wallet | Open Wallet |
| Two QRs | Open wallet | Open Wallet |
| Seed QR | Create | Create Encrypted Wallet |
| Strong password | Create | Strong Password Generator |

How the animation behaves:

- **Playback:** each flow has 4 steps. A step lasts 2.8 s, and the hero loops on its own.
- **Phone:** the screen changes with every step.
- **Centre visual:** each flow has its own: the encrypted QR, the two QRs, seed → QR, or phrase → password → QR.
- **Controls:**
  - Clicking a step jumps to it.
  - Hovering over the stage pauses the animation.
  - Arrow keys switch tabs.
  - `?how=open-password | open-two-qrs | create-seed | create-password` opens a specific flow.
- **Reduced motion:** with `prefers-reduced-motion`, the hero stops on the last step and does not auto-play.
- **Fit:** the layout scales to the viewport height, so on desktop the home page fits one screen without scrolling. On narrow and short screens it falls back to normal scrolling.

## Theme

**Settings → Appearance → Dark / Light** switches the palette. Light (stone page with black stages) is the default; Dark makes the whole page near-black. The choice is stored in `localStorage` on this device only.

The Concept C look lives in one file, `assets/css/theme-c.css`, loaded last on every page. It redefines the colour tokens and restyles the components; the other CSS files are the shared v1 structure.

## Notes for developers

- **The QR codes are decorative patterns, not real QR codes.**
- **Sample values come from the site's own animations:**
  - "Generate Random" fills the sample words: robot diary tumble …
  - the strong password output: `pO#$nC-1?!4EakoKm5b!OAYG`
  - the attack-resistance figures
- **Copy written for the prototype:** a few states had no copy I could capture from the live site, so I wrote short text for them:
  - the camera view;
  - the Drive connect state;
  - the Create step 3 result, which reuses copy from the animation.

  Each is visually consistent with the live site, but please replace it with the real copy.
- **Structure:**
  - Header and footer are injected by `assets/js/app.js`.
  - Each page sets `data-section` to highlight the active nav icon.
  - Page-specific behaviour lives in `assets/js/pages.js`.

```
index.html, restore*.html, create.html, tools pages, settings.html, help.html
assets/css/theme-c.css  Concept C theme: tokens + component skin (loaded last)
assets/css/styles.css   base tokens, header, buttons, cards, forms, footer, Master Key
assets/css/home.css     single-screen hero + phone mock
assets/css/pages.css    inner-page components (wizard, card designer, settings…)
assets/js/app.js        icons, header/footer, theme, Master Key, shared controls
assets/js/hero.js       animated hero (4 flows × 4 steps)
assets/js/pages.js      page interactions
```

© 2026 Norionsoft OÜ — design prototype.
