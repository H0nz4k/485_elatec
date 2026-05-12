# Rev F Assembly Notes

Rev F should not be assembled as a production candidate until the buck and lock/relay routing has been manually cleaned.

If a PCB is made only as a fit/bring-up experiment:

1. Assemble no lock load on first build.
2. Bring up VIN to 5 V with current limit.
3. Measure buck temperature and ripple before installing TWN4.
4. Install TWN4 only after stable 5 V is confirmed.
5. Test RS-485 locally before connecting long cable runs.
6. Test relay/lock output with dummy load before a real lock.

Do not connect a real door lock until copper width, connector current, MOSFET dissipation, and inductive protection are reviewed.
