# Anleitung: Website bearbeiten

Die Website wird aus einfachen Textdateien (YAML) im Ordner `_data/` generiert.
Du brauchst **kein HTML** zu schreiben — nur die YAML-Dateien bearbeiten.

---

## Inhalte bearbeiten (auf GitHub)

1. Gehe zu https://github.com/[DEIN-USERNAME]/ninakunz
2. Navigiere in den Ordner `_data/`
3. Klicke auf die Datei, die du bearbeiten willst (z.B. `auftritte.yml`)
4. Klicke auf das **Stift-Symbol** (oben rechts) zum Bearbeiten
5. Nimm deine Änderungen vor
6. Klicke unten auf **«Commit changes»**
7. Die Website wird automatisch in ca. 1 Minute aktualisiert

---

## Dateien im Ordner `_data/`

| Datei              | Inhalt                              |
| ------------------ | ----------------------------------- |
| `auftritte.yml`    | TV-Auftritte, Festivals etc.        |
| `moderation.yml`   | Moderationen + Fotos                |
| `lesungen.yml`     | Lesungen + Fotos                    |
| `texte.yml`        | Links zu Publikationen              |
| `termine.yml`      | Termin-Hinweis                      |
| `buch.yml`         | Buchdetails                         |
| `bio.yml`          | Biographie-Text + Foto             |
| `kontakt.yml`      | Kontakt-Links                       |

---

## Einen neuen Eintrag hinzufügen

### Auftritte (mit Link)

Öffne `_data/auftritte.yml` und füge **oben** (nach den Kommentaren) einen neuen Eintrag ein:

```yaml
- datum: "15.3.2025"
  text: "Sendung XY, SRF"
  link: "https://www.srf.ch/..."
  linktext: "srf.ch"
```

### Auftritte (ohne Link)

```yaml
- datum: "15.3.2025"
  text: "Festival-Name, Ort"
```

### Moderation

Öffne `_data/moderation.yml` und füge unter `eintraege:` oben einen neuen Eintrag ein:

```yaml
  - datum: "15.3.2025"
    text: "Autor:in, «Buchtitel», Veranstaltungsort"
```

### Lesungen

Öffne `_data/lesungen.yml` und füge unter `eintraege:` oben einen neuen Eintrag ein:

```yaml
  - datum: "15.3.2025"
    text: "Venue, Stadt (Land)"
```

### Texte

Öffne `_data/texte.yml`:

```yaml
- text: "Neuer Publikationsname"
  link: "https://..."
```

### Kontakt

Öffne `_data/kontakt.yml`:

```yaml
- text: "Bluesky"
  link: "https://bsky.app/profile/..."
```

---

## Ein neues Foto hinzufügen (Moderation/Lesungen)

1. Lade das Foto in den `images/`-Ordner hoch (auf GitHub: «Add file» → «Upload files»)
2. Öffne die entsprechende YAML-Datei (z.B. `_data/moderation.yml`)
3. Füge unter `fotos:` einen neuen Eintrag ein:

```yaml
  - bild: "images/mein-neues-foto.jpg"
    beschriftung: "Beschreibung. Foto: Fotograf:in"
```

---

## Eine komplett neue Sektion hinzufügen

Das erfordert zwei Schritte:

### Schritt 1: Neue Datendatei erstellen

Erstelle eine neue Datei im Ordner `_data/`, z.B. `_data/podcasts.yml`:

```yaml
# =============================================
# PODCASTS
# Neue Einträge OBEN anfügen.
# =============================================

- datum: "1.1.2025"
  text: "Podcast-Name, Folge 123"
  link: "https://..."
  linktext: "Anhören"
```

### Schritt 2: Template anpassen (index.html)

Öffne `index.html` und füge an der gewünschten Stelle folgenden Block ein
(z.B. vor dem Kontakt-Block):

```html
         <div class="twelve columns">
            <h3 id="podcasts">Podcasts</h3>
           <p>{%- for p in site.data.podcasts -%}
{{ p.datum }}, {{ p.text }}{% if p.link %} <a href="{{ p.link }}">{{ p.linktext }}</a>{% endif %}{%- unless forloop.last %}<br>
{% endunless -%}
{%- endfor %}</p>
               </div>
```

Und füge in der Navigation (oben in der Datei, bei den `<a href="#...">` Links)
einen neuen Link hinzu:

```html
<a href="#podcasts">Podcasts</a>
```

---

## YAML-Regeln (wichtig!)

- **Einrückung:** Immer **Leerzeichen** verwenden, **keine Tabs**
- **Bindestriche:** Jeder Listeneintrag beginnt mit `- ` (Bindestrich + Leerzeichen)
- **Anführungszeichen:** Text mit Sonderzeichen (`«`, `»`, `:`) immer in `"..."` setzen
- **Doppelpunkt:** Nach `datum:`, `text:`, `link:` etc. immer ein Leerzeichen
- **Reihenfolge:** Neue Einträge immer **oben** einfügen (neueste zuerst)

### Beispiel — richtig:

```yaml
- datum: "15.3.2025"
  text: "Veranstaltung, Ort"
```

### Beispiel — falsch:

```yaml
-datum:"15.3.2025"     # Leerzeichen fehlen
  text:Veranstaltung   # Doppelpunkt ohne Leerzeichen
```

---

## Lokal testen (optional)

Um die Seite lokal auf deinem Computer zu testen, brauchst du Ruby (Version 3+).

### Einmalig einrichten (macOS)

```bash
# Homebrew installieren (falls noch nicht vorhanden)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Ruby installieren
brew install ruby

# Gems installieren (im Projektordner)
cd pfad/zu/ninakunz
bundle install
```

### Seite starten

```bash
cd pfad/zu/ninakunz
bundle exec jekyll serve
```

Dann im Browser öffnen: http://localhost:4000

Die Seite aktualisiert sich automatisch, wenn du Dateien änderst.

### Ohne Ruby (schnelle Vorschau via GitHub)

1. Erstelle einen neuen Branch auf GitHub
2. Mache deine Änderungen dort
3. Gehe zu **Settings → Pages** und wähle den neuen Branch als Quelle
4. Vorschau prüfen, dann Branch in `main` mergen
