# ClaudeLimits

Kleines Windows-Tool, das dein Claude-Nutzungskontingent direkt im Programm-Symbol der Taskleiste anzeigt: Ring und Prozentzahl in Ampelfarbe (grün unter 50 %, gelb ab 50 %, rot ab 80 %).

**Aktuelle Version:** 1.4.1 · [Download](../../releases/latest)

## Funktionen

- Session (5 Stunden) und Woche auf einen Blick, mit Countdown bis zum Reset
- Programm-Symbol in der Taskleiste als Live-Anzeige (Session, Woche oder beides), optional Fortschrittsbalken im Taskleisten-Knopf
- Kompaktes oder großes Fenster, optional immer im Vordergrund
- Statistik: Verlauf (24 Std. / 7 / 30 Tage), Verbrauch nach Wochentag und Tageszeit, Spitzenwert, wie oft das Limit erreicht wurde
- Optional mit Windows starten (minimiert) und bei neuer Nutzung kurz einblenden

## Datenquellen

In den Optionen (⚙) unter **Datenquelle** wählbar:

| Quelle | Woher kommen die Werte? |
|---|---|
| **Claude Code** | aus der Anmeldung von Claude Code auf diesem PC (`%USERPROFILE%\.claude\.credentials.json`) |
| **Token** | aus einem selbst eingetragenen Token |
| **Home Assistant** | aus deinen Home-Assistant-Sensoren – empfohlen, siehe unten |

## Home Assistant einrichten

Mit Home Assistant als Quelle fragt nicht Claude Limits bei Anthropic nach, sondern Home Assistant – rund um die Uhr. Dadurch gibt es einen lückenlosen Verlauf, auch wenn der PC aus ist. Claude Limits liest die Werte dann nur noch aus Home Assistant und speichert selbst keinen Verlauf.

### 1. Integration „Claude Usage“ in Home Assistant installieren

Die Werte liefert die HACS-Integration **Claude Usage** ([trickv/hass-claude-usage](https://github.com/trickv/hass-claude-usage)). Der übliche Weg über HACS:

1. HACS öffnen → Menü (⋮) → **Benutzerdefinierte Repositories**
2. `https://github.com/trickv/hass-claude-usage` eintragen, Typ **Integration** → hinzufügen
3. „Claude Usage“ herunterladen und Home Assistant neu starten
4. **Einstellungen → Geräte & Dienste → Integration hinzufügen → Claude Usage** und der Anmeldung folgen

Maßgeblich für Installation und Anmeldung ist die Anleitung im Repository der Integration.

Danach muss es in Home Assistant Sensoren geben, deren Anzeigename so endet:

| Anzeigename endet auf | Bedeutung |
|---|---|
| `Session Usage` | Auslastung 5-Stunden-Fenster in % |
| `Week Usage` | Auslastung Woche in % |
| `Session Reset Time` | Zeitpunkt des nächsten Session-Resets |
| `Weekly Reset Time` | Zeitpunkt des nächsten Wochen-Resets |

Claude Limits findet die Sensoren selbst über diese Namen. Entitäts-IDs musst du nicht eintragen.

### 2. Langlebiges Zugangstoken erstellen

In Home Assistant: **Profil** (unten links) → **Sicherheit** → **Langlebige Zugangstoken** → **Token erstellen**. Den Token kopieren – er wird nur einmal angezeigt.

### 3. Claude Limits verbinden

1. Claude Limits öffnen → **⚙ Optionen**
2. **Datenquelle:** „Home Assistant“
3. **Adresse:** Adresse deines Home Assistant in die Zwischenablage kopieren und **Einfügen** klicken (Standard: `http://homeassistant.local:8123`)
4. **Token:** kopierten Token mit **Einfügen** übernehmen
5. **Speichern** – die Anzeige lädt sofort neu

Der Token wird mit Windows (DPAPI) verschlüsselt unter `%APPDATA%\Claude-Limits` gespeichert und ist nur mit deinem Windows-Konto lesbar.

### Verlauf

Die Statistik liest bis zu 31 Tage aus der Datenbank von Home Assistant (Recorder). Home Assistant hebt den Verlauf standardmäßig 10 Tage auf; für die 30-Tage-Ansicht in der `configuration.yaml` verlängern:

```yaml
recorder:
  purge_keep_days: 31
```

### Fehlermeldungen

| Meldung | Lösung |
|---|---|
| Kein HA-Zugangstoken | Token in den Optionen einfügen |
| HA lehnt den Token ab | Token ungültig oder gelöscht – neuen erstellen |
| Home Assistant nicht erreichbar | Adresse prüfen; neuer Versuch automatisch |
| Claude-Sensoren nicht gefunden | Integration „Claude Usage“ fehlt oder die Sensoren heißen anders |
| Sensor liefert noch keinen Wert | Sensoren „nicht verfügbar“, z. B. direkt nach einem HA-Neustart |

## Installation

Windows 10/11: EXE herunterladen und starten, keine Installation nötig. Das Programm ist nicht signiert, daher zeigt Windows beim ersten Start „Der Computer wurde durch Windows geschützt“ → *Weitere Informationen* → *Trotzdem ausführen*.

## Hinweis

Inoffizielles, privates Tool – keine Verbindung zu Anthropic. Die abgefragte Nutzungs-Schnittstelle ist nicht offiziell dokumentiert und kann sich jederzeit ändern. Ohne Gewähr, kein Support.

Gebaut von Justus Jonas & MCHLNWK · [cappy0815.github.io](https://cappy0815.github.io)
