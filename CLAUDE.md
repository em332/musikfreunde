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
├── obfrau.html         ← Profil Dr. Anita Einsle (Obfrau)
├── mitmachen.html      ← Einladung neue Mitglieder, Probenplan
├── archiv.html         ← Vergangene Konzerte
├── presse.html         ← Pressestimmen (mit PDF-Links zur alten Site)
├── kontakt.html        ← Kontaktinformationen
├── impressum.html      ← Impressum (§ 25 MedienG, § 5 ECG)
├── datenschutz.html    ← Datenschutzerklärung (DSGVO)
├── css/
│   └── style.css       ← Einziges Stylesheet, alle Komponenten
├── img/
│   └── notenschluessel.png  ← Vereins-Notenschlüssel, 600×1000px, transparenter Hintergrund
└── vorlagen/
    ├── Inhalt Homepage.pdf                       ← Inhaltsanforderungen
    ├── Folder_frühjahr26_Musikfreunde_klein.pdf  ← CI-Referenz (Folder Frühjahr 2026)
    ├── Unbenannt-292.eps                         ← Originalquelle Notenschlüssel (EPS/PostScript)
    ├── notenschluessel_transparent.png           ← Zwischenschritt (Originalausrichtung, Spirale oben)
    └── anita-einsle.png                          ← Foto Dr. Anita Einsle (lokal)
```

Kein Build-Schritt, kein Framework. Seiten direkt im Browser öffnen.

## Wichtige Architektur-Hinweise

**Nav und Footer sind in jeder HTML-Datei manuell dupliziert** – es gibt kein Templating. Wenn Navigation oder Footer geändert werden, müssen alle 11 HTML-Dateien aktualisiert werden. Das `active`-Attribut im Nav (`class="active"`) ist jeweils auf den Link der aktuellen Seite gesetzt.

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

| Variable     | Wert              | Verwendung                                        |
|--------------|-------------------|---------------------------------------------------|
| `--red`      | `#B34938`         | Akzent: Eyebrows, CTAs, Dekorationen (Terrakotta, aus Folder-CI extrahiert) |
| `--red-dark` | `#8A3229`         | Hover-Zustand, dunklerer Akzent                   |
| `--text`     | `#1C1C1C`         | Fliesstext                                        |
| `--muted`    | `#6A6A6A`         | Sekundärtext, Labels                              |
| `--bg-warm`  | `#F7F6F2`         | Sektionshintergründe, Karten                      |
| `--bg-dark`  | `#141414`         | Hero-Sektionen, Footer                            |
| `--serif`    | Playfair Display  | Überschriften, Werktitel (kursiv)                 |
| `--sans`     | Inter             | Fliesstext, Labels, Navigation                    |

**Notenschlüssel-Dekorationen:**
- Nav-Logo: `<img src="img/notenschluessel.png" class="nav__logo__clef">` (Höhe 34px, `height: 34px; width: auto`)
- Startseite Hero: `.hero__deco img` (Höhe 36rem, `opacity: 0.14; filter: invert(1)` → weiss auf dunkel)
- Unterseiten page-hero: `.page-hero__deco img` (Höhe 16rem, `opacity: 0.18; filter: invert(1)` → weiss auf dunkel)

**Notenschlüssel-Ausrichtung:** Spirale (Kugel) unten links, Stiel geht nach oben, Dach verläuft von links oben nach rechts unten. Die Quelldatei `vorlagen/notenschluessel_transparent.png` hat Spirale oben – für `img/notenschluessel.png` wurde `FLIP_TOP_BOTTOM` (vertikaler Spiegelung, kein 180°-Drehen) angewendet.

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
| Gegründet | 1907 |
| Mitgliedschaft | Europäische Orchester Vereinigung (EOV), seit Juni 2006 |
| Einzugsgebiet | Bregenz, Vorarlberg, Lindau (D) |
| Obfrau | Dr. Anita Einsle, M.B.L. · Tel. 05574/54447 · anita@einsle.at |
| Dirigent | Hansjörg Gruber (seit 2002) |
| Konzertmeister | Thomas Furrer (geb. 1957, Wetzikon/CH) |
| Frühjahrskonzert | 9. Mai 2026, 19:30 Uhr, Theater am Kornmarkt, Bregenz |
| Programm | Mozart: Ouv. Don Giovanni KV 527 · Mozart: Klavierkonzert KV 414 · Beethoven: Sinfonie Nr. 6 |
| Solist | Tobias Jacob (Klavier) — **Schreibung: ein b**, so im offiziellen Konzertfolder |
| Herbstkonzert | 7. November 2026, Pfarrkirche St. Gallus, Bregenz |
| Probenlokal | Volksschule Augasse (Untergeschoss), Eingang Klostergasse, Bregenz |
| Probenzeit | Montags 19:45–22:00 Uhr (Saison Feb–Juni) |
| Förderer | Stadt Bregenz · Land Vorarlberg · Hypo Vorarlberg · Burtscher Bau · RA Dr. Einsle |
| Pressedienst | Vorarlberger Nachrichten (VN) · Zeitschrift Kultur |
| Fotografie | © Martin Ender |

## Fotos (externe URLs, alte Website)

Fotos von der alten Homepage werden direkt per URL eingebunden (für Prototyp ausreichend, vor Go-live lokal hosten):
- Hansjörg Gruber: `http://www.musikfreunde-bregenz.at/wp-content/uploads/2009/02/gruber_41.jpg`
- Thomas Furrer: `http://www.musikfreunde-bregenz.at/wp-content/uploads/2009/02/furrer1.jpg`
- Dr. Anita Einsle: `vorlagen/anita-einsle.png` (lokal)

## Pressestimmen (PDF-Links)

Alle Berichte liegen als PDF vor und werden direkt von der alten Site verlinkt:
- VN 10.11.2025 – `.../uploads/2025/11/uber-dem-nebelmeer.pdf`
- Kultur 2025 – `.../uploads/2025/11/aus-freude-an-der-musik.pdf`
- VN 16.11.2024 – `.../uploads/2024/11/konzertkritik-vn-24-11-16.pdf`
- VN 18.11.2019 – `.../uploads/2019/11/kritik-vn-2019-11-18.pdf`
- VN 14.05.2019 – `.../uploads/2019/05/kritik-vn-2019-05-14.pdf`
- VN 20.11.2018 – `.../uploads/2018/11/kritik-vn-2018-11-20.pdf`
- VN 18.06.2018 – `.../uploads/2018/06/kritik-vn-2018-06-18.pdf`
- VN 20.11.2017 – `.../uploads/2017/11/vn-2017-11-20.pdf`
- VN 15.05.2017 – `.../uploads/2017/05/vn-2017-05-15.pdf`

## Erledigtes (Stand April 2026)

- Vollständiger Prototyp: 11 HTML-Seiten + CSS erstellt
- Vereinsdaten eingebaut: Gründungsjahr 1907, ZVR, EOV-Mitgliedschaft seit 2006
- Obfrau: Dr. Anita Einsle vollständig eingetragen (Tel., E-Mail, Adresse, Foto lokal)
- Dirigent Hansjörg Gruber: Biografie + Foto (externe URL) eingebunden
- Konzertmeister Thomas Furrer: Biografie + Foto (externe URL) eingebunden
- Frühjahrskonzert 2026: Datum, Uhrzeit, Ort, Programm vollständig
- Solist **Tobias Jacob**: Biografie (3 Absätze, aus Konzertfolder extrahiert) eingebaut
  — Schreibung «Jacob» (ein b) aus offiziellem Folder übernommen; in `konzert.html` und `index.html` korrigiert
- Probenplan Frühjahr 2026 vollständig (Montags 19:45–22:00, VS Augasse)
- Streicherlisten vollständig (1.+2. Violine, Viola, Cello)
- 9 Presseartikel mit direkten PDF-Links eingebunden (VN 2017–2025, Zeitschrift Kultur)
- Förderer: Stadt Bregenz, Land Vorarlberg, Hypo Vorarlberg, Burtscher Bau, RA Dr. Einsle
- Herbstkonzert-Vorschau: 7. November 2026, Pfarrkirche St. Gallus
- Datenschutzerklärung (DSGVO) und Impressum vollständig
- Gründungsjahr-Platzhalter in `index.html` (Hero) mit «1907» befüllt
- **Notenschlüssel** (`img/notenschluessel.png`) aus EPS-Quelldatei extrahiert und in alle 11 Seiten integriert:
  Nav-Logo, Hero (Startseite, 36rem weiss), page-hero (Unterseiten, 16rem weiss)
- **CI-Farbe** auf Basis Folder Frühjahr 2026 angepasst: `--red` von `#B5192E` → `#B34938` (warmes Terrakotta)
- **Nav CTA „Konzert 2026"**: `white-space: nowrap` verhindert Zeilenumbruch im Button
- **Platzhalter in Buttons**: `.btn .ph { color: #fff; }` — dunkle Platzhaltertexte auf rotem Hintergrund lesbar

## Offene Punkte vor Go-live

**Vom Vorstand einzuholen:**
- Übrige Vorstandsmitglieder (Namen + Funktionen – Obfrau Dr. Einsle ist bereits eingetragen)
- Mitgliedsbeitrag (regulär + ermässigt)
- Welche Instrumente/Stimmen werden aktuell gesucht (`mitmachen.html`)
- Fehlende Register-Namen: Kontrabass, Holzbläser, Blechbläser, Schlagwerk (`orchester.html`)
- Instagram- und Facebook-URLs (alle Seiten, Footer)
- Vereinspostadresse (falls abweichend von Kanzlei Deuringstraße 9)

**Für die Konzertseite:**
- Ticketpreise + Direktlink zum Tourismusamt Bregenz
- Foto Tobias Jacob (solist-card__img in `konzert.html` ist noch Platzhalter)

**Inhalte ergänzen:**
- Vereinsgeschichte: Detailtext für `verein.html` (Abschnitt «Weitere Vereinsgeschichte»)
- Archiv: Konzertprogramme vor November 2025 in `archiv.html` eintragen
- Pressestimmen: Archiveinträge vor 2017 ergänzen (falls vorhanden)

**Technisch:**
- Newsletter-Formular in `index.html` durch echtes Backend ersetzen (z. B. Mailchimp Embedded Form → dann auch `datenschutz.html` Abschnitt 5 ergänzen)
- Fotos von alter Website lokal hosten (statt externer URLs auf musikfreunde-bregenz.at)
- Google Fonts optional lokal hosten (für Datenschutz + Offline-Robustheit, → `datenschutz.html` Abschnitt 4 anpassen)
- Weitere Orchesterfotos (© Martin Ender) einbinden
- Impressum: Fotocredit ergänzen
- Sponsoren-Links: echte URLs eintragen
- Social-Media-Links: echte URLs eintragen

## Quellmaterialien

- `vorlagen/Inhalt Homepage.pdf` — Inhaltliche Anforderungsliste (8 Seitenbereiche)
- `vorlagen/Folder_frühjahr26_Musikfreunde_klein.pdf` — Folder Frühjahr 2026 (CI-Referenz: Weissraum, roter Akzent, elegante Seriftypografie)
