# ☀️ PV-Kachel für Home Assistant

Eine PV-Kachel im iOS-Stil für Home Assistant, gebaut mit [button-card](https://github.com/custom-cards/button-card).
Als kleine Kachel zeigt sie die aktuelle Leistung und den Tagesertrag. Ein Tippen klappt sie zu einer großen Detailansicht auf.

*English version below.*

---

## Funktionen

**Kleine Kachel**
- Aktuelle Leistung und Auslastung in % der Anlagenleistung
- Ertrag heute
- Gesamtertrag

**Große Ansicht (Antippen)**
- **Tagesverlauf**: Leistungskurve von heute, gestrichelt die Kurve von gestern. Mit dem Finger über die Kurve fahren zeigt den Wert zur jeweiligen Uhrzeit.
- **Kennzahlen**: Ertrag heute, Spitzenleistung mit Uhrzeit, Gesamtertrag
- **Sonnenbogen**: wo die Sonne gerade steht und wie lange es noch bis Sonnenunter- bzw. -aufgang dauert
- **Wechselrichter**: Fluss von Modulen (DC) über den Wirkungsgrad bis ins Haus (AC), dazu frei wählbare Messwerte wie String-Spannungen, Ströme, MPPT-Leistung und Limit. Ein Tipp auf einen Wert öffnet den Verlauf.

Der Tagesertrag kommt aus der Langzeitstatistik von Home Assistant. Ein eigener Tageszähler-Sensor ist deshalb nicht nötig, ein Gesamtzähler (`total_increasing`) reicht.

## Voraussetzungen

- Home Assistant mit einem Dashboard im Storage-Modus (Standard)
- [button-card](https://github.com/custom-cards/button-card), installiert über HACS bzw. den Marketplace
- ein Leistungssensor in W oder kW und ein Energie-Gesamtzähler in Wh, kWh oder MWh

## Installation

Die Datei [`pv-kachel.yaml`](pv-kachel.yaml) besteht aus zwei Teilen.

1. **Templates**: Dashboard öffnen, dann **⋮ → Bearbeiten → ⋮ → Raw-Konfigurationseditor**. Den Block `button_card_templates:` aus Teil 1 ganz oben einfügen, auf derselben Ebene wie `views:`. Gibt es schon `button_card_templates:`, nur die sechs Einträge darunter ergänzen:
   - `ios_kit`
   - `ios_tile`
   - `ios_morph`
   - `ios_open`
   - `ios_pv`
   - `ios_pvx`
2. **Karte**: im Dashboard **Karte hinzufügen → Manuell** wählen und Teil 2 einfügen.
3. **Anpassen**: die Entitäten und `max` auf die eigene Anlage ändern (siehe unten).

## Konfiguration

Die Karte besteht aus einer kleinen Kachel (`var_small`) und einer großen Ansicht (`var_big`). Die wichtigsten Werte:

| Wert | Bedeutung |
|---|---|
| `entity` | Leistungssensor (W oder kW), an allen drei Stellen derselbe |
| `var_small.variables.var_total` | Energie-Gesamtzähler |
| `var_small.variables.var_day` | optional: Tageszähler, sonst `null` |
| `var_small.variables.var_max` | Anlagenleistung in W (z. B. 34 × 465 W = `15810`) |
| `var_ent.power` | Leistungssensor |
| `var_ent.total` | Energie-Gesamtzähler |
| `var_ent.max` | Anlagenleistung in W |
| `var_ent.sun` | `sun.sun` |
| `var_ent.dc` | DC-Leistung der Module |
| `var_ent.ac` | optional: AC-Leistung (für den Wirkungsgrad) |
| `var_ent.eff` | optional: Wirkungsgrad-Sensor; ohne ihn wird AC/DC berechnet |
| `var_ent.limit` | optional: Leistungslimit in % (`number.*`) |
| `var_ent.reach` | optional: Status-Sensor für die Anzeige „Online/Offline“ |
| `var_ent.cells` | Liste der Messwerte: `[Name, Entität, Nachkommastellen, Einheit]` |

Das Beispiel nutzt einen **Fronius Symo GEN24** mit der offiziellen Fronius-Integration. Mit anderen Wechselrichtern (Hoymiles/OpenDTU, SMA, Huawei …) klappt es genauso, wenn man die Entitäten austauscht.

Ohne `var_ent.cells` zeigt die Kachel die Standardwerte eines Mikro-Wechselrichters (OpenDTU/AhoyDTU):

| Feld | Bedeutung |
|---|---|
| `volt` | Spannung |
| `cur` | Strom |
| `freq` | Frequenz |
| `pf` | Leistungsfaktor |
| `react` | Blindleistung |
| `temp` | Temperatur |
| `rssi` | Funksignal |

## Hinweise

- Den Look gibt es nur zusammen mit einem dunklen bzw. farbigen Dashboard-Hintergrund. Die Kachel ist halbtransparent.
- Verlaufsdaten werden etwa alle 5 Minuten aus der Statistik nachgeladen. Bei frisch eingebundenen Sensoren füllen sich Kurve und Vortagesvergleich erst mit der Zeit.

---

## 🇬🇧 English

An iOS-style solar/PV card for Home Assistant, built with [button-card](https://github.com/custom-cards/button-card).
As a small tile it shows current power and today's yield. A tap expands it into a detailed view.

**Features**
- **Small tile:** current power with % of system size, today's yield, total yield.
- **Day curve:** today's power curve with yesterday as a dashed line. Scrub with your finger to read the value at any time.
- **Stats:** today, peak with time, total.
- **Sun arc:** current sun position and time left until sunset or sunrise.
- **Inverter:** flow from panels (DC) through efficiency to the house (AC), plus configurable readings such as string voltages, currents, MPPT power and limit.
- **No daily sensor needed:** today's yield is computed from HA long-term statistics, so a total (`total_increasing`) energy sensor is enough.

**Requirements**
- [button-card](https://github.com/custom-cards/button-card) via HACS or the Marketplace
- a power sensor (W or kW) and a total energy sensor (Wh, kWh or MWh)

**Installation**
1. Open your dashboard and go to **⋮ → Edit → ⋮ → Raw configuration editor**. Paste the `button_card_templates:` block from part 1 of [`pv-kachel.yaml`](pv-kachel.yaml) at the top, on the same level as `views:`.
2. Add a **Manual** card and paste part 2.
3. Replace the entities with your own and set `max` to your system size in watts (see the table above).

The UI labels are in German. Most of them are in the `ios_pv` and `ios_pvx` templates if you want to translate them.

## Lizenz / License

MIT
