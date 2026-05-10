# AA-HA Group Website — Premium Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Upgrade the AA-HA Group website across 8 active milestones to premium B2B quality — improving visual design, adding missing content sections, enhancing mobile UX, and adding SEO foundations, without changing the existing no-build-tool HTML/CSS/JS architecture.

**Architecture:** Pure HTML/CSS/JS, no framework, no build tool. All styling in `assets/style.css`, all interactivity in `assets/main.js`. Dark mode via `[data-theme="dark"]` on `<html>`. Scroll-reveal via `[data-reveal]` + IntersectionObserver. All 6 HTML pages share the same CSS and JS files.

**Tech Stack:** HTML5 semantic markup, CSS custom properties (design tokens), vanilla JS ES6 IIFE, Inter Variable (currently rsms.me CDN → M9: self-hosted)

---

## Milestone Checkliste (Gesamtübersicht)

- [ ] **M1** — CSS Design System: Tokens + Eyebrow + Dark Mode Fix
- [ ] **M2** — Hero: Textkorrektur Numbers-Block
- [ ] **M3** — Trust Bar: SVG-Logos statt Text-Spans
- [ ] **M4** — Testimonials: Neue Sektion in index.html
- [x] **M5** — FAQ: Bereits vollständig implementiert
- [ ] **M6** — Über-uns: Timeline + Standort-Sektion
- [ ] **M7** — Kontakt: Telefon visuell stärker hervorheben
- [ ] **M8** — Mobile: Sticky CTA Bar + Telefon im Mobile-Menü
- [ ] **M9** — SEO & Performance: Inter self-hosted, Schema.org, OG, sitemap, robots

---

## File Map

| Datei | Geändert in Milestone |
|---|---|
| `assets/style.css` | M1, M3, M4, M6, M7, M8 |
| `assets/main.js` | M8 |
| `index.html` | M2, M3, M4 |
| `ueber-uns.html` | M6 |
| `kontakt.html` | M7 |
| `leistungen.html` | M9 |
| `impressum.html` | M9 |
| `datenschutz.html` | M9 |
| `assets/logos/microsoft.svg` | M3 (neu) |
| `assets/logos/vmware.svg` | M3 (neu) |
| `assets/logos/fujitsu.svg` | M3 (neu) |
| `assets/logos/sophos.svg` | M3 (neu) |
| `assets/logos/rangee.svg` | M3 (neu) |
| `assets/fonts/InterVariable.woff2` | M9 (neu) |
| `sitemap.xml` | M9 (neu) |
| `robots.txt` | M9 (neu) |

---

## Task 1: M1 — CSS Design System Tokens & Eyebrow

**Files:**
- Modify: `assets/style.css`

**Goal:** Neue Token hinzufügen, Eyebrow zu Dot+Linie upgraden, Dark Mode CTA-Button verifizieren.

- [ ] **Step 1: Amber-Tokens hinzufügen**

In `assets/style.css`, in `:root {}`, nach der Zeile `--success: #2F7D5B;` einfügen:

```css
  --amber:       #B8860B;
  --amber-soft:  rgba(184, 134, 11, 0.08);
  --section-hero: clamp(80px, 10vw, 140px);
```

- [ ] **Step 2: Eyebrow::before auf Dot + Linie upgraden**

Bestehende Regel `.eyebrow::before { ... }` (aktuell: 24×1px Linie) komplett ersetzen:

```css
/* ERSETZEN (war: 24px Linie) */
.eyebrow::before {
  content: '';
  width: 28px;
  height: 2px;
  background: linear-gradient(
    to right,
    var(--accent) 0, var(--accent) 3px,
    transparent 3px, transparent 7px,
    currentColor 7px
  );
  opacity: 0.55;
  border-radius: 1px;
  flex-shrink: 0;
}
```

Ergebnis: Kleiner Accent-Punkt, dann eine Lücke, dann eine gedimmte Linie.

- [ ] **Step 3: Dark-Mode CTA-Button verifizieren**

`python3 -m http.server 8080` im Projektordner starten, `http://localhost:8080` im Browser öffnen. Dark Mode aktivieren (Mond-Icon). Zum CTA-Block scrollen ("Lassen Sie uns kurz sprechen."). Prüfen ob der Ghost-Button "040 600 602 0" lesbar ist (cream text auf dunklem Hintergrund). Falls Kontrast zu niedrig, diese Regel nach den bestehenden `.cta .btn--ghost`-Regeln einfügen:

```css
[data-theme="dark"] .cta .btn--ghost {
  color: var(--text);
  border-color: var(--border-strong);
}
[data-theme="dark"] .cta .btn--ghost:hover {
  background: var(--surface-2);
}
```

- [ ] **Step 4: Commit**

```bash
git add assets/style.css
git commit -m "M1: add amber tokens, upgrade eyebrow dot+line, verify dark CTA"
```

---

## Task 2: M2 — Hero Textkorrektur

**Files:**
- Modify: `index.html`

- [ ] **Step 1: "seit Hamburg" korrigieren**

In `index.html`, den Numbers-Block finden. Die erste `<div class="number">` enthält `<dt>seit Hamburg</dt>`. Durch korrekten Text ersetzen:

```html
<!-- ERSETZEN -->
<div class="number">
  <dt>Gegründet in Hamburg</dt>
  <dd>1993</dd>
</div>
```

- [ ] **Step 2: Verifizieren**

Im Browser `http://localhost:8080/index.html` prüfen. Der Numbers-Block soll "Gegründet in Hamburg / 1993" zeigen (Label oben, Zahl darunter durch `order`-CSS).

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "M2: fix 'seit Hamburg' to 'Gegründet in Hamburg' in Numbers block"
```

---

## Task 3: M3 — Trust Bar mit SVG-Logos

**Files:**
- Create: `assets/logos/microsoft.svg`, `vmware.svg`, `fujitsu.svg`, `sophos.svg`, `rangee.svg`
- Modify: `index.html`, `assets/style.css`

**Goal:** Fünf SVG-Logos in `assets/logos/` erstellen, die Trust-Bar-HTML auf `<img>`-Tags umstellen, CSS auf saubere monochrome Behandlung upgraden.

- [ ] **Step 1: Logos-Verzeichnis anlegen und SVG-Dateien erstellen**

```bash
mkdir -p /mnt/data/code-projects/aa-ha-website/assets/logos
```

**`assets/logos/microsoft.svg`** — Ikonisches 4-Quadrat-Grid, rein geometrisch:

```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 21 21">
  <rect x="0"  y="0"  width="10" height="10" fill="black"/>
  <rect x="11" y="0"  width="10" height="10" fill="black"/>
  <rect x="0"  y="11" width="10" height="10" fill="black"/>
  <rect x="11" y="11" width="10" height="10" fill="black"/>
</svg>
```

**`assets/logos/vmware.svg`** — Wordmark, Helvetica Neue Bold:

```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 78 22">
  <text x="0" y="17" font-family="'Helvetica Neue',Helvetica,Arial,sans-serif"
        font-weight="700" font-size="17" fill="black" letter-spacing="-0.3">VMware</text>
</svg>
```

**`assets/logos/fujitsu.svg`** — Wordmark Großbuchstaben:

```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 74 22">
  <text x="0" y="17" font-family="'Helvetica Neue',Helvetica,Arial,sans-serif"
        font-weight="700" font-size="15" fill="black" letter-spacing="0.5">FUJITSU</text>
</svg>
```

**`assets/logos/sophos.svg`** — Wordmark:

```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 62 22">
  <text x="0" y="17" font-family="'Helvetica Neue',Helvetica,Arial,sans-serif"
        font-weight="700" font-size="17" fill="black" letter-spacing="-0.2">Sophos</text>
</svg>
```

**`assets/logos/rangee.svg`** — Wordmark:

```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 62 22">
  <text x="0" y="17" font-family="'Helvetica Neue',Helvetica,Arial,sans-serif"
        font-weight="700" font-size="17" fill="black" letter-spacing="-0.2">Rangee</text>
</svg>
```

- [ ] **Step 2: Trust Bar HTML in index.html aktualisieren**

Den gesamten `<div class="trust-list">` Block in `index.html` suchen und ersetzen:

```html
<div class="trust-list" data-reveal data-reveal-delay="1">
  <img src="assets/logos/microsoft.svg"
       alt="Microsoft" class="trust-logo trust-logo--icon"
       width="22" height="22" loading="lazy" />
  <img src="assets/logos/vmware.svg"
       alt="VMware" class="trust-logo"
       width="85" height="22" loading="lazy" />
  <img src="assets/logos/fujitsu.svg"
       alt="Fujitsu" class="trust-logo"
       width="80" height="22" loading="lazy" />
  <img src="assets/logos/sophos.svg"
       alt="Sophos" class="trust-logo"
       width="68" height="22" loading="lazy" />
  <img src="assets/logos/rangee.svg"
       alt="Rangee" class="trust-logo"
       width="68" height="22" loading="lazy" />
</div>
```

- [ ] **Step 3: Trust Bar CSS in style.css aktualisieren**

Den bestehenden Block `/* ---------- Trust bar ---------- */` (`.trust`, `.trust-label`, `.trust-list span`) finden. Die `.trust-list span` Regeln durch folgende ersetzen:

```css
/* Alte Regel entfernen: */
/* .trust-list span { font-size: ...; ... } */
/* .trust-list span + span { border-left: ...; } */
/* .trust-list span:hover { ... } */

/* Neue Regeln: */
.trust-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  gap: clamp(20px, 4vw, 52px);
}
.trust-logo {
  height: 22px;
  width: auto;
  max-width: 110px;
  object-fit: contain;
  display: block;
  filter: grayscale(1) opacity(0.32);
  transition: filter var(--dur) var(--ease);
}
.trust-logo:hover {
  filter: grayscale(0) opacity(1);
}
[data-theme="dark"] .trust-logo {
  filter: invert(1) grayscale(1) opacity(0.42);
}
[data-theme="dark"] .trust-logo:hover {
  filter: invert(1) grayscale(0) opacity(1);
}
```

- [ ] **Step 4: Verifizieren**

Im Browser prüfen:
- Trust Bar zeigt 5 Logos/Wordmarks in einer Reihe, deutlich gedimmt
- Hover macht jedes Logo voll sichtbar
- Dark Mode: Logos erscheinen hell auf dunklem Hintergrund
- Mobile (DevTools, 375px): Logos umbrechen, bleibt lesbar

- [ ] **Step 5: Commit**

```bash
git add assets/logos/ index.html assets/style.css
git commit -m "M3: trust bar — replace text spans with SVG logos, add monochrome hover treatment"
```

---

## Task 4: M4 — Testimonials Sektion

**Files:**
- Modify: `index.html`, `assets/style.css`

**Goal:** Neue `.testimonials`-Sektion mit 3 Platzhalter-Karten nach dem Numbers-Block in index.html einfügen.

⚠️ **Vor dem Launch:** Platzhalter durch echte, genehmigte Kundenzitate ersetzen.

- [ ] **Step 1: HTML in index.html einfügen**

Nach dem schließenden `</section>` des Numbers-Blocks (Ende von `<!-- NUMBERS -->`) und vor `<!-- ABOUT STRIP -->` diese neue Sektion einfügen:

```html
<!-- ============== TESTIMONIALS ============== -->
<!-- ⚠️ PLATZHALTER: Vor Launch durch echte Kundenzitate ersetzen -->
<section class="section section--tight section--no-top">
  <div class="container">
    <div class="section-header">
      <p class="eyebrow" data-reveal>Kundenstimmen</p>
      <h2 class="section-title" data-reveal data-reveal-delay="1">Was unsere Kunden sagen.</h2>
    </div>
    <div class="testimonials-grid" data-reveal>

      <blockquote class="testimonial">
        <p>„Seit AA-HA unser Netzwerk betreut, hatten wir keinen einzigen ungeplanten Ausfall mehr. Endlich ein IT-Partner, der abnimmt, wenn wir anrufen – und der weiß, was bei uns läuft."</p>
        <footer>
          <strong>[Kundenname, Funktion]</strong>
          <span>[Unternehmensname] · [Branche]</span>
        </footer>
      </blockquote>

      <blockquote class="testimonial">
        <p>„Die Basisanalyse war in 60 Minuten erledigt und hat uns mehr geklärt als zwei Jahre mit dem vorherigen Anbieter. Der Wechsel war die richtige Entscheidung."</p>
        <footer>
          <strong>[Kundenname, Funktion]</strong>
          <span>[Unternehmensname] · [Branche]</span>
        </footer>
      </blockquote>

      <blockquote class="testimonial">
        <p>„Wir schätzen besonders, dass wir immer denselben Ansprechpartner erreichen. Keine Tickets, keine Warteschleifen – das ist das, was wir unter IT-Service verstehen."</p>
        <footer>
          <strong>[Kundenname, Funktion]</strong>
          <span>[Unternehmensname] · [Branche]</span>
        </footer>
      </blockquote>

    </div>
  </div>
</section>
```

- [ ] **Step 2: CSS in style.css hinzufügen**

Nach dem `/* ---------- About strip ---------- */` Block einfügen:

```css
/* ---------- Testimonials ---------- */
.testimonials-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 20px;
}
@media (min-width: 768px) {
  .testimonials-grid { grid-template-columns: repeat(3, 1fr); }
}
.testimonial {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius-lg);
  padding: clamp(24px, 2.5vw, 36px);
  display: flex;
  flex-direction: column;
  gap: 24px;
  position: relative;
  transition: border-color var(--dur) var(--ease), box-shadow var(--dur) var(--ease);
}
.testimonial:hover {
  border-color: var(--border-strong);
  box-shadow: var(--shadow-md);
}
.testimonial::before {
  content: '\201C';
  position: absolute;
  top: 20px;
  right: 24px;
  font-size: 64px;
  line-height: 1;
  font-weight: 800;
  color: var(--accent);
  opacity: 0.10;
  font-family: Georgia, serif;
  pointer-events: none;
  user-select: none;
}
.testimonial p {
  font-size: clamp(15px, 1.2vw, 17px);
  line-height: 1.65;
  color: var(--text);
  font-style: italic;
  letter-spacing: -0.003em;
  flex: 1;
}
.testimonial footer {
  display: flex;
  flex-direction: column;
  gap: 4px;
  padding-top: 16px;
  border-top: 1px solid var(--border);
}
.testimonial footer strong {
  font-size: 14px;
  font-weight: 600;
  color: var(--text);
  font-style: normal;
}
.testimonial footer span {
  font-size: 13px;
  color: var(--text-muted);
  font-style: normal;
}
```

- [ ] **Step 3: Verifizieren**

Im Browser scrollen bis zur neuen Sektion (zwischen Numbers und About Strip). Prüfen:
- 3 Karten nebeneinander auf Desktop
- 1 Karte pro Zeile auf Mobile
- Großes gedimmtes Anführungszeichen oben rechts in jeder Karte
- Hover: leichter Rahmen + Schatten

- [ ] **Step 4: Commit**

```bash
git add index.html assets/style.css
git commit -m "M4: add testimonials section with 3 placeholder cards"
```

---

## Task 5: M6 — Über-uns: Timeline & Standort

**Files:**
- Modify: `ueber-uns.html`, `assets/style.css`

**Goal:** Firmengeschichte als kompakte 3-Punkt-Timeline visualisieren, Standort-Sektion aufwerten. Das Portrait (`bb.gif`) und die Biografie sind bereits korrekt implementiert.

- [ ] **Step 1: Timeline HTML vor der `.values`-Section einfügen**

In `ueber-uns.html`, direkt nach dem `<h3>Was uns trägt</h3>` Absatz aber VOR `<div class="values">`, diese Sektion einfügen (also die Reihenfolge wird: Text → Timeline → Werte → Team → Standort):

Den bestehenden Aufbau lesen: `<h3>Was uns trägt</h3>` + `<p>Drei Überzeugungen...` + `<div class="values">`. Die Timeline soll ZWISCHEN diesem Text und dem h3 "Was uns trägt" erscheinen. Also NACH dem ersten `<p>Wir verkaufen keine Pakete...</p>` und VOR `<h3>Was uns trägt</h3>`:

```html
      <div class="timeline" data-reveal>
        <div class="timeline-item">
          <div class="timeline-year">1993</div>
          <div class="timeline-content">
            <h4>Gründung in Hamburg</h4>
            <p>Branko Bersa gründet AA-HA Group. Fokus: LAN-Netzwerke, Infrastruktur und Support für Hamburger Betriebe.</p>
          </div>
        </div>
        <div class="timeline-item">
          <div class="timeline-year">2000er</div>
          <div class="timeline-content">
            <h4>Wachstum mit dem Mittelstand</h4>
            <p>Ausbau auf Server-Infrastrukturen, WLAN-Standortvernetzung und erste Cloud-Dienste. Über 100 Kunden im Großraum Hamburg.</p>
          </div>
        </div>
        <div class="timeline-item">
          <div class="timeline-year">Heute</div>
          <div class="timeline-content">
            <h4>Hybride Architekturen, gleiche Haltung</h4>
            <p>Microsoft 365, hybride Cloud, Sicherheitsarchitekturen – und noch immer derselbe Anspruch: IT, die nicht im Weg steht.</p>
          </div>
        </div>
      </div>
```

- [ ] **Step 2: Standort-Sektion aufwerten**

Den bestehenden `<h3>Standort</h3>` Block in `ueber-uns.html` finden:
```html
<h3>Standort</h3>
<p>Steinway Haus 7, Rondenbarg 15, 22525 Hamburg-Stellingen...</p>
```

Durch diesen reichhaltigeren Block ersetzen:

```html
<h3>Standort</h3>
<div class="standort-card" data-reveal>
  <div class="standort-address">
    <p class="standort-label">Hauptsitz</p>
    <p class="standort-name">Steinway Haus 7</p>
    <p class="standort-detail">Rondenbarg 15 · 22525 Hamburg-Stellingen</p>
  </div>
  <div class="standort-facts">
    <div class="standort-fact">
      <span class="standort-fact-label">Erreichbarkeit Auto</span>
      <span class="standort-fact-value">Direkt an der A7, Ausfahrt Volkspark</span>
    </div>
    <div class="standort-fact">
      <span class="standort-fact-label">Öffentlich</span>
      <span class="standort-fact-value">S-Bahn Diebsteich · Bus 183, 281</span>
    </div>
    <div class="standort-fact">
      <span class="standort-fact-label">Servicegebiet</span>
      <span class="standort-fact-value">Hamburg & Norddeutschland · Remote deutschlandweit</span>
    </div>
    <div class="standort-fact">
      <span class="standort-fact-label">Erreichbarkeit</span>
      <span class="standort-fact-value">Mo – Fr · 8 – 18 Uhr</span>
    </div>
  </div>
</div>
```

- [ ] **Step 3: Timeline und Standort CSS in style.css hinzufügen**

Nach den `.team-portrait` Regeln, neue Regeln einfügen:

```css
/* ---------- Über-uns Timeline ---------- */
.timeline {
  display: flex;
  flex-direction: column;
  gap: 0;
  margin-block: clamp(32px, 4vw, 48px);
  position: relative;
}
.timeline::before {
  content: '';
  position: absolute;
  left: 56px;
  top: 12px;
  bottom: 12px;
  width: 1px;
  background: var(--border);
}
@media (max-width: 600px) {
  .timeline::before { left: 36px; }
}
.timeline-item {
  display: grid;
  grid-template-columns: 112px 1fr;
  gap: 24px;
  align-items: start;
  padding-block: 20px;
}
@media (max-width: 600px) {
  .timeline-item { grid-template-columns: 72px 1fr; gap: 16px; }
}
.timeline-item + .timeline-item { border-top: 1px solid var(--border); }
.timeline-year {
  font-size: 13px;
  font-weight: 700;
  letter-spacing: 0.04em;
  color: var(--accent);
  padding-top: 3px;
  text-align: right;
  position: relative;
  z-index: 1;
}
.timeline-year::after {
  content: '';
  position: absolute;
  right: -32px;
  top: 8px;
  width: 7px;
  height: 7px;
  border-radius: 50%;
  background: var(--accent);
  border: 2px solid var(--bg);
  box-shadow: 0 0 0 1px var(--accent);
}
@media (max-width: 600px) {
  .timeline-year::after { right: -22px; }
}
.timeline-content h4 {
  font-size: 16px;
  font-weight: 600;
  letter-spacing: -0.01em;
  margin-bottom: 6px;
}
.timeline-content p {
  font-size: 15px;
  color: var(--text-muted);
  line-height: 1.6;
  margin: 0;
  max-width: 52ch;
}

/* ---------- Standort-Card ---------- */
.standort-card {
  display: grid;
  grid-template-columns: 1fr;
  gap: clamp(24px, 3vw, 40px);
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius-lg);
  padding: clamp(24px, 3vw, 36px);
  margin-top: 16px;
}
@media (min-width: 700px) {
  .standort-card { grid-template-columns: 1fr 1.8fr; }
}
.standort-label {
  font-size: 12px;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--text-muted);
  margin-bottom: 8px;
}
.standort-name {
  font-size: 20px;
  font-weight: 600;
  letter-spacing: -0.01em;
  margin-bottom: 4px;
  color: var(--text);
}
.standort-detail {
  font-size: 15px;
  color: var(--text-muted);
}
.standort-facts {
  display: flex;
  flex-direction: column;
  gap: 0;
}
.standort-fact {
  display: grid;
  grid-template-columns: 1fr 1.4fr;
  gap: 12px;
  padding-block: 12px;
  border-bottom: 1px solid var(--border);
  align-items: baseline;
}
.standort-fact:first-child { padding-top: 0; }
.standort-fact:last-child { border-bottom: 0; padding-bottom: 0; }
.standort-fact-label {
  font-size: 12px;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  color: var(--text-muted);
}
.standort-fact-value {
  font-size: 15px;
  font-weight: 500;
  color: var(--text);
}
```

- [ ] **Step 4: Testimonial-Platzhalter auf Über-uns-Seite einfügen**

In `ueber-uns.html`, zwischen dem schließenden `</section>` der `.about-rich` Section und dem `<section class="section section--no-top">` (CTA-Block), diese neue Sektion einfügen:

```html
<section class="section section--tight section--no-top">
  <div class="container container--narrow">
    <!-- ⚠️ PLATZHALTER: Vor Launch durch echtes Kundenzitat ersetzen -->
    <blockquote class="testimonial testimonial--featured" data-reveal>
      <p>„AA-HA betreut uns seit über zehn Jahren. In dieser Zeit hat sich unsere Infrastruktur komplett verändert – unsere IT-Probleme nicht mehr. Das ist kein Zufall."</p>
      <footer>
        <strong>[Kundenname, Funktion]</strong>
        <span>[Unternehmensname] · [Branche] · Kunde seit [Jahr]</span>
      </footer>
    </blockquote>
  </div>
</section>
```

CSS für die featured-Variante am Ende des `.testimonial`-Blocks ergänzen:

```css
.testimonial--featured {
  border-color: var(--border-strong);
  background: var(--surface-2);
}
.testimonial--featured p {
  font-size: clamp(17px, 1.5vw, 20px);
}
```

- [ ] **Step 5: Verifizieren**

`http://localhost:8080/ueber-uns.html` prüfen:
- Timeline zeigt 3 Einträge mit vertikaler Linie links, Accent-Punkt als Marker
- Standort-Card zeigt Adresse links, Facts-Grid rechts
- Portrait von Branko Bersa (bb.gif) wird korrekt angezeigt
- Testimonial-Platzhalter erscheint vor dem CTA-Block, etwas größere Schrift
- Mobile (375px): Timeline, Standort-Card und Testimonial stacken sauber

- [ ] **Step 6: Commit**

```bash
git add ueber-uns.html assets/style.css
git commit -m "M6: add company timeline, standort card, and testimonial placeholder to über-uns page"
```

---

## Task 6: M7 — Kontakt: Telefonnummer hervorheben

**Files:**
- Modify: `assets/style.css`

**Goal:** Den Telefon-Eintrag im Kontakt-Grid visuell als primären Kanal hervorheben.

**Hinweis:** Das FAQ-Accordion auf der Kontaktseite ist bereits vollständig implementiert.

- [ ] **Step 1: CSS für prominente Telefon-Darstellung ergänzen**

In `assets/style.css` nach den bestehenden `.contact-info` Regeln folgende Ergänzung hinzufügen:

```css
/* Telefon als primärer Kontaktweg hervorheben */
.contact-info dl > div:first-child dd {
  font-size: clamp(22px, 2.5vw, 28px);
  font-weight: 700;
  letter-spacing: -0.02em;
  color: var(--text);
}
.contact-info dl > div:first-child dd a {
  color: var(--text);
}
.contact-info dl > div:first-child dd a:hover {
  color: var(--accent);
}
.contact-info dl > div:first-child .small {
  font-size: 13px;
  color: var(--accent);
  font-weight: 500;
}
```

- [ ] **Step 2: Verifizieren**

`http://localhost:8080/kontakt.html` prüfen:
- Die Telefonnummer "040 600 602 0" ist größer und fetter als die anderen Kontaktfelder
- "Mo – Fr · 8 – 18 Uhr" erscheint in Accent-Farbe als Signal
- Dark Mode: Kontrast korrekt

- [ ] **Step 3: Commit**

```bash
git add assets/style.css
git commit -m "M7: make phone number visually prominent as primary contact method"
```

---

## Task 7: M8 — Mobile Sticky CTA Bar

**Files:**
- Modify: `assets/style.css`, `assets/main.js`, `index.html`, `leistungen.html`, `ueber-uns.html`, `kontakt.html`

**Goal:** Sticky Bottom Bar auf Mobile mit Telefon + Anfrage-Button. Erscheint nach erstem Scroll. Telefonnummer im Mobile-Menü ergänzen.

- [ ] **Step 1: Sticky CTA Bar HTML in alle 4 Hauptseiten einfügen**

In jeder der folgenden Dateien — `index.html`, `leistungen.html`, `ueber-uns.html`, `kontakt.html` — direkt VOR `<script src="assets/main.js" defer></script>` einfügen:

```html
<!-- Mobile Sticky CTA Bar -->
<div class="mobile-cta-bar" data-mobile-cta aria-hidden="true">
  <a href="tel:+494060060200" class="mobile-cta-tel" aria-label="AA-HA Group anrufen: 040 600 602 0">
    <svg viewBox="0 0 24 24" width="18" height="18" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07A19.5 19.5 0 0 1 4.69 12 19.79 19.79 0 0 1 1.61 3.4 2 2 0 0 1 3.6 1.22h3a2 2 0 0 1 2 1.72c.127.96.361 1.903.7 2.81a2 2 0 0 1-.45 2.11L7.91 8.96a16 16 0 0 0 6.13 6.13l.96-.96a2 2 0 0 1 2.11-.45c.907.339 1.85.573 2.81.7A2 2 0 0 1 22 16.92z"/></svg>
    040 600 602 0
  </a>
  <a href="kontakt.html" class="btn btn--primary mobile-cta-btn">Anfrage stellen</a>
</div>
```

- [ ] **Step 2: CSS für Mobile CTA Bar in style.css hinzufügen**

Am Ende der `/* ---------- Mobile nav ---------- */` Sektion, aber noch VOR `/* ---------- Utilities ---------- */`:

```css
/* ---------- Mobile Sticky CTA Bar ---------- */
.mobile-cta-bar {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 40;
  display: none;
  align-items: center;
  gap: 10px;
  padding: 10px var(--gutter);
  padding-bottom: calc(10px + env(safe-area-inset-bottom, 0px));
  background: color-mix(in srgb, var(--bg) 90%, transparent);
  -webkit-backdrop-filter: saturate(180%) blur(16px);
  backdrop-filter: saturate(180%) blur(16px);
  border-top: 1px solid var(--border);
  opacity: 0;
  transform: translateY(8px);
  transition: opacity var(--dur) var(--ease), transform var(--dur) var(--ease);
}
.mobile-cta-bar.is-visible {
  opacity: 1;
  transform: translateY(0);
}
@media (max-width: 768px) {
  .mobile-cta-bar { display: flex; }
}
.mobile-cta-tel {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 15px;
  font-weight: 600;
  color: var(--text);
  white-space: nowrap;
  flex: 1;
  transition: color var(--dur-fast) var(--ease);
}
.mobile-cta-tel:hover { color: var(--accent); }
.mobile-cta-btn {
  flex-shrink: 0;
  font-size: 14px;
  padding: 11px 18px;
}
```

- [ ] **Step 3: JS Scroll-Trigger in main.js hinzufügen**

In `assets/main.js`, NACH dem Block `// ---------- Active nav link by current page ----------` und VOR dem `// ---------- FAQ accordion ----------` Block, einfügen:

```js
  // ---------- Mobile sticky CTA bar ----------
  const mobileCta = document.querySelector('[data-mobile-cta]');
  if (mobileCta) {
    let shown = false;
    const showCta = () => {
      if (!shown && window.scrollY > 80) {
        mobileCta.classList.add('is-visible');
        mobileCta.setAttribute('aria-hidden', 'false');
        shown = true;
      }
    };
    window.addEventListener('scroll', showCta, { passive: true });
    showCta();
  }
```

- [ ] **Step 4: Telefonnummer im Mobile-Menü ergänzen**

In allen 4 Hauptseiten (`index.html`, `leistungen.html`, `ueber-uns.html`, `kontakt.html`) den `<div class="mobile-menu">` Block finden. Nach dem `<a href="kontakt.html" class="btn btn--primary">Basisanalyse anfordern</a>` folgendes hinzufügen:

```html
  <a href="tel:+494060060200" class="mobile-menu-tel">
    <svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07A19.5 19.5 0 0 1 4.69 12 19.79 19.79 0 0 1 1.61 3.4 2 2 0 0 1 3.6 1.22h3a2 2 0 0 1 2 1.72c.127.96.361 1.903.7 2.81a2 2 0 0 1-.45 2.11L7.91 8.96a16 16 0 0 0 6.13 6.13l.96-.96a2 2 0 0 1 2.11-.45c.907.339 1.85.573 2.81.7A2 2 0 0 1 22 16.92z"/></svg>
    040 600 602 0
  </a>
```

CSS dafür am Ende von `/* ---------- Mobile nav ---------- */` ergänzen:

```css
.mobile-menu-tel {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  font-size: 17px;
  font-weight: 500;
  color: var(--text-muted);
  margin-top: 16px;
  transition: color var(--dur-fast) var(--ease);
}
.mobile-menu-tel:hover { color: var(--accent); }
```

- [ ] **Step 5: Verifizieren**

Browser DevTools auf 375px Breite stellen:
- Seite laden, nach unten scrollen → Sticky Bar erscheint nach ~80px Scroll
- Bar zeigt Telefonnummer links + "Anfrage stellen" Button rechts
- Mobile-Menü öffnen → Telefonnummer erscheint unterhalb des CTA-Buttons
- iPhone Notch-Bereich: `env(safe-area-inset-bottom)` verhindert Überlappung (im Simulator testbar)

- [ ] **Step 6: Commit**

```bash
git add index.html leistungen.html ueber-uns.html kontakt.html assets/style.css assets/main.js
git commit -m "M8: add mobile sticky CTA bar with phone + request button, phone in mobile menu"
```

---

## Task 8: M9 — SEO & Performance

**Files:**
- Modify: alle HTML-Dateien, `assets/style.css`
- Create: `assets/fonts/InterVariable.woff2`, `sitemap.xml`, `robots.txt`

**Goal:** Inter Variable selbst hosten, Schema.org JSON-LD, OpenGraph Meta-Tags, sitemap.xml, robots.txt.

- [ ] **Step 1: Inter Variable Font herunterladen**

```bash
mkdir -p /mnt/data/code-projects/aa-ha-website/assets/fonts
curl -L "https://rsms.me/inter/font-files/Inter.var.woff2" \
  -o /mnt/data/code-projects/aa-ha-website/assets/fonts/InterVariable.woff2

# Nur Latin-Subset für schnellere Ladezeit:
curl -L "https://rsms.me/inter/font-files/Inter.var.subset.woff2" \
  -o /mnt/data/code-projects/aa-ha-website/assets/fonts/InterVariable.subset.woff2
```

- [ ] **Step 2: @font-face in style.css ergänzen**

Ganz oben in `style.css`, VOR `:root {}`, einfügen:

```css
@font-face {
  font-family: 'Inter';
  font-style: normal;
  font-weight: 100 900;
  font-display: swap;
  src: url('fonts/InterVariable.subset.woff2') format('woff2-variations'),
       url('fonts/InterVariable.woff2') format('woff2-variations');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA,
                 U+02DC, U+2000-206F, U+2074, U+20AC, U+2122, U+2191, U+2193,
                 U+2212, U+2215, U+FEFF, U+FFFD;
}
```

- [ ] **Step 3: CDN-Links in allen HTML-Dateien durch Preload ersetzen**

In ALLEN 6 HTML-Dateien (`index.html`, `leistungen.html`, `ueber-uns.html`, `kontakt.html`, `impressum.html`, `datenschutz.html`) diese beiden Zeilen:

```html
<link rel="preconnect" href="https://rsms.me/" />
<link rel="stylesheet" href="https://rsms.me/inter/inter.css" />
```

Ersetzen durch:

```html
<link rel="preload" href="assets/fonts/InterVariable.subset.woff2" as="font" type="font/woff2" crossorigin />
```

- [ ] **Step 4: Schema.org JSON-LD in index.html einfügen**

In `index.html`, direkt VOR `</head>` einfügen:

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "LocalBusiness",
  "name": "AA-HA Group KG",
  "description": "IT-Partner für den Hamburger Mittelstand. Netzwerke, Server, Cloud, Support – persönlich betreut seit 1993.",
  "url": "https://aa-ha.de",
  "telephone": "+494060060200",
  "email": "moin@aa-ha.de",
  "foundingDate": "1993",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "Rondenbarg 15",
    "addressLocality": "Hamburg",
    "postalCode": "22525",
    "addressCountry": "DE"
  },
  "geo": {
    "@type": "GeoCoordinates",
    "latitude": 53.578,
    "longitude": 9.931
  },
  "openingHoursSpecification": {
    "@type": "OpeningHoursSpecification",
    "dayOfWeek": ["Monday","Tuesday","Wednesday","Thursday","Friday"],
    "opens": "08:00",
    "closes": "18:00"
  },
  "areaServed": {
    "@type": "AdministrativeArea",
    "name": "Hamburg und Norddeutschland"
  }
}
</script>
```

- [ ] **Step 5: OpenGraph meta og:image auf allen Seiten ergänzen**

In ALLEN 6 HTML-Dateien, nach dem vorhandenen `<meta property="og:type" content="website" />` (oder dem letzten og:-Tag), einfügen:

```html
<meta property="og:image" content="https://aa-ha.de/assets/og-image.png" />
<meta property="og:image:width" content="1200" />
<meta property="og:image:height" content="630" />
```

⚠️ Hinweis: `assets/og-image.png` (1200×630px) muss noch erstellt werden (Design-Asset). Platzhalter-URL reicht für Launch.

- [ ] **Step 6: sitemap.xml erstellen**

Neue Datei `sitemap.xml` im Projektroot:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://aa-ha.de/</loc>
    <changefreq>monthly</changefreq>
    <priority>1.0</priority>
  </url>
  <url>
    <loc>https://aa-ha.de/leistungen.html</loc>
    <changefreq>monthly</changefreq>
    <priority>0.9</priority>
  </url>
  <url>
    <loc>https://aa-ha.de/ueber-uns.html</loc>
    <changefreq>monthly</changefreq>
    <priority>0.8</priority>
  </url>
  <url>
    <loc>https://aa-ha.de/kontakt.html</loc>
    <changefreq>monthly</changefreq>
    <priority>0.9</priority>
  </url>
</urlset>
```

- [ ] **Step 7: robots.txt erstellen**

Neue Datei `robots.txt` im Projektroot:

```
User-agent: *
Allow: /
Disallow: /impressum.html
Disallow: /datenschutz.html

Sitemap: https://aa-ha.de/sitemap.xml
```

- [ ] **Step 8: Verifizieren**

```bash
# Font-Datei vorhanden?
ls -lh /mnt/data/code-projects/aa-ha-website/assets/fonts/

# Schema.org valide? Seite im Browser laden, in DevTools → Network prüfen
# ob Inter aus /assets/fonts/ geladen wird statt rsms.me

# HTML-Validator (optional):
# https://validator.w3.org/
```

Im Browser: DevTools → Network → Font-Tab. Beim Laden von `index.html` muss `InterVariable.subset.woff2` aus `localhost:8080/assets/fonts/` geladen werden, NICHT von rsms.me.

- [ ] **Step 9: Commit**

```bash
git add assets/fonts/ assets/style.css index.html leistungen.html ueber-uns.html kontakt.html impressum.html datenschutz.html sitemap.xml robots.txt
git commit -m "M9: self-host Inter Variable, add Schema.org JSON-LD, OG meta tags, sitemap.xml, robots.txt"
```

---

## Gesamt-Checkliste nach Abschluss aller Tasks

- [ ] M1: CSS Tokens + Eyebrow ✓
- [ ] M2: Hero Textkorrektur ✓
- [ ] M3: Trust Bar SVG-Logos ✓
- [ ] M4: Testimonials Sektion ✓
- [x] M5: FAQ (bereits fertig)
- [ ] M6: Über-uns Timeline + Standort ✓
- [ ] M7: Kontakt Telefon Prominenz ✓
- [ ] M8: Mobile Sticky CTA Bar ✓
- [ ] M9: SEO & Performance ✓

**Offene Punkte nach Launch:**
- [ ] Testimonials durch echte Kundenzitate ersetzen (Genehmigung einholen)
- [ ] `assets/og-image.png` als reales 1200×630px Design-Asset erstellen
- [ ] Kontaktformular an echtes Backend anbinden (Formspree / Netlify / eigener Endpoint)
- [ ] Datenschutzerklärung finalisieren (Hinweis "Rahmen-Entwurf" entfernen)
