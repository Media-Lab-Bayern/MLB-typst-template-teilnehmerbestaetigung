# Teilnahmebescheinigungen

Automatische Generierung von Teilnahmebescheinigungen basierend auf einer Teilnehmerliste. Der Ordner ist eigenständig und kann überall liegen.

## 📁 Ordnerstruktur

```text
├── README.md                           # Diese Datei
├── main.typ                            # Haupttemplate
├── teilnehmende.csv                    # Teilnehmer-Datenquelle
├── assets/                             # Logos und Grafiken
│   └── MLB_Logo_2 lines-Brand-RGB.svg  # Media Lab Bayern Logo
└── template/
    └── vendor/
        └── medialab-bescheinigung.typ  # Wiederverwendbares Template-Modul
```

## 🚀 Schnelleinstieg

### 1. VS Code + Tinymist (Einfach)

```bash
# 1. Ordner in VS Code öffnen
code .

# 2. main.typ öffnen und mit Tinymist-Preview arbeiten
# → Klick auf "Export PDF" in der Preview
```

### 2. Inhalte anpassen

Öffnen Sie `main.typ` und passen Sie diese Bereiche an:

```typst
#let template = (
  titel: "Teilnahmebestätigung",           # Dokumenttitel
  veranstaltung: "Strategie Offsite",      # Veranstaltungsname
  "event-subtitle": none,                  # Optional: Untertitel
  
  einleitung: [Das Media Lab Bayern...],   # Intro-Text
  teilnahme: [hat vom 16. bis 18.11.2026 am Strategie Offsite...],  # Zeitraum & Ort
  beschreibung: [Format-Beschreibung...],  # Detaillierte Infos
  
  liste: (
    titel: "Thematische Schwerpunkte",
    text: [
      - Punkt 1
      - Punkt 2
    ]
  ),
  
  ort: [München],                          # Ausstellungsort
  
  unterschrift: (
    name: "Phil Scholz",                   # Unterschrift Name
    funktion: "Programm Manager",          # Titel
  ),
)
```

### 3. Teilnehmerliste aktualisieren

Ersetzen Sie `teilnehmende.csv`:

```csv
Vorname;Nachname
Max;Mustermann
Erika;Musterfrau
```

**Anforderungen:**

- Format: CSV mit Semikolon-Trennzeichen (`;`)
- Spalten: `Vorname` und `Nachname`
- Encoding: UTF-8 (in Excel: "CSV UTF-8" speichern)

### 4. PDF exportieren

#### Option A: Kombinierte PDF (alle Zertifikate in einer Datei)

**Mit Tinymist-Preview (Anfänger):**

1. `main.typ` in VS Code öffnen
2. Tinymist-Preview starten (Klick auf Vorschau-Icon)
3. "Export PDF" klicken → `main.pdf`

**Mit Typst CLI:**

```bash
typst compile main.typ main.pdf
```

#### Option B: Einzelne PDFs (eine pro Person)

**Mit Typst CLI:**

Ein einzelnes Zertifikat generieren (z.B. Teilnehmer #1):

```bash
typst compile main.typ cert_1.pdf --input nr=1
```

Alle Zertifikate in einer Schleife erzeugen:

```bash
# macOS/Linux - passt die Zahl nach "in {1..N}" an
for i in {1..2}; do
  typst compile main.typ "cert_$i.pdf" --input nr=$i
done

# PowerShell (Windows)
1..2 | ForEach-Object {
  typst compile main.typ "cert_$_.pdf" --input nr=$_
}
```

Die PDFs werden im aktuellen Verzeichnis gespeichert. Sie können bei Bedarf in einen Ordner verschoben werden.

#### Typst einmalig installieren

```bash
# macOS
brew install typst

# Windows
winget install --id Typst.Typst

# Oder: Manuell von https://github.com/typst/typst/releases herunterladen
```

## 🎨 Logo und Unterschrift

### Logo

Das aktuelle Logo ist `assets/MLB_Logo_2 lines-Brand-RGB.svg`.

Um ein anderes Logo zu verwenden, bearbeiten Sie in `template/vendor/medialab-bescheinigung.typ` diese Zeile:

```typst
#let logo = "../../assets/MLB_Logo_2 lines-Brand-RGB.svg"
```

Ersetzen Sie den Pfad durch die neue Logodatei (SVG oder PNG):

```typst
#let logo = "../../assets/MLB_Logo_3 lines-Brand-CMYK.png"
```

### Unterschrift

Das Template hat folgende Unterschrift-Felder:

```typst
unterschrift: (
  name: "Phil Scholz",                   # Unterschrift Name
  funktion: "Programm Manager",          # Titel
),
```

Passen Sie Name und Funktion in der `template`-Variablen in `main.typ` an.

## 📋 Template-Modul

Das wiederverwendbare Template wird aus `template/vendor/medialab-bescheinigung.typ` importiert:

```typst
#import "template/vendor/medialab-bescheinigung.typ": bescheinigung
```

Kann für andere Projekte kopiert werden.

## ⚙️ Anforderungen

- **Typst**: v0.9+ (für Vorschau und PDF-Export)
- **Text-Editor**: VS Code mit Tinymist-Extension (empfohlen)
- **Daten**: UTF-8 CSV mit `Vorname;Nachname`-Spalten

## 📝 Lizenz & Nutzung

Erstellt für das **Media Lab Bayern**. Template ist modular und kann für andere Zertifikate/Bescheinigungen angepasst werden.

---

**Zuletzt aktualisiert**: November 2026  
**Aktuelle Event**: Strategie Offsite (16.–18.11.2026, Würzburg)
