# claude-suche

Findet frühere [Claude Code](https://claude.com/claude-code)-Gespräche per Stichwort — über **alle** Projektordner hinweg — und öffnet sie direkt mit `claude --resume`.

*English: Full-text search across all your Claude Code sessions, regardless of which folder they were started in. Pick a hit with the arrow keys, press Enter, and the session resumes in its original directory. Works on Linux, macOS and Windows.*

## Das Problem

`claude --resume` zeigt nur die Sitzungen des Ordners, in dem man gerade steht. Wer nicht mehr weiß, wo er ein Gespräch gestartet hat, findet es nicht wieder.

Außerdem löscht Claude Code Gespräche standardmäßig nach 30 Tagen ohne Zugriff.

## Die Lösung

```
claude-suche hellgarda
```

```
2 Gespräche zu: hellgarda  (1 davon im Namen)
↑↓ wählen · ←→ Funde · Enter öffnet · e Kette auf/zu · q bricht ab

> 142 Treffer  2026-08-22 bis 2026-09-12
    Name   : Hellgarda Bannstrahlerin
    Ordner : /home/ich/Projects/DSA/Wiki-Scrape
    Stand  : Wir arbeiten Hellgarda als Antagonistin aus. Gerade fertig: …
    Fund   : 142/142  [CLD 2026-09-12]  …Hellgarda trägt das Sonnenzeichen offen…

── weitere Treffer nur im Text ─────────────────────

  32 Treffer (1+31)  2026-08-24 bis 2026-08-27
    Name   : Dämonen nach Domänen
    Ordner : /home/ich/Projects/DSA/Dämonen
    Stand  : …
    Fund   : …
```

- **Name:** der per `/rename` vergebene Name, sonst der Titel, den Claude Code selbst vergibt.
- **Stand:** Claudes letzter Kurzbericht („Wir bauen … Gerade fertig: …"), sonst deine letzte Eingabe.
- **Fund:** eine Textstelle mit dem Suchwort, z. B. `163/165  [DU 2026-09-16]  …`. Mit **←** und **→** springst du durch alle Funde (älter / neuer). In der Klammer steht, woher der Text stammt: `DU`, `CLD` (Claude), `Werkzeug` (Befehlsausgabe) oder `System` (Einblendungen).

**Reihenfolge:** Steht das Suchwort im Namen, den du per `/rename` vergeben hast, kommt die Sitzung ganz nach oben. Eine Linie trennt sie von den Sitzungen, die es „nur" im Text haben. Innerhalb beider Gruppen sortiert die Trefferzahl. Claudes automatische Titel zählen dafür nicht.

Enter wechselt in den Ordner und startet `claude --resume <id>`. Claude Code öffnet dabei immer die ganze Sitzung; an die Fundstelle selbst springt es nicht.

### Ketten nach `/compact`

Manchmal setzt `/compact` ein Gespräch in einer **neuen Datei** fort. Dann gehören mehrere Sitzungen zusammen. `claude-suche` zeigt davon nur das **neueste Glied** und zählt die Treffer aller Glieder zusammen. Die Klammer nennt sie in zeitlicher Folge: `(1+31)` heißt 1 Treffer im älteren, 31 im neueren Teil.

Mit `e` klappt die Kette auf. Jedes Glied lässt sich dann einzeln öffnen:

```
    └ 1    2026-08-24  Dämonen nach Domänen
      └ 31   2026-08-26  Dämonen nach Domänen   ← neueste
```

Die Zusammenfassung, die `/compact` schreibt („This session is being continued…"), wird beim Zählen übersprungen. Sie wiederholt nur, was schon im älteren Teil steht.

## Befehle

| Befehl | Was passiert |
|---|---|
| `claude-suche wort` | Auswahlliste, Enter öffnet das Gespräch |
| `claude-suche wort1 wort2` | findet EINES der Wörter |
| `claude-suche -t wort1 wort2` | findet nur, wo ALLE Wörter vorkommen |
| `claude-suche -l wort` | nur ausdrucken, keine Auswahl (Ketten stehen eingerückt darunter) |
| `claude-suche -v wort` | Fundstellen im Text zeigen |
| `claude-suche -p wort` | auch alte, gelöschte Gespräche (aus `history.jsonl`) |
| `claude-suche -h` | Übersicht (auch `-?`) |

Wird die Ausgabe weitergeleitet (`\| grep …`), erscheint automatisch die reine Liste.

## Installation

Braucht nur Python 3. Keine Abhängigkeiten.

**Linux / macOS**

```
curl -o ~/.local/bin/claude-suche https://raw.githubusercontent.com/cuinhellcat/claude-suche/main/claude-suche
chmod +x ~/.local/bin/claude-suche
```

**Windows**

1. Beide Dateien `claude-suche` **und** `claude-suche.cmd` in einen Ordner legen, der im PATH ist (z. B. `C:\Users\<name>\bin`).
   - PowerShell und cmd nehmen die `.cmd`.
   - Git Bash nimmt die Datei ohne Endung — sie sucht sich selbst einen Python-Interpreter.
2. `pip install windows-curses` (für die Pfeiltasten-Liste; ohne das gibt es nur die Textliste).
3. Terminal neu öffnen.

Im Windows Terminal funktioniert die Pfeiltasten-Liste. Im alten Git-Bash-Fenster (mintty) erscheint stattdessen die Textliste.

## Gespräche nicht mehr löschen lassen

In `~/.claude/settings.json` (Windows: `%USERPROFILE%\.claude\settings.json`) eintragen:

```json
"cleanupPeriodDays": 3650
```

Sonst sind ältere Gespräche irgendwann weg, und `claude-suche -p` findet nur noch die eigenen Eingaben.

## Wo liegen die Daten?

Claude Code speichert jedes Gespräch als `~/.claude/projects/<ordner>/<session-id>.jsonl`. `claude-suche` liest nur diese Dateien. Es schickt nichts irgendwohin.

## Lizenz

MIT
