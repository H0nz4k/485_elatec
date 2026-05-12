# 002 Schematic Cleanup

Date: 2026-05-07

## What Changed

- Updated Rev B schematic footprint candidates where KiCad 9 had direct matching library footprints.
- Generated a local project symbol library: `kicad/twn4_access_system/rev_b_symbols.kicad_sym`.
- Generated a project symbol library table: `kicad/twn4_access_system/sym-lib-table`.
- Kept the active Rev B schematic on embedded symbols because KiCad CLI netlist export remained correct in that mode.
- Exported a KiCad BOM to `manufacturing/rev_b/bom/rev_b_kicad_bom.csv`.
- Added explicit service-prototype support parts: input capacitors, input TVS placeholder, LTC4367 divider placeholders, LM5164 feedback/RON/bootstrap/output parts, RS-485 termination/bias options, lock and relay gate/pulldown/flyback parts, input pull/filter parts and testpoints.

## Why

The previous Rev B schematic had usable named nets but was still a generated draft. The cleanup step makes it more reviewable and starts separating reusable project assets from one-off generation.

## Assumptions

- Embedded symbols are acceptable for the current prototype checkpoint because they preserve the full netlist.
- The local symbol library will be used in a later GUI cleanup pass or regenerated into a fully linked KiCad library once netlist behavior is confirmed.
- Footprints remain provisional until exact orderable manufacturer part numbers are locked.

## Verified

- Netlist export: OK
- Netlist component count: 82
- Net count: 53
- ERC errors: 0
- ERC warnings: 135

## Remaining ERC Warnings

The remaining warnings are not electrical pin-connect errors. They are mostly:

- embedded/project symbol library warnings,
- intentionally single-ended setup/configuration nets,
- intentionally DNP jumper/bias/termination options.

## Risks

- Some symbols are simplified functional symbols, not final manufacturer symbols.
- Some support values are still engineering placeholders and must be reviewed against final part numbers and measured current needs.
- A PCB generated from this exact schematic would still need a senior review before fabrication.

## Next Step

Add explicit support components and testpoints into the schematic, then create the first service-oriented PCB layout.
