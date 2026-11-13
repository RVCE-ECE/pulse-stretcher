## How it works

The programmable digital pulse stretcher extends the duration of a short input pulse by a configurable number of clock cycles.

When an input pulse is detected, the circuit starts a counter. The output is held high while the counter is active. The pulse-stretching duration is controlled using the programmable configuration inputs, allowing different output pulse widths to be selected.

The design is implemented using synthesizable Verilog and is designed for the Tiny Tapeout digital ASIC flow.

## How to test

Apply a clock signal and a short pulse to the input. Set the desired pulse-stretching value using the configuration inputs.

When the input pulse is asserted, observe the output. The output should remain high for the programmed number of clock cycles, producing a longer pulse than the original input pulse.

Test different configuration values and verify that the output pulse duration changes accordingly.

## External hardware

No external hardware is required. The design can be tested using the Tiny Tapeout testbench and simulation environment.

## Credits

We gratefully acknowledge the Center of Excellence (CoE) in Integrated Circuits and Systems (ICAS) and the Department of Electronics and Communication Engineering (ECE), RV College of Engineering, for providing the necessary resources and facilities.

We sincerely thank our project guides, Shylashree N and Premananda BS, for their valuable guidance, encouragement and support throughout the development of this Programmable Digital Pulse Stretcher.

**Developed by:** Anirudh Kulkarni and Ansh Agarwal, RV College of Engineering, Bengaluru.
