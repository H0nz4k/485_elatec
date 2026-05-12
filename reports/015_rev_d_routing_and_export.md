# 015 Rev D Routing And Export

## What Changed

- Exported Rev D Specctra DSN using KiCad Python API:
  - `manufacturing/rev_d/twn4_rev_d_compact.dsn`
- Routed the compact Rev D placement with Freerouting 2.2.3:
  - `manufacturing/rev_d/twn4_rev_d_compact.ses`
  - `manufacturing/rev_d/freerouting_rev_d.log`
- Imported the routed SES into a new PCB file:
  - `kicad/twn4_access_system/twn4_access_system_rev_d_compact_routed.kicad_pcb`
- Exported routed preview:
  - `manufacturing/rev_d/rev_d_compact_routed_preview.pdf`
- Exported manufacturing workflow files:
  - Gerbers: `manufacturing/rev_d/gerbers/`
  - Filtered JLCPCB draft upload set: `manufacturing/rev_d/jlcpcb_upload_draft/`
  - Drill: `manufacturing/rev_d/drill/`
  - CPL: `manufacturing/rev_d/pick_place/rev_d_cpl.csv`
  - BOM: `manufacturing/rev_d/bom/rev_d_kicad_bom.csv`

## Why It Changed

Rev D needed proof that the compact carrier placement could actually be routed. The previous Rev C proved the routing workflow on an oversized board; this pass proves the same flow on a compact reader-carrier direction.

## Assumptions

- ASSUMPTION: Freerouting output is acceptable as a routing feasibility check, not as final power/buck/EMC-quality routing.
- ASSUMPTION: The current 5.08 mm terminals are temporary workflow placeholders and may be replaced by smaller connectors in a later mechanical/product pass.
- ASSUMPTION: Rev D excludes the RPi gateway electronics; those belong on a separate board.

## Verified

- Freerouting started with 142 unrouted connections and completed with final score 976.50.
- KiCad imported the SES successfully.
- KiCad DRC after routing:
  - 0 unconnected pads,
  - 0 footprint errors,
  - no copper clearance or shorting errors after board-edge adjustment.
- Remaining DRC items are warnings only:
  - 2 terminal block library footprint mismatch warnings,
  - 2 silkscreen-over-copper warnings.

## DRC Result

- Routed DRC report: `reports/rev_d_compact_routed_drc.rpt`.
- Current routed result: 4 warnings, 0 unconnected pads.

## Remaining Risks

- Rev D is not approved for ordering yet.
- Buck converter routing needs manual review and likely manual reroute before a real PCB order.
- Power/lock current trace widths need review against target lock current.
- RS-485 A/B polarity and OSDP behavior still need bench validation.
- X2 exact position and connector height still need physical validation.
- Gerbers currently include extra documentation layers from the default export; JLCPCB upload should use only copper, mask, paste, silkscreen, edge cuts, and drill files.

## Next Step

- Open routed Rev D in KiCad 3D/PCB viewer and visually inspect:
  - X2 reader envelope alignment,
  - field connector accessibility,
  - buck loop routing,
  - lock/relay trace widths,
  - RS-485 protection location.
- For a true orderable compact board, create the next revision with final field connectors and a manual power/RS-485/lock layout.

## Follow-up

Report 016 repeats the routing pass using the DSN exported from KiCad GUI and stores the result as `twn4_access_system_rev_d_compact_gui_routed.kicad_pcb`.
