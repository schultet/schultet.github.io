# Append-Only – Anleitung

Eine statische Notizsammlung ohne Generator: nur HTML, CSS und etwas JavaScript.
Einträge lassen sich durchsuchen und nach Tags filtern. Jede Seite darf ihr eigenes
Design haben — die Startseite ist modern (hell/dunkel), die Buch-/Beitragsseiten nutzen
den wärmeren „Papier"-Stil.

## Ordnerstruktur

```
notes/
├── index.html                  ← Startseite: modernes Design (assets/home.css), Liste + Filter
├── impressum.html              ← Impressum (Platzhalter ausfüllen)
├── datenschutz.html            ← Datenschutzerklärung (Platzhalter ausfüllen)
├── assets/
│   ├── home.css                ← Stil NUR der Startseite (Inter, hell/dunkel)
│   ├── blog.css                ← „Papier"-Stil der Beitragsseiten (Fraunces/Newsreader)
│   └── fonts/                  ← selbst gehostete Schriften (.woff2), keine Google-CDN
└── posts/
    ├── _template.html          ← leere Vorlage zum Kopieren
    ├── 2026-06-08-willkommen.html
    ├── 2026-06-05-ddia-gelernt.html
    ├── 2026-05-28-statische-seiten.html
    └── 2026-05-15-btrees-lsm.html
```

## Einen neuen Beitrag hinzufügen (2 Schritte)

**1. Beitragsdatei anlegen.** Kopiere `posts/_template.html` und benenne die Kopie
nach dem Schema `JJJJ-MM-TT-kurztitel.html`, z. B. `posts/2026-07-01-mein-thema.html`.
Trage darin Titel, Datum, Tags und Inhalt ein.

**2. In die Liste eintragen.** Öffne `index.html`, finde oben im `<script>` die
Liste `const posts = [ … ]` und ergänze **ein** Objekt:

```js
{
  title:   "Mein neuer Beitrag",
  date:    "2026-07-01",
  url:     "posts/2026-07-01-mein-thema.html",
  tags:    ["Web", "Tutorial"],   // frei wählbar; gleiche Schreibweise wiederverwenden
  excerpt: "Ein bis zwei Sätze Vorschau."
},
```

Fertig. Die Startseite sortiert automatisch nach Datum (neueste zuerst), sammelt
alle Tags ein und filtert beim Suchen oder Tag-Klick.

> Tipp: Verwende für gleiche Themen immer dieselbe Tag-Schreibweise
> ("Datenbanken", nicht mal "Datenbank", mal "DB"), damit der Filter sauber bündelt.

## Wie der Filter funktioniert

- **Suchfeld**: durchsucht Titel, Vorschau und Tags (Live, beim Tippen).
- **Tags**: Klick aktiviert/deaktiviert einen Tag. Mehrere aktive Tags zeigen nur
  Beiträge, die **alle** davon tragen (UND-Verknüpfung). „Filter zurücksetzen" leert alles.
- Möchtest du stattdessen Beiträge sehen, die **mindestens einen** der Tags haben
  (ODER), ändere in `index.html` die Funktion `matches` entsprechend (Kommentar im Code).

## Lokal ansehen

Doppelklick auf `index.html` genügt – da die Beitragsliste direkt in der Seite steht,
funktioniert alles ohne lokalen Server.

## Auf GitHub Pages veröffentlichen

1. Neues Repository auf GitHub anlegen (z. B. `mein-blog`).
2. Den **Inhalt** des `blog/`-Ordners ins Repo legen (sodass `index.html` im Wurzelverzeichnis liegt) und pushen:
   ```bash
   git init
   git add .
   git commit -m "Erster Commit"
   git branch -M main
   git remote add origin https://github.com/DEIN-NAME/mein-blog.git
   git push -u origin main
   ```
3. Im Repo: **Settings → Pages → Build and deployment → Source: „Deploy from a branch"**,
   Branch `main`, Ordner `/ (root)`, speichern.
4. Nach ein bis zwei Minuten ist die Seite erreichbar unter
   `https://DEIN-NAME.github.io/mein-blog/`.

Eigene Domain möglich über Settings → Pages → Custom domain.

## Design anpassen

Es gibt zwei getrennte Stylesheets, beide mit ihren Design-Variablen ganz oben unter
`:root { … }`:

- **`assets/home.css`** — nur die Startseite. Modernes, schlichtes Design in **Inter**,
  mit automatischem Hell-/Dunkelmodus über `prefers-color-scheme`. Farben/Abstände als
  CSS-Variablen (hell in `:root`, dunkel im `@media (prefers-color-scheme:dark)`-Block).
- **`assets/blog.css`** — die Beitragsseiten im wärmeren „Papier"-Stil
  (**Fraunces**/**Newsreader**). Farben hier ebenfalls oben unter `:root`.

Eine Variable ändern wirkt auf die jeweils zugehörigen Seiten.

## Schriften (selbst gehostet)

Keine Schrift kommt mehr von einer Google-CDN; alle liegen als `.woff2`-Dateien
(Subsets `latin` + `latin-ext`) in `assets/fonts/`. Das vermeidet eine Verbindung zu
Google beim Seitenaufruf (siehe Datenschutz) und macht die Seite offline-fähig. Die
`@font-face`-Regeln stehen jeweils oben im passenden Stylesheet:

- **Startseite:** Inter + JetBrains Mono → in `assets/home.css`.
- **Beitragsseiten:** Fraunces, Newsreader, JetBrains Mono → in `assets/blog.css`.

> Sonderfall: `posts/designing-data-intensive-applications.html` bringt sein eigenes
> `<style>` mit (statt `blog.css` zu verlinken); dort stehen die `@font-face`-Regeln
> direkt im Kopf der Datei (Pfade `../assets/fonts/…`). Wer dort die Schriften wechselt,
> muss diese Stelle separat anpassen.

Schriften wechseln: neue `.woff2`-Dateien in `assets/fonts/` legen und die
`@font-face`-Blöcke (Dateinamen + `font-family`) im jeweiligen Stylesheet anpassen.

## Impressum & Datenschutz

`impressum.html` und `datenschutz.html` liegen im Wurzelverzeichnis und sind im Footer
jeder Seite verlinkt. Beide enthalten **Platzhalter** (rot/gestrichelt hervorgehoben),
die du mit deinen echten Angaben bzw. mit Text aus einem Generator oder von einer
Anwältin / einem Anwalt füllst. Die Vorlagen sind kein Rechtsrat.
