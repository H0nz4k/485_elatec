# Rev F Prototype Test Plan

## Goal

Rev F is primarily a layout audit checkpoint. Testing should focus on validating assumptions before committing to a manual final reroute.

## Checks before ordering any PCB

- Open Gerbers in a viewer.
- Confirm JLCPCB recognizes outline and copper layers.
- Inspect buck area around U2/L1/C4/C5/C6.
- Inspect lock path around J5/D2/Q4.
- Inspect relay path around K1/Q3/D5.
- Confirm X2 mechanical location with a 1:1 print or test coupon.

## Bring-up if fabricated as a fit prototype

- Visual inspection.
- Continuity checks: VIN/GND, 5V/GND, LOCK_SW_LOW/GND.
- Power at 12 V with current limit.
- Verify 5 V without TWN4.
- Verify 5 V with TWN4.
- Test RS-485 short cable only.
- Test lock output with dummy load only.

## Data to record

- VIN voltage.
- 5 V voltage and ripple.
- Idle current.
- TWN4 current.
- Lock dummy-load current.
- Buck temperature.
- MOSFET/relay/connector temperature.
- Mechanical mismatch notes.
