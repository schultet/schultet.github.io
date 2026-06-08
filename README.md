# Mein Blog – Anleitung

Eine statische Blog-Seite ohne Generator: nur HTML, CSS und etwas JavaScript.
Beiträge lassen sich durchsuchen und nach Tags filtern.

## Ordnerstruktur

```
blog/
├── index.html                  ← Startseite mit Beitragsliste + Filter/Tags
├── assets/
│   └── blog.css                ← gemeinsames Design (Farben/Schriften hier ändern)
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

Alle Farben und Schriften stehen ganz oben in `assets/blog.css` unter `:root { … }`.
Eine Variable ändern wirkt auf die ganze Seite. Schriften kommen von Google Fonts
(im `<head>` jeder Seite verlinkt) – tauschbar gegen jede andere Schrift.
