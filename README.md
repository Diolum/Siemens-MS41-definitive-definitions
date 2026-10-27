# MS41 RomRaider Definition

**The definitive RomRaider definition for the BMW Siemens MS41 ECU.**

If you want to read, understand and reprogram an MS41 with RomRaider, this is the file to use. It is not a partial map pieced together from community guesses. It is built directly from the full decompilation of every MS41 software version, cross-checked in a simulator and confirmed against real ECUs.

## What it covers

This single definition file covers the entire MS41 family:

- **MS41.0**: ID41
- **MS41.1**: ID42, ID59 and ID60
- **MS41.2**: ID12

One file, every ECU. It is compatible with all known software indexes (**ID41, ID42, ID59, ID60 and ID12**). Each software is recognised automatically by the software number stored in the file (program identity at 0x2007, calibration identity at 0x1000C), so the correct layout is applied without any manual choice.

## How it was built

The definition was created from the **decompilation of all MS41 software versions**. Every software image was disassembled with the same reference parameters, then compared version by version to track every table that was added, removed or relocated between releases.

Because the work starts from the actual program code rather than from sampled addresses, it includes **all the scaling calculations and the comments needed to understand each table**: what it does, how its axes and values are encoded, and the units and sign conventions that apply. Nothing is left as an unexplained raw byte map.
No table and no constant has been left out. Everything is in the file.

## Software specificities

The MS41 family ships under several software indexes. Each one has its own purpose:

| ID | ECU | Part number / calibration | Market | Role |
|------|----------|----------|----------|-------------|
| **ID41** | MS41.0 | 1429861 / "0641" | Europe | Base European software, no OBD-II. Lambda control is optional: the maps of the variant without lambda control are present. |
| **ID42** | MS41.1 | 1430144 / "0942" | US | OBD-II version of the ID41: misfire, secondary air, EVAP, catalyst / O2 sensor and fuel system monitors, OBD-II service over the K-line. Always uses lambda control. |
| **ID59** | MS41.1 | 1429373 / "0659" | Europe | European MS41.1 software, no OBD-II, same functions as the ID41, plus its own anti-jerk, canister purge and O2 sensor heater items. |
| **ID60** | MS41.1 | 1437806 / "0960" | US | OBD-II evolution of the ID42: 97 fault records instead of 95, post-catalyst trim using the rear O2 sensors, lambda rich bias after overrun / secondary air. |
| **ID12** | MS41.2 | 1406464 / "0912" | US | M3 (S52) software. OBD-II like the ID42, plus rev limiters and a VANOS map that also depend on vehicle speed (3D maps instead of 2D), and max. RPM and hard-use records stored in the EEPROM, read and cleared over the K-line. |

Within the single universal file, each software's specific table layout (presence, size and location) is mapped per version, while the **scaling, units and descriptive comments stay identical across all of them**. A table means the same thing on every ECU; only its address changes where the software moved it. Differences between versions (tables that appear, disappear or shift, such as the 3D maps on ID12) are reflected directly in each version's address mapping. The tables that exist in only one software are grouped in red categories named after it ("ID42 OBD - ...", "ID60 - ...", "ID12 - M Specific", "ID59 - ...").

## Number of tables

Tables and scalars defined for each software (the 256 KB files add 5 items: the 2048 kg/h air flow meter mode and the program checksums):

| ID | Partial read (24 KB) | Full read / memory read (256 KB) |
|------|------|------|
| **ID41** | 691 | 696 |
| **ID42** | 961 | 966 |
| **ID59** | 669 | 674 |
| **ID60** | 990 | 995 |
| **ID12** | 954 | 959 |

## What you can do with it

The same definition applies to every way you get data out of the ECU, so your edits and interpretation stay consistent regardless of the source:

- **Full read**: the complete 256 KB image.
- **Partial read**: the 24 KB calibration area.
- **Memory read**: values read directly from the ECU's memory.

You can make modifications on full reads, partial reads and memory reads with the same tables, the same calculations and the same comments.

## Verification

Everything in this definition has been verified two ways:

1. **Against the decompilation, in the simulator.** Each table, address and formula was checked by running the software in an MS41 simulator and confirming the ECU behaves as the definition describes.
2. **Against real ECUs.** Values were read from physical calculators and matched to the definition.

If it is in this file, it was confirmed in software and on hardware.

---

*Created by Diolum.*
