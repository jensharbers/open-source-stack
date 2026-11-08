# Mitmachen

Danke, dass du den Open-Source-Softwarestack verbessern willst. Dieses Dokument beschreibt, welche Software aufgenommen wird und wie du Änderungen einreichst.

## Welche Software aufgenommen wird

Ein Programm oder Serverdienst kommt in den Katalog, wenn alles Folgende zutrifft:

- **Open Source** mit einer anerkannten Lizenz (OSI). Software mit Zusatzklauseln, die Rechte einschränken, nehmen wir nicht auf.
- **Läuft lokal** auf dem eigenen Rechner und ist nach der Installation **offline nutzbar**. Reine Cloud-Dienste gehören nicht in diesen Stack.
- **Wird gepflegt.** Es gibt aktuelle Releases und eine erreichbare Projektseite.
- **Sauberer Installationsweg:** ein Paket aus den Distributions-Quellen (`apt`), eine Flatpak-ID von Flathub, ein offizielles Release des Projekts oder ein Docker-Image des Projekts. Keine Mirror-Seiten, Forks mit unklarer Herkunft oder Links aus Foren.
- **Sinnvoll für kleine Unternehmen**, zum Beispiel Büro, Buchhaltung, Zusammenarbeit, Sicherheit, Design oder lokale KI.

## Vorschlag einreichen

Eröffne ein Issue und nenne:

1. Name und Link zur Projektseite
2. Lizenz
3. Wofür es im Unternehmen gebraucht wird
4. Installationsweg (Paketname, Flatpak-ID oder Docker-Image mit Version)
5. Ob es offline funktioniert
6. Ob es eine App für Android oder iOS gibt, mit Link zur Projektseite

## Änderung per Pull Request

1. Forke das Repository und bearbeite `index.html`.
2. Trage deinen Eintrag im passenden Format ein. Die Felder sind in der [README](README.md) im Abschnitt „Den Katalog erweitern" beschrieben.
3. Achte darauf, dass jede `id` **eindeutig** ist, auch zwischen Programmen und Serverdiensten.
4. Teste deine Änderung (siehe unten).
5. Beschreibe im Pull Request kurz, was du hinzufügst oder korrigierst und woher die Angaben stammen.

Kleine Korrekturen wie ein falscher Paketname sind jederzeit willkommen.

## Testen

- Öffne `index.html` im Browser, hake alles an, auch deinen neuen Eintrag, und prüfe, dass das Skript rechts erscheint.
- Kopiere das Skript in eine Datei und prüfe es mit `bash -n skript.sh` auf Syntaxfehler.
- Wenn möglich, führe das Skript in einer virtuellen Maschine mit Debian oder Ubuntu aus.
- Prüfe die Seite auch auf einem schmalen Bildschirm.

## Regeln für Serverdienste

- Ports nur an `127.0.0.1` binden, nie an alle Schnittstellen.
- Keine festen Passwörter. Zugangsdaten kommen aus der `.env`, die das Skript zufällig erzeugt (`%PW%` und `%KEY%`).
- Docker-Images mit einer konkreten Version angeben, wenn das Projekt stabile Tags anbietet.
- In `Hinweis` steht, was der Nutzer nach dem Start noch von Hand erledigen muss.

## Stil

- Texte auf Deutsch, kurz und sachlich. Für Anführungszeichen „so", für den Apostroph ’.
- Beschreibungen in einem Satz, ohne Werbesprache.
- Die Herkunft (zum Beispiel DE oder NL) nur eintragen, wenn sie sicher ist.
- Handy-Links führen zu Projektseiten, nicht zu Suchergebnissen.

## Sicherheitsprobleme melden

Wenn du eine Sicherheitslücke findest, etwa ein Skript, das unsichere Rechte setzt, melde sie bitte **nicht öffentlich**. Nutze die private Meldung von GitHub („Report a vulnerability" unter dem Reiter Security) oder die Kontaktdaten im [Impressum](https://harbers-agrarinformatik.de/impressum.html).

## Lizenz deiner Beiträge

Mit deinem Beitrag stimmst du zu, dass er unter der [MIT-Lizenz](LICENSE) des Repositorys veröffentlicht wird.
