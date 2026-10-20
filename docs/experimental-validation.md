# Experimental Validation

This page records observations from the prototype's oscilloscope captures. The captures are available in the [Waveform gallery](waveforms/README.md).

## Bench-test setup

- Target board: STM32F406G-DISC1 / STM32F406VGT6 is named in project notes; verify this against the physical board because an architecture diagram labels STM32F407G-DISC1.
- Firmware environment: STM32CubeIDE with STM32CubeMX-generated initialisation and STM32CubeF4 HAL.
- Debug/programming: onboard ST-LINK/V2-A through SWD.
- Instrument: dual-channel digital oscilloscope.
- Scope Channel A: SSI clock (red trace).
- Scope Channel B: SSI data (blue trace).
- Coupling and vertical scale: DC coupling, 2 V/division as recorded in the screenshots.
- Test configurations: nominal 10 kHz and nominal 100 kHz.

SPI1 receives the clock/data signals internally and compares each reconstructed 32-bit frame. Exact pin numbers and the full wiring table still need to be verified against the STM32CubeMX project and board wiring before reproducing the setup.

## Observations from the captures

| Observation | 10 kHz configuration | Nominal 100 kHz configuration |
|---|---|---|
| Clock burst | 32 evenly spaced pulses per frame in the described capture | 32 evenly spaced pulses per frame in the described capture |
| Inter-frame gap | Visible in multi-frame views | Visible in multi-frame view |
| Data transitions | Transitions appear within the clock-high window in the available capture | Transitions appear within the clock-high window in the available capture |
| Successive frames | Regular burst spacing is described | Regular burst spacing is described |
| Frequency counter | No readout captured in available 10 kHz screenshot | Approx. 93.08 kHz shown in one screenshot |

These are visual observations from the available captures, not the result of an independent repeat test.

## Caveat: configured vs measured clock

One capture is labelled as a nominal 100 kHz configuration, while the oscilloscope's automatic counter displays approximately 93.08 kHz. The counter may be including part of the inter-frame idle gap, but the capture does not prove that explanation. Place time cursors between adjacent rising edges inside a single clock burst and calculate the within-burst period.

## Caveat: inter-frame idle / monoflop

The design target for the frame gap is 30 µs, but the 10 kHz capture at 1 ms/div is captioned as a 65 µs monoflop time. The actual as-built interval has not been conclusively established. Treat this as an unresolved measurement discrepancy; do not claim that a 30 µs measured interval has been proven.

## Caveat: SPI counters

The SPI1 receive path is intended to compare reconstructed frames and maintain pass/fail counters, but this repository does not contain consolidated pass counts, fail counts, or bit-error-rate statistics.

## Follow-up measurements

- [ ] Measure clock period inside a pulse burst (separately at each configured setting).
- [ ] Measure actual inter-frame idle time with time cursors.
- [ ] Save SPI1 pass/fail counters with run duration and number of frames.
- [ ] Repeat for an extended test duration and log any failures.
- [ ] Record exact GPIO pins, wiring, timer clocks, PSC/ARR/CCR values and CubeMX configuration.
- [ ] Test against an external SSI master if available; use appropriate differential transceivers for a field-style interface.
- [ ] Sweep upward from 100 kHz if seeking the maximum reliable frequency.

## Evidence integrity

Keep oscilloscope evidence authentic and distinguish design targets from measured values. Add new measurements with the date, configuration, capture image, and measurement method.
