# Electrical Assumptions

## Power

ASSUMPTION: Input is 12 V to 48 V nominal.

ASSUMPTION: The 5 V rail should be designed for at least 500 mA continuous reserve, preferably up to 1 A class because the LM5164 candidate supports a 1 A output class.

ASSUMPTION: Lock power is switched from protected input voltage, not generated from the 5 V rail.

## TWN4

ASSUMPTION: TWN4 MultiTech 3 BLE is powered from 5 V on X2/UVCC.

ASSUMPTION: TWN4 COM1 TX/RX low-active TTL behavior must be verified on scope before final release.

ASSUMPTION: COM2/GPIO7 are left alone for BLE module compatibility.

## RS-485

ASSUMPTION: OSDP is half-duplex and one transceiver direction signal can drive both `DE` and `RE#`.

ASSUMPTION: Only one bus end should have termination and bias enabled.

## Door IO

ASSUMPTION: Door/REX/tamper/AUX inputs are dry-contact style inputs with local pull network, RC filtering and ESD protection.

## Mechanical TODO

- Exact TWN4 X2 pin coordinate.
- Mating connector height.
- Standoff height.
- Mounting hole positions.
- Reader keep-out area.

