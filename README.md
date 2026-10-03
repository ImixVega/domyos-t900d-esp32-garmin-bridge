# Domyos T900D → ESP32 → Garmin / Home Assistant

Mostek BLE dla bieżni **Domyos T900D**, zbudowany na **ESP32-C3 + ESPHome**.

ESP32 łączy się z bieżnią jako klient BLE, odczytuje dane **FTMS**, publikuje je do Home Assistant i jednocześnie wystawia wirtualny czujnik **Running Speed and Cadence (RSC)** dla Garmina.

## Funkcje

- automatyczne połączenie z T900D po uruchomieniu,
- automatyczny reconnect,
- prędkość,
- dystans FTMS,
- nachylenie i kąt rampy,
- energia: kcal, kcal/h, kcal/min,
- tętno,
- MET,
- czas treningu i `HH:MM:SS`,
- lokalny fallback dystansu liczony przez ESP32,
- wirtualny czujnik Garmin RSC,
- Home Assistant przez ESPHome API,
- automatyczna sekwencja BLE/LCD po połączeniu.

## Architektura

```text
Domyos T900D
   │
   │ BLE / FTMS (0x1826 / 0x2ACD)
   ▼
ESP32-C3 + ESPHome
   ├──► Home Assistant
   └──► Garmin RSC (0x1814)
```

ESP32 może być zasilany z USB bieżni, dzięki czemu startuje razem z T900D.

## Wymagania

- Domyos T900D,
- ESP32-C3 DevKitM-1,
- ESPHome,
- Home Assistant,
- opcjonalnie Garmin obsługujący Running Speed/Cadence.

Projekt był rozwijany na ESPHome 2026.8.2.

## Instalacja

1. Skopiuj `secrets.yaml.example` jako `secrets.yaml`.
2. Uzupełnij własne dane Wi-Fi, hasło OTA, MAC T900D i adresację IP.
3. Dodaj `esp32-bieznia.yaml` do ESPHome.
4. Zweryfikuj i skompiluj konfigurację.
5. Wgraj firmware do ESP32-C3.
6. W Garminie dodaj czujnik typu Running Speed/Cadence.

Szczegóły: [docs/INSTALLATION.md](docs/INSTALLATION.md)

## BLE/LCD

Po połączeniu projekt czeka 1,5 s, a następnie pięć razy wykonuje zapis `F0 CD` w odstępach 1,5 s. Na testowanym egzemplarzu T900D taka sekwencja pozwala zachować aktywną komunikację BLE/FTMS, a jednocześnie bieżnia wraca do normalnego wyświetlania zamiast pozostawać w ekranie BLE.

To zachowanie zostało potwierdzone praktycznie na jednym egzemplarzu T900D i może zależeć od wersji firmware bieżni.

Projekt nie używa FTMS Control Point do ustawiania prędkości, nachylenia ani START/STOP.

Szczegóły: [docs/PROTOCOL.md](docs/PROTOCOL.md)

## Dane wrażliwe

Repozytorium nie zawiera prawdziwego:
- SSID ani hasła Wi-Fi,
- hasła OTA,
- MAC bieżni,
- prywatnej adresacji IP.

Plik `secrets.yaml` jest wykluczony przez `.gitignore`.

## Uwaga

To projekt hobbystyczny / reverse-engineering. Używasz go na własną odpowiedzialność. Testy zapisu do vendorowego kanału Domyosa wykonywano przy zatrzymanym pasie.

## Licencja

Licencja nie została jeszcze wybrana.
