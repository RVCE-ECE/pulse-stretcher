# Programmable Digital Pulse Stretcher

A programmable digital pulse stretcher designed using Verilog HDL, verified using Cocotb-based simulation, and implemented for the Tiny Tapeout SKY26E shuttle through the ASIC design flow.

**Authors:** Anirudh Kulkarni and Ansh Agarwal
**Project Guides:** Shylashree N and Premananda BS
**Institution:** RV College of Engineering, Bengaluru

## Project Overview

The Programmable Digital Pulse Stretcher is a digital circuit designed to increase the duration of a short incoming pulse by generating an extended output pulse.

Short pulses may not remain active long enough for downstream digital circuits to reliably detect or process them. This design synchronizes the incoming pulse, detects its rising edge and maintains the output HIGH for a programmable number of clock cycles.

## Working Principle

The input pulse passes through a two-flip-flop synchronizer to synchronize it with the system clock. A rising-edge detector identifies a new pulse.

When a valid rising edge is detected, the circuit activates the output and loads a programmable countdown counter. The counter determines how long the output remains HIGH.

The design supports four operating modes:

| Mode | Operation | Description |
|------|-----------|-------------|
| 00 | Ignore while busy | Accepts a new pulse only when the output is LOW. |
| 01 | Retrigger | A new input pulse restarts the output duration. |
| 10 | Extend | A new input pulse adds its programmed duration to the remaining count. |
| 11 | Retrigger | A new input pulse restarts the output duration. |

A duration value of zero is treated as one clock cycle during normal loading.

## Architecture

The design is implemented in a single Verilog module named `tt_um_example`.

| Block | Function |
|-------|----------|
| Input Synchronizer | Two-flip-flop synchronizer for the incoming pulse. |
| Rising-Edge Detector | Detects a new pulse using the synchronized input and delayed signal. |
| Mode Selection Logic | Determines how incoming pulses affect an active output pulse. |
| Programmable Counter | Loads and decrements the configured pulse duration. |
| Output Control Logic | Maintains the output HIGH until the countdown completes. |

## Operating Flow

1. The circuit receives an input pulse through `ui_in[0]`.
2. The pulse passes through a two-stage synchronizer.
3. The rising-edge detector identifies the incoming pulse.
4. The selected operating mode determines how the counter is updated.
5. The programmable counter controls the output pulse duration.
6. When the counter reaches its terminal count, `pulse_out` becomes LOW.

**Overall flow:**

Input Pulse → Synchronizer → Rising-Edge Detection → Mode Selection → Programmable Counter → Extended Output Pulse

## Pin Mapping

| Pin | Direction | Function |
|-----|-----------|----------|
| `ui_in[0]` | Input | Incoming pulse |
| `ui_in[1]` | Input | User reset |
| `ui_in[3:2]` | Input | Operating mode selection |
| `ui_in[7:4]` | Input | Programmable pulse duration |
| `uo_out[0]` | Output | Extended output pulse |
| `uo_out[7:1]` | Output | Tied LOW |
| `uio_in[7:0]` | Input | Unused |
| `uio_out[7:0]` | Output | Tied LOW |
| `uio_oe[7:0]` | Output | All bidirectional pins configured as inputs |
| `clk` | Input | System clock |
| `rst_n` | Input | Active-low reset |
| `ena` | Input | Unused |

## How to Test

1. Apply the active-low reset through `rst_n` and release it.
2. Select the operating mode using `ui_in[3:2]`.
3. Set the required pulse duration using `ui_in[7:4]`.
4. Apply a rising-edge pulse through `ui_in[0]`.
5. Observe `uo_out[0]`, which becomes HIGH after synchronization and edge-detection latency.
6. Verify that the output remains HIGH for the programmed duration.
7. Apply additional input pulses to test retriggering and extension behaviour.

The user reset can also be asserted through `ui_in[1]`.

## Verification

The design is verified using Cocotb-based automated testbenches in `test/test.py`, with Icarus Verilog used for simulation.

The testbench covers:

1. Reset operation and output initialization.
2. Mode 00: Normal pulse stretching and ignoring new pulses while busy.
3. Mode 01: Retrigger operation.
4. Mode 10: Pulse extension operation.
5. Mode 11: Retrigger operation.
6. Different programmable durations: 1, 2, 4 and 8.

## Tools and Technologies

- Verilog HDL
- Icarus Verilog
- Cocotb
- GTKWave
- LibreLane
- Tiny Tapeout
- SkyWater 130nm (SKY130) process


## Acknowledgements

We gratefully acknowledge the Department of Electronics and Communication Engineering and the Center of Excellence in Integrated Circuits and Systems (CoE-ICAS), RV College of Engineering, for providing the necessary resources and facilities.

Special thanks to Dr. H V Ravish Aradhya (HoD - ECE), Dr. Nagamani K (HOD - ETE), Dr. K S Geetha (Vice Principal), and Dr. K N Subramanya (Principal) for their constant encouragement and support in facilitating this Tiny Tapeout SKY26E submission.

Developed at RV College of Engineering, Bengaluru, Karnataka, India.
