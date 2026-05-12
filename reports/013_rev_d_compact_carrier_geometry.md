# 013 Rev D Compact Carrier Geometry

## What Changed

- Created `kicad/twn4_access_system/twn4_access_system_rev_d_compact_carrier.kicad_pcb`.
- Added a compact working board outline of 110 mm x 70 mm.
- Added a 50 mm x 35 mm TWN4 reference envelope on the fabrication layer.
- Positioned X2 as the primary reader mating reference.
- Removed the RPi gateway footprints from this compact carrier PCB variant because the RPi side should be a separate hardware module.
- Exported a visual preview to `manufacturing/rev_d/rev_d_compact_carrier_preview.pdf` and `manufacturing/rev_d/rev_d_compact_carrier_preview.png`.

## Why It Changed

The previous Rev C routed board proved that KiCad -> DSN -> Freerouting -> SES -> KiCad works, but it was physically too large and not aligned with the intended product shape. Rev D starts a compact carrier direction: reader body first, field wiring in a controlled extension.

## Assumptions

- ASSUMPTION: TWN4 MultiTech 3 BLE body envelope is 50.00 mm x 35.00 mm from ELATEC Figure 2.3.
- ASSUMPTION: X2 uses a 2 x 12, 2.00 mm pitch mating connector based on ELATEC Figure 2.5 and Samtec `PTT-124-01-S-D`.
- ASSUMPTION: For compact Rev D, RPi gateway hardware is separate from the reader carrier.
- ASSUMPTION: The existing 5.08 mm terminal block footprints are placeholders for compact layout exploration. A production compact carrier likely needs smaller field connectors.

## Verified

- KiCad 9.0.7 can load the Rev D PCB file.
- KiCad CLI can export the Rev D preview PDF and PNG.
- ELATEC mechanical page renderings were generated under `docs/mechanical_extract/`.

## DRC Result

- DRC report: `reports/rev_d_compact_carrier_drc.rpt`.
- Manufacturing-side copy: `manufacturing/rev_d/rev_d_compact_carrier_drc.rpt`.
- Superseded by report 014 after placement cleanup.
- Original first-pass result: 77 violations, including placement/courtyard/clearance issues and unconnected items.
- Updated result after report 014: no hard shorting placement violations; remaining blockers are unrouted nets and non-critical silkscreen/library warnings.

## Remaining Risks

- X2 placement must be verified physically before any PCB order.
- The current 5.08 mm terminal blocks consume too much board edge for the desired compact form factor.
- Relay, lock output, and power input spacing still need a real manufacturable arrangement.
- No routing has been done for Rev D.
- No Gerber/Drill/CPL manufacturing package should be generated from this Rev D placement yet.

## Next Step

- Decide whether Rev D keeps 5.08 mm field terminals and grows slightly, or switches to smaller pluggable terminal blocks.
- Confirm X2 coordinate and mating height.
- Replace placeholder compact placement with a routed board floorplan:
  - X2 and reader keepout fixed first,
  - power input and buck in one corner,
  - RS-485 connector/protection together at the edge,
  - lock/relay output separated from logic,
  - inputs on a low-current connector group.
