# Rev F JLCPCB Draft Package

Rev F is an audit checkpoint generated from Rev E. It is DRC clean and has manufacturing draft outputs, but it is not order-approved.

## Files

- Gerbers: `manufacturing/rev_f/gerbers`
- JLCPCB upload draft subset: `manufacturing/rev_f/jlcpcb_upload_draft`
- Drill: `manufacturing/rev_f/drill`
- BOM: `manufacturing/rev_f/bom/rev_f_kicad_bom.csv`
- CPL: `manufacturing/rev_f/pick_place/rev_f_cpl.csv`
- DRC: `reports/rev_f_power_cleanup_drc.rpt`
- Preview PDF: `manufacturing/rev_f/rev_f_compact_power_cleanup_preview.pdf`

## Current status

- KiCad DRC: clean.
- Manufacturing draft files: generated.
- Order approval: blocked.

## Production blocker

The Rev E autorouter topology cannot be safely upgraded by globally widening power tracks. Rev F must receive a manual buck and lock/relay reroute before ordering.

## Upload note

The `jlcpcb_upload_draft` folder can be used to test JLCPCB import, layer recognition, and board outline only. Do not order from this folder as a final hardware release.
