# Styrekort — LILYGO T-Relay V1.1

**Bestilt:** 2026-05-25, AliExpress

| Spesifikasjon | Verdi |
|---|---|
| MCU | ESP32 (WiFi + Bluetooth) |
| Relékanaler | 8 |
| Relé | HRS4(H) — 10A/24VDC, 15A maks |
| Isolasjon | Optokoplerbasert |
| Spenning | 5V (USB eller ekstern) |
| IDE | Arduino / ESPHome |

Erstatter separat ESP32-kort og separat relemodul.

## Reléfordeling

| Kanal | Funksjon |
|---|---|
| 1 | Takluke 1 — ut |
| 2 | Takluke 1 — inn |
| 3 | Takluke 2 — ut |
| 4 | Takluke 2 — inn |
| 5 | Takluke 3 — ut |
| 6 | Takluke 3 — inn |
| 7 | Takluke 4 — ut |
| 8 | Takluke 4 — inn |

## Strømforsyning

- Kortet: 5V via USB-C
- Aktuatorer: separat 12V DC-forsyning
- Reléene kobler 12V-kretsen — ikke koblet til kortets 5V

## Referanser

- GitHub: github.com/Xinyuan-LilyGO/LilyGo-T-Relay
