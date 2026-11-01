
## Goal and judging

Build an open-source, reprogrammable protocol emulator ASIC.
It must support new protocols after fabrication by loading new programs
or configuration, rather than only selecting fixed protocol hardware.

Flexibility is the main judging priority. Other considerations include
demonstrated protocols, architectural originality, and design/verification
quality. There are no fixed judging weights.

A CPU is not required. Any architecture meeting the goal is allowed.

## Deadline and team

- Submission deadline: January 18, 2027.
- Students and professionals may enter.
- No team-size limit; one entry per team.
- Development may be private, but the submitted repository must be public.
- AI tools are allowed; disclose their use in the write-up.

## Hardware constraints

- Process: IHP CMOS5L through the official Tiny Tapeout flow.
- Start with 6x4 tiles. Use 8x4 only after official availability is confirmed.
- Available signals: 8 digital inputs, 8 digital outputs,
  8 bidirectional digital pins, plus clock and reset.
- No analog pads. Internal analog/mixed-signal circuitry is allowed
  if it passes precheck.
- Demo-board I/O is 3.3 V.
- External pull-ups, transceivers, and level shifters are allowed;
  document all required external hardware.
- IHP SRAM macros are allowed, including multiple macros,
  provided the design fits and passes precheck.
- No required clock frequency. Choose a frequency that meets protocol
  needs and closes timing after place-and-route.

## Protocol expectations

- Start with UART, SPI, and I2C.
- Suggested targets: common UART baud rates, SPI modes 0–3,
  and I2C at 100/400 kHz.
- There are no fixed minimum protocol speeds.
- Concurrent protocols and both controller/target roles are bonuses,
  not requirements.
- Low-speed USB and 10 Mbit Ethernet are stretch goals.
- Include a software toolchain and example programs for supported protocols.

## Loading, reset, and power

- Design and document how programs/configuration are loaded.
- The demo-board microcontroller is a suggested host, not mandatory.
- Use rst_n to establish a known, safe state.
- Do not assume flip-flops or memory power up initialized.
- Deselection powers the design down and loses its state.
  Support reloading after selection/power loss.

## Required submission

A public repository containing:

- RTL and tests.
- Passing GDS generation and precheck using the Tiny Tapeout flow.
- Architecture and instruction-set/configuration documentation.
- Assembler, compiler, or other applicable toolchain.
- Example protocol programs.
- Program-loading instructions and external hardware requirements.
- Verification approach and results.
- AI-use disclosure.

A video/demo is optional.

## Physical design flow

- Use the official Tiny Tapeout flow, not a replacement GDS flow.
- GitHub Actions jobs have a six-hour limit.
- The same Tiny Tapeout flow may run locally if necessary.
- Validate timing after place-and-route.
- After the deadline, small non-functional fixes may be allowed;
  do not plan to add functionality after submission.

## Our initial project scope (team choices, not competition rules)

- One programmable pin engine.
- Verilog RTL and Python tooling/tests.
- First milestone: load a program and demonstrate UART transmission.
- Then add input sampling, branching, UART reception, SPI, and I2C.
- Verify reset, loading, pin output enables, and protocol timing.
- Run the physical flow early on the actual engine.
- Defer USB, Ethernet, multiple engines, and SRAM integration
  unless the verified base design needs or permits them.
- Target January 4 for a complete candidate and January 15 for submission.

## References

- Competition:
  https://blog.janestreet.com/protocol-emulator-asic-competition/
- Official template:
  https://github.com/TinyTapeout/ttihp-verilog-template
- SRAM integration:
  https://kdp1965.github.io/ihp-um-janestreet-prism/ihp_sram_macro.html
- Demo board:
  https://tinytapeout.com/specs/pcb-etr/
- Pinouts:
  https://tinytapeout.com/specs/pinouts/
- ASIC reset/power considerations:
  https://tinytapeout.com/hdl/fpga_vs_asic/
- Local hardening:
  https://tinytapeout.com/guides/local-hardening/