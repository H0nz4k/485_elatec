# 007 JLCPCB Export Review

Date: 2026-05-07

## Current State

The manufacturing directory exists, but it is not yet a complete order package.

Generated:

- `manufacturing/rev_b/bom/rev_b_kicad_bom.csv`

Not generated yet:

- Gerbers
- Drill files
- Pick and Place / CPL

## Reason

There is currently no validated Rev B PCB layout file. Generating manufacturing outputs without a reviewed PCB would create false confidence.

## JLCPCB Requirements To Satisfy

- Valid Gerber set.
- Valid drill files.
- BOM with exact manufacturer/orderable parts.
- CPL/Pick and Place matching footprints and reference designators.
- Assembly notes identifying hand-soldered parts.
- Known risks documented.

## Risks

- Candidate footprints are not the same as confirmed JLCPCB-available parts.
- Some high-voltage/input-protection parts may need manual assembly.
- TWN4 mating connector may be hand-assembled unless a reliable JLCPCB-available equivalent is selected.

## Next Step

Finish schematic support components, create PCB, run DRC, then export manufacturing files.

