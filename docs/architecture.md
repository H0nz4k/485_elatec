# Architecture

## Rev B Prototype Blocks

1. TWN4 carrier interface
2. Protected 12-48 V input
3. 5 V buck supply for TWN4 and local logic
4. Optional/local 3.3 V logic rail
5. RS-485 / OSDP interface
6. Dry relay output
7. Powered lock output
8. Door/REX/tamper/AUX inputs
9. Raspberry Pi RS-485 gateway/HAT concept

## Communication

Primary wired communication for Rev B is OSDP over half-duplex RS-485.

BLE is intentionally not part of the first production prototype. It remains a future feature because the first board must validate power, COM1 polarity, RS-485 and door IO first.

## Service Prototype Philosophy

The first board should be easy to measure and rework:

- large testpoints,
- jumpers for RS-485 termination and bias,
- service disconnect for lock output,
- clearly labeled field terminals,
- conservative trace widths and clearances.

