# 016 Rev D GUI DSN Routing

## What Changed

- Used the GUI-exported DSN provided at `manufacturing/rev_d/twn4_rev_d_compact.dsn`.
- Routed it with Freerouting 2.2.3 and saved:
  - `manufacturing/rev_d/twn4_rev_d_compact_gui_export.ses`
  - `manufacturing/rev_d/freerouting_rev_d_gui_export.log`
- Imported the SES into a separate KiCad PCB file:
  - `kicad/twn4_access_system/twn4_access_system_rev_d_compact_gui_routed.kicad_pcb`
- Adjusted the routed board outline to clear one autorouter via near the top board edge.
- Exported preview:
  - `manufacturing/rev_d/rev_d_compact_gui_routed_preview.pdf`
  - `manufacturing/rev_d/rev_d_compact_gui_routed_preview.png`

## Why It Changed

This validates the same routing workflow using the DSN exported from KiCad GUI rather than only the DSN generated through KiCad Python API.

## Verified

- Freerouting started with 142 unrouted nets and completed with final score 976.50.
- SES import into KiCad succeeded.
- DRC after board-edge adjustment:
  - 0 unconnected pads,
  - 0 footprint errors,
  - no copper clearance errors,
  - no shorting errors.

## DRC Result

- DRC report: `reports/rev_d_compact_gui_routed_drc.rpt`.
- Remaining DRC items: 4 warnings.
  - 2 terminal-block footprint library mismatch warnings.
  - 2 silkscreen-over-copper warnings.

## Remaining Risks

- This is still a routed workflow prototype, not an order-approved board.
- Buck converter routing and lock-current paths need manual engineering review.
- X2 mechanical alignment and connector height must be verified physically.
- Field connectors remain large 5.08 mm placeholders and should be revisited for a compact Rev E.

## Next Step

- Use `twn4_access_system_rev_d_compact_gui_routed.kicad_pcb` as the routed reference generated from GUI DSN.
- If continuing toward manufacturing, manually review and reroute:
  - power input protection,
  - buck loop,
  - lock output current path,
  - RS-485 A/B and protection section.
