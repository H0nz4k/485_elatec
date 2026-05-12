# Report 017 - Rev E compact connectors and routing

## What changed

- Created Rev E compact carrier PCB variant:
  - `kicad/twn4_access_system/twn4_access_system_rev_e_compact_carrier.kicad_pcb`
  - `kicad/twn4_access_system/twn4_access_system_rev_e_compact_routed.kicad_pcb`
- Created Rev E schematic copy:
  - `kicad/twn4_access_system/twn4_access_system_rev_e.kicad_sch`
- Replaced large 5.08 mm field terminal footprints with smaller 3.50 mm Phoenix PT-1,5 style terminal blocks.
- Kept RPi gateway out of the compact carrier PCB. RPi gateway is treated as a separate board path.
- Routed Rev E through Specctra/Freerouting workflow:
  - DSN: `manufacturing/rev_e/twn4_rev_e_compact.dsn`
  - SES: `manufacturing/rev_e/twn4_rev_e_compact.ses`
  - log: `manufacturing/rev_e/freerouting_rev_e.log`
- Expanded final outline to approximately 102 mm x 72 mm to clear silkscreen/edge spacing.
- Exported Gerber, drill, BOM, CPL, and preview files under `manufacturing/rev_e`.

## Why it changed

Rev D proved the compact carrier direction but was still too bulky around the external connectors. Rev E moves toward a more realistic service prototype by using smaller field connectors and a compact board envelope while keeping enough area for measurement, inspection, and first bring-up.

## Assumptions

- ASSUMPTION: 3.50 mm pitch Phoenix PT-1,5 style connectors are acceptable for the first service prototype field wiring.
- ASSUMPTION: lock current is not yet finalized, so the lock connector and copper paths are not order-approved until the actual lock load is known.
- ASSUMPTION: 102 mm x 72 mm is acceptable for Rev E as a compact engineering carrier. It is still not the final mechanical product outline.
- ASSUMPTION: RPi gateway should be separated into its own board instead of being physically mixed with the TWN4 carrier.
- ASSUMPTION: X2 location is based on available ELATEC mechanical documentation and still requires physical validation against the actual reader.

## Verification

- Freerouting completed from 142 unrouted nets to 0 unrouted nets.
- Final KiCad DRC report:
  - `reports/rev_e_compact_routed_drc.rpt`
  - 0 DRC violations
  - 0 unconnected pads
  - 0 footprint errors
- Manufacturing draft outputs exist:
  - `manufacturing/rev_e/gerbers`
  - `manufacturing/rev_e/drill`
  - `manufacturing/rev_e/bom`
  - `manufacturing/rev_e/pick_place`
  - `manufacturing/rev_e/jlcpcb_upload_draft`

## Remaining risks

- Buck converter routing is still autorouter-derived and needs manual review/reroute around input capacitor, switching node, diode/inductor, feedback, and output capacitor.
- Lock/relay high-current paths are routed but not yet manually current-reviewed against the real lock current.
- X2 connector and standoff geometry are not physically verified.
- Schematic Rev E is still derived from broader Rev B content; compact carrier PCB intentionally excludes RPi gateway footprints. A dedicated carrier-only schematic split is recommended before production.
- BOM is a KiCad export, not yet an LCSC/JLCPCB sourced assembly BOM.
- The JLCPCB package is a draft output, not an order-approved release.

## Next step

The next engineering step should be a manual Rev E cleanup pass:

- manually reroute buck power loop,
- manually reroute lock/relay current paths,
- review field connector current ratings,
- split compact carrier schematic from RPi gateway schematic,
- verify X2/standoff coordinates on a printed 1:1 paper or test PCB,
- then rerun DRC and regenerate manufacturing output.
