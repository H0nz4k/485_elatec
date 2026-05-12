# Rev C JLCPCB Package

Status: ROUTED DRC-CLEAN ENGINEERING PREVIEW - MANUAL REVIEW REQUIRED BEFORE ORDER.

## What This Folder Contains

- `gerbers/` - Gerber files exported from `twn4_access_system_rev_c.kicad_pcb`
- `drill/` - Excellon drill file and drill map
- `bom/` - BOM and assembly classification copied from the current schematic export
- `pick_place/` - CPL/position file exported from the Rev C PCB
- `rev_c_preview.pdf` - PCB preview export
- `twn4_rev_c.dsn` - Specctra file exported from KiCad
- `twn4_rev_c.ses` - Freerouting routed session imported back into KiCad
- `freerouting_pass1.log` - Freerouting run log

## Current Verification

- KiCad DRC after Freerouting import: 0 violations
- Unconnected pads after Freerouting import: 0
- Gerbers, drill and CPL regenerated from routed PCB
- A separate manual-placement PCB variant now exists as a better basis for the next routing pass:
  - `kicad/twn4_access_system/twn4_access_system_rev_c_manual_placement.kicad_pcb`
  - DRC violations: 0
  - Unconnected pads/items: 164
  - Preview: `manufacturing/rev_c/rev_c_manual_placement_preview.pdf`

## Why Manual Review Is Still Required

- Autorouter output has not been reviewed for power integrity.
- Buck regulator layout is not manually optimized.
- Lock current path is not manually width/thermal reviewed.
- RS-485 protection placement and return path are not manually reviewed.
- TWN4 mechanical X2 location is not verified.
- BOM/CPL orientation and JLCPCB part availability are not finalized.

## Ordering Instruction

Do not upload this Rev C package to JLCPCB until the manual engineering review items above are resolved or explicitly accepted as prototype risks.
