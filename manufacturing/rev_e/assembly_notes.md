# Rev E Assembly Notes

## Assembly intent

Rev E is intended as a serviceable bring-up prototype. It should be easy to probe and debug before connecting a real lock or installing it in the field.

## Recommended assembly approach

1. Assemble passive power input protection parts first.
2. Assemble buck converter and 5 V output parts.
3. Bring up 5 V without TWN4 installed.
4. Assemble RS-485 section.
5. Assemble TWN4 X2 connector.
6. Assemble inputs, relay, and lock section last.

## Manual assembly candidates

- Field terminal blocks
- Any high-current lock/relay connectors
- TWN4 mating connector
- Jumpers or configuration headers

## Polarity checks

Check before first power:

- VIN polarity
- TVS polarity where applicable
- diode polarity
- electrolytic/tantalum capacitor polarity if used
- RS-485 A/B labeling
- relay/lock connector labeling
- TWN4 X2 pin 1 orientation

## Do not do on first power

- Do not connect a real lock on first power.
- Do not install TWN4 before verifying the 5 V rail.
- Do not connect the board to a long RS-485 cable before local communication testing.
