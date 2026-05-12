# Rev D Prototype Test Plan

1. Inspect PCB visually against the KiCad layout.
2. Check for shorts between VIN, GND, +5V, and +3V3.
3. Power VIN from a bench supply with current limit.
4. Verify protected VIN after input protection.
5. Verify +5V without TWN4 installed.
6. Verify +3V3 if populated.
7. Insert TWN4 and verify idle current.
8. Verify COM1 TX/RX activity.
9. Verify RS-485 DE/RE direction control.
10. Test OSDP communication without lock connected.
11. Test inputs using dry-contact simulation.
12. Test relay contact continuity without load.
13. Test lock output with dummy load.
14. Test lock output with real load only after current/thermal checks.
