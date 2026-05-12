# 010 Rev C Routing Cleanup Attempt

Date: 2026-05-07

## What Was Tried

An automated routing pass was attempted directly in the KiCad PCB file:

- parsed footprint pad coordinates,
- grouped pads by net,
- added vias at pad locations,
- added coarse bottom-layer trunk routes per net,
- ran DRC as `rev_c_drc_routing_pass1.rpt`.

## Result

The pass reduced unrouted items to zero, but created unacceptable electrical/layout errors:

- DRC violations: 638
- Unconnected items: 0
- Main failure types: track crossings, shorts between nets, clearance issues.

## Root Cause

The simple routing algorithm used net channels and vertical drops on one copper layer. It had no obstacle avoidance, no layer-pair strategy, no real maze routing and no current-aware routing. It therefore connected the ratsnest but produced invalid copper.

## Cleanup Performed

The bad autoroute tracks and vias were removed.

The large warning silkscreen text that caused DRC warnings was removed from the PCB and preserved in documentation instead.

After cleanup:

- DRC violations: 0
- Unconnected items: 164

Report: `reports/rev_c_drc_cleanup_pass1.rpt`

## Decision

Do not keep the automatic routing copper. A board with 638 DRC violations is worse than an unrouted board.

The current Rev C PCB is therefore intentionally left placed-but-unrouted, with clean geometry DRC and explicit unrouted state.

## Next Step

Routing must proceed block by block:

1. Power input, protection and buck.
2. TWN4 X2 power/GND and COM1.
3. RS-485 transceiver, TVS, termination and bias.
4. Lock output current path.
5. Relay and inputs.
6. Raspberry Pi gateway section.

For real autorouting, export through KiCad GUI/Specctra DSN or use an installed router. KiCad CLI in this environment does not expose DSN export and Java/freerouting is not installed.

