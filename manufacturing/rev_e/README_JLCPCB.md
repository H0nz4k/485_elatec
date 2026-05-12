# Rev E JLCPCB Draft Package

Rev E is a compact carrier prototype for the ELATEC TWN4 reader. It is closer to the desired physical product than Rev C/Rev D, but it is still an engineering prototype package.

## Files

- Gerbers: `manufacturing/rev_e/gerbers`
- JLCPCB upload draft subset: `manufacturing/rev_e/jlcpcb_upload_draft`
- Drill: `manufacturing/rev_e/drill`
- BOM: `manufacturing/rev_e/bom/rev_e_kicad_bom.csv`
- CPL: `manufacturing/rev_e/pick_place/rev_e_cpl.csv`
- DRC: `reports/rev_e_compact_routed_drc.rpt`
- Preview: `manufacturing/rev_e/rev_e_compact_routed_preview.png`

## Current status

- KiCad DRC: clean.
- Routed nets: complete.
- Manufacturing files: generated.
- Order approval: not approved yet.

## Why not order-approved yet

- Buck converter layout needs manual engineering review.
- Lock/relay current paths need manual current and thermal review.
- X2 and standoff mechanics need physical validation.
- BOM is not yet mapped to verified LCSC/JLCPCB assembly parts.
- RPi gateway has been intentionally separated from this compact carrier and should become its own schematic/PCB.

## Upload note

For a first JLCPCB Gerber import test, use `manufacturing/rev_e/jlcpcb_upload_draft`. Do not treat a successful upload preview as final production approval.
