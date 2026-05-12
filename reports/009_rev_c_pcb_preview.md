# 009 Rev C PCB Preview

Date: 2026-05-07

## What Changed

- Created `kicad/twn4_access_system/twn4_access_system_rev_c.kicad_pcb`.
- Rev C PCB is generated from the Rev B schematic netlist/BOM.
- Added a service-style board outline and placed all 82 schematic components as physical footprints.
- Added engineering-preview silkscreen warnings directly on PCB.

## Why

Rev B was blocked because it had no PCB, no DRC, no Gerbers, no drill files and no CPL. Rev C starts filling those gaps while keeping the manufacturing risk visible.

## Assumptions

ASSUMPTION: Rev C is an engineering preview, not an orderable routed prototype.

ASSUMPTION: Footprints are good enough for first KiCad export validation, but not final enough for purchase or assembly without human review.

ASSUMPTION: Mechanical TWN4 X2 placement is still unknown, so the PCB cannot be treated as mechanically validated.

## Verified

- KiCad PCB file loads through KiCad CLI.
- DRC runs.
- Gerber export runs.
- Drill export runs.
- Position/CPL export runs.
- PDF preview export runs.

## DRC Result

- DRC violations: 9
- Unconnected pads/items: 164

The current DRC violations are silkscreen warnings/overlaps and unrouted connections. There are no remaining clearance/courtyard collisions after moving the Raspberry Pi section.

## Risks

- The board is placed but not routed.
- DRC is not clean.
- Schematic parity has not been passed.
- Gerbers exist, but must not be ordered.
- CPL exists, but placement has not been manually reviewed.

## Next Step

Route the PCB deliberately by functional block:

1. Power input/protection/buck.
2. TWN4 X2 and 5 V/GND.
3. RS-485 transceiver, TVS, termination and bias.
4. Lock output current path.
5. Relay and IO.
6. Raspberry Pi gateway section.

