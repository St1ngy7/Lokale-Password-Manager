# Lokale-Password-Manager
Lokaler Passwortmanager zum Verwalten von Zugangsdaten. Sei nicht mehr abhängig von Firmen und ABOS


Das Projekt

Password Manager speichert deine Zugangsdaten in einem verschlüsselten Tresor auf deinem Windows-Rechner. Du entsperrst ihn mit deinem Master-Passwort und verwaltest deine Einträge in einer übersichtlichen Desktop-Oberfläche.

Die Anwendung benötigt kein Online-Konto, keinen Server und keine Internetverbindung. Sie enthält keine Telemetrie. Der Dark Mode ist standardmäßig aktiv, zum Light Mode wechselst du per Button.

## Funktionen

| Funktion | Was du damit machen kannst |
| --- | --- |
| **Verschlüsselter Tresor** | Titel, Benutzernamen, Passwörter, Websites und Notizen gemeinsam verschlüsselt speichern. |
| **Schnelle Suche** | Einträge nach Titel, Benutzername oder Website finden. |
| **Passwortgenerator** | Zufällige Passwörter mit 24 Zeichen erzeugen. |
| **CSV-Import** | Vorhandene Zugangsdaten übernehmen, Spalten zuordnen und identische Duplikate überspringen. |
| **Backups** | Verschlüsselte Sicherungen erstellen und mit dem zugehörigen Master-Passwort laden. |
| **Automatische Sicherung** | Vor einer Wiederherstellung den bisherigen Tresor als verschlüsselte Datei behalten. |
| **Automatische Sperre** | Den Tresor nach fünf Minuten Inaktivität in der Anwendung sperren. |
| **Browserhilfe** | Benutzername oder Passwort per Button in ein ausgewähltes Browserfeld einfügen. |
| **Dark und Light Mode** | Zwischen dunkler und heller Darstellung wechseln. |
| **Einträge löschen** | Einzelne Einträge oder nach Eingabe von `LÖSCHEN` den gesamten Tresorinhalt entfernen. |

## Erster Start

1. `Password Manager.exe` starten.
2. Ein Master-Passwort festlegen. Mindestens sieben Zeichen sind erforderlich; eine lange, zufällige Passphrase ist empfehlenswert.
3. Den ersten Eintrag erstellen oder vorhandene Zugangsdaten über **CSV importieren** übernehmen.
4. Über **Backup speichern** regelmäßig eine verschlüsselte Sicherung anlegen.

Das Master-Passwort lässt sich nicht zurücksetzen. Bei einem Update zuerst die laufende Anwendung schließen. Der vorhandene Tresor bleibt am selben Speicherort.

## Browserhilfe ohne Erweiterung

Bei entsperrtem Tresor erscheint eine schwebende Auswahl, wenn ein unterstützter Browser aktiv ist. Unterstützt werden Chrome, Edge, Firefox, Brave, Opera und Vivaldi.

1. Das gewünschte Eingabefeld im Browser anklicken.
2. In der Browserhilfe den passenden Zugang auswählen.
3. **Benutzername einfügen** oder **Passwort einfügen** anklicken.

Die Anwendung fügt das gewählte Feld als Tastatureingabe ein. **Strg + Alt + L** öffnet die Auswahl als zusätzliche Möglichkeit. Die Anmeldung sendest du selbst ab.

## Technik im Überblick

| Bereich | Umsetzung |
| --- | --- |
| Desktop-Anwendung | Python mit Tkinter / ttk |
| Verschlüsselung | AES-256-GCM über `cryptography` |
| Schlüsselableitung | scrypt mit zufälligem Salt, N=131072, r=8, p=1 |
| Speicherung | Lokale Tresordatei mit atomarem Austausch beim Speichern |
| Browserhilfe | Windows-APIs, ohne Browser-Erweiterung und ohne Zwischenablage |
| Windows-EXE | PyInstaller mit integriertem Python und Programmsymbol |
| Herstellerangabe | Stingy |

**Projektstatus:** Erste Version ohne unabhängiges Sicherheitsaudit. Die automatisierten Tests prüfen ausgewählte Funktionen und Fehlerfälle; sie ersetzen kein Sicherheitsaudit.

## Voraussetzungen

Windows 10/11, Python 3.12 (64 Bit) mit Tkinter. Entwickelt und geprüft unter Windows 11. Python sollte über `py` erreichbar sein. Die Installation der Abhängigkeiten benötigt Internet; die Anwendung selbst hat keine Netzwerkfunktionen.

## Aus Quellcode starten

Im Projektordner in PowerShell:

```powershell
py -3.12 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe app.py
```

## EXE erstellen

```powershell
.\.venv\Scripts\python.exe -m pip install -r requirements-build.txt
.\.venv\Scripts\python.exe build.py
```

Ergebnis: `dist/Password Manager.exe`. Python, Tkinter, Kryptografie-Bibliothek, Icon und Versionsinformationen werden gebündelt. Die EXE trägt die Herstellerangabe „Stingy“; sie ist nicht digital signiert. Der Build muss unter Windows erfolgen.

## Tests

```powershell
.\.venv\Scripts\python.exe -m unittest discover -v
```

Die Oberflächentests benötigen eine Windows-Desktopsitzung. Es werden ausschließlich temporäre Testtresore mit künstlichen Zugangsdaten verwendet.

Optionaler Selbsttest der gebauten EXE:

```powershell
Start-Process -FilePath '.\dist\Password Manager.exe' -ArgumentList '--self-test', 'selftest.json' -Wait
Get-Content selftest.json
```

## Daten und Sicherheit

Der Tresor liegt aus Kompatibilitätsgründen weiterhin in `%LOCALAPPDATA%\LokalTresor\tresor.ltv`. Ein Update oder Wechsel auf die EXE verwendet den bestehenden Tresor. Die vollständige Bedienung ist in [HANDBUCH.md](HANDBUCH.md) beschrieben.

Dies ist eine erste Version ohne unabhängiges Sicherheitsaudit. Die Browserhilfe prüft keine Website-URL und keine Formularfelder; der Benutzer muss die Website und das Eingabefeld vor dem Einfügen selbst prüfen. Python garantiert keine vollständige Löschung sensibler Daten aus dem Arbeitsspeicher. Zwischenablageverlauf und Synchronisierung können kopierte Passwörter behalten. Das Master-Passwort kann nicht zurückgesetzt werden.

## Feedback und Beiträge

Fehlerberichte und Verbesserungsvorschläge sind willkommen. Beschreibe bei einem Fehler deine Windows-Version, die verwendete Programmversion und die Schritte zum Nachstellen. Verwende für Beispiele ausschließlich erfundene Zugangsdaten.

Bitte keine Passwörter, Tresordateien oder unverschlüsselten CSV-Exporte in Issues und Pull Requests hochladen.

## Lizenz

Das Produkt ist unter einer freien Lizenz veröffentlicht. Alle Design-Dateien und Quellcodes stehen offen zur Verfügung. Wir laden Entwickler, Bastler und Maker herzlich dazu ein, das System zu forken, zu modifizieren und eigene Erweiterungen beizutragen.

## Projektstruktur

| Datei | Aufgabe |
| --- | --- |
| `app.py` | Desktop-Oberfläche und Aktionen |
| `vault.py` | Verschlüsselung, Speicherung und Wiederherstellung |
| `browser_fill.py` | Windows-Browserhilfe und Backup-Tastenkombination |
| `csv_import.py` | CSV-Verarbeitung und Spaltenzuordnung |
| `divider_tree.py` | Trennlinien der Eintragsliste |
| `exe_entry.py` | EXE-Einstiegspunkt und Selbsttest |
| `build.py` | Portabler Windows-Build |
| `version_info.txt` | Versions- und Herstellerangaben |
| `test_*.py` | Automatisierte Tests |
