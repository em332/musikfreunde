# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Statische Website für die **Gesellschaft der Musikfreunde Bregenz** (ZVR 399280347), ein klassischer Orchesterverein in Bregenz, Vorarlberg, Österreich. Die Website wird ca. zweimal pro Jahr aktualisiert (Frühjahrs- und Herbstkonzert). Alle Seiteninhalte sind auf **Deutsch**.

## Dateistruktur

```
musikfreunde/
├── index.html          ← Startseite
├── konzert.html        ← Aktuelles Konzert (wichtigster Update-Zielpunkt)
├── verein.html         ← Vereinsgeschichte, Vorstand, Sponsoren
├── orchester.html      ← Orchesterprofil, Register, Musikerporträts
├── mitmachen.html      ← Einladung neue Mitglieder, Probenplan
├── archiv.html         ← Vergangene Konzerte
├── presse.html         ← Pressestimmen
├── kontakt.html        ← Kontaktinformationen
├── css/
│   └── style.css       ← Einziges Stylesheet, alle Komponenten
└── vorlagen/
    ├── Inhalt Homepage.pdf                       ← Inhaltsanforderungen
    └── Folder_frühjahr26_Musikfreunde_klein.pdf  ← CI-Referenz (Folder Frühjahr 2026)
```

Kein Build-Schritt, kein Framework. Seiten direkt im Browser öffnen.

## Wichtige Architektur-Hinweise

**Nav und Footer sind in jeder HTML-Datei manuell dupliziert** – es gibt kein Templating. Wenn Navigation oder Footer geändert werden, müssen alle 8 HTML-Dateien aktualisiert werden. Das `active`-Attribut im Nav (`class="active"`) ist jeweils auf den Link der aktuellen Seite gesetzt.

**Google Fonts-Abhängigkeit:** `css/style.css` lädt Playfair Display und Inter via `@import url('https://fonts.googleapis.com/...')`. Ohne Internetverbindung fallen die Seiten auf System-Schriften zurück (Georgia / -apple-system). Für den Produktionsbetrieb empfiehlt sich ein lokales Font-Hosting.

**JavaScript:** Nur ein 5-Zeilen Hamburger-Toggle am Ende jeder HTML-Datei (inline `<script>`). Kein externer JS-Code.

## Konzertdaten aktualisieren (2× pro Jahr)

Alle variablen Konzertdaten stehen in `konzert.html`. Ein deutlich markierter Kommentarblock (`█ KONZERTDATEN — HIER AKTUALISIEREN`) am Dateianfang listet alle zu ändernden Felder:

- Datum, Uhrzeit, Veranstaltungsort
- Programm (Komponisten, Werke, KV-/Op.-Nummern)
- Solist(en): Name, Rolle, Biografie, Foto
- Dirigent: Name, Biografie, Foto
- Ticketpreise + Link zum Tourismusamt Bregenz
- Nächstes Konzert (Datum, Ort, Programmvorschau)

Nach dem Update von `konzert.html` sind auch folgende Stellen in `index.html` anzupassen:
- `.concert-teaser__date` (Datum im Teaser)
- `.concert-teaser__venue` (Veranstaltungsort)
- `.concert-teaser__title` (Konzertname)
- `.prog-list` (Programm-Kurzliste)
- `.hero__badge` (Datum im Hero)

## Design-System

Definiert als CSS Custom Properties in `css/style.css`:

| Variable    | Wert              | Verwendung                             |
|-------------|-------------------|----------------------------------------|
| `--red`     | `#B5192E`         | Akzent: Eyebrows, CTAs, Dekorationen   |
| `--text`    | `#1C1C1C`         | Fliesstext                             |
| `--muted`   | `#6A6A6A`         | Sekundärtext, Labels                   |
| `--bg-warm` | `#F7F6F2`         | Sektionshintergründe, Karten           |
| `--bg-dark` | `#141414`         | Hero-Sektionen, Footer                 |
| `--serif`   | Playfair Display  | Überschriften, Werktitel (kursiv)      |
| `--sans`    | Inter             | Fliesstext, Labels, Navigation         |

Das Rot wird sparsam eingesetzt (nur bei echten Schwerpunkten). Keine dekorativen Effekte ohne funktionalen Mehrwert.

## Platzhalter-Konvention

Alle noch fehlenden Inhalte sind markiert mit:
```html
<span class="ph">[Beschreibung des fehlenden Inhalts]</span>
```
Die `.ph`-Klasse rendert mit rotem Hintergrundton. Alle offenen Platzhalter finden:
```bash
grep -n 'class="ph"' *.html
```

## Bestätigte Inhaltsdaten

| Feld | Wert |
|------|------|
| Organisation | Gesellschaft der Musikfreunde Bregenz |
| ZVR | 399280347 |
| Mitgliedschaft | Europäische Orchester Vereinigung |
| Frühjahrskonzert | 9. Mai 2026, Theater am Kornmarkt, Bregenz |
| Programm | Mozart: Ouv. Don Giovanni KV 527 · Mozart: Klavierkonzert KV 414 · Beethoven: Sinfonie Nr. 6 «Pastorale» |
| Solist | Tobias Jakob (Klavier) |
| Herbstkonzert | 7. November 2026, Pfarrkirche St. Gallus, Bregenz |
| Letztes Konzert | 8. November 2025 |
| Sponsoren | Hypo Vorarlberg · Burtscher Bau · RA Dr. Einsle |
| Pressemedien | Vorarlberger Nachrichten (VN) · Kulturmagazin Vorarlberg |
| Fotografie | © Martin Ender |

## Offene Punkte vor Go-live

**Vom Vorstand einzuholen:**
- Gründungsjahr und Vereinsgeschichte
- Namen und Funktionen aller Vorstandsmitglieder
- Probenlokal (Adresse, Wochentag, Uhrzeit)
- Mitgliedsbeitrag (regulär + ermässigt)
- Vereinsadresse und offizielle E-Mail-Adressen
- Instagram- und Facebook-URLs

**Für die Konzertseite:**
- Uhrzeit des Konzerts (9. Mai 2026)
- Ticketpreise
- Direktlink zum Tourismusamt Bregenz (Kartenverkauf)
- Biografie Tobias Jakob (vom Künstler oder Veranstalter)
- Dirigent: Name + Biografie + Foto

**Inhalte ergänzen:**
- Musikerprofile: Namen, Instrumente, Fotos (nach Rücksprache mit Mitgliedern)
- Pressezitate: konkrete Zitatstexte aus VN und Kulturmagazin
- Archiv-Einträge: Konzerte vor November 2025

**Technisch:**
- Newsletter-Formular in `index.html` durch echtes Backend ersetzen (z. B. Mailchimp Embedded Form)
- Datenschutz- und Impressumsseiten erstellen (österreichisches Recht)
- Reale Fotos (© Martin Ender) einbinden und Bildpfade setzen
- Google Fonts lokal hosten (optional, für Offline-Robustheit)
- `href="#"` in Sponsoren-Links und Social-Media-Links durch echte URLs ersetzen

## Quellmaterialien

- `vorlagen/Inhalt Homepage.pdf` — Inhaltliche Anforderungsliste (8 Seitenbereiche)
- `vorlagen/Folder_frühjahr26_Musikfreunde_klein.pdf` — Folder Frühjahr 2026 (CI-Referenz: Weissraum, roter Akzent, elegante Seriftypografie)
