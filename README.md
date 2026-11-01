# Protocol-emulator-ASIC-competition
Programmable ASIC protocol emulator supporting UART, SPI, and I2C for the Jane Street Tiny Tapeout competition.

Current stage: establish the official Tiny Tapeout CMOS5L template baseline.
The hardware is still the template's example adder; UART, SPI, I2C, and the
programmable engine are not implemented yet.

- [Competition rules and agreed scope](COMPETITION_RULES.md)
- [Baseline setup, provenance, checks, and workflow instructions](docs/baseline.md)
- [Project datasheet](docs/info.md)
- [Template test instructions](test/README.md)

The template was merged from the official
[TinyTapeout/ttihp-verilog-template](https://github.com/TinyTapeout/ttihp-verilog-template).
The official GitHub Actions flow builds GDS with LibreLane, runs precheck,
and runs gate-level tests. See the baseline guide before launching it.

Repository layout:

- `src/`: unchanged example Verilog and official physical-flow configuration.
- `test/`: matching Cocotb test, Verilog harness, and simulation Makefile.
- `info.yaml`: project metadata, source list, pin descriptions, and 6x4 tile size.
- `.github/workflows/`: official RTL test, GDS, documentation, and FPGA workflows.
- `docs/`: datasheet and baseline instructions.
- `.devcontainer/` and `.vscode/`: optional upstream development setup.

AI assistance was used to inspect and merge the template and document the
baseline setup. Verification results are recorded separately from configuration.
