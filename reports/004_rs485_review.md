# 004 RS-485 Review

Date: 2026-05-07

## Design Intent

Use RS-485 for OSDP as the first wired multi-reader path. BLE is deferred to a future revision.

## Candidate Parts

- Reader side transceiver: THVD1450
- RPi side transceiver: THVD1450
- RS-485 TVS: SM712

## Source Checks

- TI THVD1450 product page describes 3.3 V to 5 V RS-485 family with high IEC ESD protection: https://www.ti.com/product/THVD1450
- Bourns SM712 datasheet identifies the device as RS-485 port protection: https://www.bourns.com/docs/product-datasheets/cdsot23-sm712.pdf

## Assumptions

- OSDP is half-duplex RS-485.
- `DE` and `RE#` are tied to one direction-control signal for the first prototype.
- Bias and termination must be selectable so only the bus end is terminated/biased.

## Schematic Additions Made

- Added DNP 120 ohm termination option.
- Added DNP 680 ohm pull-up/pull-down bias options.
- Added A/B series resistor options.
- Added RS-485 and UART-related testpoints.

## Still Required

- Decide final jumper/DIP implementation for termination and bias.
- Verify A/B polarity on bench and silkscreen.
- Add PCB placement so TVS is physically close to field connector.

## Galvanic Isolation Decision

Recommendation for Rev B: do not add isolation yet unless field wiring length, ground potential differences, or external PSU topology demand it.

Reason: first prototype should validate TWN4 COM1 polarity, OSDP timing, bus bias/termination and power/lock behavior with fewer variables. Add isolation in Rev C after bench and site conditions are known.

## Risks

- A/B naming varies by vendor. The prototype must expose testpoints and clear silkscreen so polarity can be swapped or corrected if needed.
- TWN4 COM1 is documented as low-active TTL; polarity must be checked on an oscilloscope with firmware before final PCB release.

## Next Step

Make termination and bias explicit in schematic and PCB, with accessible jumpers.
