# Rev B Prototype Test Plan

## Pre-Power

- Visual inspection.
- Continuity check VIN/GND.
- Continuity check 5V/GND.
- Continuity check 3V3/GND.
- Verify jumpers are in safe default state.

## Power

- Power from bench supply at 12 V with current limit.
- Verify protected input rail.
- Verify 5 V.
- Verify 3.3 V if assembled.
- Repeat at 24 V and 48 V after 12 V behavior is clean.

## TWN4

- Install reader only after rails are verified.
- Measure 5 V at reader.
- Confirm reader boot.
- Scope COM1 TX/RX.

## RS-485

- Check idle bus levels.
- Test with termination off and on.
- Test bias off and on.
- Verify OSDP traffic.

## IO

- Test door input.
- Test REX input.
- Test tamper input.
- Test AUX input.

## Outputs

- Test relay without load.
- Test relay with safe low-voltage dummy load.
- Test lock output without load.
- Test lock output with dummy load.
- Test lock output with real lock and current limit.

