# Report 018 - Rev F power layout audit

## What changed

- Created Rev F branch and KiCad files from the Rev E routed checkpoint:
  - `kicad/twn4_access_system/twn4_access_system_rev_f.kicad_sch`
  - `kicad/twn4_access_system/twn4_access_system_rev_f_compact_power_cleanup.kicad_pcb`
- Added Rev F title block metadata and an F.Fab audit note:
  - `REV F AUDIT CHECKPOINT - MANUAL BUCK/LOCK REROUTE REQUIRED`
- Generated Rev F manufacturing draft outputs:
  - `manufacturing/rev_f/gerbers`
  - `manufacturing/rev_f/drill`
  - `manufacturing/rev_f/bom`
  - `manufacturing/rev_f/pick_place`
  - `manufacturing/rev_f/jlcpcb_upload_draft`
- Ran DRC after restoring the DRC-clean Rev E routing baseline with Rev F metadata.

## Why it changed

Rev E was DRC clean, but the critical power nets were still routed with autorouter-style 0.20 mm tracks. Rev F started the power-layout cleanup by testing whether the existing autorouted topology could safely support wider power traces.

## Attempted cleanup

Two widening attempts were tested and rejected:

- aggressive pass:
  - VIN nets: 0.60 mm
  - LOCK_SW_LOW: 0.80 mm
  - +5V: 0.40 mm
  - SW_NODE: 0.60 mm
  - result: 152 DRC violations
- conservative pass:
  - VIN_PROT/+5V/SW_NODE: 0.25 mm
  - result: 38 DRC violations

The violations were mostly clearance errors and some actual shorting-item reports where widened tracks crossed too close to unrelated autorouted logic traces.

## Engineering conclusion

ASSUMPTION: The existing Rev E routing is acceptable as a connectivity proof, but it is not a safe basis for simply widening traces.

The correct Rev F power cleanup is a manual reroute of the buck and lock/relay sections:

- place and route the buck loop as a local power island,
- keep the switching node compact,
- keep feedback away from noisy nodes,
- route lock current paths as short, wide, intentional copper,
- keep relay coil and lock switch paths separated,
- preserve 0.20 mm or larger clearance after the wider copper is in place.

## Verification

Final Rev F checkpoint DRC:

- `reports/rev_f_power_cleanup_drc.rpt`
- 0 DRC violations
- 0 unconnected pads
- 0 footprint errors

## Remaining risks

- Buck layout still needs manual reroute before ordering.
- Lock/relay current paths still need manual reroute before ordering.
- Connector current rating and real lock current remain unverified.
- X2/standoff geometry still requires physical validation.
- Schematic Rev F is still derived from the broader Rev E/Rev B schematic and should be split into carrier-only and RPi-gateway sheets before production.

## Next step

Open the Rev F PCB in KiCad GUI and manually reroute only these blocks:

1. buck input/protection to U2,
2. U2/L1/C5/C6 5 V power loop,
3. lock connector J5 to D2/Q4/VIN_PROT/GND return,
4. relay K1/Q3/D5 coil path.

After manual reroute, rerun DRC and update manufacturing outputs.
