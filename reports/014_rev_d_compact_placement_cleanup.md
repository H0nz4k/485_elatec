# 014 Rev D Compact Placement Cleanup

## What Changed

- Continued compact Rev D placement cleanup in `kicad/twn4_access_system/twn4_access_system_rev_d_compact_carrier.kicad_pcb`.
- Increased the working board outline from 110 mm x 70 mm to 116 mm x 88 mm.
- Moved power protection, buck support parts, RS-485 connector area, relay/lock protection parts, and field connectors to remove hard pad collisions.
- Updated preview exports:
  - `manufacturing/rev_d/rev_d_compact_carrier_preview.pdf`
  - `manufacturing/rev_d/rev_d_compact_carrier_preview.png`
- Copied the updated DRC report into `manufacturing/rev_d/rev_d_compact_carrier_drc.rpt`.

## Why It Changed

The first Rev D compact geometry pass still had physical shorts and courtyard overlaps caused by using large 5.08 mm terminal blocks and placeholder production footprints in a compact carrier. This pass focused on making the placement electrically non-destructive before attempting routing.

## Assumptions

- ASSUMPTION: Rev D remains a placement/routing exploration, not a production release.
- ASSUMPTION: Keeping the current 5.08 mm terminal blocks is acceptable for this intermediate pass, even though they make the board larger than the final desired compact carrier.
- ASSUMPTION: Final compact hardware should evaluate smaller field connectors before committing to PCB manufacturing.

## Verified

- KiCad 9.0.7 can load the updated Rev D PCB.
- KiCad CLI DRC runs successfully.
- KiCad CLI PDF export runs successfully.
- Preview PNG was regenerated from the exported PDF.

## DRC Result

- DRC report: `reports/rev_d_compact_carrier_drc.rpt`.
- Current result: 12 non-unconnected DRC items plus 142 unconnected pads.
- Critical placement shorts are cleared:
  - `shorting_items`: 0
  - remaining non-routing items are silkscreen warnings and library footprint mismatch warnings.
- The board is still unrouted.

## Remaining Risks

- Rev D is not ready for manufacturing.
- The current outline is 116 mm x 88 mm, which is still larger than the desired final compact product direction.
- Large field terminal footprints dominate the board size.
- X2 mechanical position and connector mating height still require physical verification.
- DSN export for Freerouting requires KiCad GUI because KiCad CLI 9.0.7 does not expose a Specctra DSN export command.

## Next Step

- In KiCad GUI, export Rev D as Specctra DSN:
  - open `twn4_access_system_rev_d_compact_carrier.kicad_pcb`,
  - use `Soubor -> Export -> Specctra DSN`,
  - save to `manufacturing/rev_d/twn4_rev_d_compact.dsn`.
- Run Freerouting against that DSN and import the resulting SES.
- After routing, run DRC again and decide whether the large terminal block strategy is acceptable or whether Rev E should switch to smaller connectors.
