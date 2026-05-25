# Trinn 1 — Manuell lukestyring fra Home Assistant

**Mål:** åpne og lukke de fire taklukene manuelt fra HA-dashbord. Ingen automatikk ennå.

**Forutsetning:** T-Relay og HAKIWO-aktuatorer leveres og monteres.

---

## Tilkobling

### Avklaring først: test WiFi-signal

Mål signalstyrken ved drivhuset med telefon (f.eks. WiFi Analyzer-app). Grense for pålitelig ESP32-drift er omtrent -70 dBm.

| Signal | Tiltak |
|---|---|
| Bedre enn -70 dBm | Bruk WiFi direkte |
| -70 til -80 dBm | Vurder ekstern antenne på ESP32 |
| Svakere enn -80 dBm | Trekk Ethernet-kabel |

### Alternativ A — WiFi direkte
ESP32 på T-Relay kobler seg til eksisterende WiFi og snakker MQTT mot HA.
- Ingen ekstra hardware
- ESPHome håndterer reconnect automatisk

### Alternativ B — Fiber + RPi som WiFi-AP (anbefalt permanent løsning)

Fiber mellom bygninger er riktig valg på gård med flere hus:
- **Ingen jordsløyfer** — ulike bygninger har ofte ulikt jordpotensial, kobber-Ethernet kan ødelegge utstyr
- **Lynsikker** — fiber er ikke-ledende, et lynnedslag skader ikke utstyr i andre bygg
- **Avstand** — fungerer til 550m, ingen 100m-begrensning som kobber

```
HA ←→ [Mediakonverter] ~~fiber 10m~~ [Mediakonverter] ←→ RPi ←→ WiFi (lokalt) ←→ T-Relay
```

RPi kobles til mediakonverter via Ethernet og kjører `hostapd` som lokalt WiFi-nettverk.

#### Utstyr som trengs

| Komponent | Antall | Spesifikasjon |
|---|---|---|
| Fiber, armert direct burial | 1 stk | LC-LC, OM3 multimode, 15m (kjøp litt ekstra) |
| Mediakonverter | 2 stk | Gigabit, 1000Base-SX, LC multimode, 850nm |
| RPi (ledig) | 1 stk | Ethernet + WiFi |

#### Innkjøpsalternativer

**AliExpress** (billigst, 3–4 uker):
- Kabel: søk `LC LC armored outdoor fiber patch cable OM3 15m direct burial` — ca. 200 kr
- Konvertere: søk `gigabit fiber media converter 1000Base-SX LC multimode` × 2 — ca. 100–150 kr/stk
- **Totalt: ~400–500 kr**

**FS.com** (anbefalt, EU-lager, 3–7 dager):
- Kabel + 2 konvertere tilgjengelig fra fs.com
- **Totalt: ~500–850 kr**

**Lokalt i Norge** (1–2 dager):
- TP-Link MC220L (Komplett/NetOnNet, ~350–400 kr/stk) + SFP-modul (~100–150 kr/stk) × 2
- Kabel fra FS.com
- **Totalt: ~900–1200 kr**

**Velg A om WiFi-signalet er bedre enn -70 dBm. Ellers velg B — det er uansett den permanente løsningen for gården.**

---

## Programvarestack

**ESPHome** på T-Relay. Grunner:
- Offisiell støtte for T-Relay (github.com/Xinyuan-LilyGO/LilyGo-T-Relay)
- Innebygd HA-integrasjon — ingen manuell MQTT-konfigurasjon
- `cover`-komponent gir åpne/lukk/stopp direkte i HA
- Håndterer WiFi-reconnect og OTA-oppdatering

### Reléfordeling

| Relé | GPIO | Funksjon |
|---|---|---|
| 1 | TBD | Takluke 1 — ut (åpne) |
| 2 | TBD | Takluke 1 — inn (lukke) |
| 3 | TBD | Takluke 2 — ut |
| 4 | TBD | Takluke 2 — inn |
| 5 | TBD | Takluke 3 — ut |
| 6 | TBD | Takluke 3 — inn |
| 7 | TBD | Takluke 4 — ut |
| 8 | TBD | Takluke 4 — inn |

GPIO-numre fylles inn fra T-Relay-skjemaet ved konfigurasjon.

### ESPHome-konfigurasjon (skisse)

```yaml
switch:
  - platform: gpio
    id: relay_1_ut
    pin: GPIOXX
    interlock: [relay_1_inn]   # forhindrer kortslutning
  - platform: gpio
    id: relay_1_inn
    pin: GPIOXX
    interlock: [relay_1_ut]

cover:
  - platform: template
    name: "Takluke 1"
    open_action:
      - switch.turn_on: relay_1_ut
    close_action:
      - switch.turn_on: relay_1_inn
    stop_action:
      - switch.turn_off: relay_1_ut
      - switch.turn_off: relay_1_inn
```

`interlock` sikrer at begge reléer aldri kan være aktive samtidig — håndteres i firmware, ikke bare i software.

Samme mønster gjentas for luke 2–4.

---

## HA-dashbord

Fire `cover`-entiteter gir dette i HA:

```
[▲ Åpne]  [■ Stopp]  [▼ Lukk]   Takluke 1
[▲ Åpne]  [■ Stopp]  [▼ Lukk]   Takluke 2
[▲ Åpne]  [■ Stopp]  [▼ Lukk]   Takluke 3
[▲ Åpne]  [■ Stopp]  [▼ Lukk]   Takluke 4
```

Grensebryterne i aktuatorene stopper ved endepunkt — HA trenger ikke vite om det.

---

## Sjekkliste

### Forberedelse
- [ ] Mål WiFi-signal ved drivhuset (WiFi Analyzer-app, grense -70 dBm)
- [ ] Avgjør tilkoblingsmetode (A: WiFi direkte / B: fiber + RPi)
- [ ] Hvis B: bestill fiber og mediakonvertere (FS.com eller AliExpress)
- [ ] Finn GPIO-numre for reléene fra T-Relay-skjema

### Hardware
- [ ] Monter aktuatorer på takluker
- [ ] Trekk kabel fra 12V-forsyning til T-Relay og aktuatorer
- [ ] Koble aktuatorer til reléterminaler på T-Relay
- [ ] Test aktuatorbevegelse manuelt (12V direkte) før ESPHome

### Firmware
- [ ] Installer ESPHome (CLI eller HA add-on)
- [ ] Skriv konfigurasjon for T-Relay med 4 × cover
- [ ] Flash via USB første gang
- [ ] Verifiser at alle fire luker svarer i HA

### Verifisering
- [ ] Åpne og lukk hver luke fra HA
- [ ] Verifiser at grensebryterne stopper aktuatoren ved endepunkt
- [ ] Verifiser at interlock hindrer kortslutning

---

## Neste steg — Trinn 2

Når manuell styring fungerer:
- Legg til BME280-sensor i ESPHome-konfigurasjonen
- Implementer automatisk åpning basert på temperatur og fuktighet
- Ingen hardware-endringer nødvendig
