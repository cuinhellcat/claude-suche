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
3 Gespräche zu: hellgarda
Pfeiltasten wählen · Enter öffnet · q bricht ab

> 39 Treffer  2026-08-22
    Name   : Hellgarda Bannstrahlerin
    Ordner : /home/ich/Projects/DSA/Wiki-Scrape
    Beginn : Hi, du wirst mir helfen, einen wichtigen NSC auszuarbeiten…

  4 Treffer  2026-08-22
    Ordner : /home/ich/Documents/Quellenbände
    Beginn : Im PDF "Orden und Buendnisse" sollen Infos über die…
```

Enter wechselt in den Ordner und startet `claude --resume <id>`.

## Befehle

| Befehl | Was passiert |
|---|---|
| `claude-suche wort` | Auswahlliste, Enter öffnet das Gespräch |
| `claude-suche wort1 wort2` | findet EINES der Wörter |
| `claude-suche -t wort1 wort2` | findet nur, wo ALLE Wörter vorkommen |
| `claude-suche -l wort` | nur ausdrucken, keine Auswahl |
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

1. `claude-suche` und `claude-suche.cmd` in einen Ordner legen, der im PATH ist.
2. `pip install windows-curses` (für die Pfeiltasten-Liste; ohne das gibt es nur die Textliste).

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
