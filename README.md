# ESAG Materialien

Größtenteils sind das hier TeX-Dokumente, die für verschiedene
Planungsangelegenheiten der ESAG dienen.

# LaTeX-Quellen übersetzen

Es gibt ein `Rakefile`. Dieses kann auf 2 Weisen genutzt werden.

Übersetze alle .tex-Dateien im Ordner src/ in gleichnamige PDF-Dateien

    rake

Starte preview-Modus mit latexmk für eine bestimmte .tex-Datei

    rake dateinamemitpunktpdfstattpunkttex.pdf

Eine Übersicht über alle Tasks gibt es mit

    rake -T

Alle gebauten Dateien löschen geht mit

    rake clean

# conf.json

Diese Datei enthält Konfigurationen, die für einzelne Dokumente wichtig sind. Im nächsten
Abschnitt wird im Einzelnen darauf eingegangen.

# Dokumente

Hier eine Übersicht aller Dokumente:

## ausgaben.tex

Übersicht über alle Ausgaben

## fuehrungen.tex

Stationen der Campus-Führungen

### Konfiguration

In der `conf.json` kann man einmal die Anzahl der Gruppen und dann die Stationen konfigurieren.

```json
{
  "fuehrungen": {
    "num": 8,
    "sections": [
      {
        "name": "Gebäude 25",
        "stations": [
          { "name": "FS-Info", "hinweise": "Startposition -- An Frau Rennwanz vorbeilaufen beim kommen oder gehen. Frau Rennwanz macht Regen und Feuer!" }
        ]
      }
    ]
  }
}
```

Das Ausführen von `rake fuehrungen.pdf` erzeugt dann alle Führungen in `src/fuehrungen/`, was
nicht versioniert wird. __Zusätzlich__ muss in der Datei `src/fuehrungen.tex` darauf geachtet
werden, alle Führungen mit `\input` einzubinden.

## gruppenzettel_rallye.tex

Die Gruppenzettel der Rallye

## laufzettel_rallye.tex

Die Laufzettel der Rallye

## pruefungsordnung.tex

Die „Prüfungsordnung” für die Ralye

## schnitzeljagd.tex

## spielregeln.tex

## spielregeln_schnitzeljagd.tex

## stationen_rallye.tex

Übersicht über die Rallye-Stationen.

# Arbeit am Repository

Die PDF-Dateien im `build/`-Ordner sollten mit versioniert werden. Dabei sollte
darauf geachtet werden, dass die generierte PDF immer mit der TeX-Datei im
commit übereinstimmt. Die Verwendung von latexmk bzw. das Rakefile hilft dabei.

Sollten andere Tools als das Rakefile zum bauen benutzt werden, so ist darauf
zu achten, die PDFs nicht im Hauptverzeichnis zu versionieren, damit das
Repository einigermaßen sauber bleibt.
