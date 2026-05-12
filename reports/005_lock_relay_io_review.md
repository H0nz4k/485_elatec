# 005 Lock Relay IO Review

Date: 2026-05-07

## Design Intent

Rev B should support:

- dry relay contact for external door controller input,
- powered lock output from protected input rail,
- door/REX/tamper/AUX inputs for field wiring.

## Assumptions

- Lock current is unknown, so the first prototype must be conservative and serviceable.
- Powered lock output should be switchable from `VIN_PROT`, not from 5 V.
- Relay output is a dry contact only.

## Schematic Additions Made

- Added lock service link.
- Added lock gate resistor and pulldown.
- Added relay gate resistor and pulldown.
- Added relay flyback diode.
- Added input pull-up and RC filter placeholders.
- Added testpoints for lock, relay and inputs.

## Still Required

- Select final lock flyback/TVS strategy after lock type and current are known.
- Add explicit ESD/TVS input arrays near input connector.
- Confirm MOSFET current/thermal margin against target lock.

## Risks

- Door strikes and maglocks can have very different transient behavior.
- If the lock shares supply wiring with logic, poor return routing can reset TWN4 or corrupt RS-485.
- Long input cables need ESD and filtering close to the connector.

## Next Step

Promote these support parts from notes into real schematic components and keep the lock power path physically away from logic on PCB.
