![Logo](admin/divera247_long.png)
# ioBroker.divera247

**Gepflegter Fork** von [TKnpl/ioBroker.divera247](https://github.com/TKnpl/ioBroker.divera247) — repariert und erweitert, da 0.2.0 upstream nicht mehr funktionierte.

Adapter für die Alarmierungssoftware <a href="https://www.divera247.com/" target="_blank">Divera 24/7</a>.

## Funktioniert mit allen Divera-Versionen — auch FREE
Die Alarme werden über den Endpunkt `/api/v2/pull/all` gelesen, der in **allen** Divera-Versionen (FREE, ALARM, PRO) erlaubt ist.
Der von 0.2.0 genutzte Endpunkt `/api/v2/alarms` ist bei FREE-Konten gesperrt (403) und führte zum Fehler „Login not possible".

## Installation
Im ioBroker-Admin (Expertenmodus) als „Adapter aus eigener URL" installieren:
```
https://github.com/nimda2/ioBroker.divera247
```
Benötigt js-controller >= 3.0.0.

## Konfiguration
Die Einstellungen sind in drei Reiter gegliedert (moderne JSON-Config-Oberfläche):

### Verbindung
- **E-Mail-Adresse / Passwort** — die Divera-24/7-Zugangsdaten. Das Passwort wird verschlüsselt gespeichert.
  Empfehlung: einen eigenen System-/Monitor-Benutzer verwenden, keinen persönlichen Account.
- **Abfrageintervall (Sekunden)** — wie oft die API abgefragt wird (Standard 30, Minimum 10).

### Filter
- **Nur Alarme für meinen Benutzer** — Alarm wird nur ausgelöst, wenn der eingeloggte Benutzer adressiert ist.
- **Divera Benutzer-IDs / Alarm-Gruppen** — Alarme optional auf bestimmte Benutzer oder Gruppen einschränken
  (kommagetrennt; leer lassen = alle Alarme). Benutzer-IDs werden vor Gruppen geprüft.
- **Alarm-Rückmeldungen auswerten** — zählt bei einem aktiven Alarm, wie viele Mitglieder welchen Status
  gewählt haben (Datenpunkte `responses.*`, standardmäßig aus).

### Personalverfügbarkeit
- **Personalverfügbarkeit auswerten** — zählt Mitglieder je Status und ausgewählter Qualifikation aus den
  Monitor-Daten (Datenpunkte `availability.*`, standardmäßig aus; benötigt einen Account mit Monitor-Rechten).
- **Qualifikationen** — auszuwertende Qualifikationen als ID, Kürzel oder Name, kommagetrennt (z. B. `GF, MA, Atemschutz`).

## Datenpunkte
- `alarm` (bool), `title`, `text`, `foreign_id`, `divera_id`, `address`, `lat`, `lng`, `date`, `priority`,
  `addressed_users`, `addressed_groups`, `addressed_vehicle`, `lastUpdate`
- Beim Start und bei Alarmende werden alle Alarm-Datenpunkte zurückgesetzt (Booleans auf `false`, Rest auf `null`) — sie sind nie undefiniert.
- `availability.<Status>.all` und `availability.<Status>.<Qualifikation>` — Anzahl je Status/Qualifikation (optional)
- `responses.<Status>`, `responses.answered_total`, `responses.recipients` — Rückmeldungen zum aktiven Alarm (optional)

Pro Abfrageintervall erfolgt genau **ein** API-Aufruf; Alarme, Verfügbarkeit und Rückmeldungen werden aus derselben Antwort ausgewertet.

## Changelog

### 0.3.8
* (nimda2) Robustheit: 15s Request-Timeout (hängende API-Aufrufe können das Polling nicht mehr dauerhaft stoppen); Alarm wird auch zurückgesetzt, wenn der neueste Listeneintrag ein anderer, bereits geschlossener Alarm ist; Verfügbarkeit/Rückmeldungen laufen auch bei fehlendem Alarm-Inhalt; README überarbeitet

### 0.3.7
* (nimda2) Alarm-Rückmeldungen (`responses.*`) optional per Einstellung; Zurücksetzen bei Alarmende läuft wieder immer

### 0.3.5 / 0.3.4
* (nimda2) Alle Alarm-Datenpunkte werden bei Alarmende zurückgesetzt; Alarm-Rückmeldungszähler je Status hinzugefügt

### 0.3.3
* (nimda2) States werden beim Start initialisiert und sind nie mehr `null` (Verhalten wie 0.1.2)

### 0.3.2
* (nimda2) Admin-Einstellungen auf JSON-Config umgestellt (Reiter, responsiv, bedingte Felder)

### 0.3.1
* (nimda2) Verfügbarkeit nutzt denselben pull/all-Abruf wie die Alarme; Abfrageintervall wieder konfigurierbar

### 0.3.0
* (nimda2) Alarme über `/api/v2/pull/all` (behebt 403 „Login not possible" bei FREE-Konten); optionale Personalverfügbarkeit (`availability.*`)

### 0.2.1
* (nimda2) 0.2.0 repariert: Login-Konfiguration, API-Feld `ucr_adressed`, Auswahl des neuesten Alarms, diverse Abstürze

### 0.2.0 und älter
* (TKnpl) siehe [Original-Repository](https://github.com/TKnpl/ioBroker.divera247)

## License
MIT License

Copyright (c) 2022 TKnpl <dev@t-concepts.de>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
