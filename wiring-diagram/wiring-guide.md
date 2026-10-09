# Bruce V2 ST7735 5-Button Wiring Guide

## Power

```text
Li-ion battery
   │
   ▼
TP4056 B+ / B-
   │
   ▼
TP4056 OUT+ / OUT-
   │
   ▼
MT3608 IN+ / IN-
   │
   ▼
MT3608 OUT = 5.0 V
   │
   ├── ESP32 5V/VIN
   └── ESP32 GND
```

## Battery ADC

```text
Battery/TP4056 OUT+
       │
     100kΩ
       │
       ├──── GPIO10
       │
     100kΩ
       │
      GND
```

At 4.2 V battery voltage, GPIO10 sees approximately 2.1 V.

## TFT

| TFT | ESP32 |
|---|---|
| LED | GPIO4 |
| SCK | GPIO5 |
| SDA/MOSI | GPIO6 |
| A0/DC | GPIO7 |
| RESET | GPIO15 |
| CS | GPIO16 |
| SDO/MISO | GPIO41 |
| VCC | 3.3 V |
| GND | GND |

## Buttons

Connect each button between its GPIO and GND. Firmware uses internal pull-ups.

- UP → GPIO9
- SELECT → GPIO11
- LEFT → GPIO12
- RIGHT → GPIO13
- DOWN → GPIO14

## NRF24L01+

- VCC → 3.3 V
- GND → GND
- CE → GPIO21
- CSN → GPIO47
- SCK → GPIO5
- MOSI → GPIO6
- MISO → GPIO41
- IRQ → disconnected in prototype

## PN532

- SDA → GPIO18
- SCL → GPIO8
- VCC → module-rated supply
- GND → GND

## GPS

- GPS TX → GPIO38
- GPS RX → GPIO17
- VCC → 3.3 V
- GND → GND
- Serial → 9600 8-N-1

## IR

### Transmitter

```text
GPIO1 → 220Ω → IR LED long leg/anode
IR LED short leg/cathode → GND
```

### Receiver

```text
OUT → GPIO2
VCC → 3.3 V
GND → GND
```

Do not use GPIO4 for IR because it is occupied by the TFT backlight.
