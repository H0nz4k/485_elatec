# Revision History

## Rev A

Concept documentation and initial KiCad concept sheet.

## Rev B

Working production-prototype direction.

Current Rev B status:

- KiCad schematic draft exists.
- Netlist export works.
- Reports and manufacturing folder scaffold exist.
- PCB layout is not complete.
- JLCPCB package is not ready for ordering.

## Planned Rev C

Rev C engineering preview was created to close the Rev B file-output gap:

- PCB file created: `kicad/twn4_access_system/twn4_access_system_rev_c.kicad_pcb`
- Gerbers exported
- Drill file exported
- CPL exported
- BOM copied/exported into Rev C manufacturing folder
- DRC report created

Current Rev C status:

- placed but not routed
- DRC not clean because of unconnected pads/items
- not ready to order

Potential later Rev C/Rev D additions after bench validation:

- galvanically isolated RS-485,
- refined mechanical carrier,
- BLE support path if needed,
- optimized BOM/LCSC sourcing,
- production-focused layout cleanup.

## Rev D Compact Carrier

Rev D compact carrier geometry was started as a response to the oversized Rev C workflow board:

- PCB file created: `kicad/twn4_access_system/twn4_access_system_rev_d_compact_carrier.kicad_pcb`
- TWN4 50 mm x 35 mm envelope added as the primary mechanical reference.
- X2 is treated as the main mating connector.
- RPi gateway footprints were removed from this compact carrier variant and should become a separate board/schematic path.
- Preview exported: `manufacturing/rev_d/rev_d_compact_carrier_preview.pdf`

Current Rev D status:

- placement-only compact geometry pass with hard placement shorts cleared,
- not routed,
- DRC not clean because nets are still unrouted,
- not ready to order.

## Rev E Compact Carrier

Rev E is the next compact carrier iteration:

- PCB files:
  - `kicad/twn4_access_system/twn4_access_system_rev_e_compact_carrier.kicad_pcb`
  - `kicad/twn4_access_system/twn4_access_system_rev_e_compact_routed.kicad_pcb`
- Schematic copy:
  - `kicad/twn4_access_system/twn4_access_system_rev_e.kicad_sch`
- Smaller 3.50 mm Phoenix PT-1,5 style field connector footprints were used instead of the earlier 5.08 mm terminal blocks.
- Approximate board outline after cleanup: 102 mm x 72 mm.
- RPi gateway is intentionally separated from the compact TWN4 carrier.
- Routed through DSN/SES workflow and imported back into KiCad.
- Manufacturing draft files were exported under `manufacturing/rev_e`.

Current Rev E status:

- DRC clean: 0 violations, 0 unconnected pads, 0 footprint errors.
- Gerber, drill, BOM, CPL, and preview exports exist.
- Still not order-approved because buck layout, lock current paths, X2/standoff mechanics, and JLCPCB/LCSC BOM sourcing need manual review.
