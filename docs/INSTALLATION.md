# Instalacja

## 1. Przygotowanie secrets.yaml

Skopiuj:

```text
secrets.yaml.example -> secrets.yaml
```

Uzupełnij:

```yaml
wifi_ssid: "..."
wifi_password: "..."
ota_password: "..."
t900d_mac: "AA:BB:CC:DD:EE:FF"
device_ip: "192.168.1.123"
gateway_ip: "192.168.1.1"
subnet_mask: "255.255.255.0"
```

Jeżeli nie chcesz statycznego IP, usuń blok `manual_ip` z konfiguracji.

## 2. Wgranie

1. Dodaj `esp32-bieznia.yaml` do ESPHome.
2. Uruchom **Validate**.
3. Skompiluj.
4. Pierwsze wgranie wykonaj przez USB.
5. Kolejne mogą być wykonywane OTA.

## 3. Start

Po podaniu zasilania ESP32:
1. skanuje BLE,
2. znajduje T900D,
3. łączy się,
4. subskrybuje FTMS,
5. wykonuje sekwencję BLE/LCD,
6. publikuje dane do Home Assistant i Garmina.

ESP32 może być zasilany z portu USB bieżni, dzięki czemu startuje razem z nią.

## 4. Garmin

ESP32 wystawia:
- `0x1814` Running Speed and Cadence,
- `0x2A53` RSC Measurement,
- `0x2A54` RSC Feature,
- `0x2A5D` Sensor Location.

W Garminie dodaj czujnik **Running Speed/Cadence**.

## 5. Home Assistant

Publikowane są m.in.:
- T900D Prędkość,
- T900D Tempo,
- T900D Dystans FTMS,
- T900D Dystans ESP,
- T900D Dystans Garmin,
- T900D Nachylenie,
- T900D Kąt rampy,
- T900D Energia,
- T900D Tętno,
- T900D MET,
- T900D Czas treningu,
- T900D Czas treningu HH:MM:SS,
- T900D Połączona.

## 6. Pierwszy test

Przed pierwszym testem zapisu vendorowego:
- pas powinien być zatrzymany,
- T900D powinna być połączona,
- sprawdź log ESPHome.

Przycisk `T900D Aktywuj BLE/LCD` można uruchomić ręcznie w Home Assistant. Ta sama operacja jest wykonywana automatycznie pięć razy po połączeniu.
