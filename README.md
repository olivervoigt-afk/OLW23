# OLW23 — KNX Home Automation
## Raspberry Pi 5 + Home Assistant + Apple HomeKit / Siri

Dieses Repository enthält die Konfiguration für die KNX-Hausautomation
mit Apple HomeKit und Siri-Sprachsteuerung.

---

## Systemübersicht

```
KNX Bus (Beleuchtung, Jalousien, Heizung, Türen)
    ↕
KNX IP Interface (Ethernet)
    ↕
Raspberry Pi 5 (Home Assistant OS)
    ↕
Apple HomeKit / Siri (iPhone, iPad, HomePod)
```

## Stockwerke / Zones

| Präfix | Stockwerk | Räume |
|--------|-----------|-------|
| 0/x/x | Zentral | Alle Stockwerke, Wetterstation, Türen |
| 1/x/x | 2. Untergeschoss | Stiegenhaus, Wäsche, Abstellraum, Weinlager, Skiraum, Garage, TG |
| 2/x/x | 1. Untergeschoss | Stiegenhaus, Vinothek, Kino, Fitness, SPA, Kinderzimmer, Bäder |
| 3/x/x | Gästewohnung | Schlafen, Bad, Wohnen, Gang, WC |
| 4/x/x | Erdgeschoss | Garderobe, Büro, WC, Küche/Essen, Wohnen, Back Kitchen |
| 5/x/x | Obergeschoss | Stiegenhaus, Schlafen, Schrankraum, Büro, WC, Masterbad, Masterschlafen |
| 6/x/x | Außen | Einfahrt, Terrassen, Zugänge, Außenpool |

## Funktionen

- **Beleuchtung**: Ein/Aus + Dimmen pro Raum (HomeKit + Siri)
- **Jalousien**: Auf/Zu/Stop pro Jalousie und Vorhang (HomeKit + Siri)
- **Heizung/Kühlung**: Temperatur setzen und ablesen pro Raum (HomeKit + Siri)
- **Türöffner**: Haustür, Garagentor, Verbindungstüren
- **Wetterstation**: Außentemperatur, Helligkeit, Wind, Regen
- **Pool-Monitoring**: Wassertemperatur Innenpool, Außenpool, Whirlpool
- **Sauna**: Start/Stop
- **Anwesenheit**: An/Abwesend-Schalter

## Dateien

```
homeassistant/
├── configuration.yaml      — Hauptkonfiguration
├── secrets.yaml            — IP-Adressen (NICHT ins Git!)
└── knx/
    ├── lights.yaml         — Beleuchtung alle Stockwerke
    ├── covers.yaml         — Jalousien & Vorhänge
    ├── climate.yaml        — Heizung/Kühlung Thermostate
    ├── sensors.yaml        — Temperaturen, Feuchtigkeit, Pool
    └── switches.yaml       — Türöffner, Zentralfunktionen

docs/
└── installation-guide.md   — Schritt-für-Schritt Anleitung
```

## Schnellstart

Lies zuerst: [docs/installation-guide.md](docs/installation-guide.md)

## Wichtiger Hinweis zu Gruppenadressadressen

Die Adressen aus dem 2. Untergeschoss (1/x/x) wurden direkt aus dem
ETS-Export übernommen. Alle anderen Stockwerke folgen dem gleichen
Muster, müssen aber mit ETS verglichen und ggf. korrigiert werden.

Zeilen mit `# TODO: Verify` in den YAML-Dateien markieren Adressen,
die geprüft werden sollen.
