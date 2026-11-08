# Open-Source-Softwarestack für kleine Unternehmen

Wähle im Browser, was dein Unternehmen braucht, und erhalte ein fertiges Installationsskript für Linux. Alle Programme und Serverdienste sind Open Source, laufen lokal und funktionieren auch ohne Internetverbindung.

**Live:** https://jensharbers.github.io/open-source-stack/

## Was die Seite kann

- **Programme anhaken**, geordnet nach Gruppen: Büro und Wissen, Dateien und Backups, Geschäft, Büro-Alltag, Laptop-Sicherheit, Lokale KI, Kommunikation, Technik, Design.
- **Serverdienste anhaken**, die in Docker laufen und nur lokal erreichbar sind: Odoo, Nextcloud, Forgejo, Vaultwarden, Paperless-ngx, Uptime Kuma, Vikunja, Kimai und Haben (Beta, braucht Domain und HTTPS).
- **Voreinstellung „Solo-Start"** für Einzelunternehmer, außerdem „Alles" und „Zurücksetzen".
- **Installationsskript** für Debian, Ubuntu und Ableger, das sich live mit der Auswahl ändert. Kopieren oder als `firmen-stack-setup.sh` herunterladen.
- **Lokale KI** mit Ollama und Sprachmodellen nach Arbeitsspeicher gestaffelt.
- **Bereich „Fürs Handy"** mit passenden Android- und iOS-Apps und Links zu den Projektseiten.
- **Mobil nutzbar**: einspaltiges Layout, große Tippflächen, feste Leiste unten.

Die Seite ist eine einzelne HTML-Datei. Sie läuft komplett im Browser, braucht keinen Server und lädt keine externen Skripte, Schriften oder Tracker.

## So benutzt du sie

1. Seite öffnen und Programme anhaken, oder „Solo-Start" wählen.
2. Skript herunterladen und **in Ruhe lesen**.
3. Auf dem Linux-Rechner starten:

   ```bash
   bash firmen-stack-setup.sh
   ```

4. Das Skript fragt bei Bedarf nach deinem sudo-Passwort. Führe es **einmal mit Internetverbindung** aus: Pakete und Docker-Images werden dabei geladen. Danach arbeitest du offline.

## Was das Skript tut

- Installiert Systempakete über `apt`.
- Installiert Programme über Flathub (Flatpak).
- Lädt RechnungsFee als AppImage von GitHub.
- Installiert Ollama über das offizielle Skript von ollama.com und lädt die gewählten Modelle.
- Installiert Docker über das offizielle Skript von get.docker.com, wenn Serverdienste gewählt sind.
- Legt pro Serverdienst einen Ordner `~/firmen-server/<dienst>/` mit `docker-compose.yml` und einer `.env` mit zufälligen Zugangsdaten an (Rechte `600`).
- Richtet bei Auswahl ufw, automatische Sicherheitsupdates und fail2ban ein.
- Gibt am Ende Hinweise zu manuellen Schritten aus, etwa Admin-Konten oder die WireGuard-Konfiguration.

Alle Serverdienste außer Haben werden nur an `127.0.0.1` gebunden. Für Zugriff von außen brauchst du einen Reverse Proxy mit HTTPS, den das Skript nicht einrichtet. Haben bringt seinen eigenen HTTPS-Proxy (Caddy) mit, braucht dafür eine Domain und die Ports 80 und 443 und gehört deshalb auf einen Server statt auf den Laptop.

## Wichtige Hinweise

- **Status: Beta.** Das Skript wurde auf Syntaxfehler geprüft, aber nicht systematisch auf echten Systemen getestet. Probiere es zuerst in einer virtuellen Maschine oder auf einem Testrechner aus.
- **Nur Debian, Ubuntu und Ableger.** Andere Distributionen brauchen angepasste Paketnamen.
- Paketnamen, Flathub-IDs, Image-Versionen und Modell-Tags ändern sich. Prüfe sie vor dem produktiven Einsatz.
- Prüfe Skripte aus dem Internet immer, bevor du sie ausführst. Das gilt auch für die Installationsskripte von Docker und Ollama, die dieses Skript aufruft.
- RechnungsFee nur aus dem offiziellen Repository `nicolettas-muggelbude/RechnungsFee` laden. Es existieren Forks mit verdächtigen Download-Links.
- **Keine Rechts-, Steuer- oder Buchhaltungsberatung.** Ob Programme für GoBD, DSGVO, E-Rechnung oder deine Rechtsform ausreichen, klärst du mit Steuerberater oder Anwalt.
- Festplattenverschlüsselung (LUKS) richtest du bei der Systeminstallation ein, nicht per Skript.
- Lizenzen: Die gelisteten Programme haben jeweils eigene Lizenzen. Open WebUI ist bewusst nicht enthalten, weil seine Lizenz seit Version 0.6.6 nach Angabe des Herstellers nicht mehr Open Source im Sinne der OSI ist.

## Aufbau des Repositorys

```
open-source-stack/
├── index.html          Die komplette Seite (HTML, CSS, JavaScript in einer Datei)
├── README.md           Diese Datei
├── LICENSE             Lizenz des Seiten-Codes
├── .nojekyll           Sagt GitHub Pages, die Dateien unverändert auszuliefern
├── CONTRIBUTING.md     Wie man Programme ergänzt oder Fehler meldet
└── .github/
    └── ISSUE_TEMPLATE/
        └── programm-vorschlagen.md
```

## Auf GitHub Pages veröffentlichen

1. Repository `open-source-stack` anlegen und die Dateien hochladen.
2. Unter **Settings → Pages** als Quelle **Deploy from a branch** wählen, Branch `main`, Ordner `/ (root)`.
3. Nach kurzer Zeit ist die Seite unter `https://<benutzername>.github.io/open-source-stack/` erreichbar.

Die Seite kommt ohne Build-Schritt aus. Du kannst `index.html` auch lokal im Browser öffnen.

## Den Katalog erweitern

Im Skriptteil von `index.html` stehen zwei Listen.

**Programme** (`CAT`), pro Eintrag:

```js
[id, Name, Beschreibung, Art, Paket, Herkunft]
```

| Art | Bedeutung |
|---|---|
| `apt` | Systempaket, mehrere Pakete durch Leerzeichen getrennt |
| `fp` | Flatpak-ID von Flathub |
| `model` | Ollama-Modell-Tag |
| `sh` | Spezielle Installation, im Skript von Hand beschrieben (Ollama) |
| `ai` | AppImage-Download, im Skript von Hand beschrieben (RechnungsFee) |
| `note` | Kein Automatismus, nur ein Hinweis am Ende (Fakturama) |

**Serverdienste** (`SERV`), pro Eintrag:

```js
[id, Name, Beschreibung, Port, Compose-Datei, Hinweis, Herkunft, Vorbefehle]
```

In der Compose-Datei stehen `%PW%` und `%KEY%` als Platzhalter für die zufällig erzeugten Werte aus der `.env`.

Wichtig: Jede `id` muss in beiden Listen **eindeutig** sein. Zusätzlich gibt es `MOB` (Handy-Apps), `POST` (Befehle nach der Installation) und `NOTE` (Hinweise am Ende).

## Mitmachen

Vorschläge für neue Programme, Korrekturen an Paketnamen und Fehlerberichte sind willkommen, am besten als Issue mit dem Vorlage-Formular. Bitte nur Software aufnehmen, die Open Source ist, lokal läuft und auch offline nutzbar ist.

## Lizenz

Der Code dieser Seite steht unter der MIT-Lizenz (siehe `LICENSE`). Für die aufgeführten Programme gelten deren eigene Lizenzen.

## Haftungsausschluss

Die Nutzung erfolgt auf eigene Verantwortung. Es gibt keine Gewähr für Richtigkeit, Vollständigkeit oder Eignung für einen bestimmten Zweck.

## Betreiber

- [Impressum](https://harbers-agrarinformatik.de/impressum.html)
- [Datenschutz](https://harbers-agrarinformatik.de/datenschutz.html)

Beim Aufruf über GitHub Pages kann GitHub Zugriffsdaten (zum Beispiel die IP-Adresse) in Server-Logs verarbeiten. Die Seite selbst setzt keine Cookies und bindet keine externen Ressourcen ein.
