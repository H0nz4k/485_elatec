# 006 PCB Layout Review

Date: 2026-05-07

## Current State

No Rev B PCB layout has been produced or validated yet.

## Layout Requirements

- Service prototype, not miniaturized.
- Large testpoints.
- Clear connector labels.
- Buck regulator placed and routed according to datasheet layout guidance.
- Input protection close to power connector.
- RS-485 TVS close to bus connector.
- Lock transient suppression close to lock connector.
- Logic and lock current returns controlled deliberately.
- Optional jumpers for RS-485 termination and bias.

## Manufacturing Assumptions

- JLCPCB standard 2-layer or 4-layer process can be used.
- For first prototype, 4-layer is preferred if cost is acceptable because it gives a cleaner ground reference for buck, RS-485 and logic.
- Use conservative clearances and trace widths, especially for VIN and lock current.

## Risks

- Mechanical X2 position is not locked.
- Connector height and standoff height are not locked.
- Current draw of selected lock is unknown.

## Next Step

Create the initial PCB only after schematic support components and footprints are explicit enough for DRC and BOM/CPL consistency.

