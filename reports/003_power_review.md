# 003 Power Review

Date: 2026-05-07

## Design Intent

Input range target: 12 V to 48 V nominal.

The Rev B concept uses:

- protected input rail `VIN_PROT`,
- 5 V buck rail for TWN4 and relay/logic support,
- optional/local 3.3 V rail for IO expansion if needed.

## Candidate Parts

- Input protection controller: LTC4367
- Buck regulator: LM5164
- 3.3 V LDO: TLV75533

## Source Checks

- TI LM5164 product page confirms 6 V to 100 V input capability and 1 A max output class: https://www.ti.com/product/LM5164
- Analog Devices LTC4367 product page confirms 2.5 V to 60 V operating range, overvoltage protection to 100 V, reverse supply protection, and control of back-to-back N-MOSFETs: https://www.analog.com/en/products/ltc4367.html

## Assumptions

- TWN4 plus local electronics stay below roughly 500 mA on 5 V in normal operation.
- Door lock power is not taken from the 5 V rail; it is switched from protected input voltage.
- First prototype is service/debug oriented, so efficiency and board size are secondary to robust measurement and rework.

## Schematic Additions Made

- Added input bulk/HF capacitors.
- Added input TVS placeholder.
- Added LTC4367 UV/OV divider placeholders.
- Added LM5164 RON, feedback divider, bootstrap capacitor, input capacitor and output capacitors.
- Added 3.3 V LDO input/output capacitors.
- Added power testpoints.

## Still Required

- Final input TVS part number for 48 V nominal environment.
- Final LTC4367 divider calculation.
- Final LM5164 values checked against the exact switching target and load.
- Thermal/current review after exact lock and reader current are known.

## Risks

- 48 V nominal systems can have transients above nominal; TVS selection must not clamp during normal operation but must protect downstream parts.
- LM5164 layout is critical. The high di/dt loop must be compact, with SW node kept small.
- Lock transients can inject noise into VIN_PROT if flyback/TVS placement is poor.

## Next Step

Add explicit power passives and testpoints before PCB layout.
