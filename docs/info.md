<!---

This file is used to generate your project datasheet. Please fill in the information below and delete any unused
sections.

You can also include images in this folder and reference them in the markdown. Each image must be less than
512 kb in size, and the combined size of all images must be less than 1 MB.
-->

## How it works

This baseline keeps the official Tiny Tapeout example hardware unchanged.
The eight dedicated outputs show the low eight bits of `ui_in + uio_in`.
All bidirectional pins remain inputs (`uio_oe = 0`), and `uio_out = 0`.
The example is combinational and does not use clock, reset, or enable.
It is not yet a programmable protocol emulator.

## How to test

Run the Cocotb test in `test/` using `make -B`, then
`python -m cocotb_tools.check_results results.xml`.
The unchanged test applies reset, drives 20 and 30, and checks for 50.
See [baseline.md](baseline.md) for setup, provenance, and GitHub Actions steps.

## External hardware

None for simulation. A hardware demonstration needs a compatible Tiny Tapeout
demo board and a way to drive and observe the digital pins.
