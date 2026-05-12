# 012 Rev C Manual Placement

Date: 2026-05-08

## What Changed

- Created a new PCB variant:
  - `kicad/twn4_access_system/twn4_access_system_rev_c_manual_placement.kicad_pcb`
- Kept the routed Freerouting Rev C board untouched:
  - `kicad/twn4_access_system/twn4_access_system_rev_c.kicad_pcb`
- Removed autorouted copper from the manual-placement variant.
- Re-arranged footprints into more meaningful functional blocks:
  - power input/protection/buck in the upper-left area,
  - TWN4 X2 connector in the carrier area,
  - RS-485 transceiver, TVS and field connector near each other,
  - relay/lock/output section separated from low-level logic,
  - input section near field connector,
  - Raspberry Pi gateway section isolated as an optional lower block,
  - testpoints grouped into a service column.

## Why

The first routed Rev C proved that the KiCad -> DSN -> Freerouting -> SES -> KiCad workflow works and can produce a DRC-clean board. However, the placement was not electrically or mechanically sensible.

This new manual-placement variant is a better basis for a second routing pass.

## Verification

DRC report:

- `reports/rev_c_manual_placement_drc.rpt`

Result:

- DRC violations: 0
- Unconnected pads/items: 164

This is expected because the manual-placement board is intentionally unrouted.

Preview:

- `manufacturing/rev_c/rev_c_manual_placement_preview.pdf`

## Assumptions

ASSUMPTION: This is still a service prototype, not a compact final carrier outline.

ASSUMPTION: TWN4 X2 final mechanical alignment is still not verified, so this placement is electrically grouped but not mechanically final.

ASSUMPTION: The next DSN export should be done from this manual-placement board, not from the previous automatically placed board.

## Next Step

Open `twn4_access_system_rev_c_manual_placement.kicad_pcb` in KiCad 9, export Specctra DSN, run Freerouting again, import SES, then run DRC.

The expected improvement is not just a DRC-clean route, but a route based on more sensible component placement.

