# Drivhus — automatisering

## Status

Planlegging

## Mål

Automatisere vanning, ventilasjon og overvåking av drivhus.

## Bygningsbeskrivelse

Ferdigkonstruksjon (byggesett), satt opp 2021.

| Egenskap | Verdi |
|---|---|
| Grunnflate | 2,4 × 4,2 m (10,1 m²) |
| Sidevegghøyde | 1,55 m |
| Møne | 2,24 m (saltak, 30°) |
| Internvolum | ca. 19,2 m³ |
| Skjelett | Stål |
| Sidevegger | Herdet glass (enkelt) |
| Tak | Kanalplast 5 mm |
| Gulv | Treplatting, trykkimpregnert terrassebord |
| Dør | Dobbel skyvedør i gavlvegg |
| Takluker | 4 stk, 0,6 × 0,75 m, topphengslede |

## Termisk ytelse

| Bygningsdel | U-verdi (ca.) | Kommentar |
|---|---|---|
| Herdet glass (sidevegger) | ~5,8 W/m²K | Rask varmetap om natten |
| Kanalplast 5 mm (tak) | ~3,5 W/m²K | Bedre enn enkeltglass |
| Stålskjelett | Høy | Termisk bro, kondensrisiko |
| Treplatting (gulv) | — | Ingen termisk masse |

Huset overopphetes raskt på solrike dager og taper varme raskt om natten. Automatisk ventilasjon er kritisk.

## Ventilasjon

- 4 takluker = 1,8 m² åpningsareal = **17,8 % av gulvarealet** (anbefalt minimum 15–20 %)
- Topphengslede luker slipper ut varm luft ved mønet — god skorsteinseffekt
- Skyvedør i gavl fungerer som lavt inntak
- Ingen dedikerte lave sideveggsventiler — begrenset passiv ventilasjon uten åpen dør

## Automasjonspunkter

### Ventilasjonsstrategi
```
Temp > 25°C  →  åpne takluker gradvis
Temp > 30°C  →  alle luker åpne + vurder vifte
Temp < 18°C  →  lukk alt
Fukt > 85%   →  åpne minimum én luke uavhengig av temp
```

## Hardware

| Komponent | Detaljer |
|---|---|
| Styrekort | [LILYGO T-Relay V1.1](hardware/styrekort.md) — ESP32 + 8-kanal relé |
| Aktuatorer | [HAKIWO 12V, 300mm, IP65](hardware/aktuatorer.md) × 4 |
| Sensorer | [BME280, jordfuktighet](hardware/sensorer.md) — ikke bestilt |

Kommunikasjon: MQTT → Home Assistant

## Konstruksjon

- [Montering](konstruksjon/montering.md) — aktuatorer, styrekort, kabeltrekk
- [Vanning](konstruksjon/vanning.md) — rør, dyser, pumpe
- [Inventar](konstruksjon/inventar.md) — bed, planter

## Integrasjoner

- Home Assistant via MQTT
