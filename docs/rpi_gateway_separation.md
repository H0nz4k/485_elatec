# RPi Gateway Separation

Rev E compact carrier intentionally excludes the Raspberry Pi gateway hardware from the TWN4 carrier PCB.

## Reason

The TWN4 carrier and the RPi gateway have different mechanical and electrical roles:

- TWN4 carrier lives at the reader/door side.
- RPi gateway lives at the controller side.
- TWN4 carrier handles reader power, local I/O, lock/relay, and RS-485 bus connection.
- RPi gateway handles Raspberry Pi UART/USB interface and RS-485 master/control-panel role.

Keeping them separate makes the first hardware bring-up easier and avoids making the door-side carrier unnecessarily large.

## Rev E impact

- Rev E PCB contains the compact TWN4 carrier only.
- RPi gateway footprints inherited from older broad concepts are not placed on the compact Rev E PCB.
- A dedicated RPi gateway schematic/PCB should be created as its own revision path.

## Recommended RPi gateway Rev A

- Raspberry Pi 40-pin header or USB interface, depending on selected gateway style.
- 3.3 V RS-485 transceiver.
- SM712 or equivalent RS-485 TVS protection.
- Optional 120 ohm termination jumper.
- Optional bias jumper.
- A/B/GND connector.
- Optional VIN_BUS distribution connector.
- TX/RX/power LEDs.
