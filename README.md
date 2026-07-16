# AirWick Pro

A small custom PCB ("AIRWICK_PRO", board `Board1`, rev `V1.0`) designed in
[EasyEDA Pro](https://pro.easyeda.com/). Based on the schematic, it's a 5V-in,
regulated-output driver board with a GPIO-switched low-side MOSFET output —
the kind of add-on board used to electronically trigger an aerosol/spray
dispenser (matching the project's name) from an external microcontroller
module. The specific module isn't part of this schematic; see
[Hardware notes](#hardware-notes) below for what is and isn't certain from
the source file.

## Schematic

![Schematic](hardware/images/schematic.webp)

## PCB layout

![PCB layout](hardware/images/pcb_layout.webp)

## Bill of materials

| Designator | Part | Role |
| --- | --- | --- |
| H2 | 2-pin header, 2.54mm (`HX PH254-01-02-Z-L11.5`) | +5V power input (pin 1 = +5V, pin 2 = GND) |
| U1 | LM317 | Adjustable linear voltage regulator |
| R1 | 220 Ω | LM317 feedback resistor (VOUT → ADJ) |
| R2 | 360 Ω | LM317 feedback resistor (ADJ → GND) |
| D1 | SS34 (Schottky) | In line with the regulated output, before H1 |
| H1 | 2-pin header, 2.54mm (`HX PH254-01-02-Z-L11.5`) | Regulated output connector |
| Q1 | AO3400A (N-channel MOSFET) | Low-side switch |
| R3 | 10 kΩ | Gate pull-down for Q1 |
| U3, U4 | Two 1×8 headers, 2.54mm (`JXT-PM2.54-1X8P-A9`) | Module/expansion interface |

## Hardware notes

- **Power**: +5V and GND come in through H2. U1 (LM317) regulates this down
  to a fixed rail set by the R1/R2 divider:
  `VOUT = 1.25V × (1 + R2/R1) ≈ 1.25V × (1 + 360/220) ≈ 3.3V`.
  The regulated rail passes through Schottky diode D1 on its way to H1.
- **Switch**: Q1 (AO3400A) is a logic-level N-channel MOSFET wired as a
  low-side switch, with R3 holding its gate low (off) by default until
  driven by an external control signal.
- **U3/U4 headers**: these are two 1×8, 2.54mm-pitch headers placed opposite
  each other along the board edges (visible in the PCB layout). That
  arrangement is consistent with a socket for a pluggable microcontroller
  module (e.g. a Wemos D1 mini / ESP8266-style board), which would be the
  natural source of the MOSFET's gate-drive signal — but the module itself
  isn't part of this schematic/PCB, and the exact pin mapping wasn't fully
  recoverable from the source file, so treat that as an educated read of the
  layout rather than a confirmed netlist.

## Repository contents

```
hardware/
  images/
    schematic.webp     — full schematic render
    pcb_layout.webp     — PCB layout render (top view)
  source/
    ProPrj_AIRWICK_PRO_2026-07-16.epro2  — original EasyEDA Pro project archive
```

## Opening the project

The `.epro2` file in `hardware/source/` is the original EasyEDA Pro project
archive (a zip containing the schematic/PCB data plus its own image
previews). Open it in [EasyEDA Pro](https://pro.easyeda.com/) via
**File → Import → Project**. It was last saved with editor version
`3.2.149.88089769`.
