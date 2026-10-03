# Protokół BLE

## FTMS

T900D udostępnia standardowy serwis:

```text
0x1826 Fitness Machine Service
0x2ACD Treadmill Data
```

Konfiguracja parsuje:
- Instantaneous Speed,
- Total Distance,
- Inclination,
- Ramp Angle,
- Expended Energy,
- Heart Rate,
- MET,
- Elapsed Time.

Dystans FTMS jest raportowany jako `uint24` w metrach.

ESP32 dodatkowo całkuje prędkość w czasie jako fallback, zanim pojawi się pierwszy niezerowy dystans FTMS.

## Garmin RSC

ESP32 wystawia:

```text
0x1814 Running Speed and Cadence
0x2A53 RSC Measurement
0x2A54 RSC Feature
0x2A5D Sensor Location
```

Pakiet zawiera prędkość i całkowity dystans. Kadencja jest ustawiona na 0, ponieważ T900D jej nie udostępnia.

## Vendor Domyos BLE/LCD

Na testowanym T900D dostępny był serwis:

```text
Service:
49535343-FE7D-4AE5-8FA9-9FAFD205E455

WRITE:
49535343-8841-43F4-A8D4-ECBE34729BB3

NOTIFY:
49535343-1E4D-4BD9-BA61-23C647249616
```

Projekt używa ramki `F0 CD 01 ...`.

Ramka ma 27 bajtów. Dystans jest kodowany w bajtach 3–4 jako `km × 10`, a ostatni bajt jest 8-bitową sumą kontrolną pierwszych 26 bajtów.

Ramka jest wysyłana jako:
- pierwsze 20 bajtów,
- 150 ms przerwy,
- pozostałe 7 bajtów.

Po połączeniu cała operacja wykonywana jest 5 razy w odstępach 1,5 s.

Na testowanym egzemplarzu T900D dopiero taka seria powodowała stabilne zachowanie: FTMS działał, a konsola wracała do normalnego wyświetlania.

## Czego projekt nie robi

Projekt nie używa standardowego FTMS Control Point do:
- ustawiania prędkości,
- ustawiania nachylenia,
- START/STOP,
- pauzy/wznowienia.

Nie należy zakładać, że vendorowa ramka działa identycznie na wszystkich wersjach T900D.
