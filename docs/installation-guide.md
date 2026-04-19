# OLW23 — Home Assistant Installation Guide
## KNX + Apple HomeKit / Siri

---

## Was du brauchst / What you need

- Raspberry Pi 5 (min. 4GB RAM empfohlen)
- MicroSD card, min. 32GB (empfohlen: 64GB A2-rated)
- Ethernet-Kabel (LAN) — Wi-Fi nicht empfohlen für stabilen Betrieb
- KNX IP Interface (Ethernet-angeschlossen, IP-Adresse notieren)
- Mac oder Windows-PC zum Erstellen der SD-Karte
- iPhone mit Home-App

---

## Schritt 1: Home Assistant OS auf Raspberry Pi 5 installieren

### 1.1 Raspberry Pi Imager herunterladen
Lade den **Raspberry Pi Imager** herunter:
→ https://www.raspberrypi.com/software/

### 1.2 Home Assistant OS Image auswählen
1. Starte den Imager
2. Klicke auf **"Choose Device"** → wähle **Raspberry Pi 5**
3. Klicke auf **"Choose OS"** → scrolle ganz nach unten zu **"Other specific-purpose OS"**
4. Wähle **"Home automation"** → **"Home Assistant OS"**
5. Wähle die aktuellste Version für **Raspberry Pi 5**

### 1.3 Auf SD-Karte schreiben
1. Klicke auf **"Choose Storage"** → wähle deine SD-Karte
2. Klicke auf **"Next"** → dann **"Yes"** (überschreibt die Karte)
3. Warte bis der Vorgang abgeschlossen ist (ca. 5-10 Minuten)

### 1.4 Raspberry Pi starten
1. SD-Karte in den Raspberry Pi einlegen
2. Ethernet-Kabel anschließen (an deinen Router/Switch)
3. Stromversorgung anschließen
4. Warte 3-5 Minuten beim ersten Start

---

## Schritt 2: Home Assistant einrichten

### 2.1 Erster Zugriff
Öffne auf deinem Computer (im gleichen Netzwerk):
```
http://homeassistant.local:8123
```
Falls das nicht funktioniert, findest du die IP-Adresse in deinem Router.

### 2.2 Ersteinrichtung
1. Klicke auf **"Create my smart home"**
2. Erstelle einen Account (Benutzername + Passwort — gut merken!)
3. Folge dem Einrichtungsassistenten
4. Du kannst Standort und Zeitzone setzen (Wien / Europe/Vienna)

---

## Schritt 3: KNX Integration einrichten

### 3.1 KNX Integration hinzufügen
1. Gehe zu **Einstellungen → Integrationen**
2. Klicke auf **"+ Integration hinzufügen"**
3. Suche nach **"KNX"** und klicke darauf
4. Wähle **"Tunneling"** (für KNX IP Interface)
5. Gib die **IP-Adresse deines KNX IP Interface** ein
   - Port: **3671** (Standard)
   - Local IP: leer lassen (automatisch)
6. Klicke auf **"Absenden"**

> ⚠️ Stelle sicher, dass der KNX IP Interface und der Raspberry Pi
> im gleichen Netzwerk (gleicher Switch/Router) sind.

### 3.2 Konfigurationsdateien hochladen
Kopiere die Dateien aus diesem Repository auf den Raspberry Pi:

**Option A: Über die Home Assistant UI (Datei-Editor)**
1. Gehe zu **Einstellungen → Add-ons → Add-on Store**
2. Installiere **"File editor"** (oder "Studio Code Server" für mehr Komfort)
3. Öffne den File Editor
4. Ersetze `configuration.yaml` mit dem Inhalt aus diesem Repository

**Option B: Über SSH (fortgeschritten)**
1. Installiere das **"SSH & Web Terminal"** Add-on
2. Verbinde dich per SSH: `ssh root@homeassistant.local`
3. Dateien liegen in `/config/`

### 3.3 Konfiguration validieren und neu starten
1. Gehe zu **Einstellungen → System → Neu starten**
2. Oder: **Entwicklerwerkzeuge → YAML → Konfiguration prüfen**

---

## Schritt 4: Apple HomeKit Bridge einrichten

### 4.1 HomeKit Integration aktivieren
Die HomeKit Bridge wird über `configuration.yaml` aktiviert
(bereits in der mitgelieferten Konfiguration enthalten).

Nach dem Neustart erscheint eine Benachrichtigung in Home Assistant
mit einem **QR-Code** oder **8-stelligem PIN**.

### 4.2 Mit iPhone verbinden
1. Öffne die **Home** App auf dem iPhone
2. Tippe auf **"+"** → **"Zubehör hinzufügen"**
3. Scanne den QR-Code oder gib den PIN ein
4. Wähle einen Namen für das Zuhause (z.B. "OLW23")
5. Weise Geräten Räume zu

### 4.3 Siri aktivieren
Sobald Geräte in der Home-App sind, funktioniert Siri automatisch:
- *"Hey Siri, Wohnzimmerlicht einschalten"*
- *"Hey Siri, Jalousien im Schlafzimmer schließen"*
- *"Hey Siri, Temperatur im Büro auf 21 Grad"*

---

## Schritt 5: Gruppenadressadressen verifizieren

> ⚠️ **WICHTIG**: Die Konfigurationsdateien in diesem Repository enthalten
> für viele Räume (1.UG, EG, OG, Gästewohnung) **geschätzte Adressen**
> basierend auf dem erkannten Muster aus dem ETS-Export.
>
> Die **2.UG Adressen (1/x/x)** wurden direkt aus dem ETS-XML übernommen
> und sollten korrekt sein.
>
> Alle anderen Adressen (2/x/x, 3/x/x, 4/x/x, 5/x/x, 6/x/x) sind
> nach dem gleichen Muster erstellt — müssen aber mit ETS verglichen werden.

### Wie du Adressen prüfst:
1. Öffne ETS auf deinem PC
2. Gehe zu **Gruppenadressansicht**
3. Vergleiche die Adressen in den YAML-Dateien mit ETS
4. Korrigiere falsche Adressen in den YAML-Dateien
5. Starte Home Assistant neu

### Schnelltest in Home Assistant:
1. Gehe zu **Entwicklerwerkzeuge → KNX → Gruppenadresse senden**
2. Gib eine Adresse ein (z.B. `4/0/1`) mit Wert `true`
3. Prüfe ob das entsprechende Licht reagiert

---

## Raumzuordnung in HomeKit

Empfohlene Raumstruktur für die Home-App:

| Home Assistant Name | HomeKit Raum |
|---------------------|--------------|
| EG Wohnen Gesamt | Wohnzimmer |
| EG Kochen Essen Gesamt | Küche |
| EG Buero Gesamt | Büro EG |
| OG Masterschlafen Gesamt | Schlafzimmer |
| OG Masterbad Gesamt | Bad Master |
| 1UG Kino Gesamt | Heimkino |
| 1UG Fitness Gesamt | Fitnessraum |
| 1UG SPA Gesamt | SPA |
| 2UG Garage Gesamt | Garage |
| Gaeste Wohnen Gesamt | Gästezimmer |

---

## Sonos Integration (Phase 3)

Sobald Sonos-Lautsprecher im gleichen Netzwerk sind:
1. **Einstellungen → Integrationen → Sonos** hinzufügen
2. Wird automatisch erkannt
3. In HomeKit als Speaker verfügbar
4. Siri: *"Hey Siri, Musik im Wohnzimmer lauter"*

---

## Troubleshooting

### KNX verbindet nicht:
- KNX IP Interface im gleichen Netzwerk?
- Maximal **1 Tunneling-Verbindung** gleichzeitig — ETS disconnecten!
- IP-Adresse des Interface korrekt?

### HomeKit nicht sichtbar:
- Port 21063 in der Firewall freigegeben?
- Raspberry Pi und iPhone im gleichen WLAN/Netzwerk?
- Home Assistant neustarten

### Licht reagiert nicht:
- KNX Gruppenadresse korrekt?
- Test: Entwicklerwerkzeuge → KNX → Gruppenadresse senden
- ETS öffnen und Adresse prüfen

---

## Nächste Schritte

- [ ] Schritt 1-4 abgeschlossen
- [ ] KNX Verbindung getestet
- [ ] 2.UG Beleuchtung getestet (Adressen aus ETS direkt übernommen)
- [ ] EG Adressen in ETS geprüft und ggf. korrigiert
- [ ] HomeKit eingerichtet
- [ ] Siri-Befehle getestet
- [ ] Sonos Integration (später)
