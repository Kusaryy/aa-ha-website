# Design Spec: AA-HA Group Website — Premium Redesign
**Datum:** 2026-05-10  
**Projekt:** AA-HA Group KG — IT-Partner für den Hamburger Mittelstand  
**Ansatz:** Option A — Phasen-Sequential nach Planungsdokument (9 Meilensteine)

---

## Kontext

Bestehende 6-Seiten-Website (reines HTML/CSS/JS, kein Build-Tool, kein Framework).  
Die Website hat ein solides technisches Fundament, aber visuelle und inhaltliche Schwächen, die Conversion und Vertrauen kosten.  
Ziel: Schichtenweise Verbesserung auf Premium-Niveau — kein Big-Bang-Rewrite.

**Offene Inhaltsfragen (vor Launch klären):**
- Echte Kundenzitate (aktuell Platzhalter)
- Kontaktformular-Backend (aktuell client-seitig, Platzhalter vereinbart)
- OG-Image-Asset (für M9)

---

## Designprinzipien

- **Typografie führt** — Hierarchie ist sofort lesbar, kein Element konkurriert mit Text
- **Ruhige Asymmetrie** — Text-schwere linke Seite, leichtere rechte Seite
- **Substanz schlägt Dekor** — kein Element ohne semantischen Wert
- **Zurückhaltende Bewegung** — Animationen haben einen Auslöser, dauern 160–600ms
- **Ehrliche Materialität** — Schatten und Tiefe aus Layer-Logik, kein Fake-Glassmorphism

**Explizit NICHT:**
- Keine Gradient-Blobs, keine Icon-Libraries, kein Glassmorphism
- Kein Counter-Up bei Numbers, keine Page-Transitions
- Kein Dashboard-Screenshot-Hero, kein "Feature Grid mit Icons"

---

## Farbe

Bestehendes Token-System bleibt. Ergänzungen:

```css
--amber:      #B8860B   /* Gold — "Seit 1993"-Badge, einmalige Highlights */
--amber-soft: rgba(184,134,11,0.08)
```

Navy `#1B3A6F` bleibt die einzige echte Farbe — Buttons, Links, Akzentlinien.  
Alle anderen Töne: Grau, Cream, Fast-Schwarz.

---

## Typografie

Inter Variable, Self-Hosting (M9). Skala unverändert:

| Element | Größe | Gewicht |
|---|---|---|
| Display H1 | `clamp(44px, 6.5vw, 96px)` | 800 |
| Section H2 | `clamp(30px, 4vw, 50px)` | 700 |
| Card H3 | `clamp(20px, 2vw, 24px)` | 600 |
| Lead | `clamp(18px, 1.4vw, 21px)` | 400 |
| Body | 17px | 400 |
| Eyebrow | 13px · 0.08em tracking · uppercase | 500 |
| Numbers | `clamp(48px, 6.5vw, 72px)` | 800 |

---

## Neue / überarbeitete Komponenten

### Trust Bar (M3)
- 5 Partner-Logos als inline SVG: Microsoft, VMware, Fujitsu, Sophos, Rangee
- CSS: `filter: grayscale(1) opacity(0.4)` im Ruhezustand
- Hover: `opacity: 1`, volle Sichtbarkeit
- Dark Mode: `filter: invert(1) grayscale(1) opacity(0.5)`
- Responsive: Flexbox-Zeile Desktop → 3+2 Grid auf Mobile

### Testimonial Card (M4)
- Kein Sterne-Rating
- Großes `❝` als `::before` Pseudo-Element (accent-Farbe, opacity 0.15)
- Felder: Zitattext, Name, Funktion, Firmenname · Branche
- Hintergrund: `--surface`, Border: `--border`
- 2-spaltig Desktop / 1-spaltig Mobile

### Portrait Branko Bersa (M6)
- `assets/bb.gif` eingebunden auf Über-uns-Seite
- Umgeben von strukturiertem Card-Frame mit Name, Titel, Biografie

### Timeline (M6)
- 3 Punkte: 1993 / 2000er / Heute
- Horizontale Linie mit Accent-Dot-Markierungen

### Sticky Mobile CTA Bar (M8)
- `position: fixed; bottom: 0; left: 0; right: 0`
- Inhalt: Telefon-Link + "Anfrage stellen"-Button
- Erscheint nach erstem Scroll (scroll-Event in main.js)
- `padding-bottom: calc(12px + env(safe-area-inset-bottom))`
- Nur sichtbar auf `max-width: 768px`

---

## Meilenstein-Checkliste

### M1 — Design System & Tokens (`style.css`)
- [ ] `--amber` und `--amber-soft` Token hinzufügen
- [ ] `.eyebrow::before` auf Dot + Linie upgraden
- [ ] Dark Mode `.btn--ghost` Fix (border-color im CTA-Block)
- [ ] `--section-tight` und `--section-hero` als explizite Tokens ergänzen
- [ ] `font-display: swap` vorbereiten

### M2 — Hero-Fixes (`index.html`)
- [ ] "seit Hamburg" → "Gegründet Hamburg" in Numbers-Block korrigieren
- [ ] Hero-Rings Z-Index sicherstellen (Status-Card liegt drüber)

### M3 — Trust Bar (`index.html`, `style.css`)
- [ ] 5 SVG-Logos als inline SVG erstellen
- [ ] CSS: grayscale + opacity, Hover-State
- [ ] Dark Mode Anpassung
- [ ] Responsive: 3+2 auf Mobile

### M4 — Testimonials (`index.html`, `style.css`)
- [ ] Neue `.testimonials`-Sektion nach Numbers einfügen
- [ ] 3 Testimonial-Cards mit Platzhalter-Inhalt
- [ ] Anführungszeichen als `::before`
- [ ] 2-spaltig Desktop / 1-spaltig Mobile

### M5 — FAQ (bereits fertig)
- [x] FAQ-Accordion vorhanden und funktional

### M6 — Über-uns-Seite (`ueber-uns.html`, `style.css`)
- [ ] `bb.gif` als echtes Portrait einbinden
- [ ] Timeline-Sektion: 1993 / 2000er / Heute
- [ ] Biografie Branko Bersa auf 2 Absätze erweitern
- [ ] Standort-Sektion mit Adresse und Anfahrt-Info
- [ ] 1 Testimonial-Platzhalter

### M7 — Kontakt-Seite (`kontakt.html`, `style.css`)
- [ ] FAQ-Accordion auf Kontakt-Seite ergänzen
- [ ] Telefonnummer visuell prominenter
- [ ] Formular bleibt client-seitig (Platzhalter)

### M8 — Mobile (`style.css`, `main.js`)
- [ ] Sticky Bottom CTA Bar implementieren
- [ ] `safe-area-inset-bottom` für iPhone Notch
- [ ] Scroll-Trigger in main.js
- [ ] Telefonnummer prominent im Mobile-Menu

### M9 — SEO & Performance (alle HTML, neue Dateien)
- [ ] Inter Variable selbst hosten (`assets/fonts/`)
- [ ] Schema.org `LocalBusiness` JSON-LD auf `index.html`
- [ ] `og:image` Meta-Tag auf allen Seiten
- [ ] `sitemap.xml` erstellen
- [ ] `robots.txt` erstellen

---

## Dateien & Abhängigkeiten

```
assets/
  style.css        ← M1, M3, M4, M6, M7, M8
  main.js          ← M8
  bb.gif           ← M6
  fonts/           ← M9 (neu)
index.html         ← M2, M3, M4, M9
leistungen.html    ← M9
ueber-uns.html     ← M6, M9
kontakt.html       ← M7, M9
impressum.html     ← M9
datenschutz.html   ← M9
sitemap.xml        ← M9 (neu)
robots.txt         ← M9 (neu)
```

---

## Technische Randbedingungen

- Kein Build-Tool, kein Framework — reines HTML/CSS/JS
- Dark Mode via `data-theme="dark"` auf `<html>`
- Scroll-Reveal via IntersectionObserver (`[data-reveal]`)
- Responsive Breakpoints: 768px (Mobile→Desktop), 960px (Hero 2-spaltig)
- Font-Feature-Settings: `'cv11', 'ss01', 'ss03'` beibehalten
- Kein Counter-Up, keine Page-Transitions, kein Continuous-Parallax
