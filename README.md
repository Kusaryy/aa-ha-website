# AA-HA Group – Website

Statische Unternehmenswebsite der AA-HA Group KG (IT-Service · Hamburg · seit 1993).

## Stack

Vanilla HTML, CSS, JavaScript. Keine Build-Tools, keine Frameworks. Inter via [rsms.me/inter](https://rsms.me/inter).

## Struktur

```
.
├── index.html          # Startseite
├── leistungen.html     # 4 Kernleistungen im Detail
├── ueber-uns.html      # Geschichte, Werte, Geschäftsführung
├── kontakt.html        # Kontaktwege + Formular
├── impressum.html
├── datenschutz.html
└── assets/
    ├── style.css       # Design-System + Layout
    └── main.js         # Theme-Toggle, Reveal-on-Scroll, Mobile-Nav
```

## Lokal anschauen

```bash
python3 -m http.server 8000
# http://localhost:8000
```

## Designsystem

- **Farben:** warmes Off-White `#F8F7F4`, Anthrazit `#0B0D10`, Akzent navy `#1B3A6F`
- **Typografie:** Inter (variable), fluid type scale via `clamp()`
- **Spacing:** 8-pt-Skala, `clamp()`-basierte Section-Paddings
- **Motion:** dezente Reveal-on-Scroll-Effekte, `prefers-reduced-motion` respektiert
- **Theme:** Light / Dark via Toggle, persistiert in `localStorage`

## Deployment

Statisch deploybar auf jedem Webserver, Netlify, Vercel oder GitHub Pages.

### GitHub Pages

```bash
# Im GitHub-Repo: Settings → Pages → Source: main / root
```
