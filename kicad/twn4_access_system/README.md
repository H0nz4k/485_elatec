# KiCad: TWN4 access system

This is a concept KiCad project for two related hardware blocks:

- reader-side TWN4 carrier / door module
- Raspberry Pi-side RS-485 gateway / HAT

The project contains two schematic sheets:

- `twn4_access_system.kicad_sch` - Rev A concept drawing
- `twn4_access_system_rev_b.kicad_sch` - Rev B schematic draft with concrete symbols, named nets, candidate footprints, and both reader-side and Raspberry Pi-side hardware

The Rev B electrical design notes and preliminary BOM are documented in:

- `../../schema_electrical_rev_b.md`
- `../../bom_rev_b.csv`

Current Rev B KiCad status:

- netlist export works and contains the real schematic components/nets
- Rev B now includes explicit service-prototype passives, RS-485 termination/bias options, lock/relay/input support parts and testpoints
- ERC has no error-severity findings after the current pass
- remaining ERC messages are warnings from embedded one-off symbols and intentionally single-ended configuration nets
- no Rev B PCB has been validated yet

Next KiCad step: replace the embedded placeholder-style symbols with proper library symbols/aliases, lock exact manufacturer footprints, then create the first service PCB layout.
