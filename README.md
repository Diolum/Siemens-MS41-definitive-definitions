# MS41 RomRaider Definition

**The definitive RomRaider definition for the BMW Siemens MS41 ECU.**

If you want to read, understand and reprogram an MS41 with RomRaider, this is the file to use. It is not a partial map pieced together from community guesses: it is built directly from the full decompilation of every available MS41 software version, and checked against the program code, in an MS41 simulator and on real ECUs (see Verification).

## What it covers

This single definition file covers the entire MS41 family:

- **MS41.0**: ID59, ID41 (index 00 and 01) and ID43
- **MS41.1**: ID42, ID60 (US and EOBD) and ID85
- **MS41.2**: ID12 (index 01 and 02)

One file, every ECU. It is compatible with ten software versions (**ID59, ID41 index 00, ID41, ID43, ID42, ID60, ID60 EOBD, ID85, ID12 index 01 and ID12**). Each software is recognised automatically by the software number stored in the file, so the correct layout is applied without any manual choice: program identity at 0x2007 in a memory read and at 0x6007 in a full read (16 KB blocks swapped), calibration identity at 0x000C in a partial read. The part number is not checked.

## How it was built

The definition was created from the **decompilation of all the MS41 software versions available**. Every software image was disassembled with the same reference parameters, then compared version by version to track every table that was added, removed or relocated between releases.

Because the work starts from the actual program code rather than from sampled addresses, it includes **all the scaling calculations and the comments needed to understand each table**: what it does, how its axes and values are encoded, and the units and sign conventions that apply. Nothing is left as an unexplained raw byte map.
No table and no constant has been left out: every calibration address read by the program code is defined, except the table headers (axis pointers) that RomRaider does not need.

## Software specificities

The MS41 family ships under several software indexes. Each one has its own purpose:

| ID | ECU | Part number / calibration | Market | Role |
|------|----------|----------|----------|-------------|
| **ID41** | MS41.0 | 1429861 / "0641" | Europe | Base European software, no OBD-II. Lambda control is optional: the maps of the variant without lambda control are present. |
| **ID42** | MS41.1 | 1430144 / "0942" | US | OBD-II version of the ID41: misfire, secondary air, EVAP, catalyst / O2 sensor and fuel system monitors, OBD-II service over the K-line. Always uses lambda control. |
| **ID59** | MS41.0 | 1429373 / "0659" | Europe | First flashable MS41.0 software (1995), predecessor of the ID41, no OBD-II, same functions as the ID41, plus its own anti-jerk, canister purge and O2 sensor heater items. The stock program limits K-line memory reads to 0x2FFFF (program CRC 0xD158). |
| **ID41 index 00** | MS41.0 | 1432401 / "0641" | Europe | First index of the ID41 program, same calibration layout as the ID41 (recognised by the program number "064100"). |
| **ID43** | MS41.0 | 7505858 / "0643" | Europe | Revision of the ID41: A/C compressor cut-off on coolant over-temperature. |
| **ID60** | MS41.1 | 1437806 / "0960" | US | OBD-II evolution of the ID42: 97 fault records instead of 95, post-catalyst trim using the rear O2 sensors, lambda rich bias after overrun / secondary air. |
| **ID60 EOBD** | MS41.1 | 1440176 / "0960" | Europe | Index 01 of the ID60 for the European M52B25: EOBD standard, check engine light limited to the EOBD faults. Same calibration layout as the ID60. |
| **ID85** | MS41.1 | 1437804 / "0985" | US | Revision of the ID60 for the E39 with ORVR: EVAP leak test by leak detection pump (LDP) and barometric pressure sensor, 98 fault records. Its partial definition also opens the S52B32 calibration 1438137 (Alpina B10 3.2). |
| **ID12 index 01** | MS41.2 | 1405854 / "0912" | US | Previous index of the ID12 program, same calibration layout as the ID12 (recognised by the program number "091201"). |
| **ID12** | MS41.2 | 1406464 / "0912" | US | M3 (S52) software, index 02. OBD-II like the ID42, plus rev limiters and a VANOS map that also depend on vehicle speed (3D maps instead of 2D), and max. RPM and hard-use records stored in the EEPROM, read and cleared over the K-line. |

Within the single universal file, each software's specific table layout (presence, size and location) is mapped per version, while the **scaling, units and descriptive comments stay identical across all of them**. A table means the same thing on every ECU; only its address changes where the software moved it. Differences between versions (tables that appear, disappear or shift, such as the 3D maps on ID12) are reflected directly in each version's address mapping. The tables that exist in only one software are grouped in red categories named after it ("ID42 OBD - ...", "ID60 - ...", "ID85 OBD - LDP Leak Detection Pump", "ID12 - M Specific", "ID59 - ...").

## Number of tables

Tables and scalars defined for each software (the 256 KB files add 5 items: the 2048 kg/h air flow meter mode and the program checksums):

| ID | Partial read (24 KB) | Full read / memory read (256 KB) |
|------|------|------|
| **ID59** | 669 | 674 |
| **ID41 index 00** | uses the ID41 partial definition | 696 |
| **ID41** | 691 | 696 |
| **ID43** | 693 | 698 |
| **ID42** | 961 | 966 |
| **ID60** | 990 | 995 |
| **ID60 EOBD** | uses the ID60 partial definition | 995 |
| **ID85** | 1001 | 1006 |
| **ID12 index 01** | uses the ID12 partial definition | 959 |
| **ID12** | 954 | 959 |

## What you can do with it

The same definition applies to every way you get data out of the ECU, so your edits and interpretation stay consistent regardless of the source:

- **Full read**: the complete 256 KB image.
- **Partial read**: the 24 KB calibration area.
- **Memory read**: values read directly from the ECU's memory.

You can make modifications on full reads, partial reads and memory reads with the same tables, the same calculations and the same comments.

## Verification

The definition was checked three ways:

1. **Against the program code.** Every item was matched with the code that reads it in the decompiled program of each version: address, size, scaling and meaning. A last check confirms that every calibration address read by each of the ten programs is defined.
2. **In the simulator.** The software was run in an MS41 simulator to confirm that the ECU behaves as the definition describes.
3. **On real ECUs.** Values read from physical ECUs were compared with the definition.

The simulator and real ECU checks were done on the versions available as hardware or full reads. The ID43, ID85, ID41 index 00, ID60 EOBD and ID12 index 01 definitions were derived from their decompiled programs, by matching every item with the code that reads it, and checked against the ROM files of these versions.

## Wanted: missing software versions

BMW delivered more MS41 programs than the ten covered here. The ones below are still missing. If you own an ECU running one of them, or a full read of it, I will gladly receive it: I will decompile and analyse it, then add it to this definition.

| Program (part number) | ECU | Line | Typical car | What it would bring |
| --- | --- | --- | --- | --- |
| 1429374, 1429519, 1429542, 1432402, 1429773 | MS41.1 | US, E36 | E36 / Z3 US 1996-1997 | The first OBD-II versions, before the ID42: how the monitors (misfire, catalyst, EVAP) appeared version by version |
| 1429088 | MS41.1 | US, E36 | E36 US 1995-1996 | The very first US program |
| 4509679, 1430275 | MS41.1 | US, E39 ORVR | E39 528i US 1997-1998 | The E39 versions before the ID85: when the LDP pump and the barometric sensor appeared |
| 1405491 | MS41.2 | US, M3 S52 | M3 E36 US 1996 | The first M3 S52 version, before the ID12 index 01 |
| 1427365 | MS41.0 | Europe | M52 Europe 1995 | The program before the ID59 |
| hardware 1427703 | MS41.0 (not flashable) | Europe | early M52 Europe | The program of the first MS41, which cannot be reprogrammed over the K-line |

What is needed is a **full read (256 KB)** of an ECU that has never been updated: a partial read (24 KB) only contains the calibration, not the program. In a read, the program number is the "1110..." reference of the identity block at 0x2000. For the non-flashable ECU (hardware 1427703), a read of the memory chip with a programmer also works.

You can send it through diolum.fr.

---

*Created by Diolum.*
