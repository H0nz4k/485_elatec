# Bring-Up Plan

## 1. Bare PCB Inspection

- Inspect solder mask, drill registration and silkscreen.
- Check polarity markings.
- Check connector orientation.
- Check there are no visible copper shorts.

## 2. Resistance Checks

- Measure VIN to GND.
- Measure 5 V to GND.
- Measure 3.3 V to GND.
- Measure RS485_A to RS485_B.
- Verify termination jumper state before applying power.

## 3. First Power

- Use a bench supply with current limit.
- Start at 12 V input.
- Current limit initially low.
- Power without TWN4 installed.
- Confirm 5 V rail.
- Confirm 3.3 V rail if fitted.

## 4. TWN4 Power Test

- Install TWN4.
- Power at 12 V with current limit.
- Measure 5 V at TWN4 connector.
- Check for reset/brownout behavior.

## 5. UART / RS-485 Test

- Probe COM1 TX/RX polarity.
- Probe DE/RE.
- Check RS-485 A/B idle level.
- Test OSDP traffic at low baud first.

## 6. Inputs

- Test each dry-contact input.
- Confirm input filtering delay is acceptable.
- Confirm no false triggers during RS-485 traffic and relay switching.

## 7. Relay

- Switch relay without external load.
- Measure coil current.
- Confirm flyback suppression behavior.

## 8. Lock Output

- Test lock output with no load.
- Test with dummy load and current limit.
- Test with real lock only after dummy-load behavior is clean.

## 9. Record Results

Create a bench log with:

- supply voltage,
- current draw,
- measured rails,
- thermal observations,
- scope screenshots of RS-485 and lock switching,
- firmware version.

