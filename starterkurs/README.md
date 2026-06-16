# Tierkommunikations-Starterkurs 2.0 — Landing Pages

Zwei statische Single-File-Landing-Pages für moni.goes.wild (Ramona Hoffmann).
Kein Build-Step, kein Framework — CSS + JS sind inline. Direkt deploybar.

| Datei | Rolle | CTA |
|---|---|---|
| `index.html` | **Opt-in-Seite** – sammelt E-Mail für den Gratis-Einstieg | Formular → `{{OPTIN_FORM_ENDPOINT}}` |
| `kurs.html` | **Sales Page** – verkauft den Starterkurs für 97 € | Button → `{{CHECKOUT_URL_97}}` |

---

## 1. Platzhalter füllen

Alles, was Adnan setzen muss, steht im Format `{{...}}`. Schnell finden:

```bash
grep -rn "{{" index.html kurs.html
```

| Platzhalter | Wo | Was eintragen |
|---|---|---|
| `{{OPTIN_FORM_ENDPOINT}}` | `index.html` – `<form action>` | Formular-Ziel (Easy2 / ManyChat / Zapier). Felder: `email`, `consent` |
| `{{CHECKOUT_URL_97}}` | `kurs.html` – 5× (Topbar, Hero, Angebot, Final-CTA, Sticky-CTA) | Checkout-Link 97-€-Produkt |
| `{{WHATSAPP_COMMUNITY_LINK}}` | `kurs.html` – PS im Final-CTA | Einladungslink zur **kostenlosen** WhatsApp-Community (separater Bonus, **nicht** Kursbestandteil) |
| `{{HERO_IMAGE_URL}}` | `index.html` – Foto-Slot im Hero | Foto von Ramona (+ Tier). Aktuell dezenter Platzhalter; echtes Foto deutlich besser für Opt-in-Conversion |
| `{{DATENSCHUTZ_URL}}` | beide – Consent-Text + Footer | Link zur Datenschutzerklärung |
| `{{IMPRESSUM_URL}}` | beide – Footer | Link zum Impressum |
| `{{OG_IMAGE_URL}}` | beide – `<head>` `og:image` | Social-Sharing-Bild (empf. 1200×630) |
| `{{FAVICON_URL}}` | beide – `<head>` `<link rel="icon">` | Favicon |
| `{{ANZAHL}}` | `index.html` – **auskommentiert** | Nur einsetzen, wenn verifiziert (siehe unten) |

### Sozialer Beweis (`{{ANZAHL}}`) — bewusst deaktiviert
In `index.html` liegt eine optionale Zeile *„Über {{ANZAHL}} Menschen auf der Warteliste"* als **HTML-Kommentar**.
Erst einkommentieren **und** echte Zahl eintragen, wenn die Zahl real ist (Harte Regel 4: keine erfundenen Zahlen). Sonst gelöscht lassen.

---

## 2. Design-System (echte Marke moni.goes.wild)

Farben & Fonts sind von **monigoeswild.de** übernommen (extrahiert via Firecrawl, siehe `.firecrawl/`).
Brand-Tokens stehen oben in jeder Datei im `:root`-Block — **identisch in beiden Dateien halten:**

```css
--bg:          #ffffff;   /* warmes Weiß */
--text:        #33302e;   /* Body, hoher Kontrast */
--accent:      #8f6e60;   /* Mokka · Markenakzent (echte Brand-Farbe) */
--accent-soft: #efe0d8;   /* zartes Rosé für Flächen */
--cta:         #6f5247;   /* Button-Fläche */
--cta-text:    #ffffff;   /* Button-Text */
```

**Wichtig (Barrierefreiheit):** Die Original-Markenfarbe `#8F6E60` erreicht mit weißer Schrift nur ~3,3:1
und fällt durch WCAG AA. Deshalb ist `--cta` ein leicht tieferes Mokka `#6F5247` (≥ 4,5:1).
Das Rosé `#B79485` ist **nur dekorativ** (Linien, Akzente) — nie als Fließtext (Kontrast zu niedrig).

**Fonts** (Google Fonts):
- **Cormorant Garamond** – Headlines (warm-elegant, „leicht magisch")
- **Open Sans** – Fließtext (Ramonas echte Website-Font)
- **Caveat** – handschriftliche Akzente (Marken-Wordmark, Signatur)

Markenzeichen ist der **Fuchs 🦊** (keine Pfötchen). Benefit-Icons sind saubere Inline-SVGs (kein Emoji).
Stil-Richtung: *Organic Biophilic* — weiche Rundungen, organische Blob-Formen, viel Weißraum, zentriert.

---

## 3. Eingebaute Constraints (Harte Regeln — bitte beim Editieren nicht brechen)

1. **Keine Geld-zurück-Garantie / keine Rückgabe** — taucht nur im FAQ-Wortlaut auf („Nein – aus Prinzip …"). Nirgends sonst Erstattung andeuten.
2. **Kein Community-Zugang im Produkt** — die WhatsApp-Community steht ausschließlich als separater PS-Bonus, klar getrennt vom Kursinhalt.
3. **Kein Gendering** — auch in allen Verbindungstexten/Microcopy bewusst vermieden.
4. **Keine erfundenen Zahlen / Testimonials / Fake-Scarcity** — es gibt keine Testimonial-Sektion; jede Zahl ist Platzhalter.
5. **DSGVO** — Opt-in hat Consent-Checkbox (`required`) + Link zur Datenschutzerklärung; Footer verlinkt Impressum + Datenschutz.

Gesamte Copy ist **wortgetreu** aus dem Brief übernommen (Ramonas Stimme erhalten).

---

## 4. Technik & Verhalten

- **Mobile-first**, responsive, zentriertes Layout (Markenpräferenz), viel Weißraum.
- **Motion** (Vanilla JS, keine Libs): dezente Scroll-Reveals via `IntersectionObserver`, weicher Hero-Reveal, **Sticky-CTA** unten auf Mobile (≤ 720 px). `prefers-reduced-motion` wird respektiert (Animationen + Smooth-Scroll aus).
- `lang="de"`, sauberes `<head>` mit Title, Meta-Description, Open-Graph-Tags, Favicon.
- Keine externen Abhängigkeiten außer Google Fonts.

---

## 5. Deploy (Vercel)

Rein statisch — kein Build nötig.

**Variante A — Vercel CLI:**
```bash
cd mgw-starterkurs
vercel        # Preview
vercel --prod # Live
```
Bei „Build Command?" leer lassen, „Output Directory?" = `./`.

**Variante B — Vercel Dashboard:** Ordner per Drag-&-Drop hochladen oder Git-Repo verbinden. Framework Preset: **Other**, kein Build Command.

**Erreichbar nach Deploy:**
- Opt-in: `https://<domain>/`  (`index.html`)
- Sales: `https://<domain>/kurs`  (`kurs.html`)

> Funktioniert genauso auf **GitHub Pages** oder jedem Static-Host — einfach beide HTML-Dateien hochladen.

### Vor dem Live-Gang prüfen
- [ ] Alle `{{...}}` ersetzt (`grep -rn "{{" .` liefert nichts mehr)
- [ ] Opt-in-Formular sendet an echten Endpoint, Test-Submit kommt an
- [ ] Checkout-Link öffnet das 97-€-Produkt
- [ ] Impressum / Datenschutz / WhatsApp-Link gesetzt
- [ ] OG-Image + Favicon hinterlegt
