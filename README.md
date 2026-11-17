# Nixie HV Supply

KiCad projects for a small 5 V → ~170 V switching supply, sized to drop onto a nixie
clock board. Two layouts of the same circuit are kept here side by side.

> **Provenance:** I make no claims to these designs. I found them online and redrew them
> in KiCad so I could build them. No license, no copyright — do whatever you like with
> the files in this repo. Recently there have been problems finding a supplier for the 
> transformer used, the original sourse was Tokyo Coil if I remember correctly, but in 
> recent times, the price of these parts has become unpredeictable. Perhaps there is a 
> better shource now.

## Contents

| Folder | Project | Notes |
| --- | --- | --- |
| `Alternative1/` | `NixiePS1` | "Nixie HV power supply alternative 1" |
| `Alternative2/` | `NixiePS2` | "Nixie HV power supply alternative 2" |

Both carry the same schematic and the same bill of materials; they differ only in how
the board is routed. Pick whichever lays out better for your build.

## The board

- 25 × 20 mm, 2 copper layers, 1.6 mm FR4
- Boost/flyback converter around a MAX668 PWM controller
- `TPH1R403NL` N-channel switch, `TTRN-060S` transformer, `ES1JL` rectifier
- Output filtered by a 2.2 µF / 250 V electrolytic
- Silkscreen marks it `HVSMPS 1.0Adj`

### Connectors

| Connector | Pins | Function |
| --- | --- | --- |
| `J1` (IN) | `5V`, `GND` | Supply in |
| `J2` (OUT) | `GND`, `RT`, `170V` | HV out, plus the adjust pin |

`J2` pin 2 is `Rext`: fit a resistor from it to ground to raise the output voltage.
Left open, the board delivers roughly 170 V.

## Bill of materials

| Ref | Value | Footprint |
| --- | --- | --- |
| U3 | MAX668 | MAX668 |
| Q1 | TPH1R403NL | TPH1R403NL |
| T1 | TTRN-060S | TTRN-060S |
| D4 | ES1JL | SOD-123F |
| C1 | 220 pF | 0805 |
| C15 | 220 µF 16 V | CP_Elec_SMD_6.3×7.7 |
| C17 | 1 µF | 0805 |
| C18 | 220 nF | 0805 |
| C19 | 2.2 µF 250 V | CP_Elec_6.3×5.3 |
| R8 | 0R001 | 1206 |
| R9 | 499 k | 0805 |
| R10 | 69.8 k | 0805 |
| R11 | 10 M | 0805 |
| J1 | IN | Header MA02 (edge) |
| J2 | OUT | Header MA03 (edge) |
| FID1, FID2 | Fiducial | 0.75 mm / 1.5 mm mask |

R8 is the current-sense shunt, R9 sets the switching frequency, and R10/R11 form the
output feedback divider — `Rext` on `J2` sits across the lower leg to trim the voltage
upwards.

## Opening the files

Made with **KiCad 10**. Earlier versions will not read these files.

Symbols come from a `NixieClockMasterNew` library and footprints from `NixieClockMaster`,
which live outside this repo — `fp-lib-table` and `sym-lib-table` point at them. Without
those libraries the schematic and board still open and the cached footprints still
render, but library links will show as missing.

Autosave backups, CAM output, and generated zips are gitignored.

## Safety

The output side runs at a few hundred volts and the bulk capacitor holds charge after
power is removed. Discharge it before handling the board.
