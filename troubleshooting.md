# Troubleshooting Notes

## GPS timeout

Verify:

- GPS TX → ESP32 GPIO38
- GPS RX → ESP32 GPIO17
- 9600 baud, 8-N-1
- GPS supply is within the module's specified range
- NMEA data is visible in a standalone serial test

## GPS has serial data but no location update

Make sure the TinyGPSPlus parser is fed continuously. Do not stop calling the GPS update routine while waiting in a UI loop.

## Battery percentage remains 0%

Verify:

- GPIO10 is the configured battery ADC.
- Two 100 kΩ resistors form the divider.
- Divider midpoint goes to GPIO10.
- Divider bottom goes to common GND.
- Divider top is connected to the battery/load-side positive voltage, not the regulated 5 V output.
- At 4.2 V battery voltage, GPIO10 is approximately 2.1 V.

## NRF24 instability

Verify 3.3 V supply, common ground, SPI wiring and CSN/CE assignments. A local bulk capacitor near a PA/LNA NRF24 module can improve supply stability.

## ESP32 brownouts

Measure the MT3608 output while the full prototype is running. It should remain close to 5.0 V under load. Check for loose breadboard power connections and excessive voltage drop.
