# TWN4 MultiTech 3 BLE Mechanical Notes

Source: `TWN4DevPack507.zip`, extracted document `_extracted_docs/TWN4 MultiTech 3 DocRev10.pdf`, sections 2.2 and 2.3.2.

## Extracted Dimensions

- Reader PCB envelope: 50.00 mm x 35.00 mm.
- Figure 2.3 top view also shows reference dimensions 12.50 mm, 10.00 mm, 16.50 mm, 11.20 mm, 20.00 mm, and 2.80 mm around component and mounting geometry.
- Figure 2.3 bottom view shows additional reference dimensions 22.80 mm, 8.00 mm, and USB connector offset 4.00 mm.
- X2 generic interface:
  - double row header/socket,
  - 2.00 mm pitch,
  - 2 x 12 positions,
  - example populated connector: Samtec `PTT-124-01-S-D`,
  - pin orientation shown in Figure 2.5, with pin 1/2 at one end and pin 23/24 at the opposite end.

## Rev D Layout Assumptions

- ASSUMPTION: The compact carrier should use the TWN4 50.00 mm x 35.00 mm body as the primary mechanical envelope.
- ASSUMPTION: The reader should plug into the carrier through X2; the carrier PCB may extend to one side/bottom for field wiring.
- ASSUMPTION: The RPi gateway electronics are not part of this compact reader carrier. They should become a separate PCB or separate schematic variant.
- ASSUMPTION: Current 5.08 mm terminal blocks are too large for a carrier only slightly bigger than the reader. Rev D keeps them as workflow placeholders, but a real compact layout should move to smaller pluggable connectors, likely 3.50 mm or 2.50/2.54 mm depending on current and installer ergonomics.

## Open Mechanical Risks

- The exact X2 pin-1 center coordinate relative to the 50 x 35 mm TWN4 body still needs physical confirmation against a real reader, a printed 1:1 drawing, or an original mechanical CAD/DXF if ELATEC provides one.
- Connector mating height is not yet fixed. A carrier socket/pin solution must be chosen before production.
- Mounting hole/standoff geometry is not yet implemented in PCB.
- Component height under the reader body is not validated. Keep tall components in the side/bottom extension unless the reader standoff height is known.
