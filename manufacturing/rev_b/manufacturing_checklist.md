# Rev B Manufacturing Checklist

## Schematic

- [x] ERC run
- [x] ERC has no error-severity findings
- [x] Netlist exports
- [x] BOM exports
- [x] Testpoints added to schematic
- [ ] All symbols migrated to stable project/library symbols
- [ ] All support values reviewed against datasheets
- [ ] All configuration nets intentionally resolved or documented

## PCB

- [ ] PCB layout created
- [ ] Board outline defined
- [ ] Mounting holes defined
- [ ] TWN4 X2 mechanical placement verified
- [ ] Buck layout reviewed
- [ ] RS-485 protection close to connector
- [ ] Lock current path reviewed
- [ ] DRC clean
- [ ] Silkscreen reviewed

## JLCPCB Outputs

- [ ] Gerbers generated
- [ ] Drill files generated
- [ ] CPL / Pick and Place generated
- [ ] BOM mapped to final MPN/LCSC parts
- [ ] Hand-solder parts listed
- [ ] Gerbers opened in viewer

## Electrical Before Order

- [ ] COM1 polarity verified on scope
- [ ] Lock current target known
- [ ] Input transient/TVS selection reviewed
- [ ] RS-485 A/B convention checked
- [ ] Termination/bias defaults selected
- [ ] Galvanic isolation deferral accepted for Rev B

