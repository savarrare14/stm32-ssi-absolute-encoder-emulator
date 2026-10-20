# SSI Protocol Background

This page provides protocol background needed to understand the SSI emulator. For an actual integration, use the target encoder manufacturer's specifications.

## Why an absolute encoder?

Absolute encoders provide position feedback in systems such as servo drives, CNC axes, robotic joints, and rotary tables. An absolute encoder reports a position code directly, including after power-up; an incremental encoder typically reports relative movement through pulses and depends on counting and reference procedures.

The project's frame combines single-turn position with a multiturn revolution count.

## SSI transaction model

SSI is a synchronous, unidirectional data interface in which the receiving controller normally supplies the clock. The encoder shifts out the current position word, typically MSB-first. A transaction consists of a clock burst for the required number of bits, followed by an idle interval before a new position is loaded for the next transaction.

A typical SSI transaction follows this sequence:

1. The first falling clock edge latches the current position into the output register.
2. The following rising edge presents the MSB.
3. Subsequent clock edges advance the output register one bit at a time.
4. After the expected number of bits, the master stops clocking.
5. The clock remains idle for the specified monoflop interval before the next transaction.

The emulator's bench setup generates the clock internally with TIM2 and observes the signals on an oscilloscope. It does not use an independent external SSI master, so the measurements validate the local signal path rather than a complete master-to-encoder connection.

## Electrical interface

Field SSI wiring commonly uses differential signalling through RS-422 or RS-485 transceivers, with clock and data pairs (Clock+, Clock−, Data+, Data−). Differential signalling improves tolerance to common-mode interference in electrically noisy industrial environments.

The development-board GPIO signals are not a substitute for the differential line-driver stage required by a typical cable-based field interface. This project does not currently include a field-ready line-driver design.

## Timing parameters

Important SSI timing concepts include:

- **Clock period T:** time between clock edges / cycles.
- **Propagation delay tv:** time from a clock edge to the corresponding data transition.
- **Monoflop / idle time tm:** required idle interval between frames.

The design aims to keep data transitions within the clock-high window, using the constraint tv < T/2 as a timing rationale. TIM2 CC4 is intended to update the data line within the PWM high phase so the data settles before the next sampling edge.

The inter-frame monoflop design target is 30 µs, but one 10 kHz capture is captioned as 65 µs. The actual idle interval still needs a direct measurement.

## Cable-length guidance

The cited SICK SSI guidance gives the following example values:

| Cable length | Example recommended baud rate from the cited SSI guidance |
|---|---:|
| Less than 50 m | Less than 400 kHz |
| Less than 100 m | Less than 300 kHz |
| Less than 200 m | Less than 200 kHz |
| Less than 400 m | Less than 100 kHz |

These values are reference guidance only; check the current encoder manufacturer's documentation for a real installation.

## Strengths and limitations

SSI offers low protocol overhead, simple fixed-frame operation, broad vendor adoption, and robustness when used with differential signalling. Limitations include no generally mandatory CRC in base SSI, vendor-specific error fields, unidirectional data, and lower practical baud rates as cable length increases.

For the project's comparison with bidirectional EnDat 2.2, see [SSI and EnDat comparison](EnDat-comparison-and-future-work.md).
