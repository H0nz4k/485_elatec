# Rev E Known Risks

## Mechanical

- X2 position and height must be validated on a real TWN4 reader.
- Standoff height and mounting hole positions are not final.
- Board outline is an engineering carrier outline, not final enclosure geometry.

## Power

- 12-48 V input concept is present, but buck layout must be manually reviewed.
- Switching loop geometry is not yet optimized.
- Input protection and TVS ratings should be checked against installation surge expectations.

## Lock and relay

- Real lock current is unknown.
- Copper width and connector current rating must be checked against the selected lock.
- Inductive protection should be confirmed for the final lock wiring topology.

## RS-485 / OSDP

- A/B polarity must be validated with the actual RPi gateway/control panel.
- Bias and termination policy must be confirmed for multi-reader bus topology.
- Galvanic isolation is not included in this first compact carrier pass.

## Manufacturing

- BOM is not yet a verified JLCPCB/LCSC assembly BOM.
- CPL exists but must be checked in JLCPCB preview.
- Some parts may need manual assembly.

## Documentation

- Rev E schematic was copied from the broader Rev B schematic. Compact carrier PCB intentionally excludes the RPi gateway. A later cleanup should split schematics by product board.
