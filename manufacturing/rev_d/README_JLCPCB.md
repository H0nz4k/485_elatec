# Rev D Manufacturing Package

Rev D is a routed workflow prototype, not a final order-approved production package.

## Included Files

- Routed PCB: `../../kicad/twn4_access_system/twn4_access_system_rev_d_compact_routed.kicad_pcb`
- Gerbers: `gerbers/`
- Drill: `drill/`
- BOM: `bom/rev_d_kicad_bom.csv`
- CPL: `pick_place/rev_d_cpl.csv`
- DRC report: `rev_d_compact_carrier_drc.rpt`
- Routing log: `freerouting_rev_d.log`

## Current Status

- Freerouting completed all connections.
- KiCad DRC reports 0 unconnected pads.
- Remaining DRC issues are warnings only:
  - terminal block library footprint mismatch warnings,
  - silkscreen-over-copper warnings.

## Do Not Order Yet

This package should not be sent to JLCPCB as a final production order without manual review. Main blockers:

- X2 position and mating height are not physically verified.
- Current 5.08 mm terminal blocks make the compact carrier much larger than the desired final direction.
- Buck converter routing is autorouted and should be manually reviewed/rerouted.
- Lock current path widths are not yet validated for a real electric strike/maglock load.
- BOM contains placeholder/generic parts and is not fully mapped to LCSC orderable parts.

## JLCPCB Upload Note

The full `gerbers/` folder includes extra documentation layers because it was exported from the default board layer set.

For convenience, a filtered draft upload folder exists:

- `jlcpcb_upload_draft/`

For a real JLCPCB upload, include only:

- `F_Cu.gtl`
- `B_Cu.gbl`
- `F_Mask.gts`
- `B_Mask.gbs`
- `F_Paste.gtp`
- `B_Paste.gbp`
- `F_Silkscreen.gto`
- `B_Silkscreen.gbo`
- `Edge_Cuts.gm1`
- drill `.drl`

This filtered folder is still not order-approved until the known risks are resolved.
