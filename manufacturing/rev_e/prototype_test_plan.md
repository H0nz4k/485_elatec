# Rev E Prototype Test Plan

## 1. Bare PCB inspection

- Confirm board outline and connector positions.
- Check silkscreen labels.
- Inspect X2 connector footprint orientation.
- Inspect copper around buck and lock paths.

## 2. Continuity checks

- VIN to GND: no short.
- 5V to GND: no short.
- RS485_A to RS485_B: no unexpected short.
- Lock output to GND/VIN: no unexpected short.

## 3. Power bring-up without TWN4

- Use bench supply with current limit.
- Start at 12 V input.
- Confirm 5 V output.
- Confirm buck does not heat abnormally.
- Repeat at 24 V input.
- Repeat at 48 V input only after 12 V and 24 V are stable.

## 4. TWN4 bring-up

- Install TWN4 after 5 V has been verified.
- Confirm 5 V does not sag.
- Confirm reader boots and enumerates/communicates as expected.

## 5. RS-485 / OSDP test

- Probe TX, RX, DE/RE.
- Verify A/B polarity.
- Test communication over short cable first.
- Enable termination only at bus end.
- Enable bias only at the intended master/control-panel point unless topology requires otherwise.

## 6. Inputs

- Test each input locally.
- Confirm pull-up/pull-down logic.
- Confirm software interpretation.

## 7. Relay and lock

- Test relay/lock output without real lock first.
- Test with dummy load.
- Test real lock through current-limited supply.
- Check heating on MOSFET/relay path, connector, and copper.

## 8. Result log

Record:

- VIN voltage
- idle current
- TWN4 active current
- RS-485 communication result
- lock current
- measured temperatures after 5 min lock tests
- any mechanical mismatch around X2/standoffs
