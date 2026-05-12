# Rev F Known Risks

## Critical blockers

- Buck converter is still routed as an autorouter-derived layout.
- Lock/relay power routing has not been manually current-reviewed.
- Wider power traces cannot be applied safely without manual rerouting because the existing topology has tight clearances.

## Mechanical risks

- X2 position and height must be validated against a real TWN4 reader.
- Standoff height and mounting holes are not final.
- Board outline remains an engineering carrier outline.

## Electrical risks

- Real lock current is unknown.
- 3.50 mm field connector current rating must be checked against the selected lock and cable.
- Input protection ratings should be checked against expected installation surges.
- RS-485 bias/termination policy must be validated in the final bus topology.

## Manufacturing risks

- BOM is a KiCad export, not a sourced LCSC/JLCPCB assembly BOM.
- CPL must be reviewed in JLCPCB preview.
- Some connectors and the TWN4 mating connector may require manual assembly.
