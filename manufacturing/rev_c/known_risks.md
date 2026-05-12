# Rev C Known Risks

## Critical

- TWN4 connector mechanics are not validated.
- Autorouted PCB has not had manual senior layout review.

## High

- CPL coordinates come from automatic placement and need manual review.
- Board dimensions are service-preview dimensions, not final enclosure dimensions.
- Lock current path is routed but not width/thermal reviewed.
- Buck regulator route exists but is not manually optimized against datasheet layout guidance.
- RS-485 TVS/protection physical placement needs review.

## Medium

- Silkscreen has warning/overlap findings.
- BOM still needs final MPN/LCSC mapping.
- Some parts may require hand assembly.
