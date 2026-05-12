# Rev B Assembly Notes

## Assembly Intent

First prototype should favor serviceability over compactness.

## Likely Manual or Special-Review Parts

- TWN4 X2 mating connector until exact Samtec/JLCPCB-compatible part is locked.
- High-voltage/protection MOSFETs if selected footprint is not in JLCPCB basic/extended library.
- Terminal blocks if exact color/orientation is important.
- Relay if not available through JLCPCB assembly.

## Polarity-Sensitive Parts

- Input TVS
- RS-485 SM712
- Lock flyback diode / TVS
- Buck regulator
- LDO
- MOSFETs
- Relay coil diode

## Assembly Checks

- Confirm no solder bridges on TWN4 X2 connector.
- Confirm terminal block orientation.
- Confirm relay pinout.
- Confirm MOSFET pinout against final selected part.
- Confirm SM712 orientation and A/B/GND connection.

