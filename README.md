# MS41 RomRaider Definition

**The definitive RomRaider definition for the BMW Siemens MS41 ECU.**

If you want to read, understand and reprogram an MS41 with RomRaider, this is the file to use. It is not a partial map pieced together from community guesses. It is built directly from the full decompilation of every MS41 software version, cross-checked in a simulator and confirmed against real ECUs.

## What it covers

This single definition file covers the entire MS41 family:

- **MS41.0**
- **MS41.1**
- **MS41.2**

One file, every ECU. It is compatible with all known software indexes (**ID41, ID42, ID59, ID60 and ID12**), with each software recognised by its Soft Idx so the correct layout is applied automatically.

## How it was built

The definition was created from the **decompilation of all MS41 software versions**. Every software image was disassembled with the same reference parameters, then compared version by version to track every table that was added, removed or relocated between releases.

Because the work starts from the actual program code rather than from sampled addresses, it includes **all the scaling calculations and the comments needed to understand each table**: what it does, how its axes and values are encoded, and the units and sign conventions that apply. Nothing is left as an unexplained raw byte map.

## Software specificities

The MS41 family ships under several software indexes. Each one has its own purpose:

| ID | Software | Description |
|------|----------|-------------|
| **ID41** | MS41.0 | EU version. |
| **ID42** | MS41.1 | US / OBD-compliant version. |
| **ID59** | MS41.1 | Update of ID41. |
| **ID60** | MS41.1 | Update with OBD compliance. |
| **ID12** | MS41.2 | M3 US. Uses a specific K-line command for hard use, and 3D maps instead of 2D. |

Within the single universal file, each software's specific table layout (presence, size and location) is mapped per version, while the **scaling, units and descriptive comments stay identical across all of them**. A table means the same thing on every ECU; only its address changes where the software moved it. Differences between versions (tables that appear, disappear or shift, such as the 3D maps on ID12) are reflected directly in each version's address mapping.

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
